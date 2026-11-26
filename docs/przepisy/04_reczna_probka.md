# Przepis 4. Ręczna próbka: Filmweb, Reddit i inne serwisy bez API

**Kiedy:** serwis nie ma API dostępnego dla was, a regulamin albo `robots.txt` zabrania programów, które pobierają strony. Tak jest dziś z Filmwebem i Reddittem.
**Wynik:** tabela CSV z 20–30 tekstami przepisanymi ręcznie, sprawdzona w notatniku i zapisana w `data/processed/teksty.csv`.
**Czas:** ok. 45 minut na 20 tekstów.

> [!NOTE]
> **Dlaczego ręcznie?** Regulamin Filmwebu (punkt 8 d) zabrania używania programów (botów), które wysyłają zapytania do serwerów serwisu. Reddit w `robots.txt` blokuje wszystkie programy (`Disallow: /`), a od listopada 2025 dostęp do jego API wymaga wniosku i zgody Reddita. Czytać i notować jak zwykły użytkownik wolno. Mała, dobrze dobrana próbka to w humanistyce pełnoprawna metoda, pod warunkiem że zapiszecie zasadę doboru.

## Krok po kroku

**Krok 1. Karta.** Skopiujcie gotową kartę jako `prompts/karta_zrodla_v1.md`: [Filmweb](../../prompts/karty/lalka_recepcja_filmweb.md) albo [Reddit](../../prompts/karty/lalka_recepcja_reddit.md). W polu „Skąd” wpiszcie adresy stron, z których bierzecie teksty. Inny serwis? Najpierw `/zrodlo`: jeśli wyjdzie, że automatu nie wolno, wracacie tutaj.

**Krok 2. Zasada doboru.** Zanim otworzycie stronę, zapiszcie w dzienniku decyzji w `README.md`, które teksty bierzecie i dlaczego. Przykłady:

| Serwis | Zasada doboru |
|---|---|
| Filmweb | 10 najnowszych i 10 najwyżej ocenianych opinii pod serialem; pomijamy opinie krótsze niż 10 słów |
| Reddit | komentarze najwyższego poziomu z 3 wątków o serialu, sortowanie „Najlepsze”, po 8–10 z wątku |

Bez zasady doboru próbka sama wybiera tezę: bierze się teksty, które pasują do tego, co chcemy pokazać.

**Krok 3. Tabela.** Najwygodniej w arkuszu (Google Sheets, Excel, LibreOffice). Pierwszy wiersz to nagłówki, dokładnie takie:

```
id,tekst,id_autora,ocena,data_publikacji,url,zrodlo,data_pobrania
```

| Kolumna | Co wpisać |
|---|---|
| `id` | kolejny numer: `fw_001`, `fw_002`… albo `rd_001`… |
| `tekst` | cały tekst opinii albo komentarza, skopiowany ze strony |
| `id_autora` | **zamiast nicka** identyfikator: `autor_001`, `autor_002`…; ten sam nick dostaje zawsze ten sam numer |
| `ocena` | Filmweb: ocena autora (1–10); Reddit: liczba punktów albo puste |
| `data_publikacji` | data pod tekstem, w formacie `2026-11-20` |
| `url` | adres konkretnej opinii albo komentarza; na Reddicie: **Udostępnij → Kopiuj link** |
| `zrodlo` | `Filmweb` albo `Reddit` |
| `data_pobrania` | dzisiejsza data |

> [!IMPORTANT]
> **Nicków nie przepisujecie do tabeli.** Od razu wpisujecie `autor_001`. Jeśli musicie pamiętać, kto jest kim (np. żeby rozpoznać tę samą osobę w dwóch wątkach), zapiszcie słownik `nick → autor_001` w osobnym pliku `data/raw/slownik_autorow.csv`. Ten plik nie trafia do repozytorium.

**Krok 4. Plik w codespace.** W arkuszu: **Plik → Pobierz → CSV** (w Excelu: **Zapisz jako → CSV UTF-8**). W codespace w **Eksploratorze** przeciągnijcie plik do folderu `data/raw/` i nazwijcie go np. `filmweb_opinie.csv`. Plik będzie szary: `data/raw/` nie trafia do repozytorium.

**Krok 5. Kontrola w notatniku.** W notatniku `01_pobranie.ipynb` wklejcie trzy komórki z kodu wzorcowego poniżej, w pierwszej wpiszcie nazwę swojego pliku i uruchomcie je po kolei. Możecie też poprosić Copilota:

```
Napisz komórki, które wczytują ../data/raw/filmweb_opinie.csv (kolumny: id, tekst, id_autora, ocena,
data_publikacji, url, zrodlo, data_pobrania), sprawdzają puste teksty, duplikaty i czy id_autora
ma postać autor_001, a potem zapisują tabelę do ../data/processed/teksty.csv.
```

