# Ćwiczenie 8. Zgłoszenie u prowadzącego

**Cel:** zgłosić projekt w jednym wspólnym miejscu, żeby prowadzący miał linki do wszystkich zespołów.

> [!IMPORTANT]
> Zgłoszenie dodajecie w **repozytorium szablonu prowadzącego**, nie w swoim. Jeden komentarz na zespół.

## Kroki (jedna osoba z zespołu)

1. Otwórz [Issues w repozytorium szablonu](https://github.com/alblesniak/szablon-projektu/issues) i wybierz przypięte zgłoszenie **Zgłoszenia projektów**.
2. Na dole strony, w polu komentarza, wklej poniższy wzór i uzupełnij go.
3. Kliknij **Comment**.
4. Poprawki wprowadzasz przez edycję własnego komentarza: ⋯ → **Edit**. Nie dodawaj nowego komentarza.

```
**Zespół:** Imię Nazwisko (login), Imię Nazwisko (login)
**Temat:** „Lalka”: [warstwa, np. recepcja 2026] / własny temat: [jaki]
**Repozytorium:** https://github.com/...
**Strona (GitHub Pages):** https://....github.io/.../
**Pytanie badawcze w jednym zdaniu:** ...
**Materiał:** skąd teksty i ile mniej więcej
**Metody:** sieć / korpus / LLM / mapa
**Największy problem na dziś:** co nie działa albo czego nie wiecie
```

## Na koniec zajęć (oboje)

- W swoim codespace: **Commit** i **Sync**.
- Na [github.com/codespaces](https://github.com/codespaces): ⋯ → **Stop codespace**.

✅ **Punkt kontrolny:** wasz komentarz jest widoczny pod zgłoszeniem prowadzącego, a linki w nim działają.

## Na następne zajęcia

1. `prompts/karta_zrodla_v1.md`: rozpoznanie źródła poleceniem `/zrodlo` i samodzielne sprawdzenie pól oznaczonych ✋ ([przepisy](../przepisy/README.md)).
2. Pola 5–6 karty projektu w README: karta kodowa i plan walidacji.
3. `prompts/karta_kodowa_szablon.md`: pierwsza wersja promptu i test na 5 trudnych tekstach.
4. `notebooks/01_pobranie.ipynb` na próbce 20 tekstów, z zapisem `data/processed/teksty.csv` (własne źródło: polecenie `/pobierz`).
5. Wszystko zapisane commitem i zsynchronizowane.

[← Ćwiczenie 7](07_github_pages.md) · [Spis treści](README.md)
