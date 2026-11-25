# Przepis 2. Komentarze z YouTube (API z kluczem)

**Kiedy:** badacie recepcję filmu, serialu albo wideoeseju w komentarzach pod filmem na YouTube.
**Wynik:** pliki JSON w `data/raw/` i tabela `komentarze` z tekstem, datą, liczbą polubień i odpowiedzi.

> [!NOTE]
> YouTube ma oficjalne **YouTube Data API v3**. Do komentarzy publicznych wystarczy **klucz API** z Google Cloud. Komentarze pobiera metoda [commentThreads.list](https://developers.google.com/youtube/v3/docs/commentThreads/list): jedno zapytanie zwraca do 100 komentarzy i kosztuje 1 jednostkę z dziennego limitu, który domyślnie wynosi 10 000 jednostek na projekt.

## Część A. Klucz API (raz na zespół, ok. 10 minut)

1. Wejdźcie na [console.cloud.google.com](https://console.cloud.google.com) i zalogujcie się kontem Google.
2. Utwórzcie projekt: u góry lista projektów → **New project** → nazwa, np. `nh-komentarze` → **Create**.
3. **APIs & Services → Library** → wyszukajcie **YouTube Data API v3** → **Enable**.
4. **APIs & Services → Credentials → Create credentials → API key**. Skopiujcie klucz.
5. Przy kluczu kliknijcie **Edit API key** (albo **Restrict key**) → **API restrictions → Restrict key** → zaznaczcie **YouTube Data API v3** → **Save**. Wtedy klucz działa tylko dla YouTube.
6. Na GitHubie, w repozytorium zespołu: **Settings → Secrets and variables → Codespaces → New repository secret**. Nazwa `YOUTUBE_API_KEY`, wartość: skopiowany klucz.
7. Zatrzymajcie i uruchomcie codespace ponownie, żeby zobaczył nowy sekret.

> [!WARNING]
> Klucza nie wklejamy do notatnika, czatu ani Issue. Kto ma klucz, może zużyć wasz limit. Jeśli klucz wycieknie, usuńcie go w Google Cloud (**Credentials** → kosz) i utwórzcie nowy. Domyślny limit YouTube Data API jest bezpłatny. Jeśli Google prosi o kartę płatniczą, przerwijcie i zapytajcie prowadzącego.

## Część B. Pobranie próbki

1. **Wybierzcie film** i skopiujcie jego adres. Sprawdźcie, czy ma włączone komentarze.
2. **Poproście Copilota o kod**, np.:

    ```
    /pobierz 100 komentarzy spod filmu https://www.youtube.com/watch?v=… przez YouTube Data API v3
    (commentThreads.list, part=snippet, textFormat=plainText). Klucz w os.environ["YOUTUBE_API_KEY"].
    Zapisz każdą stronę odpowiedzi jako JSON w ../data/raw/ i zbuduj tabelę: tekst, autor,
    data publikacji, polubienia, liczba odpowiedzi.
    ```

3. **Sprawdźcie w kodzie od Copilota**: czy klucz jest czytany z `os.environ` i nigdzie nie jest wypisywany, czy jest przerwa między zapytaniami, czy kod korzysta z `nextPageToken`, żeby pobrać więcej niż 100 komentarzy.
4. **Uruchomcie na 100 komentarzach** i obejrzyjcie tabelę. Dopiero potem zwiększcie `LIMIT`.

## Po uruchomieniu powinniście zobaczyć

- `✅ Klucz jest dostępny.` i 11-znakowe ID filmu.
- Plik `youtube_<ID>_<data>_001.json` w `data/raw/`.
- Tabelę z kolumnami `tekst`, `autor`, `data_publikacji`, `polubienia`, `liczba_odpowiedzi` i najwyżej `LIMIT` wierszy.

**Typowe błędy** (komunikat w kodzie wzorcowym pokazuje kod i przyczynę):

| Komunikat | Co znaczy | Co zrobić |
|---|---|---|
| `403 … commentsDisabled` | komentarze pod filmem są wyłączone | wybierzcie inny film |
| `403 … quotaExceeded` | wyczerpany dzienny limit | poczekajcie do następnego dnia (limit odnawia się o północy czasu pacyficznego) |
| `400 … API key not valid` | zły klucz albo nieodświeżony codespace | sprawdźcie sekret i uruchomcie codespace ponownie |
| `404 … videoNotFound` | złe ID filmu | sprawdźcie adres |

<details>
<summary>▶ Kod wzorcowy (sprawdzony na odpowiedziach w formacie z dokumentacji API, 25.11.2026)</summary>

```python
# --- komórka 1: parametry ---
import os, json, time, datetime, pathlib
from urllib.parse import urlparse, parse_qs
import requests
import pandas as pd

FILM = "https://www.youtube.com/watch?v=XXXXXXXXXXX"   # adres filmu albo samo ID
LIMIT = 100              # na start: najwyżej 100 komentarzy (1 zapytanie)
KOLEJNOSC = "time"       # "time" = od najnowszych, "relevance" = najtrafniejsze
RAW = pathlib.Path("../data/raw")
RAW.mkdir(parents=True, exist_ok=True)

KLUCZ = os.environ.get("YOUTUBE_API_KEY")
print("✅ Klucz jest dostępny." if KLUCZ else "❌ Brak sekretu YOUTUBE_API_KEY.")

def id_filmu(adres):
    """Wyciąga 11-znakowe ID filmu z adresu YouTube albo zwraca samo ID."""
    u = urlparse(adres)
    if u.netloc.endswith("youtu.be"):
        return u.path.lstrip("/")
    if "/shorts/" in u.path or "/live/" in u.path:
        return u.path.rstrip("/").split("/")[-1]
    return parse_qs(u.query).get("v", [adres])[0]

VIDEO_ID = id_filmu(FILM)
print("ID filmu:", VIDEO_ID)
```

```python
# --- komórka 2: pobranie komentarzy (każde zapytanie = 1 jednostka limitu) ---
def pobierz_komentarze(video_id, limit):
    adres = "https://www.googleapis.com/youtube/v3/commentThreads"
    parametry = {"part": "snippet", "videoId": video_id, "maxResults": 100,
                 "order": KOLEJNOSC, "textFormat": "plainText", "key": KLUCZ}
    wiersze, strona = [], 0
    while len(wiersze) < limit:
        odp = requests.get(adres, params=parametry, timeout=30)
        dane = odp.json()
        if odp.status_code != 200:
            # Nie wypisujemy adresu zapytania, bo zawiera klucz.
            blad = dane.get("error", {})
            print("❌ Błąd", odp.status_code, "|", blad.get("message", ""),
                  "|", [e.get("reason") for e in blad.get("errors", [])])
            break
        strona += 1
        plik = RAW / f"youtube_{video_id}_{datetime.date.today()}_{strona:03d}.json"
        plik.write_text(json.dumps(dane, ensure_ascii=False), encoding="utf-8")
        for watek in dane.get("items", []):
            k = watek["snippet"]["topLevelComment"]["snippet"]
            wiersze.append({
                "id": watek["id"],
                "tekst": k["textDisplay"],
                "autor": k["authorDisplayName"],
                "data_publikacji": k["publishedAt"],
                "polubienia": k["likeCount"],
                "liczba_odpowiedzi": watek["snippet"].get("totalReplyCount", 0),
            })
        token = dane.get("nextPageToken")
        if not token:
            break
        parametry["pageToken"] = token
        time.sleep(1)
    return wiersze[:limit]
```

```python
# --- komórka 3: tabela ---
komentarze = pd.DataFrame(pobierz_komentarze(VIDEO_ID, LIMIT))
if not komentarze.empty:
    komentarze["zrodlo"] = "YouTube"
    komentarze["url"] = f"https://www.youtube.com/watch?v={VIDEO_ID}"
    komentarze["data_pobrania"] = datetime.date.today().isoformat()
    komentarze["liczba_slow"] = komentarze["tekst"].str.split().str.len()
    print(komentarze.shape)
    print(komentarze[["polubienia", "liczba_slow"]].describe())
    print(komentarze[["tekst"]].head())
```

</details>

## Prawo i etyka

- **Nicki to dane osobowe.** Kolumnę `autor` zamieńcie na identyfikatory (notatnik `01_pobranie`, krok 1C), zanim cokolwiek trafi do `data/processed/`.
- **Czas przechowywania.** [Zasady dla deweloperów YouTube](https://developers.google.com/youtube/terms/developer-policies) ograniczają przechowywanie danych z API: po 30 dniach trzeba je usunąć albo pobrać od nowa. Przeczytajcie aktualne brzmienie, zapiszcie decyzję w karcie źródła (pole „Przechowywanie”) i w dzienniku decyzji. Publikujemy dane zbiorcze i krótkie cytaty bez nicków.
- **Odpowiedzi pod komentarzami** API zwraca tylko częściowo. Jeśli są ważne dla pytania badawczego, poproście Copilota o pobranie ich metodą `comments.list` z parametrem `parentId`.
- **Wyszukiwanie filmów** przez API (`search.list`) kosztuje 100 jednostek za zapytanie. Listę filmów lepiej przygotować ręcznie.

[← Przepis 1](01_api_bez_klucza.md) · [Przepisy](README.md) · [Przepis 3: scraping →](03_scraping.md)
