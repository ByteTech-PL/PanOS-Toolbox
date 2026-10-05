# CHANGELOG — PanOS-Toolbox

Indeks istotnych zmian. Format: data (Europe/Warsaw) — obszar — opis.

## 05.10.2026 — dokumentacja

- `STATUS.md`, `CHANGELOG.md`: usunięto wewnętrzne szczegóły środowiska i procesu pracy
  (nazwy hostów, sieci, odwołania do repozytoriów prywatnych, nazwy gałęzi roboczych);
  pozostawiono neutralny opis stanu repo, CI i komponentów. Historia Git nie jest przepisywana.
- `STATUS.md`: problem `npm audit` w jobie `frontend` (25.09.2026) oznaczono jako historyczny
  (job przechodzi); pliki `api pano.txt`, `Ilumio_API`, `ilumio.txt` opisano jako zweryfikowane
  — wyłącznie placeholdery, bez sekretów.
- Usunięto katalog `sessions/` z drzewa repozytorium; zapisy przebiegu pracy są prowadzone poza tym
  publicznym repozytorium, a `/sessions/` dodano do `.gitignore`.

## 25.09.2026 — dokumentacja / rekonsyliacja

- Utworzono `STATUS.md` (źródło prawdy o stanie repo: live footprint, CI, gałęzie/PR, komponenty,
  higiena repozytorium) w ramach rekonsyliacji stanu repo.
- Dodano zapis przebiegu pracy w repozytorium (usunięty 05.10.2026, patrz wyżej).
- `README.md`: dodano sekcję „Stan projektu i przebieg pracy" (wskaźniki STATUS/CHANGELOG).
- Zweryfikowano read-only: brak usług live tego repo (brak kontenerów/usług/endpointów),
  CI na main zielone, wszystkie 9 zdalnych gałęzi PR zmergowane do main, brak otwartych PR.
- Odnotowano aktywny problem CI: job `frontend` (npm audit) pada na nowo opublikowane
  advisories (nanoid high GHSA-2v37-7h3g-55p8, vitest moderate) — pre-existing, niezależny
  od treści zmian; follow-up bump lockfile wymagany w osobnym PR.
