# Arabic interface

> **Last reviewed:** 2026-10-14 · **Last updated:** 2026-10-14 · **Change:** minor (Arabic interface catalog, right-to-left display order and the installed Arabic font fallback) · **Target:** WordCraft 0.5.0

Choose **File › Options › Interface language › العربية**, or follow an Arabic system display
language (`ar`, `ar-SA`, `ar-EG`, `ar_MA.UTF-8` and the other `ar-*` locales). The catalog code is
`ar`; `ui.language` with `{"value":"ar"}` selects it explicitly, and `WORDCRAFT_LANGUAGE=ar-EG`
does the same for a headless screenshot or a launch while the preference is `auto`. The choice is
saved with the other interface settings. Document text, command ids, CLI/MCP/control responses and
file paths do not change.

`crates/ui-egui/src/i18n/ar.tsv` has 2,061 entries: every key of the German catalog (built-in
style names, equation gallery labels, paper sizes, watermarks and greetings included). It is
written from the plain meaning of the English labels; no proprietary translation resources were
consulted. The catalog format has no plural forms; the count labels word a number and its noun
neutrally (`{words} كلمة`). Placeholders, `\n` breaks in ribbon labels and dialog ellipses are
kept, and strings added later fall back to English until they are translated.

## Right to left

The interface text reads right to left. egui lays each line out left to right, so the catalog
keeps its translations in logical order on disk and `LangInfo::rtl` puts them into display order
when the catalog loads (`bidi::display_rtl`), the way the sibling apps do it. Latin names and
placeholders keep their own direction inside a phrase (`About WordCraft` →
`WordCraft حول`, `Page {page} of {pages}` → `الصفحة {page} من {pages}` →
`12 من 3 الصفحة`).

Text the user typed — file names in the caption and the file browser, document names on the Home
screen, styles the author named — is stored as typed and reordered only where it is painted
(`bidi::visual`), so an Arabic name shows its parts in reading order while the document, the file
system and saved content keep the logical text.

The window's chrome itself is not mirrored: the ribbon, panes and dialogs stay in their
left-to-right arrangement, as in the sibling apps' first Arabic interfaces. Mirroring is tracked
in [localization-parity.md](localization-parity.md) (Hebrew with RTL, #20).

## Fonts

The embedded interface faces (Inter, JetBrains Mono) have no Arabic glyphs. On the desktop the
interface borrows an installed Arabic font — Tahoma or Segoe UI on Windows, Geeza Pro on macOS,
Noto Sans Arabic (then DejaVu or FreeSans) on Linux — chosen once against the characters the
catalog actually uses, after the embedded faces (`theme::font_definitions`,
`wordcraft_fonts::system_arabic_ui_font`). The same fallback covers the language names in the
Options list when another language is active, as the CJK fallback does (#241, #486). On the web
there are no system fonts to read; Arabic UI text there needs a craft-fonts Arabic face.

No Arabic font file is bundled; fonts live in the craft-fonts repository
(`../craftrules` standards, `AGENTS.md`).

## Conventions and terminology

- Modern Standard Arabic; labels use common computing vocabulary, written from the meaning of the
  English text, never through another catalog or a proprietary product's localization.
- Product, format and technology names stay in Latin letters (WordCraft, PDF, DOCX, Markdown, MCP,
  Zotero, SmartArt).
- Commands and buttons use the verbal noun (الحفظ، الإدراج، الفتح); messages address the user in
  the second person.
- Placeholders stay wrapped in braces and are filled in place in display order.

| Source | Arabic |
|---|---|
| Home / Insert / Draw / Design / Layout | الشريط الرئيسي / إدراج / رسم / تصميم / تخطيط |
| References / Mailings / Review / View / Help | المراجع / المراسلات / مراجعة / عرض / تعليمات |
| File / Clipboard | ملف / الحافظة |
| Font / Paragraph / Styles | الخط / فقرة / الأنماط |
| Header / Footer / Heading | الرأس / التذييل / عنوان |
| Footnote / Endnote / Caption | حاشية سفلية / حاشية خاتمية / تسمية توضيحية |
| Table of Contents / Index | جدول المحتويات / الفهرس |
| Citation / Bibliography | اقتباس / قائمة المراجع |
| Track Changes / Accept / Reject | تعقّب التغييرات / قبول / رفض |
| Comment / Markup | تعليق / توصيف |
| Mail Merge / Recipients / Merge Field | دمج المراسلات / المستلمون / حقل الدمج |
| Section / Section Break | مقطع / فاصل مقطعي |
| Undo / Redo / Repeat | تراجع / إعادة / تكرار |
| Page {page} of {pages} | الصفحة {page} من {pages} |
| Ribbon / Pane / Quick Access Toolbar | الشريط / الجزء / شريط أدوات الوصول السريع |
| Proofing / Word Count | التدقيق / عدد الكلمات |
| Find / Replace | بحث / استبدال |
| Print / Save As | طباعة / حفظ باسم |

## Verification and remaining scope

`cargo test -p wordcraft-ui-egui --lib i18n::tests` covers `ar` and `ar-*` locale tags and
preference order, saving and restoring `ar`, unchanged document content, every key of the German
catalog, display order of lookups and filled placeholders, the installed-font fallback and its
glyph coverage, and that only the Arabic interface asks for an installed Arabic font. The shared
catalog checks (validation, every tab and command translated) include Arabic automatically, and
`bidi`'s tests cover the reordering and odd input. The interface was drawn and checked headlessly
(`cargo run -p wordcraft-ui-egui --example ui_shot`).

Spelling, grammar and hyphenation have no Arabic resources: `editing_lang` stays `None` and
documents keep the default editing language. The chrome is not mirrored yet, there are no
Eastern Arabic numerals in the interface, and a native-speaker review of the catalog is welcome.

## Revision history

| Date | Change | Summary |
|---|---|---|
| 2026-10-14 | minor | Arabic interface (2,061 entries, the German catalog's key set), `LangInfo::rtl` display order, `bidi` module, installed Arabic font fallback, `ar-*` locale resolution, docs |
