# Tom OS WordDocEdit — theme and branding notes

Tom OS WordDocEdit is WordCraft re-skinned to match the Tom OS desktop
(TDM login screen, Quickshell bar, kitty, GTK theme). Every change is in
the GUI layer; the document engine, file formats and the control channel
are unchanged from upstream WordCraft.

## The palette

| Name | Hex | Where it shows |
|---|---|---|
| Neon pink | `#FF2D96` | accent: selected ribbon tab, checked buttons, focus, AutoSave on, account bubble, window edges |
| Pink ink | `#FF6FB5` | accent text and hover in the dark theme |
| Purple | `#C74CFF` | middle of the neon rule, author colour |
| Cyan | `#00F0FF` | end of the neon rule, first comment / track-changes author |
| Yellow | `#FFE14D` | the Tom OS bolt, Find highlights |
| Night | `#0A0A12` | title bar and status bar base, text on pink |
| Ribbon | `#15131F` | ribbon and tab strip |
| Panel | `#181626` | side panes, menus `#1B1829` |
| Text | `#ECEAF7` / dim `#B9B5D6` | interface text |

The light theme uses the same idea with deeper magenta (`#C2186B`, ink
`#9E1258`) on lavender-white surfaces, so white text on the accent still
passes 4.5:1.

All colours were checked against WordCraft's own contrast tests (4.5:1 for
text, 3:1 for input borders and author colours).

## What changed in the GUI

- `crates/ui-egui/src/theme.rs` — new light and dark token sets, a
  `theme::neon` palette module, a `neon_rule()` painter (pink → purple →
  cyan), rounder corners (windows 12, menus 10, widgets 6), a faint pink
  window edge and a pink glow on pop-ups in the dark theme. Dark is the
  default appearance (System and Light are still in File › Options).
- `crates/ui-egui/src/chrome.rs` — a **TOM OS badge** (lightning bolt +
  "TOM OS") at the start of the title bar, which opens About; a neon rule
  under the title bar and above the status bar. The community button is
  off by default (it can be switched back on in Options).
- `crates/ui-egui/src/lib.rs` — `APP_NAME = "Tom OS WordDocEdit"`, used for
  window titles, the no-document title and the Save Changes dialog.
- `crates/ui-egui/src/dialogs/dispatch.rs` — a Tom OS About page with links
  to this repo, Tom OS and upstream WordCraft (credits and licence kept).
- `assets/app-icon/` — a new neon icon (page + Tom OS bolt) in every size,
  plus `.ico` and SVG. The macOS `.icns` is still the upstream one.
- `packaging/linux/` — the desktop entry and AppStream name are now
  "Tom OS WordDocEdit". The app id and binary name stay
  `ai.storyteller.wordcraft` / `wordcraft` so packaging and file
  associations keep working.

## Tests

The full `wordcraft-ui-egui` test suite passes (`cargo test -p wordcraft-ui-egui --lib`:
1136 passed, 3 ignored). Tests that assumed upstream's defaults were adjusted:
the default-theme tests now expect Dark, the window-title test uses `APP_NAME`,
the equation-tile test accepts the tinted Tom OS greys (each channel between the
tile and the text colour), and the off-page I-beam test asks for the light theme.
On macOS the TOM OS badge leaves the File page's back-chevron spot clear (#2972).
