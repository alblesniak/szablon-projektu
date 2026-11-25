---
name: pobierz
description: Kod do notatnika, który pobiera małą próbkę danych z wybranego źródła, zapisuje surowe dane w data/raw i buduje tabelę z metadanymi. Używaj po rozpoznaniu źródła (polecenie /zrodlo).
argument-hint: źródło i liczba rekordów, np. 20 komentarzy spod filmu https://www.youtube.com/watch?v=…
disable-model-invocation: true
---

# Pobranie próbki danych

Napisz kod do notatnika Jupyter, który pobiera **małą próbkę** z podanego źródła. Jeśli w `prompts/` jest karta źródła (`karta_zrodla_v*.md`), trzymaj się jej ustaleń. Gotowe wzory są w [docs/przepisy/](../../../docs/przepisy/README.md): sprawdź, czy któryś pasuje, i opieraj się na nim.

## Podział na komórki

1. **Parametry**: adres lub identyfikator źródła, `LIMIT` (domyślnie 20), nazwa projektu do nagłówka `User-Agent`. Klucz API tylko z `os.environ`.
2. **Sprawdzenie zasad**: przy scrapingu sprawdź `robots.txt` przez `urllib.robotparser` i zatrzymaj się z czytelnym komunikatem, jeśli adres jest zabroniony.
3. **Pobranie**: `requests`, przerwa co najmniej 1 sekunda między zapytaniami, obsługa błędów bez przerywania pętli, licznik pobranych i błędów. Najwyżej `LIMIT` rekordów.
4. **Zapis surowych danych** do `../data/raw/` (JSON albo HTML, tak jak przyszły), z datą w nazwie pliku.
5. **Tabela pandas** z kolumnami: `id`, `tekst`, `zrodlo`, `url`, `data_publikacji` (jeśli jest), `data_pobrania`, `liczba_slow` oraz `autor`, jeśli źródło go podaje.
6. **Kontrola**: liczba wierszy, 5 pierwszych wierszy bez długich tekstów, statystyki `liczba_slow`, liczba pustych tekstów i duplikatów.

## Zasady

- Nie pseudonimizuj jeszcze i nie zapisuj do `data/processed/`: to osobny krok (notatnik `01_pobranie`, krok 1C).
- Komentarze w kodzie po polsku. Bez dodatkowych bibliotek spoza `requirements.txt`.
- Po kodzie napisz sekcję **„Po uruchomieniu powinieneś zobaczyć”** i jedno zdanie, jak zwiększyć próbkę, gdy ta jest dobra.
- Jeśli nie znasz dokładnego formatu odpowiedzi API, najpierw napisz komórkę, która pobiera jeden rekord i wypisuje jego strukturę.
