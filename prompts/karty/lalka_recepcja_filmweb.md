# Karta źródła: recenzje i opinie o „Lalce” na Filmwebie (2026)

Wersja: v1 · Data rozpoznania: 2026-11-26 · Przygotował: prowadzący · Zespół: [imiona] · **Status: gotowa (droga ręczna)**

Dla warstwy `lalka-recepcja` (recepcja 2026). Dalej: [przepis 4: ręczna próbka](../../docs/przepisy/04_reczna_probka.md).

| Pole | Co wpisać | Odpowiedź |
|---|---|---|
| Co pobieramy | rodzaj tekstów i jednostka analizy | opinie widzów i recenzje pod filmem Kawalskiego i serialem (2026); jednostka: jedna opinia albo jedna recenzja |
| Skąd | serwis i dokładny adres | [wpiszcie adres strony filmu lub serialu na filmweb.pl i zakładki z opiniami] |
| Droga | gotowy plik / API / scraping / ręcznie | **ręcznie**: serwis nie ma publicznego API, a regulamin zabrania botów |
| Dokumentacja ✋ | link do dokumentacji | brak publicznego API; [regulamin](https://www.filmweb.pl/regulations) |
| Klucz lub konto | czy potrzebne | do czytania opinii niepotrzebne |
| Limity ✋ | ile zapytań | nie dotyczy: czytacie strony jak zwykły użytkownik |
| Regulamin ✋ | link i jedno zdanie | [Regulamin Filmweb](https://www.filmweb.pl/regulations), punkt 8 d: zakaz używania programów (botów) generujących zapytania do serwerów serwisu |
| robots.txt ✋ | czy ścieżki są dozwolone | [robots.txt](https://www.filmweb.pl/robots.txt) nie blokuje stron z opiniami, ale regulamin ma pierwszeństwo: automatu nie używamy |
| Dane osobowe | jakie i jak pseudonimizujemy | nicki autorów opinii → `autor_001`…; nicków nie przepisujemy do tabeli, wpisujemy od razu identyfikator |
| Prawa autorskie | co publikujemy | opinie i recenzje są utworami ich autorów; publikujemy dane zbiorcze i krótkie cytaty bez nicków |
| Przechowywanie ✋ | gdzie i jak długo | tabela w `data/raw/` (poza repozytorium); do `data/processed/` po pseudonimizacji |
| Próbka na start | ile rekordów | 20 opinii: 10 najnowszych i 10 najwyżej ocenianych, z adresem każdej |
| Czego czat nie był pewien | co sprawdziliście | regulamin i robots.txt przeczytane 26.11.2026 |

## Wniosek

Opinie z Filmwebu zbieramy ręcznie i w małej liczbie: kilkadziesiąt dobrze dobranych tekstów zamiast tysięcy. Dobór (które opinie, dlaczego) zapisujemy w dzienniku decyzji. Jeśli potrzeba więcej tekstów, szukamy źródła z API, np. komentarze na YouTube.
