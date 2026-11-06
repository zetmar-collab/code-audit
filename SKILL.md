---
name: code-audit
description: Użyj, gdy użytkownik prosi o audyt, przegląd kodu lub code review aplikacji (całej albo wskazanego modułu), szuka błędów, luk bezpieczeństwa, problemów wydajności, brakujących testów albo pomysłów na rozwój projektu. Domyślnie tryb tylko do odczytu; kod zmieniaj wyłącznie po wyraźnej prośbie o naprawę.
---

# Code Audit

Przeprowadź dokładny, oparty na dowodach przegląd kodu aplikacji. Raport pisz po polsku.

## Zakres

1. Ustal zakres audytu:
   - Jeśli użytkownik wskazał moduł, katalog lub obszar (np. „backend", „API", „moduł płatności"), audytuj tylko to, a resztę kodu czytaj wyłącznie w celu zrozumienia zależności.
   - Jeśli nie wskazał, audytuj całą aplikację. W dużym repozytorium zacznij od punktów wejścia, warstwy danych, uwierzytelniania i najnowszych zmian, a w raporcie napisz, czego nie zdążyłeś sprawdzić.

2. Ustal kontekst projektu:
   - Odczytaj `README`, `AGENTS.md`, pliki konfiguracyjne, manifest zależności oraz strukturę katalogów.
   - Zidentyfikuj język, framework, punkt startowy aplikacji, warstwę danych, testy i skrypty build/uruchomieniowe.
   - Jeśli istnieje `git`, użyj poleceń tylko do odczytu (`git status`, `git diff`, `git log`), aby priorytetyzować świeże zmiany.

3. Przeanalizuj kod w następujących obszarach:
   - Poprawność logiki i możliwe regresje.
   - Obsługa błędów, wyjątków, wartości pustych i nieprawidłowych danych wejściowych.
   - Bezpieczeństwo: walidacja danych, uwierzytelnianie, autoryzacja, wstrzyknięcia, sekrety, CORS, CSRF, uprawnienia plików, podatne zależności.
   - Wydajność: złożoność, zbędne zapytania, blokujące operacje, memory leaks, N+1 queries.
   - Architektura: separacja odpowiedzialności, duplikacja, martwy kod, spójność nazewnictwa.
   - Jakość: typowanie, czytelność, konwencje projektu, obsługa konfiguracji, logowanie.
   - Testy: pokrycie krytycznych ścieżek, brakujące testy jednostkowe i integracyjne.
   - Dokumentacja i developer experience: instrukcja uruchomienia, zmienne środowiskowe, migracje, przykłady.

4. Priorytetyzuj realne problemy, a nie stylistyczne drobiazgi.

## Zasady

- Nie zmieniaj kodu podczas audytu, chyba że użytkownik wyraźnie poprosi o naprawę.
- Każdy problem potwierdź konkretną lokalizacją: plik oraz numer linii. Przed podaniem linii otwórz plik i sprawdź ją; nie podawaj numerów z pamięci ani z przypuszczenia.
- Wyjaśnij wpływ problemu na działanie aplikacji, użytkownika lub bezpieczeństwo.
- Proponuj minimalną, konkretną poprawkę, a nie ogólniki.
- Przy każdym problemie oznacz pewność: **Potwierdzony** (widać go w kodzie) albo **Hipoteza** (wymaga weryfikacji; napisz, jak ją sprawdzić). Nie przedstawiaj hipotez jako faktów.
- Nie wymyślaj problemów, by wypełnić sekcję. Jeśli w danym obszarze niczego nie znalazłeś, napisz „Brak istotnych uwag" i wskaż, co sprawdziłeś.
- Nie podawaj w raporcie wartości sekretów, tokenów ani haseł; wskaż jedynie plik, linię i typ problemu.
- Testy, lintery i type checker uruchamiaj tylko wtedy, gdy projekt jest wiarygodny (należy do użytkownika lub jest przez niego wskazany) i polecenie jest wymienione w jego konfiguracji. Nie instaluj zależności ani nie uruchamiaj skryptów z nieznanego repozytorium bez zgody użytkownika. Jeśli czegoś nie uruchomiłeś, zaznacz to w raporcie.

## Format raportu

Przygotuj raport w tym układzie. Każdy problem opisuj raz, w jednej, najbardziej pasującej sekcji.

### 1. Podsumowanie
- Krótko: co robi aplikacja, jaki jest ogólny stan kodu i największe ryzyka.
- Zakres audytu: co sprawdzono, czego nie, jakie narzędzia uruchomiono.

### 2. Błędy i problemy poprawności
Dla każdego problemu podaj:
- Ważność: Krytyczny / Wysoki / Średni / Niski
- Pewność: Potwierdzony / Hipoteza
- Lokalizacja: `ścieżka/do/pliku:linia`
- Problem
- Wpływ
- Proponowana poprawka
- Przykład kodu, jeśli to zwiększy jasność

Porządkuj od najwyższej ważności.

### 3. Bezpieczeństwo
- Wymień podatności, braki walidacji, niewłaściwą obsługę sesji, uprawnień i sekretów.
- Użyj tego samego formatu co w sekcji 2 (ważność, pewność, lokalizacja, wpływ, poprawka).

### 4. Wydajność i jakość
- Wskaż wąskie gardła, duplikację, zbyt złożone funkcje oraz miejsca do refaktoryzacji.

### 5. Brakujące testy
- Wymień najważniejsze scenariusze, które powinny być pokryte testami.

### 6. Propozycje funkcji
Zaproponuj 5–10 usprawnień aplikacji, wynikających z tego, co faktycznie robi i czego brakuje. Dla każdej podaj:
- Nazwę funkcji
- Problem użytkownika, który rozwiązuje
- Krótki opis działania
- Priorytet: MVP / Ważne / Później
- Szacowany zakres zmian: frontend, backend, baza danych, testy

Jeśli użytkownik poprosił o audyt wyłącznie błędów lub bezpieczeństwa, pomiń tę sekcję.

### 7. Plan działania
- Uporządkuj rekomendacje w plan: najpierw krytyczne poprawki, potem testy, refaktoryzacja i nowe funkcje.

## Tryb naprawy

Wejdź w ten tryb tylko wtedy, gdy użytkownik wyraźnie poprosi o naprawę (np. „napraw", „wdróż poprawki"). Jeśli prośba dotyczy wybranych problemów, napraw tylko je; w przeciwnym razie zacznij od najwyższej ważności. Wdrażaj poprawki stopniowo:
1. Najpierw popraw błędy krytyczne.
2. Po każdej zmianie uruchom dostępne testy, lint i type check (z zachowaniem zasad bezpieczeństwa powyżej) i nie przechodź dalej, dopóki nie przechodzą albo nie wyjaśnisz, dlaczego nie.
3. Nie wykonuj migracji bazy danych, nie usuwaj plików ani nie twórz commitów bez potwierdzenia.
4. Na końcu wygeneruj listę zmienionych plików, wynik weryfikacji i pozostałe ryzyka.
