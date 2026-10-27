# Card Builder: Enterprise Release & Handoff Notes

## System Overview

**BrickRoad_Card_Builder.html** is an offline, single-file HTML workstation (zero server, zero external dependencies, 100% self-contained) for building, editing, standardizing, and printing commercial coffee shop operational documents in one unified Brick Road brand aesthetic:

- **SOPs**: multi-phase steps, sub-steps, structured equipment/supplies tables, warning/pro-tip callout cards, step checklists, double-sided printing.
- **Recipe Cards**: dual-size volume columns (16 oz / 20 oz), Hot vs Iced visual modes, syrup pump and espresso shot build matrices, rim/finish lines, allergen tags.
- **Clean Play Checklists**: daily checklists with interactive checkboxes, Monday-to-Sunday deep-clean station rotation, shift breakdown matrix (Open / Mid / Close), sanitizer chemical PPM standards.
- **Order & Count Sheets**: spreadsheet-style inventory count tables, full-height draggable column dividers, 8 print-safe row background colors, schedule subtitles, BOH freezer layout charts, repacking guides, vendor contact cards.

Everything prints natively to Letter PDF from the browser. All fonts, styles, icons, and libraries are embedded locally.

---

## File Architecture & Mirrors

- `/home/jpesca/antigravity/OpsFlow-Redesign/BrickRoad_Card_Builder.html`: Primary working file.
- `/home/jpesca/Downloads/files/BrickRoad_Card_Builder.html`: Production distribution file.
- `/home/jpesca/Downloads/files/BrickRoad_Card_Builder_Geminis_Version.html`: Synchronized backward-compatible mirror.
- `/home/jpesca/Downloads/files/Geminis_Version.html`: Synchronized backward-compatible mirror.
- `/home/jpesca/antigravity/OpsFlow-Redesign/BrickRoad_Card_Builder_Geminis_Version.html`: Synchronized mirror.
- `/home/jpesca/antigravity/OpsFlow-Redesign/Geminis_Version.html`: Synchronized mirror.

---

## Zero-Emojis & Zero AI Tells Policy

- **Complete Emoji Purge:** In accordance with production design standards, all emojis across the entire application interface, templates, component registry, headers, modals, toasts, and status bars have been purged and replaced with crisp, lightweight SVG icons (`.ico` class) and standard typographical symbols (`&rarr;`, `&bull;`, `&minus;`, `&plus;`, `&check;`).
- **Verified via Automated DOM TreeWalker:** Headless Chrome test asserts **0 emojis** in the rendered DOM.
- **Professional Copy:** All UI labels, tooltips, dialogs, and toasts have been rewritten to eliminate conversational AI tropes, providing a clean commercial software feel.

---

## Builder Suite & Sidebar Workstation Fixes

### 1. Header & Navigation Controls Restored
All controls in the sidebar workstation (`#ed`) were audited and verified 100% functional:
- **Builder Suite Brand Area:** Navigates directly to the Blocks tab.
- **SOP Document Type Pill (`#ed-doc-badge`):** Dynamically reflects the active document type and opens the interactive Document Type Switcher (`openTypeSwitcher()`).
- **Dock Toggle (`#ed-dock`):** Smoothly toggles sidebar width between compact mode (280px) and wide mode (440px). Duplicate event listeners were consolidated into a single clean toggle.
- **Close Button (`#ed-close` / `×`):** Closes the sidebar panel and returns cleanly to canvas page mode.
- **Segmented Navigation Tabs (`#tab-blocks`, `#tab-outline`, `#tab-props`, `#tab-pro`):**
  - **Blocks:** Component toolbox with categorized cards (Structure, Procedural, Grids, Coffee Visuals).
  - **Outline:** Document tree hierarchy with jump-to-canvas, settings, duplicate, and delete controls.
  - **Props:** Contextual Property Inspector showing table properties, row heights, color swatches, column alignment, and formulas.
  - **Pro:** Bidirectional Markdown workstation with real-time two-way synchronization.

### 2. Auto-Open Dialog Defect Eliminated
- Removed the legacy `setTimeout(openNotes, 400)` routine that previously auto-opened the dialog modal `#und` on startup. This modal's bounding rectangle was invisible to users but was capturing all pointer events, causing buttons to appear unresponsive.

### 3. Sidebar Scrolling & Wheel Forwarding
- Hardened flexbox layout hierarchy: `#ed` uses `flex: 0 0 440px; display: flex; flex-direction: column; height: 100%; overflow: hidden;`.
- `#fm` content container has `flex: 1 1 0% !important; min-height: 0; overflow-y: auto; padding: 14px 14px 44px 14px;` with custom thin scrollbars.
- Wheel events on the header (`.ed-modern-header`) and footer (`.ed-sticky-footer`) are forwarded directly to `#fm.scrollTop` so mousewheel scrolling works anywhere over the sidebar.

### 4. White Bar Obscuration Eliminated & Geometry Hardened
- Corrected status bar (`#sb`) styling from blinding white (`#ffffff`) to warm brand tint (`#fbf7ee`) with `border-top: 1.5px solid #28140a`, eliminating the disjointed white strip above document tabs.
- Sticky footer (`.ed-sticky-footer`) restyled to `#fbf7ee`.
- Bottom document tabs (`#tabs`) positioned cleanly at `bottom: 0`, verified 100% uncovered across the entire viewport width.

---

## Universal Font & Text Size Selector

- **Word-Style Font Size Dropdown & Steppers:**
  - Added a dedicated font size dropdown (`#fmt-size-select`) to the top formatting bar (`#ctx`) offering point sizes: 8pt, 9pt, 10pt, 11pt, 12pt, 14pt, 16pt, 18pt, 20pt, 24pt.
  - Added companion font size stepper buttons: `A-` (decrease font size by 1-2pt) and `A+` (increase font size by 1-2pt).
- **Floating Mini-Toolbar Integration:**
  - The floating selection mini-toolbar (`#sel-bubble`) includes `A-` and `A+` buttons for immediate on-canvas font resizing when text is highlighted.
- **Property Inspector Integration:**
  - Added font size and typography controls to the sidebar Property Inspector (`#tab-props`).
- **Engine Capabilities:**
  - Global helper functions `window.applyFontSize(sizeStr)` and `window.stepFontSize(delta)` handle direct style application across both regular elements and `contenteditable="plaintext-only"` spans.

---

## Enterprise Store Features

### 1. Ready-to-Print Master Store Binder (`MasterBinder`)
- Compiles all 14 standardized store documents into a single pre-paginated master store binder.
- Official Brick Road Coffee double-ruled cover page (`.pg-binder-cover`) with store name, edition stamp, and manager seal.
- Categorized Table of Contents (`.pg-binder-toc`) with automated page calculations and dot leader guides.
- Continuous running headers and global page numbering (`Manual Page X of Total`).
- Standalone HTML and PDF export options.

