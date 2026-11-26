# Karta źródła: „Lalka”, tekst powieści (Wolne Lektury)

Wersja: v1 · Data rozpoznania: 2026-11-26 · Przygotował: prowadzący · Zespół: [imiona] · **Status: gotowa**

Dla warstwy `lalka-powiesc` i dla każdego zespołu, który potrzebuje tekstu powieści. Dalej: [przepis 1](../../docs/przepisy/01_api_bez_klucza.md).

| Pole | Co wpisać | Odpowiedź |
|---|---|---|
| Co pobieramy | rodzaj tekstów i jednostka analizy | pełny tekst powieści w dwóch tomach, z metadanymi (autor, epoka, gatunek); jednostkę analizy (rozdział, scena, akapit) ustalacie w polu 4 karty projektu |
| Skąd | serwis i dokładny adres | [wolnelektury.pl/katalog/lektura/lalka/](https://wolnelektury.pl/katalog/lektura/lalka/); tomy: `lalka-tom-pierwszy`, `lalka-tom-drugi` |
| Droga | gotowy plik / API / scraping | oficjalne API bez klucza, które wskazuje gotowe pliki TXT; najprostsza droga z możliwych |
| Dokumentacja ✋ | link do dokumentacji | [wolnelektury.pl/api/](https://wolnelektury.pl/api/); w przeglądarce dopiszcie `?format=json`, bez tego przeglądarka dostaje XML |
| Klucz lub konto | czy potrzebne | niepotrzebne |
| Limity ✋ | ile zapytań | brak opisanych limitów; cały tekst to 3 zapytania (książka i dwa tomy) plus 2 pliki TXT; przerwa 1 s i tak |
| Regulamin ✋ | link i jedno zdanie | [Zasady wykorzystania Wolnych Lektur](https://wolnelektury.pl/info/zasady-wykorzystania/); stopka każdego pliku TXT pozwala wykorzystywać i rozpowszechniać zasoby przy zachowaniu licencji i tych zasad |
| robots.txt ✋ | czy ścieżki są dozwolone | [robots.txt](https://wolnelektury.pl/robots.txt) nie zabrania `/api/` ani `/media/book/txt/`; zabronione są m.in. `/szukaj/` i `/uzytkownik/` |
| Dane osobowe | jakie i jak pseudonimizujemy | brak: postaci są fikcyjne |
| Prawa autorskie | co publikujemy | tekst w domenie publicznej; przypisy i motywy Wolnych Lektur na Licencji Wolnej Sztuki 1.3; stopkę licencyjną odcinamy przed analizą, a przy publikacji podajemy źródło |
| Przechowywanie ✋ | gdzie i jak długo | bez ograniczeń czasowych; surowe TXT w `data/raw/`, tabele w `data/processed/` |
| Próbka na start | ile rekordów | cały tekst obu tomów; próbą jest podział na jednostki: najpierw 5 pierwszych rozdziałów tomu I |
| Czego czat nie był pewien | co sprawdziliście | karta przygotowana ręcznie i sprawdzona w przeglądarce 26.11.2026; kod przepisu 1 uruchomiony 25.11.2026 |

## Wniosek

Materiał da się pobrać w kilka minut, bez klucza i bez ryzyka prawnego. Prawdziwa praca zaczyna się później: podział powieści na jednostki analizy (rozdziały, sceny) i decyzja, co zrobić z pamiętnikiem Rzeckiego pisanym w pierwszej osobie.
