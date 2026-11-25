# AI w projekcie: zasady (AGENTS.md) i polecenia (skills)

**Po co ten rozdział:** żeby wiedzieć, co dzieje się „pod spodem”, kiedy piszecie w Czacie `/zrodlo`, i umieć dopisać własne polecenie. To lektura na 15 minut, bez zadań na klawiaturze.

## Dwie role AI w projekcie

Ta sama technologia pełni w waszym projekcie dwie bardzo różne role.

| | Asystent | Koder w badaniu |
|---|---|---|
| Co robi | pomaga pisać kod, szukać informacji, rozumieć błędy | czyta teksty i przypisuje im kategorie z karty kodowej, z cytatem-dowodem |
| Gdzie | Czat w codespace, polecenia `/zrodlo`, `/pobierz`, `/wyjasnij` | notatnik, API modelu językowego, `prompts/karta_kodowa…` |
| Typowy błąd | wymyśla parametry, adresy i limity | błędy systematyczne, np. myli ironię z pochwałą |
| Jak się bronimy | sprawdzamy w dokumentacji i na małej próbce | walidacja: próba kodowana ręcznie, bez patrzenia na wynik modelu |

Błędy asystenta zwykle widać od razu: kod nie działa albo dokumentacja mówi co innego. Błędy kodera stają się danymi w waszym badaniu i łatwo ich nie zauważyć. Dlatego koder wymaga karty kodowej, cytatu-dowodu i walidacji (pole 6 karty projektu).

> [!IMPORTANT]
> Przy was zostaje: pytanie badawcze, kategorie, interpretacja i ocena wyników.

## Kto decyduje na każdym etapie

| Etap | Wy | AI |
|---|---|---|
| Pytanie badawcze | formułujecie i uzasadniacie | nie bierze udziału |
| Źródło | wybieracie, czytacie regulamin | `/zrodlo`: szkic karty źródła |
| Próbka | oglądacie i akceptujecie | `/pobierz`: kod pobrania |
| Karta kodowa | definiujecie kategorie | pomaga sformułować prompt |
| Kodowanie | kodujecie ręcznie próbę do walidacji | koder: kategorie i cytaty |
| Wykres | wybieracie pytanie i formę | `/wykres`: kod wykresu |
| Interpretacja | piszecie wnioski i granice | pomaga zredagować, nie stawia tezy |

Każdą decyzję zapisujecie w dzienniku decyzji w `README.md`. Ten dziennik to wasza metodologia: bez niego wyników nie da się powtórzyć ani obronić.

## Co widzi model

Model językowy nie pamięta waszego projektu. Każda rozmowa zaczyna się od zera, a model widzi tylko to, co dostanie w **kontekście**, czyli w tekście wysłanym razem z waszym pytaniem. W tym szablonie do kontekstu trafiają:

| Co | Kiedy |
|---|---|
| wasza prośba | zawsze |
| `AGENTS.md`, czyli zasady projektu | zawsze, przy każdym pytaniu |
| opisy wszystkich poleceń (nazwa i jedno zdanie) | zawsze: dzięki nim asystent wie, że polecenia istnieją i kiedy ich użyć |
| pełna treść pliku `SKILL.md` polecenia | dopiero po wywołaniu; w Czacie widać wtedy znacznik **Read skill** |
| otwarte pliki, np. notatnik | gdy są widoczne w edytorze |

Wniosek praktyczny: **czego nie ma w kontekście, tego model nie wie.** Jeśli ma znać regulamin serwisu albo decyzję sprzed tygodnia, musi to być zapisane w pliku (karta źródła, dziennik decyzji) albo w waszej prośbie.

> [!TIP]
> Listę plików instrukcji, które VS Code wykrył w repozytorium, zobaczycie w Czacie: ikona **Otwórz dostosowania** (*Open Customizations*) → **Instrukcje**. W tym szablonie są tam trzy pliki: `AGENTS.md`, `CLAUDE.md` i `.github/copilot-instructions.md`.

## AGENTS.md: zasady projektu

`AGENTS.md` leży w głównym folderze repozytorium. To zwykły tekst po polsku, który opisuje, jak asystent AI ma pracować w waszym projekcie: po polsku i małymi krokami, z kluczami tylko z sekretów, z danymi pobieranymi w kolejności plik → API → scraping i z pseudonimizacją nicków.

- **Kto go czyta:** GitHub Copilot i Codex czytają `AGENTS.md` bezpośrednio. Claude czyta `CLAUDE.md`, który w tym szablonie tylko importuje `AGENTS.md`. Plik `.github/copilot-instructions.md` też odsyła do `AGENTS.md`, na wypadek gdyby któryś tryb Copilota go pominął. Zasady są więc w jednym miejscu.
- **Czy można go zmieniać:** tak, to wasza metoda pracy z AI. Jeśli dopisujecie zasadę (np. „wykresy zawsze w tych samych kolorach kategorii”), zapiszcie zmianę w dzienniku decyzji.
- **Czego tam nie wpisywać:** kluczy, haseł, danych osobowych. Plik jest publiczny razem z repozytorium.

## Polecenia (skills)

Polecenie to folder w `.github/skills/` z plikiem `SKILL.md`. Na górze jest krótki nagłówek, niżej zwykły tekst instrukcji:

