---
name: wyjasnij
description: Wyjaśnia po polsku, prostym językiem, co robi zaznaczony kod albo co znaczy komunikat błędu, i proponuje najmniejszą poprawkę. Używaj, gdy student nie rozumie kodu lub błędu w notatniku.
argument-hint: wklej komunikat błędu albo zaznacz komórkę z kodem
---

# Wyjaśnij kod albo błąd

Student jest humanistą i dopiero uczy się programowania.

## Gdy chodzi o kod

1. Jedno zdanie: co ta komórka robi jako całość.
2. Potem fragment po fragmencie (najwyżej 6 punktów): co robi każda część, nazwy funkcji i bibliotek objaśnione jednym zdaniem.
3. Co się stanie z danymi: skąd przychodzą, dokąd trafiają (plik, tabela, wykres).
4. Jedno pytanie kontrolne, którym student sprawdzi, czy rozumie (np. „co trzeba zmienić, żeby pobrać 50 zamiast 20 rekordów?”).

## Gdy chodzi o błąd

1. Co znaczy komunikat, przetłumaczony na zwykły polski.
2. Najbardziej prawdopodobna przyczyna w tym notatniku (sprawdź zmienne, ścieżki `../data/`, nazwy kolumn, wybrane jądro, brak klucza w `os.environ`).
3. **Najmniejsza poprawka**: tylko zmienione linie, nie cała komórka od nowa.
4. Jak sprawdzić, że pomogło.

Jeśli błąd dotyczy braku biblioteki, najpierw każ sprawdzić jądro notatnika (prawy górny róg: Python 3.12, `/usr/local/bin/python`), a dopiero potem instalację. Nie proponuj kodu, który omija zabezpieczenia serwisów.
