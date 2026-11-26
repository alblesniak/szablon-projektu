# AGENTS.md: zasady pracy asystenta AI w tym projekcie

Ten plik czytają asystenci AI pracujący w repozytorium: GitHub Copilot, Codex, Claude i inni. Studenci też mogą go czytać i poprawiać: to opis waszej metody pracy z AI. Zmiany w tym pliku zapisujcie w dzienniku decyzji w `README.md`.

## Kim jesteśmy

To projekt studencki z nowej humanistyki (kulturoznawstwo, studia magisterskie). Pracują nad nim dwuosobowe zespoły, które dopiero uczą się programowania; niektóre osoby pracują w pojedynkę i wtedy pełnią obie role (zob. `docs/podrecznik/README.md`, „Praca w pojedynkę”). Projekt ma trzy etapy: **pobranie tekstów** (pliki, API, scraping), **analiza** (zwykłe liczenie i kodowanie tekstów przez model językowy według karty kodowej) i **wizualizacja**. Pracujemy w GitHub Codespaces, w notatnikach Jupyter (`notebooks/`).

## Jak odpowiadać

- Pisz po polsku, prostym językiem. Pojęcie techniczne wyjaśnij jednym zdaniem przy pierwszym użyciu.
- Najpierw plan w 2–4 punktach, potem kod. Gdy pytanie dotyczy sposobu („jak pobrać…?”), odpowiedz bez kodu i zapytaj, czy go napisać.
- Kod dziel na małe komórki: jedna komórka to jedna czynność. Komentarze w kodzie po polsku.
- Po każdym kodzie napisz: co uruchomić i co powinno się pokazać, gdy wszystko działa.
- Zmieniaj tylko to, o co proszono. Nie przepisuj działających komórek, nie usuwaj danych, nie rób commitów: commit i synchronizację robi student w Kontroli źródła.
- Jeśli nie jesteś pewien nazwy endpointu, parametru albo zasady serwisu, powiedz to wprost i wskaż, gdzie w oficjalnej dokumentacji to sprawdzić. Nie zgaduj adresów API.

## Struktura repozytorium

- `README.md`: karta projektu (pytanie badawcze, źródła, karta kodowa, walidacja) i dziennik decyzji. Zanim zaproponujesz rozwiązanie, przeczytaj kartę projektu.
- `notebooks/`: notatniki; ścieżki do danych zaczynają się od `../data/`.
- `data/raw/`: surowe dane. Ten folder **nie trafia do repozytorium** (`.gitignore`).
- `data/processed/`: tabele po pseudonimizacji i wyniki. Te pliki publikujemy.
- `prompts/`: prompty, karta kodowa i karty źródeł, w wersjach (v1, v2…).
- `docs/`: strona projektu (GitHub Pages); `docs/przepisy/`: przepisy na pobieranie danych.
- Biblioteki: tylko z `requirements.txt` (pandas, numpy, matplotlib, plotly, networkx, requests, beautifulsoup4, lxml, openai, scikit-learn). Jeśli potrzebna jest inna, powiedz dlaczego i dopisz ją do `requirements.txt`.

## Pobieranie danych

- Kolejność: **gotowy plik lub eksport → oficjalne API → scraping → ręczna próbka** (gdy regulamin albo `robots.txt` zabrania programów, np. Filmweb, Reddit). Zawsze podaj link do oficjalnej dokumentacji.
- We wspólnym projekcie „Lalka” gotowe karty źródeł są w `prompts/karty/`: zacznij od nich.
- Przed scrapingiem sprawdź `robots.txt` (np. `urllib.robotparser`) i przypomnij o regulaminie serwisu.
- Każde zapytanie: nagłówek `User-Agent` z opisem projektu, przerwa co najmniej 1 sekunda, obsługa błędów bez przerywania całej pętli.
- Zaczynaj od małej próbki (5–20 rekordów). Skalowanie dopiero, gdy student potwierdzi, że próbka jest dobra.
- Surowe odpowiedzi zapisuj w `data/raw/`, a metadane (źródło, adres, data pobrania) zapisuj od razu przy pobieraniu.
- Nie omijaj zabezpieczeń: logowania, CAPTCHA, paywalli, blokad i limitów. Jeśli serwis ich używa, zaproponuj inne źródło albo API.

## Klucze API

- Klucze czytaj tylko ze zmiennych środowiskowych (sekrety Codespaces): `os.environ["OPENAI_API_KEY"]`, `os.environ["YOUTUBE_API_KEY"]`.
- Nigdy nie wpisuj klucza w kod ani w notatnik i nigdy go nie wypisuj. Jeśli klucza brakuje, wyjaśnij, jak dodać sekret: Settings → Secrets and variables → Codespaces.

## Dane osobowe i prawo

- Nicki i nazwiska osób prywatnych zamieniaj na identyfikatory (`autor_001`…). Słownik zamian zapisuj tylko w `data/raw/`.
- Na stronę i do `data/processed/` trafiają krótkie cytaty bez nicków i dane zbiorcze.
- Przy danych z API sprawdzaj zasady przechowywania (np. YouTube wymaga usunięcia albo odświeżenia danych po 30 dniach) i przypominaj o wpisie w polu 9 karty projektu.

## Kodowanie tekstów przez model językowy

- Biblioteka `openai`, nazwa modelu w zmiennej `MODEL`, `temperature=0`.
- Kategorie i definicje bierz z karty kodowej w `prompts/`. Model ma zwracać JSON z polami karty i z polem `evidence_quote` (dosłowny cytat z tekstu).
- Sprawdzaj, czy odpowiedź jest poprawnym JSON-em i czy wartości należą do dozwolonych kategorii; błędne odpowiedzi oznaczaj, nie poprawiaj ich po cichu.
- Najpierw test na 5 trudnych tekstach, potem większa próba. Do wyników zapisuj wersję promptu i nazwę modelu.

## Wykresy

- `plotly` albo `matplotlib`. Jeden wykres odpowiada na jedno pytanie z karty projektu.
- Tytuł, podpisy osi i legenda po polsku; podaj liczebność (n).
- Wykresy na stronę zapisuj jako HTML w `docs/wykresy/`.

## Polecenia (skills)

W folderze `.github/skills/` są polecenia przygotowane dla kursu. Każde to plik `SKILL.md` z nazwą, opisem i krokami. Gdy student wpisze polecenie albo gdy prośba pasuje do opisu, przeczytaj odpowiedni plik i trzymaj się jego kroków.

| Polecenie | Kiedy |
|---|---|
| `/zrodlo` | rozpoznanie źródła danych, bez kodu |
| `/pobierz` | kod pobrania małej próbki do notatnika |
| `/koduj` | kodowanie tekstów modelem językowym według karty kodowej |
| `/wykres` | jeden wykres odpowiadający na jedno pytanie |
| `/wyjasnij` | wyjaśnienie kodu albo błędu i najmniejsza poprawka |

Studenci mogą dopisywać własne polecenia: nowy folder w `.github/skills/` z plikiem `SKILL.md`.
