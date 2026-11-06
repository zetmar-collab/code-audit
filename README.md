# Code Audit

Skill do audytu kodu aplikacji w **Codex** i **Claude Code**, przygotowany przez [Marka Zettla — zetmar-collab](https://github.com/zetmar-collab). Instrukcje i raporty są w języku polskim.

Analizuje aplikację lub wskazaną część projektu, znajduje błędy i ryzyka oraz proponuje konkretne poprawki, brakujące testy i nowe funkcje. Każde wykryte zagrożenie powinno mieć lokalizację w kodzie, opis wpływu i priorytet.

## Co robi skill

- Rozpoznaje strukturę projektu, język, framework, warstwę danych i dostępne narzędzia.
- Sprawdza logikę, obsługę błędów, wartości puste i nieprawidłowe dane wejściowe.
- Analizuje bezpieczeństwo API, uwierzytelnianie, autoryzację, walidację, wstrzyknięcia, CORS, CSRF i zarządzanie sekretami.
- Wskazuje problemy wydajności, zbędne zapytania, duplikację i miejsca do refaktoryzacji.
- Identyfikuje brakujące testy krytycznych ścieżek oraz luki w dokumentacji.
- Proponuje 5–10 usprawnień aplikacji z priorytetem i zakresem zmian.
- Układa plan działania: krytyczne poprawki, testy, refaktoryzacja i rozwój.

Domyślnie wykonuje przegląd bez modyfikowania kodu. Na wyraźną prośbę o naprawę wdraża poprawki stopniowo i uruchamia dostępne testy, lint oraz sprawdzanie typów. Migracje bazy danych i usuwanie plików wymagają potwierdzenia zgodnie z instrukcją skilla.

To zestaw instrukcji dla agenta AI, a nie samodzielny skaner. Wyniki zależą od dostępnego kodu, modelu, uprawnień i możliwości uruchomienia narzędzi projektu. Skill rozróżnia potwierdzone błędy i hipotezy wymagające weryfikacji.

## Wymagania

- Zainstalowany Codex lub Claude Code obsługujący lokalne skille.
- Git do instalacji poniższymi poleceniami.
- Dostęp agenta do katalogu aplikacji, którą chcesz przeanalizować.

Sam skill nie wymaga dodatkowych bibliotek ani własnych kluczy API. Do uruchomienia testów aplikacji potrzebne są jej zależności.

## Instalacja w Codex

### Windows — PowerShell, dla wszystkich projektów użytkownika

```powershell
New-Item -ItemType Directory -Force -Path "$HOME/.agents/skills" | Out-Null
git clone https://github.com/zetmar-collab/code-audit.git "$HOME/.agents/skills/code-audit"
```

### macOS / Linux, dla wszystkich projektów użytkownika

```bash
mkdir -p ~/.agents/skills
git clone https://github.com/zetmar-collab/code-audit.git ~/.agents/skills/code-audit
```

Plik instrukcji powinien znaleźć się pod ścieżką `~/.agents/skills/code-audit/SKILL.md`. Jeżeli chcesz udostępnić skill tylko w jednym projekcie, sklonuj repozytorium do `.agents/skills/code-audit` w katalogu tego projektu.

Otwórz projekt aplikacji w Codex. Jeśli skill nie pojawi się na liście, uruchom ponownie Codex i rozpocznij nową rozmowę. Wywołuj go jako `$code-audit`.

### Przykłady w Codex

Audyt całej aplikacji i propozycje rozwoju:

```text
$code-audit przeanalizuj całą aplikację, znajdź błędy i zaproponuj nowe funkcje
```

Audyt backendu:

```text
$code-audit przeanalizuj backend, skup się na bezpieczeństwie API, obsłudze błędów i brakujących testach
```

## Instalacja w Claude Code

### Windows — PowerShell, dla wszystkich projektów użytkownika

```powershell
New-Item -ItemType Directory -Force -Path "$HOME/.claude/skills" | Out-Null
git clone https://github.com/zetmar-collab/code-audit.git "$HOME/.claude/skills/code-audit"
```

### macOS / Linux, dla wszystkich projektów użytkownika

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/zetmar-collab/code-audit.git ~/.claude/skills/code-audit
```

Plik instrukcji powinien znaleźć się pod ścieżką `~/.claude/skills/code-audit/SKILL.md`. Instalacja tylko dla projektu: sklonuj repozytorium do `.claude/skills/code-audit` w katalogu tego projektu.

Uruchom Claude Code w katalogu aplikacji. Jeśli skill nie jest widoczny, rozpocznij nową sesję. Wywołuj go jako `/code-audit`.

### Przykłady w Claude Code

```text
/code-audit przeanalizuj całą aplikację, znajdź błędy i zaproponuj nowe funkcje
```

```text
/code-audit przeanalizuj backend, skup się na bezpieczeństwie API, obsłudze błędów i brakujących testach
```

Polecenia z `$code-audit` i `/code-audit` wpisuj w rozmowie z agentem, nie w terminalu PowerShell lub Bash.

## Aktualizacja

Jeśli skill został zainstalowany przez `git clone`, pobierz aktualizację odpowiednim poleceniem:

```bash
# Codex
git -C ~/.agents/skills/code-audit pull --ff-only

# Claude Code
git -C ~/.claude/skills/code-audit pull --ff-only
```

W PowerShell użyj ścieżki w cudzysłowie, np. `git -C "$HOME/.agents/skills/code-audit" pull --ff-only`. Przy instalacji dla projektu podaj jego ścieżkę. Jeżeli katalog docelowy zawiera już inną kopię skilla, nie klonuj do niego ponownie; najpierw porównaj obie wersje. Unikaj instalowania kilku kopii o nazwie `code-audit` w lokalizacjach odczytywanych przez tego samego agenta.

## Jak wygląda raport

1. **Podsumowanie** — przeznaczenie aplikacji, stan kodu i największe ryzyka.
2. **Błędy krytyczne** — ważność, plik i linia, problem, wpływ i proponowana poprawka.
3. **Bezpieczeństwo** — podatności i braki w zabezpieczeniach.
4. **Wydajność i jakość** — wąskie gardła i miejsca do refaktoryzacji.
5. **Brakujące testy** — najważniejsze scenariusze do pokrycia.
6. **Propozycje funkcji** — problem użytkownika, działanie, priorytet i zakres zmian.
7. **Plan działania** — kolejność wdrażania rekomendacji.

## Zawartość repozytorium

- [SKILL.md](SKILL.md) — instrukcje wykonywania audytu i trybu naprawy.
- [README.md](README.md) — opis, instalacja i przykłady użycia.

## Dokumentacja narzędzi

Ścieżki instalacji sprawdzono 6 listopada 2026 r. w oficjalnej dokumentacji: [lokalne skille w Codex](https://learn.chatgpt.com/docs/build-skills) oraz [skille w Claude Code](https://code.claude.com/docs/en/skills).
