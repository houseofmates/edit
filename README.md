<h1 align="center">edit</h1>

<p align="center">
  a terminal-style text editor widget for flutter — extracted from <a href="https://github.com/houseofmates/termisol">termisol</a>
</p>

<hr>

<h2 align="center">what it is</h2>

<p align="center">
  <code>EditTerminal</code> is a lightweight, self-contained <code>TextField</code>-based text editor widget built for Flutter apps that want a <code>nano</code> or <code>vim</code>-like editing surface inside a terminal emulator.  it's how <code>edit filename.txt</code> opens files in Termisol.
</p>

<hr>

<h2 align="center">features</h2>

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
- **dark theme** — warm amber-on-charcoal palette (<code>#0a0a0a</code> background, <code>#d8ba75</code> text, <code>#f6b012</code> accent)
- **language detection** — shows <code>dart</code>, <code>python</code>, <code>rust</code>, <code>bash</code>, <code>json</code>, <code>yaml</code>, <code>html</code>, <code>css</code>, <code>markdown</code>, and many more based on file extension

<hr>

<h2 align="center">installation</h2>

<p align="center">add the local path or a pub source to your <code>pubspec.yaml</code>:</p>

<pre align="center"><code>dependencies:
  flutter:
    sdk: flutter
  file_picker: ^8.1.2
  path: ^1.9.0
  edit:
    path: ./edit  # or a git/path reference to this package
</code></pre>

<p align="center">run:</p>

<pre align="center"><code>flutter pub get
</code></pre>

<hr>

<h2 align="center">usage</h2>

<p align="center">import the widget and pass a file path plus initial content:</p>

<pre align="center"><code>import 'package:flutter/material.dart';
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
</code></pre>

<p align="center">the editor will auto-load the file from disk if <code>initialContent</code> is empty.</p>

<hr>

<h2 align="center">api</h2>

<h3 align="center"><code>EditTerminal</code></h3>

<div align="center">
<table>
  <thead>
    <tr><th>parameter</th><th>type</th><th>required</th><th>default</th><th>description</th></tr>
  </thead>
  <tbody>
    <tr><td><code>filePath</code></td><td><code>String</code></td><td>yes</td><td>—</td><td>path of the file being edited</td></tr>
    <tr><td><code>initialContent</code></td><td><code>String</code></td><td>yes</td><td>—</td><td>initial text to display in the editor</td></tr>
    <tr><td><code>onClose</code></td><td><code>VoidCallback?</code></td><td>no</td><td><code>null</code></td><td>fired when <kbd>Ctrl</kbd>+<kbd>W</kbd> or the back button is tapped</td></tr>
    <tr><td><code>readOnly</code></td><td><code>bool</code></td><td>no</td><td><code>false</code></td><td>disables editing and save</td></tr>
  </tbody>
</table>
</div>

<h3 align="center">callbacks / bindings</h3>

<p align="center">the editor uses <code>CallbackShortcuts</code> internally — no state management required. wrap it in a <code>Focus</code> widget and call <code>unawaited</code> on the save/open futures if you don't need to await them.</p>

<hr>

<h2 align="center">architecture</h2>

<p align="center">
  the editor is intentionally simple: it wraps a standard Flutter <code>TextField</code> (unlimited lines) with a custom <code>TextEditingController</code> and a <code>ScrollController</code> pair for synchronized gutter / text scrolling.  no virtual pty, no language server protocol, no tree-shaking hackery.
</p>

<p align="center">
  state is local to the widget.  saving writes to a <code>dart:io</code> <code>File</code> directly; opening uses <code>file_picker</code> to let the user browse the filesystem.  all paths are absolute and come from the host app.
</p>

<hr>

<h2 align="center">license</h2>

<p align="center"><a href="license">mates license</a></p>
