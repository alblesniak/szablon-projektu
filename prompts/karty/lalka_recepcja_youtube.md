# Karta źródła: komentarze o „Lalce” na YouTube (2026)

Wersja: v1 · Data rozpoznania: 2026-11-26 · Przygotował: prowadzący · Zespół: [imiona] · **Status: gotowa**

Dla warstwy `lalka-recepcja` (recepcja 2026). Dalej: [przepis 2](../../docs/przepisy/02_youtube.md).

| Pole | Co wpisać | Odpowiedź |
|---|---|---|
| Co pobieramy | rodzaj tekstów i jednostka analizy | komentarze widzów pod filmami o „Lalce” (zwiastuny, recenzje, wideoeseje); jednostka: komentarz najwyższego poziomu |
| Skąd | serwis i dokładny adres | [wpiszcie adresy wybranych filmów]; przykład z przepisu: [wideorecenzja „Lalki” Tomasza Raczka](https://www.youtube.com/watch?v=gaTtwtC9StY) |
| Droga | gotowy plik / API / scraping | oficjalne API z kluczem: YouTube Data API v3, metoda `commentThreads.list`; scraping stron YouTube odpada |
| Dokumentacja ✋ | link do dokumentacji | [commentThreads.list](https://developers.google.com/youtube/v3/docs/commentThreads/list) |
| Klucz lub konto | czy potrzebne, nazwa sekretu | klucz API z Google Cloud, ograniczony do YouTube Data API v3; sekret `YOUTUBE_API_KEY` w Codespaces (przepis 2, części A i B) |
| Limity ✋ | ile zapytań, koszt | 10 000 jednostek dziennie na projekt; jedno zapytanie zwraca do 100 komentarzy i kosztuje 1 jednostkę; wyszukiwanie filmów (`search.list`) kosztuje 100, więc listę filmów robimy ręcznie |
| Regulamin ✋ | link i jedno zdanie | [Zasady dla deweloperów YouTube API](https://developers.google.com/youtube/terms/developer-policies): API jest drogą przewidzianą przez serwis; przeczytajcie fragment o przechowywaniu danych |
| robots.txt ✋ | czy ścieżki są dozwolone | nie dotyczy: korzystamy z API, nie pobieramy stron |
| Dane osobowe | jakie i jak pseudonimizujemy | nicki i kanały autorów; od razu w notatniku zamieniamy je na `autor_001`…, słownik zamian zostaje tylko w `data/raw/` |
| Prawa autorskie | co publikujemy | komentarze są tekstami ich autorów; publikujemy dane zbiorcze i krótkie cytaty bez nicków |
| Przechowywanie ✋ | gdzie i jak długo | według zasad dla deweloperów dane z API trzeba usunąć albo pobrać od nowa po 30 dniach; datę pobrania wpisujemy do dziennika decyzji |
| Próbka na start | ile rekordów | 100 komentarzy (1 zapytanie) spod jednego filmu, od najnowszych |
| Czego czat nie był pewien | co sprawdziliście | karta przygotowana ręcznie; przepis 2 uruchomiony na prawdziwym kluczu 26.11.2026: 100 komentarzy, 94 autorów |

## Wniosek

Materiał da się pobrać szybko i zgodnie z zasadami serwisu. Ograniczenia: API zwraca tylko część odpowiedzi pod komentarzami (resztę daje `comments.list`), a dobór filmów robimy ręcznie i uzasadniamy w dzienniku decyzji.