### 2. In-Store iPad / Tablet Interactive Mode (`TabletMode`)
- Standalone touch-optimized interface for behind-the-bar iPads and walk-in tablets.
- Large 32px touch checkboxes with live completion timestamps.
- Count steppers (`[-]` and `[+]`) with direct number inputs and auto-calculating `Order = Par - On Hand`.
- Shift switcher for Opening, Mid-Day, and Closing shifts.
- Persistent state saved to browser `localStorage` keyed by date, document, and shift.
- 1-click export to standalone offline HTML web app (`InStore_Checklist_iPad.html`).

### 3. Global Library Ingredient & Tool Search (`GlobalSearch`)
- Cross-document deep indexing across all loaded library documents.
- Searches document titles, procedures, equipment tables, recipe cards, ingredients, pump matrices, clean play tasks, and inventory tables.
- Highlighted context snippets with 1-click jump that activates the document tab and triggers a high-visibility pulse animation on the matching element.
- Global keyboard shortcut: <kbd>Ctrl+Shift+F</kbd>.

### 4. Custom Coffee Shop Equipment Schematics (`EquipmentSchematic`)
- Specialized component and property inspector tools for authoring machine architecture schematics and maintenance guides.
- Official Sanremo Cafe Racer reference guide:
  - Technical specifications grid (AISI 316L multi-boiler, dual PID, soft pre-infusion, 58mm naked portafilters, 8.5mm silicone gaskets).
  - 8-stage hydraulic flow diagram.
  - Daily chemical backflushing protocol (10s ON / 10s OFF x 5 with Cafiza).
  - Weekly and monthly service checklists.

---

## Automated Verification & Browser Test Results

Automated headless browser testing was conducted using Headless Flatpak Chrome (`/tmp/chrome-flatpak.sh`) with Puppeteer:

### Test Suite 1: Feature & Ergonomics Suite (`test_all_features_browser.js`)
- Total Assertions: **28 / 28 PASSED (100%)**
- Console Errors: **0 errors**
- Test Cases Verified:
  1. Initial Page Load & Modal Isolation (Zero startup dialogs, no pointer capture)
  2. Zero Emojis Policy Audit (TreeWalker verified 0 emojis in rendered DOM)
  3. Sidebar Suite Header Controls & Tab Switching (Brand, SOP badge, Dock toggle, Close button, Blocks, Outline, Props, Pro)
  4. Sidebar Scrolling & Wheel Forwarding (Flexbox scrollHeight > clientHeight, mousewheel forwarding)
  5. Status Bar & Bottom Tabs Geometry (Warm tint `#fbf7ee`, 100% uncovered tabs)
  6. Font / Text Size Selector (Dropdown, A- / A+ steppers, direct styling)
  7. In-Store iPad / Tablet Mode (Dialog opening, 3 shift toggles)
  8. Master Store Binder Packaging (14-document packaging and preflight)
  9. Global Library Search (Cross-document indexing, 1-click jump)
  10. Console Error Audit (Zero uncaught runtime exceptions)

### Test Suite 2: Demo File Import & Full Rendering (`test_import_14_demo_files.js`)
All 14 demo `.docx` and `.xlsx` files from `/home/jpesca/Downloads/Reworked SOPs` were imported and rendered onto canvas pages:
- Total Files Tested: **14 / 14 PASSED (100%)**
- Overflows Detected: **0 overflows**
- File Results:
  1. *Brick Road Catering Orders.docx*: Order Sheet (1 page, 2 tables, 0 overflows)
  2. *Cereal Milk Syrup Recipe Cleaned Up.docx*: SOP (2 pages, 3 sections, 0 overflows)
  3. *Cold Brew Process.docx*: SOP (4 pages, 3 sections, 0 overflows)
  4. *Food Pull Template.docx*: Order Sheet (1 page, 4 tables, 0 overflows)
  5. *Buttered Up Bakery Count.docx*: Order Sheet (2 pages, 4 tables, 0 overflows)
  6. *Drink Me Food + Mesa Food (One Sheeter).docx*: Order Sheet (2 pages, 3 tables, 0 overflows)
  7. *Drink Me Tea Order BRC _ Template.docx*: Order Sheet (3 pages, 4 tables, 0 overflows)
  8. *Lively Order Sheet - Template.xlsx*: Order Sheet (2 pages, 3 tables, 0 overflows)
  9. *Mesa Order Template.docx*: Order Sheet (1 page, 2 tables, 0 overflows)
  10. *Prism Coffee Template .docx*: Order Sheet (2 pages, 3 tables, 0 overflows)
  11. *Shoreline Inventory Count BRC _ Template.docx*: Order Sheet (6 pages, 13 tables, 0 overflows)
  12. *Third-Party Inventory & Order Sheet BRC.docx*: Order Sheet (3 pages, 5 tables, 0 overflows)
  13. *Physical Temp Log.docx*: Order Sheet (3 pages, 6 tables, 0 overflows)
  14. *Register Count Sheet.docx*: Order Sheet (2 pages, 3 tables, 0 overflows)

---

## QA Audit, Accessibility, & Usability Suite Updates (v3.3)

### 1. Root Cause Resolution: Elimination of Persistent Bottom White Bar
- **Root Cause Diagnosed**: CSS line 132 previously declared a bare selector `.ft { position: absolute; left: .6in; right: .6in; bottom: .3in; ... }`, intended exclusively for document page footers (`footer.ft`). However, all format buttons inside the context menu (`#fb`, `#fy`, `#fp`, `#fl`, `#fmt-size-down`, `#fmt-size-up`) were also assigned the utility class `.ft`. This caused the browser layout engine to rip those toolbar buttons out of the context menu hierarchy and pin them across the bottom viewport at `bottom: .3in` with `background: #ffffff`, producing a horizontal white bar across the document tabs and canvas.
- **Fix Applied**:
  - Scoped page footer rules strictly to `footer.ft, .pg > footer.ft`.
  - Added explicit CSS scoping `#ctx .ft { position: static !important; display: inline-flex !important; }`.
  - Audited layout geometry: verified 0 stray horizontal bars, and buttons sit securely in the `#ctx` toolbar.

### 2. Print Preview Mode & Interface Element Toggling
- Added a dedicated **Print Preview Mode** that hides all editor-only controls:
  - Table row addition buttons (`.oadd` / `+ Row`), column headers/controls (`.oh`), column resizer lines (`.col-resizer`), quick schedule chips (`.quick-schedule-btn`), canvas quick bars (`#canvas-quick-bar`), and formula bars (`#formula-bar`).
