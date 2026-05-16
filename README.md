<h1 align="center">edit</h1>

<p align="center">
  a terminal-style text editor widget for flutter — extracted from <a href="https://github.com/houseofmates/termisol">Termisol</a>
</p>

---

<h2>what it is</h2>

<p>
  <code>EditTerminal</code> is a lightweight, self-contained <code>TextField</code>-based text editor widget built for Flutter apps that want a <code>nano</code> or <code>vim</code>-like editing surface inside a terminal emulator.  it's how <code>edit filename.txt</code> opens files in Termisol.
</p>

---

<h2>features</h2>

- **line numbers** — a persistent gutter shows the current line on every row
- **status bar** — displays line &amp; column, detected language, character count, and encoding; a dirty <code>*</code> indicator shows unsaved changes
- **keyboard shortcuts**
  - <kbd>Ctrl</kbd> + <kbd>S</kbd> — save the current file
  - <kbd>Ctrl</kbd> + <kbd>W</kbd> — close the editor
  - <kbd>Ctrl</kbd> + <kbd>O</kbd> — open a file dialog (uses <code>file_picker</code>)
  - <kbd>Ctrl</kbd> + <kbd>F</kbd> — toggle find bar (forward / backward search with wrap-around)
  - <kbd>Tab</kbd> — insert 2 spaces (or indent selected block)
  - <kbd>Shift</kbd> + <kbd>Ctrl</kbd> + <kbd>D</kbd> — duplicate the current line
- **auto-indent** — pressing <kbd>Enter</kbd> carries forward the current line's leading whitespace; trailing <code>{</code>, <code>(</code>, or <code>[</code> adds one extra indent level
- **dark theme** — warm amber-on-charcoal palette (`#0a0a0a` background, `#d8ba75` text, `#f6b012` accent)
- **language detection** — shows `dart`, `python`, `rust`, `bash`, `json`, `yaml`, `html`, `css`, `markdown`, and many more based on file extension

---

<h2>installation</h2>

add the local path or a pub source to your <code>pubspec.yaml</code>:

```yaml
dependencies:
  flutter:
    sdk: flutter
  file_picker: ^8.1.2
  path: ^1.9.0
  edit:
    path: ./edit  # or a git/path reference to this package
```

run:

```bash
flutter pub get
```

---

<h2>usage</h2>

import the widget and pass a file path plus initial content:

```dart
import 'package:flutter/material.dart';
import 'package:edit/edit.dart';

Navigator.of(context).push(
  MaterialPageRoute(
    builder: (_) => EditTerminal(
      filePath: '/home/user/project/main.dart',
      initialContent: '// paste existing file contents here',
      onClose: () => Navigator.of(context).pop(),
      readOnly: false,
    ),
  ),
);
```

the editor will auto-load the file from disk if <code>initialContent</code> is empty.

---

<h2>api</h2>

### <code>EditTerminal</code>

| parameter | type | required | default | description |
|---|---|---|---|---|
| <code>filePath</code> | <code>String</code> | yes | — | path of the file being edited |
| <code>initialContent</code> | <code>String</code> | yes | — | initial text to display in the editor |
| <code>onClose</code> | <code>VoidCallback?</code> | no | <code>null</code> | fired when <kbd>Ctrl</kbd>+<kbd>W</kbd> or the back button is tapped |
| <code>readOnly</code> | <code>bool</code> | no | <code>false</code> | disables editing and save |

### callbacks / bindings

the editor uses <code>CallbackShortcuts</code> internally — no state management required. wrap it in a <code>Focus</code> widget and call <code>unawaited</code> on the save/open futures if you don't need to await them.

---

<h2>architecture</h2>

<p>
  the editor is intentionally simple: it wraps a standard Flutter <code>TextField</code> (unlimited lines) with a custom <code>TextEditingController</code> and a <code>ScrollController</code> pair for synchronized gutter / text scrolling.  no virtual pty, no language server protocol, no tree-shaking hackery.
</p>

<p>
  state is local to the widget.  saving writes to a <code>dart:io</code> <code>File</code> directly; opening uses <code>file_picker</code> to let the user browse the filesystem.  all paths are absolute and come from the host app.
</p>

---

<h2>credits</h2>

<p>extracted from <a href="https://github.com/houseofmates/termisol">houseofmates/termisol</a> by Hermes.  MIT licence.</p>
