---
name: code-audit
description: Analizuje kod aplikacji pod kątem błędów, bezpieczeństwa, wydajności, jakości i brakujących funkcji. Użyj, gdy użytkownik prosi o przegląd kodu, audyt aplikacji, znalezienie błędów lub propozycje rozwoju. Nie modyfikuj kodu bez wyraźnej prośby.
---

# Code Audit

Przeprowadź dokładny, oparty na dowodach przegląd kodu aplikacji.

## Zakres

1. Najpierw ustal kontekst projektu:
   - Odczytaj `README`, `AGENTS.md`, pliki konfiguracyjne, manifest zależności oraz strukturę katalogów.
   - Zidentyfikuj język, framework, punkt startowy aplikacji, warstwę danych, testy i skrypty build/uruchomieniowe.
   - Jeśli istnieje `git`, sprawdź `git status` oraz `git diff`, aby priorytetyzować świeże zmiany.

2. Przeanalizuj kod w następujących obszarach:
   - Poprawność logiki i możliwe regresje.
   - Obsługa błędów, wyjątków, wartości pustych i nieprawidłowych danych wejściowych.
   - Bezpieczeństwo: walidacja danych, uwierzytelnianie, autoryzacja, wstrzyknięcia, sekrety, CORS, CSRF, uprawnienia plików.
   - Wydajność: złożoność, zbędne zapytania, blokujące operacje, memory leaks, N+1 queries.
   - Architektura: separacja odpowiedzialności, duplikacja, martwy kod, spójność nazewnictwa.
   - Jakość: typowanie, czytelność, konwencje projektu, obsługa konfiguracji, logowanie.
   - Testy: pokrycie krytycznych ścieżek, brakujące testy jednostkowe i integracyjne.
   - Dokumentacja i developer experience: instrukcja uruchomienia, zmienne środowiskowe, migracje, przykłady.

3. Priorytetyzuj realne problemy, a nie stylistyczne drobiazgi.

## Zasady

- Nie zmieniaj kodu podczas audytu, chyba że użytkownik wyraźnie poprosi o naprawę.
- Każdy problem potwierdź konkretną lokalizacją: plik oraz numer linii.
- Wyjaśnij wpływ problemu na działanie aplikacji, użytkownika lub bezpieczeństwo.
- Proponuj minimalną, konkretną poprawkę, a nie ogólniki.
- Odróżnij pewne błędy od hipotez wymagających weryfikacji.
- Nie zgłaszaj sekretów, tokenów ani haseł w raporcie; wskaż jedynie plik i typ problemu.
- Jeśli projekt ma testy, lintery lub type checker, uruchom je, jeśli są bezpieczne i dostępne.

## Format raportu

Przygotuj raport w tym układzie:

### 1. Podsumowanie
- Krótko: co robi aplikacja, jaki jest ogólny stan kodu i największe ryzyka.

### 2. Błędy krytyczne
Dla każdego problemu podaj:
- Ważność: Krytyczny / Wysoki / Średni / Niski
- Lokalizacja: `ścieżka/do/pliku:linia`
- Problem
- Wpływ
- Proponowana poprawka
- Przykład kodu, jeśli to zwiększy jasność

### 3. Bezpieczeństwo
- Wymień podatności, braki walidacji, niewłaściwą obsługę sesji, uprawnień i sekretów.

### 4. Wydajność i jakość
- Wskaż wąskie gardła, duplikację, zbyt złożone funkcje oraz miejsca do refaktoryzacji.

### 5. Brakujące testy
- Wymień najważniejsze scenariusze, które powinny być pokryte testami.

### 6. Propozycje funkcji
Zaproponuj 5–10 usprawnień aplikacji. Dla każdej podaj:
- Nazwę funkcji
- Problem użytkownika, który rozwiązuje
- Krótki opis działania
- Priorytet: MVP / Ważne / Później
- Szacowany zakres zmian: frontend, backend, baza danych, testy

### 7. Plan działania
- Uporządkuj rekomendacje w plan: najpierw krytyczne poprawki, potem testy, refaktoryzacja i nowe funkcje.

## Tryb naprawy

Jeśli użytkownik napisze „napraw”, wdrażaj poprawki stopniowo:
1. Najpierw popraw błędy krytyczne.
2. Po każdej zmianie uruchom dostępne testy, lint i type check.
3. Nie wykonuj migracji bazy danych ani nie usuwaj plików bez potwierdzenia.
4. Na końcu wygeneruj listę zmian i pozostałe ryzyka.