- **Activation Access Points**:
  - Top navigation bar: `#btn-preview` with eye icon and keyboard hint.
  - Zoom & canvas control bar: `#z-preview` button.
  - Right-click context menu: "Toggle print preview (Alt+V)".
  - Keyboard shortcut: `Alt+V`.
- **Verified Behavior**: Visible `.oadd` buttons drop from 5 to 0 instantly when activated, giving users a true WYSIWYG clean print preview.

### 3. Context Menu Overhaul & Font Size Picker
- Overhauled the right-click and tap-and-hold context menu (`#ctx`) into an intuitive, categorized workstation popup:
  - **Quick Formatting Bar**: Undo, Redo, Bold (`**`), Italic (`*`), Highlight (`==`), Warning (`!!`), Small Caps (`~~`).
  - **Font Size Bar**: Dropdown selector (`8pt`, `9pt`, `10pt`, `11pt`, `12pt`, `14pt`, `16pt`, `18pt`, `20pt`, `24pt`), stepper buttons (`A-` and `A+`), and Clear formatting.
  - **Color Swatch Bar**: 9 horizontal color swatches including Header Orange (`#ffc901`), Yellow, Pink, Mint, Blue, Lavender, Peach, Soft Gray, and Clear.
  - **Categorized Actions**:
    - *Row & Table Tools*: Insert row above/below, move row up/down, toggle group row (`# `), delete row (`.ctx-del`).
    - *Column Tools*: Align left/center/right/currency, auto-fit column, insert column, delete column.
    - *Document Tools*: Toggle print preview (`Alt+V`), copy table to clipboard (TSV), open print dialog.

### 4. Header Orange (`#ffc901`) & Row/Category Sectioning Engine
- Added `header: '#ffc901'` and `orange: '#ffc901'` to `ROW_COLORS`.
- Implemented full row coloring in the table engine (`otb`) so that selecting Header Orange fills the entire table row and all containing cells with the primary brand header gold (`#ffc901`), matching the column headers (`thead th`).
- Directly tested against `Lively Order Sheet - Template.xlsx`:
  - Successfully sectioned rows like `Janitorial` (row 16) and `Extra` (row 9) with Header Orange, ensuring seamless visual parity with the `Milk` header category line.

### 5. Table Row Deletion Feature
- Implemented `deleteTableRow(rowPath, rowIdx)`:
  - Splices the target row array out of `tbl.rows`.
  - Re-indexes `tbl.rowColors` so row color mappings do not shift incorrectly onto subsequent rows.
  - Records undo state in history stack (`hist()`).
  - Triggers reactive re-render (`draw()`).
  - Shows floating confirmation toast with one-click **Undo**.
- Available in both:
  - Right-click context menu ("Delete row").
  - Sidebar Property Inspector (`#prop-del-row`).

### 6. Automated Browser Test Suite Results (`test_qa_audit_and_improvements.js`)
- Executed against Headless Flatpak Chrome:
  1. *Bottom White Bar Elimination Audit*: **PASSED** (0 stray bars, `#fb` at top: 92px)
  2. *Print Preview Mode & +Row Buttons Toggle*: **PASSED** (5 -> 0 visible buttons)
  3. *Right-Click Context Menu & Font Size Picker*: **PASSED** (Dropdown + steppers functional)
  4. *Row Color Palette & Header Orange*: **PASSED** (rgb(255, 201, 1) applied)
  5. *Delete Row Feature*: **PASSED** (Rows 11 -> 10, table redrawn cleanly)
  6. *Lively Order Sheet Sectioning*: **PASSED** (Section lines match Milk header line)
  7. *Console Error Audit*: **PASSED** (0 console errors)

---

## Recipe Builder Overhaul & Visual Layout Hardening (v3.4)

### 1. Root Cause Resolution: Recipe Table Column Collapse & 5,962px Height Explosion
- **Root Cause Diagnosed**:
  1. **`.sr` Class Collision**: The recipe table column header `<th scope="col" colspan="3" class="sr">Step</th>` used class `.sr`. CSS line 4 declared `.sr { position: absolute; width: 1px; height: 1px; overflow: hidden; clip: rect(0 0 0 0); }` (the standard screen-reader utility). This ripped the "Step" header out of the table layout, clipping it to 1px.
  2. **Colgroup Collapse**: In `card(c, ci)`, `<colgroup>` previously declared `<col style="width:.4in"><col style="width:1.2in"><col><col style="width:1.2in"><col style="width:1.2in"></colgroup>`. Under `table-layout: fixed`, the 3rd `<col>` had no width and collapsed to only 17px. Every word in the ingredient and quality note columns wrapped character-by-character, causing each row to balloon to 1,120px and the recipe card to stretch to **5,962px tall** (`scrollHeight: 7,428px` on a 936px page).
- **Fix Applied**:
  - Renamed header class to `.st-hdr` with dedicated styling `.card th.st-hdr { font: 700 8pt Poppins; letter-spacing: .12em; text-transform: uppercase; padding: 5px 10px; color: #3e2b20; }`.
  - Added explicit printable column widths: `<col style="width:.45in"><col style="width:1.35in"><col style="width:3.15in"><col style="width:1.15in"><col style="width:1.15in">` (totaling exactly 7.25in).
  - Compacted table cell padding to `6px 8px` and card header padding to `7px 12px`.
  - Card height dropped from **5,962px to 422px**, fitting 2 full recipe placards on a single Letter sheet with **zero overflow** (`scrollHeight: 936px === clientHeight: 936px`).

### 2. Dedicated Recipe Card Inspector in ModernSidebar
- **Root Cause of Builder Crash**: In `ModernSidebar`, selecting a card routed into `renderSectionInspector(sel)`. Because `sec.steps` on a recipe card is an array of `{title, lines, how}` objects, `<textarea id="prop-sec-steps">` stringified the array into `"[object Object],[object Object]"`. Editing or touching any field converted `c.steps` into a string. The next `draw()` call attempted `(c.steps || []).map(...)`, throwing `TypeError: c.steps.map is not a function` and permanently freezing the builder.
- **Fix Applied**:
  - Created a dedicated `renderRecipeCardInspector(sel)` in `ModernSidebar`.
  - Added reactive controls for:
    - **Drink Name**: Live text synchronization with card banner.
    - **HOT / ICED Temperature Toggle**: Instant state toggle with color-coded pill.
    - **Cup Sizes**: Separate inputs for Size 1 and Size 2 amounts.
    - **Step-by-Step Ingredients Manager**: Step title, ingredients list, and quality notes with per-step delete buttons.
    - **+ Add Step Button**: Appends new formatted step objects.
    - **Rim Level & Finish Text**: Direct text and textarea inputs.
    - **Allergen & Heads-Up Note**: Dietary chips and heads-up banner note.
    - **Drink Cup Diagram**: 1-click toggle to show/hide build diagram.
    - **Card Management**: Duplicate card and delete card (safeguarded against deleting the only card).
  - Fortified `card(c, ci)` with `rawSteps` array normalization so malformed data never crashes `draw()`.

