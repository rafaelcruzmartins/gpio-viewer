# CLAUDE.md

## Regras gerais

- NUNCA commitar com autoria ou coautoria do Claude. O autor é sempre Rafael Cruz <rafaelcruzlm@gmail.com>. Não adicionar trailers `Co-Authored-By`, `Claude-Session` ou similares.
- SEMPRE commitar na `main`. Não criar branches novas.

## Notas técnicas

- Toda a biblioteca está em `src/gpio_viewer.h`. Exige ESP32 Arduino Core 3.x.
- `#include <gpio_viewer.h>` deve vir primeiro: macros interceptam `pinMode`/`ledcAttach*`.
- A UI web (build Vue minificado) fica em `docs/`, servida pelo GitHub Pages (branch `main`, pasta `/docs`) em `https://rafaelcruzmartins.github.io/gpio-viewer/` = `baseURL` em `src/gpio_viewer.h`. URLs absolutas dessa base também aparecem em `docs/GPIOViewerVue.js` e `docs/assets/main.css`. Endpoints (`/events`, `/release`, `/sampling`, `/free_psram`, `/espinfo`, `/partition`, `/pinmodes`, `/pinfunctions`) e eventos SSE (`gpio-state`, `free_heap`, `free_psram`) são acoplados a ela; não alterar sem atualizar a UI.
- Manter `release` (`src/gpio_viewer.h`) e `version` (`library.properties`) sincronizados.
