---
name: wykres
description: Kod do notatnika, który rysuje jeden wykres odpowiadający na jedno pytanie z karty projektu i zapisuje go na stronę projektu. Używaj, gdy w data/processed są gotowe wyniki.
argument-hint: pytanie i plik z danymi, np. jak zmienia się ocena adaptacji w czasie? data/processed/kodowanie_v1.csv
disable-model-invocation: true
---

# Jeden wykres, jedno pytanie

Napisz kod do notatnika, który odpowiada wykresem na jedno pytanie. Przeczytaj pola 1 i 7 karty projektu w `README.md`.

## Kroki

1. Zanim napiszesz kod, w 2–3 zdaniach zaproponuj rodzaj wykresu i wyjaśnij, dlaczego pasuje do pytania (porównanie grup: słupki; zmiana w czasie: linia; udziały kategorii: słupki skumulowane; relacje: graf sieci). Odradzaj wykresy kołowe i 3D.
2. **Przygotowanie danych**: wczytaj plik z `../data/processed/`, policz to, co trzeba (liczebności, odsetki), i pokaż małą tabelę z liczbami, na których opiera się wykres.
3. **Wykres**: `plotly` (interaktywny, na stronę) albo `matplotlib` (statyczny). Tytuł w formie odpowiedzi albo pytania, podpisy osi i legenda po polsku, liczebność `n` w podtytule albo podpisie. Te same kategorie zawsze w tych samych kolorach.
4. **Zapis**: `../docs/wykresy/<krotka_nazwa>.html` (plotly) albo `.png` (matplotlib) i jedna linia HTML do wklejenia w `docs/index.html`.
5. **Interpretacja**: 2–3 zdania, co widać, i jedno zdanie, czego wykres nie pokazuje (pole 10 karty projektu).

Komentarze w kodzie po polsku. Po kodzie sekcja **„Po uruchomieniu powinieneś zobaczyć”**.