```markdown
---
name: zrodlo
description: Rozpoznanie źródła danych przed pobieraniem, bez pisania kodu. …
argument-hint: co i skąd chcesz pobrać
---

# Rozpoznanie źródła danych

Nie pisz kodu. Rozpoznaj, czy i jak można pobrać materiał.

## Kroki
1. Ustal: co, skąd, ile i po co.
2. Opisz trzy drogi: plik, API, scraping.
3. Wypisz sprawy prawne i etyczne.
4. Zaproponuj próbkę na start.
5. Wypełnij kartę źródła.
```

- **`name`** to nazwa po ukośniku: `/zrodlo`. Musi być taka sama jak nazwa folderu.
- **`description`** to zdanie, po którym asystent poznaje, kiedy użyć polecenia. Opisy wszystkich poleceń są w kontekście zawsze.
- **`argument-hint`** to podpowiedź w polu Czatu: co dopisać po nazwie polecenia.
- **Treść** to opis metody: kroki, zasady, co oddać na końcu. Polecenie nie jest programem, tylko instrukcją, taką jak dla nowej osoby w zespole badawczym.

| Polecenie | Co robi | Kiedy działa |
|---|---|---|
| `/zrodlo` | rozpoznanie źródła i szkic karty źródła, bez kodu | na wywołanie albo samo, gdy prośba pasuje do opisu |
| `/pobierz` | kod pobrania małej próbki do notatnika | tylko na wywołanie |
| `/koduj` | kod kodowania tekstów modelem według karty kodowej | tylko na wywołanie |
| `/wykres` | kod jednego wykresu odpowiadającego na jedno pytanie | tylko na wywołanie |
| `/wyjasnij` | wyjaśnienie kodu albo błędu i najmniejsza poprawka | na wywołanie albo samo |

Polecenia, które piszą kod, mają w nagłówku `disable-model-invocation: true`. Dzięki temu asystent nie uruchomi ich bez waszej wyraźnej prośby. Jak wywołać polecenie i jak wygląda lista w Czacie: [ćwiczenie 4](04_notatniki.md#osoba-a-zadanie-3-z-copilotem).

## Gdy asystent prosi o zgodę

Asystent może zaproponować uruchomienie polecenia w terminalu, np. skryptu, który pobierze stronę z dokumentacją. Zobaczycie wtedy okno **Uruchomić polecenie…? Zezwalaj / Pomiń**.

- Przeczytajcie polecenie, zanim klikniecie. Nie rozumiecie go? Kliknijcie **Pomiń** i zapytajcie `/wyjasnij`.
- **Pomiń** wybierajcie zawsze, gdy polecenie usuwa pliki, robi commit lub push, instaluje nieznane pakiety albo wysyła wasze dane na zewnątrz.
- Odmowa niczego nie psuje: asystent odpowie bez tego kroku.

## Napiszcie własne polecenie

1. Wybierzcie czynność, którą powtarzacie w projekcie, np. wybór cytatów na stronę.
2. Utwórzcie plik `.github/skills/<nazwa>/SKILL.md`. Nazwa: małe litery, cyfry i myślniki, taka sama jak pole `name`. Inaczej polecenie się nie załaduje.
3. Na górze `name` i `description`: jedno zdanie, kiedy go użyć.
4. Niżej kroki i zasady. Dopiszcie kryterium **„Gotowe, gdy”**: po czym poznać dobry wynik.
5. Przetestujcie na 3 przykładach, poprawcie, zróbcie Commit i Sync i zapiszcie decyzję w dzienniku.

```markdown
---
name: cytaty
description: Wybiera 5 krótkich cytatów na stronę projektu, bez nicków.
---

1. Weź tabelę z data/processed/.
2. Wybierz cytaty do 25 słów, po jednym z każdej kategorii.
3. Usuń nicki i nazwiska osób prywatnych.
4. Oddaj tabelę: cytat, kategoria, id tekstu.

Gotowe, gdy: cytaty są dosłowne i da się je znaleźć w tekście.
```

Inne pomysły: `/sprawdz-probke` (lista kontrolna próbki), `/dziennik` (dopisuje decyzję do dziennika w ustalonym formacie), `/pseudonimizuj` (zamiana nicków według reguł projektu).

## Copilot, Codex, Claude: ten sam przepis

| Asystent | Firma | Gdzie działa | Czyta zasady z |
|---|---|---|---|
| GitHub Copilot | GitHub (Microsoft) | VS Code, Codespaces, github.com | `AGENTS.md`, `.github/skills/` |
| Codex | OpenAI | terminal, edytor, aplikacja | `AGENTS.md` |
| Claude Code | Anthropic | terminal, edytor, aplikacja | `CLAUDE.md`, który odsyła do `AGENTS.md` |

Wiedza o projekcie mieszka w repozytorium, a nie w narzędziu. Zmienicie asystenta albo skończy się wam limit, a zasady zostaną te same: wtedy możecie skopiować treść `AGENTS.md` albo `SKILL.md` do dowolnego czatu AI ([Co jeśli…](09_co_jesli.md)). Plany, ceny i modele zmieniają się co kilka miesięcy, więc sprawdzajcie aktualną dokumentację.

[← Co jeśli…](09_co_jesli.md) · [Spis treści](README.md) · [Przepisy na dane →](../przepisy/README.md)
