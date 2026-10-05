# STATUS — PanOS-Toolbox

> Klasa repo: **narzędziowe** (zestaw skryptów + aplikacja lokalna PanOS Toolbox).
> Repo **nie prowadzi żadnych usług** (brak manifestów deployu w repo; backend Flask
> uruchamiany jest ręcznie przez operatora na stacji roboczej, nie jako usługa).
> Każdy wpis podaje datę ostatniej weryfikacji (`last_verified_at`); strefa czasowa: Europe/Warsaw.

## Live footprint

**BRAK.** Weryfikacja read-only 25.09.2026 (last_verified_at: 2026-09-25T06:40:00+0200;
dowody: `gh run list`, przeszukanie repo, przegląd środowiska uruchomieniowego): brak
kontenerów, usług i publikowanych endpointów związanych z PanOS-Toolbox. Aplikacja
`PanOS-Toolbox/backend` jest uruchamiana lokalnie (`start_toolbox.ps1`) — brak stanu LIVE
do monitorowania w tym repo.

## CI/CD (GitHub Actions)

- Workflow `Security and tests` (.github/workflows/security.yml): 3 joby — Python (unittest +
  pip-audit + bandit), frontend (npm test/check/audit/build), portable-windows (PowerShell 5.1 +
  ConstrainedLanguage smoke test). Dodatkowo CodeQL (actions, javascript-typescript, python).
- Stan bieżący (last_verified_at: 2026-10-05T13:15:00+0200): wszystkie joby zielone na
  PR #18 — `Security and tests` run 37301403434 (python, frontend, portable-windows: success),
  CodeQL run 37301399802 (success).
- Historyczne (25.09.2026): job `frontend` padał na `npm audit --audit-level=high` po
  publikacji nowych advisories (nanoid <3.3.18, GHSA-2v37-7h3g-55p8; vitest / @vitest/mocker,
  GHSA-82fw-gwwq-j7x9). Stan z 05.10.2026: job `frontend` przechodzi; problem nieaktywny.
- Lokalna replikacja testów (last_verified_at: 2026-09-25T06:36:00+0200, Python 3.14.7):
  - backend: 118 testów OK
  - panorama_cleaner: OK (log bez błędów unittest)
  - Uwaga: lokalnie pominięte pip-audit/bandit/frontend (wymagają sieci/Node); pełny zestaw
    wykonuje CI.

## Gałęzie i PR

- Otwarte PR na 25.09.2026: **brak** (`gh pr list --state open` → pusta lista,
  last_verified_at: 2026-09-25T06:33:00+0200).
- Gałęzie zdalne: 9 gałęzi roboczych z wcześniejszych zmian — wszystkie potwierdzone jako
  zmergowane do main (`git merge-base --is-ancestor`, last_verified_at:
  2026-09-25T06:34:00+0200). Pozostały po squash-merge; kandydaci do usunięcia.

## Komponenty (stan na 25.09.2026)

| Komponent | Opis | Stan źródłowy |
|---|---|---|
| `PanOS-Toolbox/` | Aplikacja lokalna (Flask backend + Vite/React frontend), portable Windows package (`build_release.ps1`) | main @ b1faded, CI zielone |
| `panorama_cleaner/` | Planner/cleanup/audit/restore z testami | main @ b1faded, CI zielone |
| Skrypty legacy (`panorama_group_checker_update`, `Panorama_rules_checker.py`, `Panorama_Rule_Finder`, `Panorama_object_cleanup.py`, `legacy_panorama_http.py`, `generate_disable_commands.py`, `pa_ad_group_generator.ps1`, `Ilumio_API`) | Narzędzia CLI dokumentowane w README | main @ b1faded |

## Higiena repozytorium

- Pliki `api pano.txt`, `Ilumio_API` i `ilumio.txt` zweryfikowano (bieżąca zawartość i historia
  Git, 05.10.2026): nie zawierają sekretów, wyłącznie placeholdery; klucze są podawane przez
  stdin lub zmienne środowiskowe. Brak działań wymaganych.
- Duży artefakt w git: `PanOS-Toolbox-20260811-191350.zip` (~1.5 MB) — kandydat do usunięcia
  z repo (release artifacts powinny żyć w GitHub Releases).

## Historyczne

- (przed 25.09.2026) Repo działało bez STATUS.md; stan repo był opisywany wyłącznie w README.md.
  Rekonsyliacja stanu z 25.09.2026 wprowadziła ten plik jako źródło prawdy o stanie repo.
- 05.10.2026: usunięto z tego pliku wewnętrzne szczegóły środowiska i procesu pracy
  (nazwy hostów, sieci, odwołania do repozytoriów prywatnych i nazwy gałęzi roboczych);
  odświeżono stan CI i wynik weryfikacji plików z placeholderami.
