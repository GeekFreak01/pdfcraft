# M11: допечатная подготовка — кто что делает и где мы помогаем

> **Last reviewed:** 2026-10-29 · **Last updated:** 2026-10-29 · **Change:** major (план собственной реализации заменён заметкой: работа уже идёт в открытых PR и в craft-libs)

**Статус:** собственную реализацию не ведём. Сепарации, Output Preview и PDF/X уже делаются в
открытых PR и в `storytold/craft-libs`. По правилу `CLAUDE.md` («если открытый PR покрывает
задачу, его ревьюят или достраивают, а не открывают конкурирующий») мы помогаем им.

Состояние на 2026-10-29. Перед любой работой проверяйте его заново: PR и issues в
`storytold/pdfcraft` (`gh pr list -R storytold/pdfcraft --state all --search "<terms>"`) и
`main` (`git merge-base --is-ancestor`).

## Что уже в работе

| Что | Где | Состояние |
|---|---|---|
| Output Preview: сепарации, покрытие, TAC, overprint, soft proofing, инструмент `print_output_preview` | storytold/pdfcraft#2770 (guillehr2) | открыт |
| То же как общая библиотека: пластины, покрытие и TAC, композит, ink manager, флаттенер прозрачности | `craft-libs/crates/prepress` (перенос из #2770) | в `craft-libs`, PdfCraft не подключает |
| Управление цветом ICC на чистом Rust, профиль FOGRA39 | `craft-libs/crates/cms` | в `craft-libs` |
| Тестовые сцены по краскам (27 шт.) и проверка через Ghostscript `tiffsep` | `craft-libs/crates/plate-test` | в `craft-libs` |
| Save as PDF/X-1a/3/4, PDF/A уровней u и a | storytold/pdfcraft#2777 (guillehr2) | открыт |
| Показ рамок art, trim и bleed | storytold/pdfcraft#2779 (marcoarnulfo) | открыт |
| Preflight идёт по цветовым пространствам до device space | storytold/pdfcraft#2412 (guillehr2) | открыт |
| Проходы по CMYK-краскам для групп прозрачности в `hayro` | storytold/pdfcraft#5199, storytold/pdfcraft#5228 (KenM76) | в `main` |
| Запрос Separation Preview | storytold/pdfcraft#3405 | issue, priority: low |

В `parity/acrobat-features.toml` все пункты `print.*` про допечатную подготовку пока `planned`.

## Где пусто (кандидаты, если мейнтейнеры захотят)

Ни в одном известном PR этого нет. Перед началом проверить ещё раз:

- `print.printer-marks`, `print.print-marks-bleeds`: метки реза, обреза, приводки и шкалы
  цветов при печати. Расширение импозиции в `crates/print`, где метки реза уже есть у постера.
- `print.fix-hairlines`: замена слишком тонких линий.
- Подключение `crates/engine/src/printcheck.rs`, проверки готовности к печати без исправлений.
  Модуль есть, но не вызывается ни из `automation`, ни из UI.
- Подключение `craft-prepress` и `craft-plate-test` к PdfCraft. Решать мейнтейнерам и автору
  #2770: от этого зависит, остаётся ли код #2770 в `crates/render` или уходит в общую библиотеку.

## Как помогаем

По логике проекта (`CLAUDE.md`, `.github/pull_request_template.md`, `docs/releasing.md`):

1. Ревью и тесты к открытым PR: проверяем provenance, never-crash, тест, который падает без
   исправления, и честную запись в `parity/`.
2. Свои изменения — только PR из форка в `main` с веткой вида `<type>/<slug>` и одной задачей
   на коммит (`M11: …`). `main` вливаем в ветку merge-коммитом в тот же день, когда он ушёл
   вперёд. Ветку чужого автора не переписываем.
3. Если нас добавят в редакторы, доводим застрявшие PR до зелёного CI, как это делают
   мейнтейнеры (ветки `integration/<автор>-<дата>`), не трогая историю авторов.
4. В релиз попадают смерженные PR: заметки генерируются из них автоматически. Себя в About
   каждый добавляет сам, своим PR в `contributors/people.toml`.

## Revision history

| Date | Change | Summary |
|---|---|---|
| 2026-10-29 | major | План собственной реализации заменён: сепарации и Output Preview уже в #2770 и `craft-prepress`, PDF/X — в #2777. Осталась карта работ и пустые места. |
| 2026-10-27 | major | Первая версия: поэтапный план сепараций, TAC, overprint, ink manager и конвертации цветов. |
