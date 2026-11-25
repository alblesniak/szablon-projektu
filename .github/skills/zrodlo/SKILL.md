---
name: zrodlo
description: Rozpoznanie źródła danych przed pobieraniem, bez pisania kodu. Porównuje trzy drogi (gotowy plik lub eksport, oficjalne API, scraping), sprawdza klucze, limity, regulamin, robots.txt i dane osobowe, a wynik zapisuje jako kartę źródła. Używaj, gdy student pyta, skąd i jak pobrać teksty, komentarze, recenzje albo napisy.
argument-hint: co i skąd chcesz pobrać, np. komentarze pod filmem o „Lalce” na YouTube
---

# Rozpoznanie źródła danych

Student chce zebrać materiał do projektu. **Nie pisz kodu.** Twoim zadaniem jest rozpoznanie, czy i jak można go pobrać, oraz pokazanie, co student musi sprawdzić sam.

## Kroki

1. Ustal z prośby studenta: **co** (jaki rodzaj tekstów), **skąd** (serwis, adres), **ile** mniej więcej i **po co** (pytanie badawcze; zajrzyj do pola 1 karty projektu w `README.md`).
2. Opisz trzy drogi, każdą w kilku zdaniach:
   - **Gotowy plik lub eksport** (np. pobieralne pliki, zbiory danych, eksport z serwisu).
   - **Oficjalne API**: nazwa API, czy potrzebny klucz lub konto, limity, jakie pola zwraca, link do oficjalnej dokumentacji.
   - **Scraping**: czy strona pokazuje dane w zwykłym HTML, czy ładuje je JavaScriptem; co mówi `robots.txt`; ryzyka.
3. Dla wybranej drogi wypisz sprawy prawne i etyczne: regulamin serwisu (co mówi o automatycznym pobieraniu), dane osobowe (nicki, nazwiska), prawa autorskie (krótkie cytaty, dozwolony użytek, eksploracja tekstów i danych), zasady przechowywania danych z API.
4. Zaproponuj **próbkę na start**: ile rekordów (5–20) i jak je wybrać.
5. Wypełnij kartę źródła według szablonu [prompts/karta_zrodla_szablon.md](../../../prompts/karta_zrodla_szablon.md) i podaj ją jako blok Markdown do wklejenia do pliku `prompts/karta_zrodla_v1.md`.

## Uczciwość

- Każde twierdzenie o API (adres, parametr, limit) opatrz linkiem do oficjalnej dokumentacji.
- Jeśli czegoś nie wiesz albo informacja mogła się zmienić, wpisz to do pola „Czego czat nie był pewien” i napisz, gdzie to sprawdzić.
- Na końcu podaj **3 rzeczy do samodzielnego sprawdzenia** w dokumentacji albo regulaminie, zanim student napisze pierwszą linię kodu.
- Nie proponuj omijania logowania, CAPTCHA, paywalli ani limitów.

Zakończ pytaniem: „Czy mam przygotować kod pobrania próbki? Użyj polecenia /pobierz”.
