# CHANGELOG — PanOS-Toolbox

Indeks istotnych zmian. Format: data (Europe/Warsaw) — obszar — opis.

## 25.09.2026 — dokumentacja / rekonsyliacja

- Utworzono `STATUS.md` (SSOT stanu repo: live footprint, CI, gałęzie/PR, komponenty,
  findings hygiene) w ramach rekonsyliacji main==live (Target
  ByteTech-PL/agents-global-hub#98, sesja
  `sessions/2026/09/2026-09-25_06-37-02-opencode-live-reconcile.md`).
- Utworzono `sessions/` (indeks + pierwsza sesja) jako audytowalny zapis pracy.
- `README.md`: dodano sekcję „Stan projektu i przebieg pracy" (wskaźniki STATUS/sessions/CHANGELOG).
- Zweryfikowano read-only: brak usług live tego repo (brak kontenerów/usług/endpointów),
  CI na main zielone, wszystkie 9 zdalnych gałęzi PR zmergowane do main, brak otwartych PR.
- Odnotowano aktywny problem CI: job `frontend` (npm audit) pada na nowo opublikowane
  advisories (nanoid high GHSA-2v37-7h3g-55p8, vitest moderate) — pre-existing, niezależny
  od treści zmian; follow-up bump lockfile wymagany w osobnym PR.