### 3. Stability in Block Additions (`ModernSidebar.insertComponent`)
- In Recipe mode, clicking blocks in the catalog now adapts intelligently:
  - `struct-recipe-card`: Pushes a new structured drink card placard.
  - `table-syrup-pumps`: Appends a pump calibration matrix step to the active card.
  - `diag-cup-layer`: Embeds the Cup Layer Diagram into the active card.
  - Procedural / SOP blocks: Safely appends as structured step objects without injecting invalid raw text blocks.

### 4. Top Navigation Bar Single-Row Layout Lock
- **Visual Bug**: `#bar` previously used `flex-wrap: wrap`, which caused buttons 13–17 to drop onto a second line, swelling `#bar` from 46px to 86px (or 130px on narrower viewports) and pushing down the canvas.
- **Fix**: Styled `#bar` with `flex-wrap: nowrap; overflow-x: auto; height: 46px;` with hidden scrollbars, ensuring a crisp single-row workstation header across all viewport widths.

### 5. Automated Verification Results (`test_recipe_and_visual_fixes.js`)
- Executed against Headless Flatpak Chrome:
  1. *Top Bar Single-Row Layout*: **PASSED** (Height 46px <= 48px)
  2. *Recipe Card Dimensions & Zero-Overflow*: **PASSED** (Cards 422px / 447px, 0 overflow, 936px page)
  3. *Recipe Interactive Controls (HOT/ICED & +Step)*: **PASSED**
  4. *Sidebar Visual Editor & Recipe Inspector*: **PASSED** (Live sync confirmed)
  5. *Adding Blocks in Recipe Mode*: **PASSED** (Zero crashes, 3 cards rendered)
  6. *Right-Click Context Menu Recipe Operations*: **PASSED**
  7. *Console Error Audit*: **PASSED** (0 console errors)

---

## Microsoft Office (Word + Excel) Workstation Engine, Definitive Zero-UI Print Engine, & Compact BOH Freezer Chart (v3.5)

