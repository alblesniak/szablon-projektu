# Przepis 3. Scraping strony WWW

**Kiedy:** teksty są tylko na stronach WWW (recenzje, opinie, wpisy na forum), serwis nie ma API ani eksportu, a regulamin i `robots.txt` nie zabraniają automatycznego pobierania.
**Wynik:** zapisane strony HTML w `data/raw/` i tabela z tekstami wyciągniętymi ze stron.

> [!NOTE]
> **Scraping** to pobieranie stron WWW programem i wyciąganie z nich tekstu. Program robi to samo co wy w przeglądarce, tylko szybciej, dlatego musi być grzeczny: przedstawia się w nagłówku `User-Agent`, robi przerwy i nie wchodzi tam, gdzie `robots.txt` zabrania.

## Część A. Ćwiczenie na stronie do nauki

Najpierw ćwiczymy na [quotes.toscrape.com](https://quotes.toscrape.com/), stronie stworzonej do nauki scrapingu: cytaty z autorem i tagami, podzielone na strony.

1. **Obejrzyjcie stronę jak program.** W Chrome kliknijcie prawym przyciskiem na cytat → **Zbadaj** (*Inspect*). Zobaczycie, że każdy cytat jest w `<div class="quote">`, tekst w `<span class="text">`, autor w `<small class="author">`, a link do następnej strony w `<li class="next">`. Te nazwy (*selektory*) podacie Copilotowi.
2. **Poproście Copilota o kod**, np.:

    ```
    /pobierz Cytaty z 3 pierwszych stron https://quotes.toscrape.com/. Najpierw sprawdź robots.txt.
    Każdy cytat to div.quote: tekst w span.text, autor w small.author, tagi w a.tag.
    Następna strona: link w li.next a. Zapisz surowy HTML stron w ../data/raw/, przerwa 1 s.
    ```

3. **Uruchomcie** i porównajcie z wynikiem poniżej.

### Po uruchomieniu powinniście zobaczyć

- `Czy wolno pobierać stronę startową? True`.
- `Pobrano stron: 3` i pliki `strona_001.html` … `strona_003.html` w `data/raw/`.
- Tabelę 30 × 6: po 10 cytatów z każdej strony, z autorem, tagami i liczbą słów.

<details>
<summary>▶ Kod wzorcowy (sprawdzony 25.11.2026)</summary>

```python
# --- komórka 1: parametry ---
import time, datetime, pathlib
from urllib import robotparser
from urllib.parse import urljoin
import requests
import pandas as pd
from bs4 import BeautifulSoup

START = "https://quotes.toscrape.com/"   # strona stworzona do ćwiczeń ze scrapingu
LIMIT_STRON = 3
UA = "projekt-studencki-nowa-humanistyka (cele edukacyjne)"
RAW = pathlib.Path("../data/raw")
RAW.mkdir(parents=True, exist_ok=True)
```

```python
# --- komórka 2: czy wolno? robots.txt ---
rp = robotparser.RobotFileParser()
rp.set_url(urljoin(START, "/robots.txt"))
rp.read()
print("Czy wolno pobierać stronę startową?", rp.can_fetch(UA, START))
```

```python
# --- komórka 3: pobranie stron i zapis surowego HTML ---
adres, strony = START, []
while adres and len(strony) < LIMIT_STRON:
    if not rp.can_fetch(UA, adres):
        print("robots.txt zabrania:", adres); break
    odp = requests.get(adres, headers={"User-Agent": UA}, timeout=30)
    odp.raise_for_status()   # przy błędzie 403 lub 429 zatrzymujemy się
    nr = len(strony) + 1
    (RAW / f"strona_{nr:03d}.html").write_text(odp.text, encoding="utf-8")
    strony.append((adres, odp.text))
    nastepna = BeautifulSoup(odp.text, "lxml").select_one("li.next a")
    adres = urljoin(adres, nastepna["href"]) if nastepna else None
    time.sleep(1)
print("Pobrano stron:", len(strony))
```

```python
# --- komórka 4: wyciągnięcie danych z HTML ---
wiersze = []
for url, html in strony:
    for blok in BeautifulSoup(html, "lxml").select("div.quote"):
        tekst = blok.select_one("span.text").get_text(strip=True)
        wiersze.append({
            "tekst": tekst,
            "autor": blok.select_one("small.author").get_text(strip=True),
            "tagi": ", ".join(t.get_text() for t in blok.select("a.tag")),
            "url": url,
            "data_pobrania": datetime.date.today().isoformat(),
            "liczba_slow": len(tekst.split()),
        })
cytaty = pd.DataFrame(wiersze)
print(cytaty.shape)
cytaty.head()
```

</details>

## Część B. Prawdziwy portal

1. **Rozpoznanie.** `/zrodlo` z adresem portalu. Potem sami: regulamin (szukajcie słów „automatyczne”, „roboty”, „kopiowanie”, „scraping”) i `https://adres-portalu/robots.txt`. Wynik do karty źródła.
2. **Czy tekst jest w HTML?** Otwórzcie stronę z tekstami, naciśnijcie **Ctrl+U** (na Macu **Cmd+Option+U**), czyli źródło strony, i wyszukajcie (**Ctrl+F**) fragment komentarza albo recenzji.
    - **Jest** → scraping jak w części A: selektory znajdziecie przez **Zbadaj**.
    - **Nie ma** → tekst dociąga JavaScript i `requests` go nie zobaczy. Sprawdźcie, czy serwis ma oficjalne API, i porozmawiajcie z prowadzącym. Nie korzystajcie z ukrytych, nieopisanych adresów serwisu bez sprawdzenia regulaminu.
3. **Selektory.** Przez **Zbadaj** znajdźcie element, w którym jest jeden tekst (recenzja, komentarz), i elementy z autorem, datą, oceną. Wypiszcie ich klasy i podajcie je Copilotowi jak w części A.
4. **Próbka**: najwyżej 2–3 strony, przerwa co najmniej 1 sekunda. Obejrzyjcie tabelę: czy teksty nie są ucięte, czy nie zebrały się menu i stopki.
5. **Skala** dopiero po sprawdzeniu próbki. Przy dużej liczbie stron zapisujcie postęp (które strony już pobrano), żeby po przerwie nie zaczynać od nowa.

> [!WARNING]
> **Kiedy przestać:** strona wymaga logowania, pokazuje CAPTCHA, ma paywall, zwraca błąd **403** (brak dostępu) albo **429** (za dużo zapytań). Nie obchodzimy tych zabezpieczeń: przerywamy, zapisujemy to w karcie źródła i szukamy innej drogi.

[← Przepis 2](02_youtube.md) · [Przepisy](README.md)
