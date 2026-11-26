# Karta źródła: dyskusje o „Lalce” na Reddicie (2026)

Wersja: v1 · Data rozpoznania: 2026-11-26 · Przygotował: prowadzący · Zespół: [imiona] · **Status: gotowa (droga ręczna)**

Dla warstwy `lalka-recepcja` (recepcja 2026). Dalej: [przepis 4: ręczna próbka](../../docs/przepisy/04_reczna_probka.md).

| Pole | Co wpisać | Odpowiedź |
|---|---|---|
| Co pobieramy | rodzaj tekstów i jednostka analizy | komentarze w wątkach o serialu i filmie (2026), np. w polskich subredditach o filmach i serialach; jednostka: komentarz |
| Skąd | serwis i dokładny adres | [wpiszcie adresy wybranych wątków] |
| Droga | gotowy plik / API / scraping / ręcznie | **ręcznie**: API wymaga zgody Reddita, a automatyczne pobieranie stron jest zablokowane |
| Dokumentacja ✋ | link do dokumentacji | [Public Content Policy](https://support.reddithelp.com/hc/en-us/articles/26410290525844-Public-Content-Policy); dostęp dla badaczy: [r/reddit4researchers](https://www.reddit.com/r/reddit4researchers/) |
| Klucz lub konto | czy potrzebne | do czytania wątków niepotrzebne; dostęp do API tylko po wniosku i akceptacji Reddita (Responsible Builder Policy, od listopada 2025) |
| Limity ✋ | ile zapytań | nie dotyczy przy pracy ręcznej |
| Regulamin ✋ | link i jedno zdanie | Public Content Policy ogranicza dostęp do treści i ich wykorzystanie; przeczytajcie, czego dotyczy, zanim wyślecie teksty do modelu językowego |
| robots.txt ✋ | czy ścieżki są dozwolone | [robots.txt](https://www.reddit.com/robots.txt): `Disallow: /` dla wszystkich programów; publiczne adresy `.json` odpowiadają błędem 403 (sprawdzone 26.11.2026) |
| Dane osobowe | jakie i jak pseudonimizujemy | nazwy użytkowników → `autor_001`…; nie przepisujemy ich do tabeli |
| Prawa autorskie | co publikujemy | komentarze są tekstami ich autorów; publikujemy dane zbiorcze i krótkie cytaty bez nazw użytkowników |
| Przechowywanie ✋ | gdzie i jak długo | tabela w `data/raw/` (poza repozytorium); usunięte komentarze nie wracają do analizy |
| Próbka na start | ile rekordów | 20–30 komentarzy z 2–3 wątków, z adresem każdego komentarza |
| Czego czat nie był pewien | co sprawdziliście | robots.txt i odpowiedź `.json` sprawdzone 26.11.2026; zasady API według ogłoszenia Reddita z listopada 2025: wróćcie do nich przed wnioskiem |

## Wniosek

Na zajęcia: mała, ręcznie zebrana próbka z kilku wątków. Wniosek o dostęp do API ma sens tylko przy dłuższym projekcie i może zostać odrzucony. Ważna decyzja do dziennika: czy teksty z Reddita wysyłamy do modelu językowego (`/koduj`), czy kodujemy je ręcznie.
