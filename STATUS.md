# STATUS — PanOS-Toolbox

> Klasa repo: **narzędziowe** (zestaw skryptów + aplikacja lokalna PanOS Toolbox).
> Repo **nie prowadzi żadnych usług floty** (brak manifestów deployu w repo; backend Flask
> uruchamiany jest ręcznie przez operatora na stacji roboczej, nie jako usługa).
> Klasy stanu: OBSERVED (dowód + `last_verified_at`) / LAST_KNOWN / UNKNOWN.
> Strefa czasowa: Europe/Warsaw.

## Live footprint

**UNKNOWN → rozstrzygnięte: BRAK.** Weryfikacja read-only 25.09.2026 06:30–06:45 CEST
(last_verified_at: 2026-09-25T06:40:00+0200; dowody: `docker ps` na PRIMUS, `tailscale status`,
`gh run list`, przeszukanie repo): brak kontenerów, usług systemd i endpointów w tailnecie
związanych z PanOS-Toolbox. Aplikacja `PanOS-Toolbox/backend` jest uruchamiana lokalnie
(`start_toolbox.ps1`), poza infrastrukturą floty — brak stanu LIVE do monitorowania w tym repo.

## CI/CD (GitHub Actions) — OBSERVED

- Workflow `Security and tests` (.github/workflows/security.yml): 3 joby — Python (unittest +
  pip-audit + bandit), frontend (npm test/check/audit/build), portable-windows (PowerShell 5.1 +
  ConstrainedLanguage smoke test).
- Ostatnie przebiegi na main (dowód: `gh run list -R ByteTech-PL/PanOS-Toolbox --branch main`,
  last_verified_at: 2026-09-25T06:33:00+0200):
  - CodeQL: success, 2026-09-22 (run 35782396021, headSha b1faded)
  - Dependency Graph: success, 2026-09-16 (run 35153736720, headSha b1faded)
  - Security and tests: success, 2026-08-12 (run 31571156567, headSha b1faded)
- **Znany problem CI (aktywny, last_verified_at: 2026-09-25T06:44:00+0200):** job `frontend`
  pada na `npm audit --audit-level=high` — 1 high (nanoid <3.3.18, GHSA-2v37-7h3g-55p8)
  + 2 moderate (vitest / @vitest/mocker, GHSA-82fw-gwwq-j7x9). Warunek **pre-existing**:
  main b1faded przechodził audit 12.08.2026, ale od tego czasu opublikowano nowe advisories
  dotykające zablokowany lockfile. Job `python` i `portable-windows` na tym samym runie
  success (run 36095321599). Wymagany follow-up: `npm audit fix` / bump lockfile w osobnym
  PR (poza zakresem rekonsyliacji).
- Lokalna replikacja testów (last_verified_at: 2026-09-25T06:36:00+0200, Python 3.14.7 na PRIMUS):
  - backend: 118 testów OK
  - panorama_cleaner: OK (log bez błędów unittest)
  - Uwaga: lokalnie pominięte pip-audit/bandit/frontend (wymagają sieci/Node); pełny zestaw
    wykonuje CI.

## Gałęzie i PR — OBSERVED

- Otwarte PR: **brak** (`gh pr list --state open` → pusta lista,
  last_verified_at: 2026-09-25T06:33:00+0200).
- Gałęzie zdalne (9): `agent/panos-ad-group-generator`, `agent/panos-delete-targets`,
  `agent/panos-portable-package`, `codex/locate-cleanup-script-for-panorama-objects{,-jr5hj9,-qtt236}`,
  `codex/panorama-application-override`, `codex/panorama-cleanup-planner`,
  `codex/panorama-icmp-retry-partial-publish` — wszystkie potwierdzone jako zmergowane do main
  (`git merge-base --is-ancestor`, last_verified_at: 2026-09-25T06:34:00+0200).
  Pozostały jako reference remnants po squash-merge; usunięcie poza zakresem runu.

## Komponenty — LAST_KNOWN

| Komponent | Opis | Stan źródłowy |
|---|---|---|
| `PanOS-Toolbox/` | Aplikacja lokalna (Flask backend + Vite/React frontend), portable Windows package (`build_release.ps1`) | main @ b1faded, CI zielone |
| `panorama_cleaner/` | Planner/cleanup/audit/restore z testami | main @ b1faded, CI zielone |
| Skrypty legacy (`panorama_group_checker_update`, `Panorama_rules_checker.py`, `Panorama_Rule_Finder`, `Panorama_object_cleanup.py`, `legacy_panorama_http.py`, `generate_disable_commands.py`, `pa_ad_group_generator.ps1`, `Ilumio_API`) | Narzędzia CLI dokumentowane w README | main @ b1faded |

## Hygiene — findings (bez zmian w tym runie)

- Pliki śledzone w git o nazwach sugerujących dane dostępowe: `api pano.txt`, `Ilumio_API`
  (wzorce: api-key/password/token; wartości **nie były czytane** — twarda reguła sekretów).
  Rekomendacja: sanitizacja/przeniesienie poza git w osobnym, jawnie autoryzowanym zakresie.
- Duży artefakt w git: `PanOS-Toolbox-20260811-191350.zip` (~1.5 MB) — kandydat do usunięcia
  z repo (release artifacts powinny żyć w GitHub Releases).

## Historyczne

- (przed 25.09.2026) Repo działało bez STATUS.md; stan repo był opisywany wyłącznie w README.md.
  Ta rekonsyliacja (Target ByteTech-PL/agents-global-hub#98) wprowadza SSOT stanu.
