# Ćwiczenie 6. Konflikt i jak go rozwiązać

**Cel:** celowo wywołać konflikt i rozwiązać go: najpierw w przeglądarce, potem (dla chętnych) w codespace.

> [!NOTE]
> **Konflikt** oznacza, że dwie osoby zmieniły tę samą linię tego samego pliku. Git nie zgaduje, która wersja jest właściwa, i oddaje decyzję wam. To nie awaria i nic nie ginie.

## Część A. W przeglądarce, przez pull request

> [!NOTE]
> **Gałąź** to brudnopis obok głównej wersji `main`. **Pull request** to prośba o włączenie brudnopisu do `main`. Właśnie w pull requeście GitHub pozwala rozwiązać konflikt na stronie.

1. **Osoba B, na github.com:** `README.md` → ołówek → zmień pierwszą linię (tytuł z `#`) na swoją propozycję → **Commit changes…** → wybierz **Create a new branch for this commit and start a pull request**, nazwa gałęzi `tytul-b` → **Propose changes** → **Create pull request**. Pull requesta jeszcze nie scalaj.
2. **Osoba A, dopiero po kroku B:** wejdź na stronę główną repozytorium (zakładka **Code**; nad listą plików musi być gałąź `main`) → `README.md` → ołówek → zmień tę samą linię na inny tytuł → **Commit changes…** → **Commit directly to the main branch**.
3. Otwórz pull request (zakładka **Pull requests**) i odśwież stronę. Pojawi się komunikat **This branch has conflicts that must be resolved**. Kliknij **Resolve conflicts**.
4. Edytor pokaże obie wersje między znacznikami:

    ```
    <<<<<<< tytul-b
    # tytuł osoby B
    =======
    # tytuł osoby A
    >>>>>>> main
    ```

5. Ustalcie wspólny tytuł. Zostawcie jedną linię albo napiszcie nową, a trzy linie ze znacznikami (`<<<<<<<`, `=======`, `>>>>>>>`) usuńcie w całości.
6. **Mark as resolved** → **Commit merge**, potem **Merge pull request** → **Confirm merge** → **Delete branch**.

> [!TIP]
> **Dlaczego najpierw gałąź, potem `main`?** Gałąź `tytul-b` powstała ze starej wersji pliku. Gdy potem ktoś zmieni tę samą linię w `main`, Git ma dwie różne zmiany jednego miejsca i nie wie, którą wybrać. W odwrotnej kolejności osoba B zobaczyłaby w edytorze już nowy tytuł osoby A i konfliktu by nie było.

> [!NOTE]
> 👤 **Pracujesz w pojedynkę?** Zrób kroki 1 i 2 w karcie z github.com, w tej kolejności: najpierw gałąź i pull request, potem zmiana w `main`. W kroku 5 wybierz jeden z dwóch tytułów albo napisz nowy.

✅ **Punkt kontrolny:** w `README.md` na `main` jest jeden tytuł, bez znaczników `<<<<<<<`, a pull request ma status **Merged**.

## Część B. W codespace (dla chętnych)

1. **Osoba B, na github.com:** zmień w `README.md` linię „Zespół:” i zrób commit do `main`.
2. **Osoba A, w codespace, bez wcześniejszego Sync:** zmień tę samą linię inaczej → zapisz → **Commit** → **Sync Changes**.
3. VS Code oznaczy `README.md` jako plik z konfliktem (litera **!** w **Kontroli źródła**). Otwórz go.
4. Nad konfliktem są przyciski: **Accept Current Change** (twoja wersja), **Accept Incoming Change** (wersja z GitHuba), **Accept Both Changes** (obie). W polskim interfejsie mają przetłumaczone nazwy, w tej samej kolejności. Jest też **Resolve in Merge Editor**. Wybierz wersję albo popraw tekst ręcznie i usuń znaczniki.
5. Zapisz plik → w **Kontroli źródła** kliknij **+** przy pliku (albo **Complete Merge**) → **Zatwierdź** (*Commit*; opis jest już wpisany) → **Synchronizuj zmiany** (*Sync Changes*).

> [!NOTE]
> 👤 **Pracujesz w pojedynkę?** Krok 1 zrób w karcie z github.com, krok 2 w karcie z codespace. Między nimi nie klikaj w codespace **Synchronizuj zmiany**: konflikt powstaje właśnie dlatego, że codespace nie zna jeszcze zmiany z github.com.

> [!WARNING]
> **Konflikty w notatnikach są trudniejsze.** Notatnik to w środku jeden długi plik z kodem, wynikami i numerami uruchomień. Gdy dwie osoby uruchomią ten sam notatnik, zmiany są w wielu miejscach naraz i konflikt robi się nieczytelny. Dlatego jeden notatnik ma jednego właściciela. Jeśli konflikt w notatniku jednak się zdarzy:
> 1. Naciśnij **F1** i wybierz **Merge Conflict: Accept All Incoming**, czyli wersję z GitHuba w całości (albo **Accept All Current**, czyli swoją).
> 2. Zapisz, zrób Commit i Sync.
> 3. Brakujące komórki z drugiej wersji przepisz ręcznie albo poproś prowadzącego o pomoc.
>
> Każda wcześniejsza wersja jest w historii: na github.com otwórz plik → **History**.

> [!TIP]
> **Błąd „Need to specify how to reconcile divergent branches”:** w codespace **View → Terminal**, wpisz `git config --global pull.rebase false`, naciśnij Enter i kliknij Sync jeszcze raz. W nowych codespace to ustawienie jest już włączone.

[← Ćwiczenie 5](05_commit_i_sync.md) · [Spis treści](README.md) · [Ćwiczenie 7 →](07_github_pages.md)