**Krok 6. Sprawdzenie oczami, dziennik, Commit i Sync.** Przeczytajcie 5 pierwszych wierszy: czy tekst nie jest ucięty i czy polskie znaki są czytelne? Liczbę tekstów i datę zapiszcie w dzienniku decyzji, potem **Zatwierdź** i **Synchronizuj zmiany**. Do repozytorium trafia `data/processed/teksty.csv` bez nicków, a surowa tabela zostaje w `data/raw/`.

## Po uruchomieniu powinniście zobaczyć

- `Wczytano 20 wierszy, 8 kolumn` i `Brakujące kolumny: brak`.
- `Puste teksty: 0`, `Duplikaty tekstów: 0`, `Wiersze bez identyfikatora autor_NNN: 0`.
- Tabelę 5 pierwszych tekstów z kolumną `id_autora`.
- `Zapisano ../data/processed/teksty.csv | 20 tekstów`.

**Typowe kłopoty:**

| Komunikat albo objaw | Co znaczy | Co zrobić |
|---|---|---|
| `Brakujące kolumny: ['ocena']` | inna nazwa nagłówka w arkuszu | popraw nagłówek, zapisz CSV jeszcze raz |
| `ParserError: Error tokenizing data` | przecinek w tekście bez cudzysłowu | zapisz CSV z arkusza, nie pisz go ręcznie; arkusz sam doda cudzysłowy |
| krzaki zamiast „ą”, „ł” | plik nie jest w UTF-8 | w Excelu wybierz **CSV UTF-8** |
| `Wiersze bez identyfikatora autor_NNN: 3` | w kolumnie `id_autora` zostały nicki | popraw w arkuszu, zanim cokolwiek zapiszesz do `data/processed/` |

<details>
<summary>▶ Kod wzorcowy (sprawdzony na przykładowej tabeli 26.11.2026)</summary>

```python
# --- komórka 1: parametry ---
import pathlib
import pandas as pd

PLIK = pathlib.Path("../data/raw/filmweb_opinie.csv")   # wasz plik z ręczną próbką
KOLUMNY = ["id", "tekst", "id_autora", "ocena", "data_publikacji", "url", "zrodlo", "data_pobrania"]
```

```python
# --- komórka 2: wczytanie i kontrola ---
teksty = pd.read_csv(PLIK, dtype=str, keep_default_na=False)
print("Wczytano", len(teksty), "wierszy,", len(teksty.columns), "kolumn")
brakujace = [k for k in KOLUMNY if k not in teksty.columns]
print("Brakujące kolumny:", brakujace or "brak")

teksty["tekst"] = teksty["tekst"].str.strip()
teksty["liczba_slow"] = teksty["tekst"].str.split().str.len()
puste = (teksty["tekst"] == "").sum()
print("Puste teksty:", puste)
print("Duplikaty tekstów:", teksty["tekst"].duplicated().sum())
print("Autorów:", teksty["id_autora"].nunique())

# Kontrola pseudonimizacji: każdy autor ma postać autor_001
zle = teksty[~teksty["id_autora"].str.fullmatch(r"autor_\d{3}")]
print("Wiersze bez identyfikatora autor_NNN:", len(zle))

pd.set_option("display.max_colwidth", 60)
teksty[["id", "id_autora", "data_publikacji", "liczba_slow", "tekst"]].head()
```

```python
# --- komórka 3: zapis do data/processed, tylko gdy kontrola jest czysta ---
if len(zle) == 0 and puste == 0:
    wynik = pathlib.Path("../data/processed/teksty.csv")
    teksty.to_csv(wynik, index=False)
    print("Zapisano", wynik, "|", len(teksty), "tekstów")
else:
    print("❌ Najpierw popraw plik w data/raw/: puste teksty albo nicki zamiast identyfikatorów.")
```

</details>

## Prawo i etyka

- **Regulamin ma pierwszeństwo przed `robots.txt`.** Filmweb nie blokuje stron w `robots.txt`, ale regulamin zabrania botów, więc nie używamy programów. Nie korzystamy też z rozszerzeń do przeglądarki, które pobierają strony za nas.
- **Nicki to dane osobowe.** W tabeli od początku są identyfikatory. Na stronę projektu trafiają dane zbiorcze i krótkie cytaty bez nicków.
- **Teksty należą do autorów.** Opinie, recenzje i komentarze są chronione prawem autorskim: publikujemy krótkie cytaty, nie całe teksty.
- **Model językowy.** Zanim wyślecie teksty z Reddita do modelu (`/koduj`), przeczytajcie [Public Content Policy](https://support.reddithelp.com/hc/en-us/articles/26410290525844-Public-Content-Policy) i zapiszcie decyzję w dzienniku. Kodowanie ręczne jest bezpieczną alternatywą.
- **Dostęp do API Reddita dla badaczy** przechodzi przez wniosek ([r/reddit4researchers](https://www.reddit.com/r/reddit4researchers/)); ma sens tylko przy dłuższym projekcie i może zostać odrzucony.

To nie jest porada prawna. Wątpliwości: Issue z oznaczeniem prowadzącego.

[← Przepis 3](03_scraping.md) · [Przepisy](README.md)
