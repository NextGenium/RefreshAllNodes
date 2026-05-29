# Refresh All Nodes

> **Upstream documentation — nachomonkey / RefreshAllNodes**
>
> Базовая архитектура (bulk Blueprint nodes refresh + optional compilation, toolbar button, Content Browser context menu, Project Settings конфиг) — в upstream от nachomonkey: **https://github.com/nachomonkey/RefreshAllNodes**. Pre-compiled releases: **https://github.com/nachomonkey/RefreshAllNodes/releases**.
>
> Этот README описывает плагин на high-level + **NextGenium-доработки** относительно upstream.

## Overview

Editor plugin для batch-refresh всех Blueprint nodes в проекте (опционально с компиляцией). Добавляет кнопку `Refresh All Blueprint Nodes` в Blueprints toolbar + `Refresh Blueprints` в Content Browser context menu (для refresh per-folder). Полезно при API breaking changes в C++ коде, которые ломают существующие Blueprint nodes.

В Next Framework входит как `3rdParty, Tooling` плагин. Используется на этапе maintenance проекта при крупных C++ refactor'ах.

## When to use

- C++ refactor поменял node signatures, надо рефрешнуть все BP-консамеры разом.
- Engine upgrade ломает BP nodes — bulk refresh без открытия каждого BP вручную.
- Plugin update — refresh blueprints в конкретном плагине через `Additional Blueprint Paths`.
- Содержимое `Content Browser` нужно полностью пересохранить после API change.

## Boundary

- **Что плагин делает** — вызывает built-in UE `Refresh All Nodes` для каждого BP в заданных путях, опционально компилирует.
- **Что не делает** — не чинит broken nodes, не мигрирует deprecated functions, не делает diff / merge.
- **Source Control** — может вызвать массовое resave всех BP; использовать осторожно (см. upstream warning).

## NextGenium-доработки (поверх upstream)

| PR | Что добавлено |
|---|---|
| — | NextGenium-доработок поверх upstream на момент написания не зафиксировано. Repo — clean fork upstream `nachomonkey/RefreshAllNodes` (MIT). |

## Modules

| Модуль | Тип | LoadingPhase | Назначение |
|---|---|---|---|
| `RefreshAllNodes` | Editor | `PostEngineInit` | Toolbar button + Content Browser action + Project Settings config. |

**Plugin dependencies:** `Engine`, `CoreUObject` (AdditionalDependencies).
**Platform:** не ограничено в .uplugin (но pre-compiled releases — Windows 10 64-bit).
**Version:** 1.5.

## Installation

Через Next Framework Loader: **Refresh All Nodes**.

Или вручную:

```bash
cd <YourProject>/Plugins
git clone https://github.com/NextGenium/RefreshAllNodes.git
```

Pre-compiled releases (для UE 5.1+) — `https://github.com/nachomonkey/RefreshAllNodes/releases`. Для UE 5.0 и старее — release v1.4 (`v1.4+1-UE5.0.3`).

## How to use

1. Включить `RefreshAllNodes` плагин в проекте.
2. Открыть любой Blueprint → нажать кнопку **`Refresh All Blueprint Nodes`** в toolbar.
3. Или в Content Browser → правый клик по папке → **`Refresh Blueprints`** (refresh per-folder).
4. Опции в **Project Settings → Plugins → Refresh All Nodes**:
   - `Compile Blueprints` — компилировать после refresh (slower, но ловит ошибки).
   - `Refresh Level Blueprints` — рефрешить level BP (открывает уровни, потребляет память).
   - `Refresh Game Blueprints` — refresh BP в `Content/`.
   - `Refresh Engine Blueprints` — refresh BP в `Engine/Content/` (осторожно).
   - `Additional Blueprint Paths` — массив дополнительных путей (имена плагинов).
   - `Exclude Blueprint Paths` — исключаемые пути.

Полная upstream usage — см. https://github.com/nachomonkey/RefreshAllNodes#usage.

## TODO

- Carry-over from upstream: см. [issues nachomonkey/RefreshAllNodes](https://github.com/nachomonkey/RefreshAllNodes/issues).
- NextGenium-specific: TBD — добавится по мере использования.

## Limitations

- Refresh limited by UE built-in `Refresh All Nodes` функцией — плагин не делает больше, чем сам UE.
- Refresh может сломать nodes / изменить variable types (особенно после HotReload).
- Source Control — может trigger'нуть resave всех BP; использовать вне горячих веток.
- Не несёт ответственности за data loss / damage к BP (см. upstream Limitations).

## Origin

Upstream — **`nachomonkey/RefreshAllNodes`** (https://github.com/nachomonkey/RefreshAllNodes), **MIT License**, author **NachoMonkey** (https://github.com/nachomonkey), version **1.5**.

NextGenium-форк — clean fork upstream MIT плагина, без feature-патчей.

## Maintainers

- TBD — студийный (мейнтейнер не закреплён; плагин — форк upstream).

## References

- **Upstream repo:** https://github.com/nachomonkey/RefreshAllNodes
- **Pre-compiled releases:** https://github.com/nachomonkey/RefreshAllNodes/releases
- **MIT License:** [LICENSE](./LICENSE)
- **Upstream usage docs:** https://github.com/nachomonkey/RefreshAllNodes#usage