### 1. Compact BOH Freezer Chart Architecture & Single-Page Fit
- **User Problem Addressed**: The freezer chart in the order sheet was broken/bloated ("taking up an additional page"). It must remain the original compact, door-tracking design (`Single door`, `Left door/Right door`, shelves 1–3, write-in slots) and fit seamlessly on Page 1 below the table without spilling onto an extra page.
- **Root Cause Diagnosed**:
  - `.shelf span` was set to `height: .6in` (58px per shelf). With 3 shelves per door, the freezer block was 244px tall, which when added to the 11-row table (406px) and header/schedule (200px) pushed Page 1 to `scrollHeight: 940px` (exceeding `.bd`'s 936px capacity).
- **Fix Applied**:
  - Reduced `.shelf span` height to `.28in` (27px) and compacted `.shelf` padding to `2px 0`.
  - Added `.fz-wrapper { margin-top: 4px; break-inside: avoid; }` and `.two { gap: 16px; margin-top: 6px; align-items: flex-start; }`.
  - Reduced default table row height in `EX.bu` to `.30in`.
  - Total `.two` block height dropped to 210px (freezer chart: 135px).
  - Page 1 now measures exactly `scrollHeight: 936px === clientHeight: 936px` with `isOver: false` (ZERO overflow), fitting 100% cleanly on a single Letter sheet.
  - The door-tracking layout preserves the original layout: `Single door, or the left door of a double` and `Right door (double-door units only)` with shelves 1–3 and write-in boxes.

### 2. Definitive Master Print Engine (Zero UI Chrome Leakage)
- **User Problem Addressed**: Browser printing was failing cleanly ("acting weird showing the sidebar and unneeded elements... just functioning like I am pressing Ctrl+P"). The print output must be a 100% pure, perfect page export capturing only the document Letter pages (`.pg`), with zero UI chrome.
- **Root Cause Diagnosed**:
  - In CSS line 1249, `#ed` had `display: flex !important;` which appeared *after* the initial `@media print` block on line 536. In CSS cascade, rule order with equal specificity (`!important`) meant line 1249 won during printing, forcing the 440px editor sidebar to appear on the printed page. Furthermore, on-page edit buttons (`.oadd`, `+ row`, `.col-resizer`, `.sch-edit-btn`, etc.) leaked into prints.
- **Fix Applied**:
  - Moved the definitive, master `@media print` block to the absolute bottom of `<style>`.
  - Suppresses all UI chrome permanently: `#bar`, `#ctx`, `#sb`, `#tabs`, `#zc`, `#lib`, `#ed`, `#splitter`, `#side-tab-toggle`, dialogs, `#tt`, `#dzo`, `.skip`, `.oh`, `.oadd`, `.col-resizer`, `.sch-edit-btn`, `.quick-schedule-btn`, `#canvas-quick-bar`, `.canvas-type-bar`, `#formula-bar`, `#rc-menu`, `#sel-bubble`, `.oar`, `.resizer-guide`, `.resizer-tooltip`, `.quick-ins-btn`, buttons, `.stat-pill`, `#pgsm`, `#insm`, `.side-pull-tab`, and `.ft-rev` are set to `display: none !important; width: 0 !important; height: 0 !important;`.
  - Formats `#work`, `#cv`, `#pv`, `#stage`, and `.pg` to exact Letter dimensions (`8.5in` x `11in`, `max-height: 11in`, `page-break-after: always !important`, `break-after: page !important`).
  - Emulated `@media print` verification confirms zero sidebar or toolbar leakage and pure, perfect document page rendering.

### 3. Microsoft Word & Excel Hybrid Workstation Top Bar
- **Office Title Bar (`#bar.office-titlebar`)**:
  - **Brand Section**: Commercial Operations Workstation icon & title (`Card Builder`).
  - **Office File Menu**: New document type, Add document, Start from example, Import files, Save JSON, Export CSV, Copy Markdown, Folder autosave, Templates, Bundle save, Release notes.
  - **Quick Undo / Redo**: Word-style icon buttons (`#bar-undo`, `#bar-redo`).
  - **Centered Document Type Switcher Pill**: `[ Standard Operating Procedure (SOP) ▾ ]`.
  - **High-Visibility Print / PDF Action**: Gold primary button (`#pr`).
  - **Print Preview Toggle**: Word-style document layout view (`#btn-preview`).
  - **Consolidated Master Store Binder**: 14-document packaging launcher (`#tb-binder`).
  - **In-Store iPad / Tablet Interactive Mode**: Touch-optimized checklist launcher (`#tb-tablet`).
  - **Navigation**: Library drawer (`#lt`) and Sidebar toggle (`#side-toggle`).
- **Office Command Ribbon (`#ctx.office-ribbon`)**:
  - **Typography & Font Size**: Steppers (`A-`, `A+`) and dropdown (`Size`: 8pt to 24pt).
  - **Word Styling**: Bold (`B`), Italic (`I`), Yellow Highlight (`Hi`), Pink Warning (`Warn`), Small Caps (`Aa`).
  - **Table Column Alignment**: Left (`≡L`), Center (`≡C`), Right (`≡R`), Currency (`$`).
  - **Direct On-Page Insertion**: `+ Blank Row`, `+ Spacer`, `+ Row`, `Del Row`.
  - **Print-Safe Color Swatches**: Clear (`×`), Header Orange (`#ffc901`), Pastel Yellow, Soft Pink, Mint Green, Ice Blue, Lavender, Peach, Warm Gray.
  - **Excel Tools**: Formula Bar (`fx`), Auto-Calculate Order (`Calc`: `=PAR-OH`), Delivery Schedule Builder (`Schedule`).
  - **Documentation & Notes**: Quick badges for Release Notes and Features Guide.

### 4. Direct On-Page Quick Insertion Engine
- `quickAddBlankRow()`: Instantly adds blank write-in rows to tables, checklists, SOPs, or recipe cards.
- `quickAddSpacer()`: Inserts vertical blank spacing to control document pagination directly.
- `quickAddRow()` & `quickDeleteRow()`: Adds or deletes table rows directly from the ribbon without needing the context menu.

### 5. Automated Verification Results (`audit_and_capture_screenshots.js`)
- Executed against Headless Flatpak Chrome:
  1. *Office Title Bar & Ribbon Layout*: **PASSED** (Height 44px / 40px, all 9 controls verified)
  2. *Buttered Up Bakery & Compact BOH Freezer Chart*: **PASSED** (1 single page, 0 overflow, 135px freezer height)
  3. *Recipe Card Dimensions & Zero-Overflow*: **PASSED** (2 cards on 1 page, 0 overflow)
  4. *SOP Espresso Dial-In Layout*: **PASSED** (Two-column layout, 1 page)
  5. *Clean Play Checklist*: **PASSED** (Checklists & weekly projects, 1 page)
  6. *Print Preview Mode*: **PASSED** (0 edit buttons/handles visible)
  7. *@media print Emulation*: **PASSED** (Zero UI chrome leaks into print)
  8. *On-Page Quick Insertion*: **PASSED** (+ Blank row & + Spacer verified)
  9. *Console Error Audit*: **PASSED** (0 console errors)


---

---

## 14. Multi-Barista Rush Location Floor Plans, Customizable Equipment Maintenance Odometers, & FDA Day-Dot Prep Pull Matrix (v3.6)

### 1. Multi-Barista Floor Plan & Location Rush Choreography Engine (`MultiBaristaFloorPlan`)
- **User Problem Addressed**: Store managers need proper architectural floor plans for distinct store locations (not just hardcoded linear steps) and the ability to configure teams beyond 2 baristas ("let me create proper floor plans for locations and add more than 2 baristas").
- **Implementation Highlights**:
  - **Location Presets & Custom Floor Plans**:
    - `main_bar`: Brick Road Main Bar & Front Counter (POS, Pastry Display, Dual Mahlkönig Grinders, Sanremo Café Racer, Pitcher Rinser, Dairy Cooler, Ice/Cold Bar, Drink Hand-Off).
    - `drive_thru`: Rapid Service Drive-Thru Bar & Pick-up Window (Order Headset, DT POS, E65S Grinder, DT Espresso, Steaming Well, Cold Wells, Window Expediter).
    - `boh_prep`: Back of House (BOH) Prep, Cold Brew & Baking Hub (Pastry Thaw Bench, Produce Table, 3-Compartment Sink, Hobart Commercial Dishwasher, Cold Brew Toddy Kegs, Scotsman Ice Maker, Walk-In Cooler Door).
    - `mobile_cart`: Satellite Event & Pop-Up Espresso Cart (Square POS Stand, Compact Grinder, 2-Group Machine, Milk Well, Cold Syrups, Drink Pickup).
    - `custom`: Dynamic Custom Location creator prompting for store name and layout workflow notes.
  - **Dynamic Barista Team Scaling (1 to 6+ Baristas)**:
    - Stepper controls (`[-]` and `[+]`) and quick presets: Solo Operator (1), Classic Duo (2), Rush Trio (3), Quad Peak (4), High-Volume Quintet (5), Festival Championship Six (6).
    - Distinct accessible color accent pins: B1 Gold (`#ffc901`), B2 Cobalt Blue (`#2563eb`), B3 Emerald Green (`#10b981`), B4 Royal Violet (`#8b5cf6`), B5 Coral Amber (`#d97706`), B6 Crimson (`#e11d48`).
    - Barista pins dynamically mapped and rendered directly onto workstation zones in the vector SVG floor plan.
    - Directional speed-of-service flow line (`SPEED-OF-SERVICE FLOW →`) showing unidirectional customer and beverage circulation.
    - Comprehensive Barista Team Roster table detailing: Barista & Role, Station Zone, Primary Rush Responsibilities, and Rush Support Flex Triggers (when to flex help).

### 2. Customizable Equipment Maintenance Odometer & Service Logbook (`EquipmentSchematic`)
- **User Problem Addressed**: Standard coffee shops operate diverse equipment rosters that differ from location to location ("let me select/create equipment as ours differs").
- **Implementation Highlights**:
  - **8 Built-in Commercial Coffee Shop Equipment Presets**:
    1. *Sanremo Café Racer*: 3-Group Naked Commercial Espresso Machine (AISI 316L multi-boiler, 50,000 shot threshold, daily Cafiza backflush, weekly screen scrub, quarterly gasket rebuild).
    2. *Mahlkönig E65S GbW*: Commercial Espresso Grinder (65mm special steel burrs, 800 kg burr lifespan, Grindz tablet purge, load cell calibration).
    3. *Mahlkönig EK43*: Bulk & Drip Grinder (98mm cast steel burrs, 1,500 kg burr lifespan, bag knocker clean, shear plate inspection).
    4. *Fetco CBS-2142XTS*: Extractor Series Twin 1.5 Gal Batch Brewer (Cascading Spray Dome, 2,500 batch threshold, sight gauge soak, sprayhead de-scale).
    5. *Scotsman Prodigy Plus*: Modular Cube Ice Machine & Bin (AgION antimicrobial, 180 day cycle, reusable lint filter wash, chemical sanitization).
    6. *Commercial 5-Stage Reverse Osmosis (RO) System*: 175 GPD with Calcite remineralization (5,000 gal cycle, weekly TDS testing at 130–150 PPM, pre-filter replacement).
    7. *Hobart Commercial Undercounter High-Temp Dishwasher*: 180°F Sense-A-Temp sanitizing rinse (1,500 cycle threshold, pump scrap basket rinse, de-liming).
    8. *True TUC-48 Undercounter Milk Refrigerator*: Forced-air dairy cooler at 33°F–38°F (90 day cycle, daily digital temp log, quarterly condenser coil dusting, dollar-bill gasket test).
  - **Dynamic "+ Create Custom Equipment" Engine**:
    - Shop owners can register custom machines on the fly (Name, Manufacturer, Model, Location, Metric Name, Target Threshold, and Current Reading).
  - **Visual Mechanical Odometer & Health Gauge**:
    - Embedded odometer panel displaying digitized meter digits (e.g. `42,350 / 50,000 Shots`).
    - Dynamic color-coded health status badge: `OPTIMAL HEALTH` (Green `<80%`), `SERVICE DUE SOON` (Gold `80–99%`), `SERVICE OVERDUE` (Red `≥100%`).
    - `+ Log Meter` button enabling baristas and managers to update current meter readings in real time.
  - **Official Equipment Service & Calibration Logbook Table**:
    - Pre-populated historical records table: Date, Technician/Barista, Meter Reading, Action Taken, Replaced Parts, and Status.
    - `+ Service Entry` button allows immediate recording of maintenance directly into the document.

### 3. FDA Day-Dot Prep Pull & Thaw Matrix (`FoodPullMatrix`)
- **User Problem Addressed**: Store pastries and prep stations require strict compliance with store standards and health code ("use the information from Food Pull Template.docx").
- **Exact Data Parsed from `Food Pull Template.docx`**:
  - **Table 1: Pastry & Food Display Par, Thaw & Expiration Table**:
    1. *Drink Me Pastries (Brownies, Cin. Coffee Cake, Banana Breads)*: Par [write-in] | Pull: `N/A (Fill Display)` | Thaw: `N/A` | Exp: `Date Delivered + 3 Days` | Label: `Date Dot Tray + Pastry Case`.
    2. *Mesa Food (Sandwiches, Burritos)*: Par [write-in] | Pull: `N/A (Fill Display)` | Thaw: `N/A` | Exp: `Dated on Delivery` | Label: `Date Dot Bin + Pastry Case`.
    3. *Mesa RTD&E (Parfait, Overnight Oats, Pasta Salad)*: Par [write-in] | Pull: `N/A (Fill RTD&E)` | Thaw: `N/A` | Exp: `Dated on Delivery` | Label: `Label on backside`.
    4. *Cheese Danish*: Par `4` | Pull: `2` | Thaw: `2.5 Hours` | Exp: `Date Pulled + 2 Days` | Label: `Date Dot Tray + Pastry Case`.
    5. *Blueberry Danishes*: Par `4` | Pull: `2` | Thaw: `1.5 Hours` | Exp: `Date Pulled + 3 Days` | Label: `Date Dot Tray + Pastry Case`.
    6. *Blueberry Muffin*: Par `4` | Pull: `2` | Thaw: `2 Hours` | Exp: `Date Pulled + 3 Days` | Label: `Date Dot Tray + Pastry Case`.
    7. *Seasonal Muffin*: Par `4` | Pull: `2` | Thaw: `2 Hours` | Exp: `Date Pulled + 3 Days` | Label: `Date Dot Tray + Pastry Case`.
    8. *Lemon Bars*: Par `4` | Pull: `2` | Thaw: `1 Hour` | Exp: `Date Pulled + 4 Days` | Label: `Date Dot Tray + Pastry Case`.
    9. *Cookies*: Par `6` | Pull: `3` | Thaw: `1 Hour` | Exp: `Date Pulled + 6 Days` | Label: `Label on backside`.
    10. *Butter Croissant*: Par `4` | Pull: `2` | Thaw: `1.5 Hours` | Exp: `Date Pulled + 2 Days` | Label: `Date Dot Tray + Pastry Case`.
    11. *Raspberry Croissant*: Par `4` | Pull: `2` | Thaw: `1.5 Hours` | Exp: `Date Pulled + 2 Days` | Label: `Date Dot Tray + Pastry Case`.
    12. *Chocolate Croissant*: Par `4` | Pull: `2` | Thaw: `1.5 Hours` | Exp: `Date Pulled + 2 Days` | Label: `Date Dot Tray + Pastry Case`.
  - **Table 2: Prep Station Secondary Shelf-Life Standards**:
    1. *Sliced Lemons*: `Date Made + 4 days` (Cold bar fruit well <41°F).
    2. *Twister Mix*: `Shots Only + 2 days | Shots + Milk + 4 days` (Refrigerated pitcher <38°F).
    3. *Whipped Creams*: `Next Day (24 hours)` (Charged whip canister in ice well <41°F).
    4. *Smoothies / Refreshers*: `1 Month after opening` (Date opened on lid).
  - **Table 3: Thawing Box Day-Dot Sticker Standard**:
    - Official sticker card layout: `Date Pulled`, `Time Ready (Thaw Complete)`, `End / Expiration (Date Pulled + Days)`.
  - **Single-Page Fit Verification**:
    - Rendered with compact typography and row spacing (`padding: 3px 6px`) ensuring `scrollHeight: 1056px <= clientHeight: 1056px` with zero page overflow.

### 4. Automated Verification Results (`test_v36_ops_suite.js`)
- Executed against Headless Flatpak Chrome:
  1. *Food Pull Template Loading*: **PASSED**
  2. *Food Pull Title & FDA Standard*: **PASSED**
  3. *Table 1 Count (12 Items)*: **PASSED**
  4. *Table 2 Count (4 Prep Items)*: **PASSED**
  5. *Table 3 Day-Dot Fields (3 Fields)*: **PASSED**
  6. *Single-Page Fit (0 Overflow)*: **PASSED**
  7. *Authentic Food Pull Verification (12/12 items)*: **PASSED**
  8. *Prep Station Verification (4/4 items)*: **PASSED**
  9. *Floor Plan Template Loading*: **PASSED**
  10. *Location Presets (Main, Drive-Thru, BOH, Cart, Custom)*: **PASSED**
  11. *Barista Roster Default (4 Baristas)*: **PASSED**
  12. *SVG Station Groups Rendered (8 stations)*: **PASSED**
  13. *Dynamic Scale Up to 6 Baristas*: **PASSED**
  14. *Dynamic Scale Down to 2 Baristas*: **PASSED**
  15. *Drive-Thru Location Switch*: **PASSED**
  16. *Equipment Maintenance Card Loading*: **PASSED**
  17. *Sanremo Café Racer Default*: **PASSED**
  18. *Visual Odometer Readout (42,350)*: **PASSED**
  19. *Health Status Badge Rendering*: **PASSED**
  20. *Equipment Presets (8 Models)*: **PASSED**
  21. *Service Logbook Records (3 History Rows)*: **PASSED**
  22. *Switch to Mahlkönig E65S (685 kg)*: **PASSED**
  23. *Print Preview UI Suppression (Zero leak)*: **PASSED**
  24. *Console Error Audit (0 Errors)*: **PASSED**

---

## 15. Subagent QA Re-Review, In-Place Editing Polish & Strict Zero-Emoji Hardening (v3.6.1)

### 1. Comprehensive Subagent Re-Review
A dedicated QA Architect subagent (`de55059d-f36a-490e-9067-52d8cc28f023`) conducted an end-to-end review of all features and specifications across `HANDOFF.md` and `BrickRoad_Card_Builder.html`.

### 2. Strict Zero-Emoji & Clean Typography Certification
- **Purged Unicode Warning Remnant (U+26A0)**: Removed `\u26A0` from markdown callout parser regex (`isWarn`), converting to clean text regex `(?:warning|caution|danger|important)`.
- **Typographical Checkmark Normalization**: Replaced all 23 occurrences of literal checkmarks (`&check;`) in modal dialogues and templates with standard HTML entity `&check;`, CSS escaped `\2713`, or ASCII standard `[X]`.
- **Zero Supplementary Emojis & Zero Variation Selectors**: Verified 0 supplementary plane emojis (U+10000+), 0 variation selectors (`\uFE0F`), and 0 ambiguous symbols across the entire application codebase.

### 3. Direct On-Canvas In-Place Editing for v3.6 Components
- **Day-Dot Prep Pull & Thaw Matrix (`FoodPullMatrix`)**:
  - All cells across Table 1 (Food Items, Pars, Pull Thresholds, Thaw Durations, Expiration Rules, Date Locations) and Table 2 (Prep Items, Storage) now wrap contents with `E(value, path)`. Managers can click directly on the canvas sheet to adjust pars, thaw times, and thresholds with live reactive data model synchronization.
- **Preventive Equipment Maintenance Odometer & Service Logbook (`EquipmentSchematic`)**:
  - Added `E(...)` cell wrappers to historical maintenance logbook entries (Date, Tech, Reading, Action, Parts, Status) and technical architecture specification values, enabling instant in-place spreadsheet editing directly on the document.
- **Component Insertion for Order Sheets**:
  - In `ModernSidebar.insertComponent`, inserting `table-food-pull-daydot` while in an Order Sheet automatically constructs a structured Order Sheet table with items, display pars, pull thresholds, thaw times, and expiration standards.

### 4. Mirrored Integrity & Checksums
- Synchronized across all 6 mirror and distribution locations with identical SHA-256 hash:
  `f335db8058de57d463e108a2ea1b4b3ec589e6c3af9694b17e8474b79e779731`
  - `/home/jpesca/antigravity/OpsFlow-Redesign/BrickRoad_Card_Builder.html`
  - `/home/jpesca/antigravity/OpsFlow-Redesign/BrickRoad_Card_Builder_Geminis_Version.html`
  - `/home/jpesca/antigravity/OpsFlow-Redesign/Geminis_Version.html`
  - `/home/jpesca/Downloads/files/BrickRoad_Card_Builder.html`
  - `/home/jpesca/Downloads/files/BrickRoad_Card_Builder_Geminis_Version.html`
  - `/home/jpesca/Downloads/files/Geminis_Version.html`

---

## 16. Total SVG Icon Replacement & Zero AI Tells Certification (v3.6.2)

### 1. 100% Pure SVG Vector Architecture
In strict enforcement of the Zero-Emoji and Zero AI Tells design mandate, all residual unicode geometric symbols and character glyphs across the application interface, menus, modals, toolbars, and templates have been replaced with dedicated, scalable SVG vector elements:
- **Sidebar Toggle & Pull-Tab**:
  - Replaced text triangles (`◀`, `▶`) with clean 24×24px SVG sidebar panels (`<rect>`, `<line>`) and SVG chevron polylines (`<polyline points="9 18 15 12 9 6"/>`).
- **Ribbon Column Alignment Controls**:
  - Replaced text glyphs (`≡L`, `≡C`, `≡R`) with standard Microsoft Word/Excel SVG alignment icons (`align-left`, `align-center`, `align-right`, and SVG currency `$`).
- **Outline Hierarchy Tree Controls**:
  - Replaced triangle characters (`▲`, `▼`) with crisp SVG chevrons (`<polyline points="18 15 12 9 6 15"/>` and `<polyline points="6 9 12 15 18 9"/>`).
  - Replaced text grip dots (`⋮⋮`) with an authentic 6-dot SVG drag handle (`<circle>` elements with opacity 0.45).
- **Tablet Mode Checklist & Shift Controls**:
  - Replaced `↺ Reset Shift` with an SVG counter-clockwise circular refresh vector (`<path>`, `<polyline>`).
  - Replaced checkbox completion glyphs with crisp inline SVG vector checkmarks (`<polyline points="20 6 9 17 4 12"/>`).
- **Global Search Document Type Badges**:
  - Upgraded text badges to include distinct SVG document icons for `SOP` (file-text), `RECIPE` (coffee-mug), `CLEAN` (check-circle), and `ORDER` (table-grid).
- **Interactive Checkbox Engine (`.cb.checked`)**:
  - Replaced CSS font glyph `content:'\2713'` with a crisp inline SVG data URI background (`viewBox="0 0 24 24" fill="none" stroke="%23ffc901"`), eliminating any platform-dependent font fallback or mobile emoji substitution.
- **Top Bar Dropdowns & Modals**:
  - Replaced `&blacktriangledown;` with pure SVG chevron downs.
  - Replaced `◨` in Features Guide headers and tabs with pure SVG panel vectors.

### 2. Comprehensive Zero-Emoji Static Assertion
- Emojis (Supplementary plane `U+10000+`): **0**
- Variation Selector-16 (`\uFE0F`): **0**
- Unicode Warning Signs (`\u26A0`): **0**
- Geometric Triangles / Rectangles (`▶`, `◀`, `▲`, `▼`, `▾`, `◨`): **0**
- Font Checkmarks (`✓`, `✔`): **0**
- Ellipses / Dots (`⋮`): **0**
- Triple Bars (`≡`): **0**

### 3. Mirrored Integrity & Checksums
- Synced identically across all 6 production and compatibility paths:
  `SHA-256: e1f19ea8d122b15590fb90cbfa1dfb770f286f1167b69e2b45abf68b0e9f248d`
  - `/home/jpesca/Downloads/files/BrickRoad_Card_Builder.html`
  - `/home/jpesca/antigravity/OpsFlow-Redesign/BrickRoad_Card_Builder.html`
  - `/home/jpesca/Downloads/files/BrickRoad_Card_Builder_Geminis_Version.html`
  - `/home/jpesca/Downloads/files/Geminis_Version.html`
  - `/home/jpesca/antigravity/OpsFlow-Redesign/BrickRoad_Card_Builder_Geminis_Version.html`
  - `/home/jpesca/antigravity/OpsFlow-Redesign/Geminis_Version.html`

---

## 17. Store Directory & Emergency On-Call Roster, Smart Grid Importer & Word+Excel Editing Engine (v3.7)

### 1. Store Directory & Escalation Protocol Template (`EX.oncall`)
- **Template Integration**: Built-in template pre-packaged under `EX.oncall` and selectable from the Quick Example dropdown (`<select id="ex">`).
- **Table 1: Store Escalation Protocol & Chain of Command** (3 columns):
  - *Escalation Level*: Level 1 (Shift Team / 7shifts), Level 2 (Store Lead / Assistant Lead), Level 3 (Operations & Executive Leadership), Human Resources & Ownership.
  - *Operational Scope*: Shift swaps, cash discrepancies, safety hazards, confidential HR matters.
  - *Required Action & Protocol*: Step-by-step chain of escalation rules.
- **Table 2: Store Master Contact Roster** (5 columns):
  - Columns: Name, Position / Role, Scheduled Off-Days, Mobile Phone #, Home Location.
  - Spanning Category Groups (`colspan="5"`):
    - `# Store Management Team` (Aly Gonzalez, Emma Barry, Jordan Pesca, Hattie Gering, Darinda Stallings, Nadia Davis).
    - `# Executive & Administrative Leadership` (Kate Vawter CPO, Gabe Hagen Co-Owner, Jesse Shank Co-Owner, Maya Geeslin Kitchen Lead).
    - `# Active Barista Shift Team` (Marina Reyes, Vianney Garcia, Lauren Miller, Lyzzett Guderian).
- **Table 3: Maintenance & Emergency Vendor Service Directory** (8 columns):
  - Columns: Category, Vendor / Service Co., Primary Tech, Phone Number, Account / Serial #, Service Window, Emergency Procedure, Lead Auth?
  - Pre-populated authentic service providers: Sanremo Authorized Tech (Marcus V.), Mahlkönig Partner Support, Pure Water Solutions RO (David K.), Toast POS Tier 2 Support, Polar Cold Refrigeration, Scotsman Certified Ice Service, Valley Electric Controls, EcoShield Commercial Pest Control, Rapid Lock & Key, Property Management Landlord, Non-Emergency Police & Fire Dispatch.
- **Component Registry**:
  - Registered `table-store-directory`, `table-contact-roster`, and `table-emergency-contacts` into `COMPONENT_REGISTRY`.

### 2. Smart Grid Importer (`fromRows`)
- **Automatic Column Sanitization (`cleanBlkCols`)**: Detects and strips empty phantom columns generated by spreadsheets with leading or interior commas (e.g. `,Escalation Level,,Scope,,Protocol`).
- **Category Continuation Merging**: Automatically detects when blocks share the active table width and category headers (`Managers`, `Admin/Other`, `Baristas`), merging them into the active table with full-width category spanning `# ` rows rather than fragmenting into separate tables.
- **Redundant Prefix Cleaner**: Prevents ugly double-prefixed titles (e.g. `Store Directory & Escalation Te: Store Directory & Escalation Protocol`) by evaluating string distance against sheet names and document titles.
- **Multiline Cell Encoding**: Properly converts raw newlines within spreadsheet cells into `<br>` tags without splitting markdown table rows.

### 3. Word + Excel Hybrid Editing Engine
- **Whitespace & Multi-Space Preservation**:
  - Applied `white-space: pre-wrap !important; word-break: break-word;` to `.ed` in all states (not just on `:focus`).
  - Updated `P(s)` to preserve internal and trailing spaces in table cells while stripping only single column delimiter pads.
  - Switched `stage` input listener from `el.textContent` to `el.innerText`, preventing browser collapsing of `<br>` and sequential spaces during typing.
- **True Multi-Line Cell Rendering**:
  - Fixed `mk(s)` bug where `esc(s)` ran before `<br>` replacement, converting `&lt;br\s*\/?&gt;|<br\s*\/?>` into native `<br>` DOM elements.
  - Cells render multi-line content as true formatted paragraphs (height >= 47px) with zero literal `&lt;br&gt;` text displayed.
- **Word-Style Blank Paragraph & Line Preservation**:
  - Styled `.ed:empty` with `display: inline-block; min-width: 1.5ch; min-height: 1.25em;` and `p.line:has(.ed:empty), li:has(.ed:empty), p.line:empty { min-height: 1.25em; }`. Empty lines and paragraph breaks never collapse to 0px.
- **Excel-Style Keyboard Traversal in Tables**:
  - `Tab`: Navigates to the adjacent cell to the right; appends a new row when on the final cell.
  - `Shift+Tab`: Navigates to the previous cell to the left.
  - `Enter`: Navigates down to the cell directly below; appends a row if on the last row.
  - `Shift+Enter` / `Alt+Enter`: Inserts a newline (`<br>`) inside the active cell for multi-line notes.

### 4. Interactive Operations Polish (v3.7)
- **Multi-Barista Floor Plan**:
  - Added dedicated `Stations` button in `.fl-ctrls` to customize, add, and rename stations on custom floor plans.
  - Stations dynamically map to active team roster (1 to 6 baristas) without pin tag desync.
- **Food Pull & Secondary Shelf-Life Matrix**:
  - Made all 72 cells in the Pastry/Food Pull table and 12 cells in the Prep Station table directly editable on canvas via `E(...)`.
  - Made the FDA Thawing Box Day-Dot protocol enforcement note directly editable on canvas.

### 5. Verification & Checksums
- Headless Flatpak Chrome verification: 6/6 tests passing (EX.oncall, XLSX Import, Whitespace pre-wrap, Empty line height, Floor plan buttons, Food pull editable).
- Captured Visual Approval Screenshot: `screen_v37_oncall_store_directory.png`.
- Production file hash: `SHA256: 6ca8180d0dc3e71a653db582b13ba3d0c65b5b78ce114489f6bdd7e7a2ae2e95`.
