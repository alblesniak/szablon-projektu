---
name: koduj
description: Kod do notatnika, który koduje teksty modelem językowym według karty kodowej z folderu prompts i zwraca tabelę kategorii z cytatem-dowodem. Używaj, gdy dane są już w data/processed i karta kodowa jest gotowa.
argument-hint: plik z tekstami i karta kodowa, np. data/processed/teksty.csv, prompts/karta_kodowa_v1.md
disable-model-invocation: true
---

# Kodowanie tekstów przez model językowy

Napisz kod do notatnika, który przepuszcza teksty przez model językowy zgodnie z kartą kodową. Przeczytaj najpierw kartę kodową w `prompts/` (najnowszą wersję) i pola 4–6 karty projektu w `README.md`.

## Podział na komórki

1. **Parametry**: ścieżka do tabeli z tekstami, ścieżka do karty kodowej, `MODEL` (nazwa modelu do uzupełnienia przez studenta, jeśli nie ma jej w karcie), `WERSJA_PROMPTU`, `LIMIT` (na start 5).
2. **Prompt**: wczytaj prompt systemowy z karty kodowej. Wypisz dozwolone wartości każdego pola jako słownik Pythona, żeby dało się je sprawdzić.
3. **Funkcja `koduj(tekst)`**: biblioteka `openai`, klient `OpenAI()` (klucz z `OPENAI_API_KEY`), `temperature=0`, odpowiedź w formacie JSON. Zwraca słownik z polami karty i polem `evidence_quote`.
4. **Sprawdzenie odpowiedzi**: czy JSON się wczytuje, czy wartości są dozwolone, czy `evidence_quote` naprawdę występuje w tekście. Błędy zapisuj w kolumnie `uwagi`, nie poprawiaj ich po cichu.
5. **Test na próbce**: najpierw `LIMIT` tekstów, wynik jako tabela obok fragmentu tekstu, żeby student mógł ocenić go oczami.
6. **Zapis**: `../data/processed/kodowanie_<WERSJA_PROMPTU>.csv` z kolumnami `id`, pola karty, `evidence_quote`, `uwagi`, `model`, `wersja_promptu`, `data_kodowania`.

## Zasady

- Przerwa między zapytaniami i obsługa błędów API (limit, brak klucza) z czytelnym komunikatem.
- Nie wysyłaj do modelu nicków ani danych osobowych, których nie potrzeba do kodowania.
- Po kodzie: sekcja **„Po uruchomieniu powinieneś zobaczyć”** i przypomnienie o walidacji: losowa próba kodowana ręcznie bez patrzenia na wynik modelu (pole 6 karty projektu).
