# Crestron Recovery

**[Download CTI Crestron Recovery Setup v0_7_0.exe](https://dev.azure.com/CTI-AV/Skunkworks/_apis/git/repositories/Crestron-Recovery/items?path=/dist/CTI%20Crestron%20Recovery%20Setup%20v0_7_0.exe&versionDescriptor.version=main&versionDescriptor.versionType=branch&download=true&%24format=octetStream&api-version=7.1)**
(run it once; it installs for all users, and a newer setup upgrades in place)

Formerly **System Saver** (0.1.0 to 0.3.1): installing 0.4.0 or later over it
replaces it, and its settings carry over.

One program for recovering editable Crestron sources from compiled files
and running hardware. **Touch Panel Recovery** (today's window)
rebuilds an editable VTPro-e project (`.vtp`) from a compiled touch panel
(`.vtz`): Smart Graphics panels (XPanel 2.0: TSW, TS, UC) and native DGE /
TPMC panels (the older XPanel engine), from an XPanel 2.0 build (the `.c3p`
in a `.c3prj` folder) and from an XPanel 1.x build (the `.vtx` in a
`.xexe\bin` or `.xweb` folder). Give a Smart Graphics panel or a `.c3p` its
original `.sgd` too, and the rebuilt project keeps the SIMPL program's project
and smart object GUIDs; a native panel or an XPanel 1.x build has no `.sgd`.

**Status 2026-11-24 (0.6.0; not on ATSG yet):** the
*Touch Panel* tab is Recreate VTP 1.6.0 under its own name; the *Program* tab
is Recreate SMW's engine (an editable SIMPL Windows `.smw` from a compiled
`.lpz` or a processor's program folder); every pull starts on the *System*
tab's device table -- a touch panel, a processor's programs, or a whole
system with its XPanels -- and files itself under one root folder. It installs beside Recreate VTP (its own setup id) and keeps its
own settings. Coming next: the C# source of a SIMPL# Pro program. The
engines are unchanged from their origins: what each panel family and program
rebuilds, and how it was proven, is in [CLAUDE.md](CLAUDE.md) and in
Recreate VTP's own README up to 1.6.0.
Working rules: [CLAUDE.md](CLAUDE.md).

The download link above is the `dist\` setup on `main` of the Azure DevOps repo
(CTI-AV org, Skunkworks project, repo `Crestron-Recovery`). Changes to `main`
come in as pull requests, which the maintainer approves.

## Installing

1. Download the setup with the link above and run it. Windows asks for
   administrator rights once.
2. The setup isn't code-signed, so SmartScreen may say *"Windows protected
   your PC"*: choose *More info > Run anyway*. That happens once for each
   new setup, and never for the installed program.
3. **Crestron Recovery** is then in the Start menu, and in *Settings > Apps*,
   where it can be uninstalled.

### Updates

At start-up the program checks whether a newer version exists. If one does,
a banner in the Windows accent colour says so. **Click it to install the new
version**: the program downloads the setup with your own git sign-in to
Azure DevOps (Git for Windows needed), checks it against the SHA-256 the
version file gives, and runs it silently; Windows asks for administrator
approval once, and the program closes, updates and opens again with its
settings. A download that does not match is deleted unrun, and if anything
fails (or git is missing) the banner offers the download in the browser
instead, as it always does on a development build. To check on demand, click
the **...** button (*More options*) at the top right and choose **Check for
updates...**. The same menu has **Change log...** (what each version brought,
built into the program from [CHANGELOG.md](CHANGELOG.md)) and **About**. All
three are also in the window's system menu (Alt+Space). The check reads one
small file from `raw.githubusercontent.com`; it sends nothing about the PC.

## Using it

The window has three tabs: **System**, **Program** and **Touch Panel**. Each
has its own log (*Copy all*, *Export...* and *Clear* work on the one shown;
the settings file and update lines appear in all three), and the window
opens on the tab used last. Every *Convert* and every pull saves its own log
lines without asking, as `Crestron-Recovery_<tab>_log_<date>_<time>.log`: a
Convert's beside the rebuilt `.vtp` or `.smw`, a pull's in its root folder.

**System**: type the user name and password, *Discover* the Crestron
devices on this PC's networks (or *Add* one by its *Hard Link* address:
another network, a VPN), then click a row's blue arrow. A device pulled in
this run carries a green check. A touch panel's console names its processor
and IP-ID; the processor's every program slot, its `NVRAM` and `USER`
folders and what its `HTML` folder hosts come off with the panel, each
`.vtp` rebuilt with the project's `.sgd` (out of the program's archive, or a
SIMPL# Pro program's own files) -- the job repo's own layout, with no trip
through Toolbox.

Every pull files itself under the **Project Save Root Folder** (the row
under the sign-in; *Browse...* changes it, and the first pull asks for it
while it is empty). Its help icon shows the layout:

```
<root>\
  Control\<model> <hostname>\      NVRAM, USER, Slot0n files, controller info.txt
  Interface\<model> <hostname>\    the panel's files and its info file
  Interface\Hosted XPanel <controller hostname>\
  Interface\Hosted CH5 <controller hostname>\
  Discovered Devices.txt           every device found
  <zip>                            offered at the end
```

A folder is made only when there is something to put in it. A hosted Web
XPanel or CH5 project is saved laid out as it is on the processor (and
rebuilt beside it, for a Web XPanel); the processor's own loose `HTML` files
are not saved -- a project loaded onto it writes them again.

Every arrow's pull confirms itself, start to end. While it runs, a small
window shows its log lines as they come (a busy bar: a pull does not know
its total until it ends); the first question at the end takes its place.
After a green or blue arrow that saved a program archive, the archives are
offered **extracted into the Control folder** (one Yes / No for all of them):
each archive's files go flat beside its `.lpz` and `_archive.zip`, as a job
repo's `Control` folder holds them, and never over a different file -- an
archive with a clash goes into its own `SlotNN_<name>_archive` folder. Then
the zip is offered: Yes puts the whole root folder into
`<root>\<root's name>.zip`, beside `Control` and `Interface`, to hand over
(an earlier zip in the root is left out of it). *Zip*, on the root row, does
the same at any time. Once a pull's questions are answered, its own log lines
are written into the root as `Crestron-Recovery_<tab>_log_<date>_<time>.log` (the
name *Export...* proposes), and into the zip too when one is made. Then a
**report** opens: the rebuild's when the pull rebuilt something, else the
pull's own (the root and zip, the pull's warnings and errors, its log lines).
Its **Done** opens the root in Explorer, the zip selected when one was made,
else `Control` (green arrow) or `Interface` (orange); its X only closes it.

The table's first column holds pull arrows in the tabs' colours: a touch
panel has a blue one (its full system) and an orange one (the panel only); a
processor has a green one (its programs, NVRAM and USER), an orange one when
it hosts a Web XPanel and a blue one when it hosts anything; other devices
have none. The orange and green pulls run on their own tab. A double-click
on a processor is its green arrow; on a touch panel it asks *Full System*,
*Touch Panel Only* or *Go Back*.

**Program** (Program Recovery): *Browse...* for a compiled SIMPL program
(`.lpz`), or drop an `.lpz` or a processor's program folder (a copy of
`/ProgramNN`: the `.lpz` extracted) anywhere on the window, which switches
to this tab. **Convert** asks where to save the rebuilt `.smw` (it proposes
`<name> rebuilt.smw` beside the program, never the original's name) and
rebuilds it; recovered macros (`.umc`) and SIMPL+ modules (`.ush`, `.usp`,
`.clz`) are written beside it. It reads SIMPL Windows' symbol and device
databases (`Cresdb\Programming`), so it needs SIMPL Windows on the PC;
without it Convert stays off and the tab says so. What the rebuild recovers,
and how it was proven, is in [CLAUDE.md](CLAUDE.md) (Program Recovery).
**Or pull the programs off a processor** (the green arrow of a 3-Series or
4-Series processor's row on the System tab, signed in with the System card's
user name and password). Every program slot, `/Program01` to
`/Program10`, is read over SFTP (port 22, a processor's default), or FTP
(port 21) when SFTP is off -- a processor keeps no `.lpz`: each slot folder
is the `.lpz` extracted, plus a `.zig` holding the signal names -- and each
loaded slot is saved into `Control\<model> <hostname>\` under the root
folder as `Slot01_<name>.lpz`, the program exactly as the processor holds it (an
identical file already there is reused; a different file of that name is left
alone and `Slot01_<name> (from <address>).lpz` written instead). Empty slots
are named in the log; a slot whose program does not read whole (a `.bin` cut
short, no `.dsc`, no signal names) is still saved and warned about. A slot
that carries SIMPL's own archive of the project gives it beside the `.lpz` as
`Slot01_<name>_archive.zip` (*File > Import Archived Program* in SIMPL
Windows), and the pull's end offers it extracted beside it (see above). The
user name is remembered; the password is kept DPAPI-protected.

**After the pull, one question** lists that controller's slots with no
archive, each ticked (a System pull asks it too, once per controller, when
everything it pulled is saved; a pull whose slots all carry their archive asks
nothing). Each offers to **reconstruct it now**, with a rough time: it is rebuilt as
*Convert* would, into `Slot01_<name> rebuilt\` beside the `.lpz` (the `.smw`
and its recovered macros and modules); without SIMPL Windows on the PC it is
not offered, and the log says to convert the `.lpz` where SIMPL is installed.
Untick what is not wanted and *Continue*, or *Skip all* to leave every slot
as pulled.

**When the controller refuses the login**: SFTP, then FTP, both with the
typed sign-in, then plain FTP with none (as `anonymous`), once -- a
controller whose authentication was turned on while its plain FTP stayed on.
If plain FTP lets the pull in, its console cannot be read, so the pull takes
everything it can, in case the controller is about to be factory reset:
every slot, `NVRAM`, `USER` and the whole `HTML` folder as it is (beside them
in `Control\<model> <hostname>\`), and the log says so. If plain FTP fails
too, a prompt says the login was incorrect, plain FTP was also unsuccessful,
and the login information is required.

**SIMPL# Pro programs** are saved as they run. A 4-Series unpacks a loaded
`.cpz` into its slot and deletes it, so a slot with no `.bin` and a
`ProgramInfo.config` is zipped back into `Slot01_<name>.cpz` (the name is the
config's `FriendlyName`, else its `EntryPoint`) -- the same files the `.cpz`
that was loaded held, ready to load again. Nothing is rebuilt and nothing
warned about; recovering the C# source is a planned future addition.

**XPanels** come along with a processor pull. On a 4-Series the `html` folder
also holds the processor's own web pages (`userlogin.html`,
`index_loginInfo.html`, `AVF`, `AVFConfig`, `resources`), which are never
pulled, and a CH5 project (an HTML5 XPanel) sits there as a folder holding
`<its name>_manifest.json`: that folder is pulled whole as `<name>.zip`,
saved and not rebuilt. A Web XPanel loaded onto the
processor sits in its `HTML` folder as the compiled `.c3p` (beside a
`.c3prj.info`), or unpacked in a folder holding `Core3XPanel.html`. Discover
finds it, and the processor's row then has an orange arrow (the Web XPanel
alone) and a blue one (everything on the processor); the green arrow takes
the programs only and says where the XPanel is pulled from. It is saved in
`Interface\Hosted XPanel <controller hostname>\` and rebuilt to a `.vtp`
there, with the `.sgd` the program names for its IP-ID (else one of its own
name in a pulled slot).
Other subfolders of `HTML` are left on the processor, and the log says so. Each
loaded slot's device list (its `.dsc`) names its XPanels by IP-ID; it names a
Web and a PC XPanel alike, so the log lists each one with what came off the
processor, and for one that is not there says to get its files off the PC
that runs it, by USB or email (the `.c3p` in its `.c3prj` folder, or its
`.xexe` folder), and drop them on Touch Panel. An HTML5 XPanel is a CH5
project and is not rebuilt.

**The info files** (`controller info.txt`, `touch panel info.txt`) are
written in each pulled device's folder, and the root's **Discovered
Devices.txt** lists every device found. An info file holds
the device pulled from, read off its console over SSH with display commands only
(`ver -v`, `hostname`, `ipconfig`, `ipt`, `authentication`, `ipconfig /all`,
`time`, `timezone`, `uptime`) -- host name, IP address, model,
serial number (a 4-Series processor's; a 3-Series processor or a panel prints
none, so its TSID is given), MAC address, firmware, IP table -- plus, for a
processor, each loaded slot's `progcomments` (the original source path, the
compile date) and `proginfo` (the statistics, the load timings left out) and
its XPanels (each slot's XPanel IP-IDs, the files off its `HTML` folder), and
for a panel its project's `~.LocalInfo.vtpage`. Discovered Devices.txt gives
every device's model, host name and address -- what Discover found, the
devices pulled from, and those added by their address (one that never
answered: `manually added`, `(not found)` after its address). An earlier file that
differs only in its "Written by" line stands; one with other facts is kept,
and the new one is written as `<name> (from <address>).txt` beside it.

**Touch Panel** (Touch Panel Recovery): the steps below. Any other file
dropped on the window switches to Touch Panel.

1. **Choose the compiled panel:** *Browse...* beside *Compiled panel*,
   or drop the `.vtz` anywhere on the window (a `.vtz` named on the command
   line, or dropped on the exe, opens the same way). For an XPanel 2.0 build,
   choose the `.c3p` in its `.c3prj` folder. For an XPanel 1.x build,
   choose the `.vtx` in its `.xexe\bin` (or `.xweb`) folder; a `Gateway.ini`
   beside it brings back the additional gateways. For an XPanel 2.0 (Web)
   build, drop its folder (the one holding `Core3XPanel.html`), or choose its
   `<name>_Project.c3prjz`, its `Core3XPanel.html` or the folder zipped. A dropped folder (a compiled
   XPanel shows as one in the project's `Interface` folder) stands for the one
   compiled panel in it, up to three levels down; a folder holding several is
   refused, the log listing them, so they are chosen one at a time.
   **Or pull it off the panel** (Mike Krejci's idea: the orange arrow of a
   touch panel's row on the System tab, signed in with the System card's user
   name and password). The project running on the panel is read over SFTP
   (port 22, a Crestron panel's default), or FTP (port 21) when SFTP is off,
   from its `display` folder -- the `.vtz` extracted, less the two files the
   panel adds -- and written into `Interface\<model> <hostname>\` under the
   root folder with `touch panel info.txt` (an identical `.vtz` already there
   is reused; a different file of that name is left alone and
   `<name> (from <address>).vtz` written instead), then rebuilt beside it
   under the project's name (never over a `.vtp` already there:
   `<name> rebuilt.vtp` then). A `.sgd` chosen before the pull is kept, since
   the panel itself holds none.
2. **The original .sgd:** when a `.sgd` with the same name sits beside the
   `.vtz` (any case: `Panel.vtz` and `PANEL.SGD`; for a `.c3p`, in the project
   folder that holds its `.c3prj` folder), it is used by itself, and
   the line under the field says so. Otherwise choose it with the `.sgd`'s
   *Browse...* (or drop it on the window with or after the `.vtz`). *Clear*
   stops using it. A new `.vtz` starts over: a `.sgd` chosen for the
   previous one is not kept.
3. **Convert** asks where to save the rebuilt project (it proposes the
   panel's name, in the `.vtz`'s folder; for a `.c3p`, the project folder above its `.c3prj`), then rebuilds. The log first lists what no rebuild can
   recover, and, when no `.sgd` is in use, warns:
   `WARNING: no original .sgd -- the project and smart object GUIDs will NOT match the SIMPL program; modify them to match the program before syncing the panel to it`
   A native panel (DGE / TPMC) or an XPanel 1.x build has no `.sgd`: its row is
   greyed and nothing is warned.
4. **The results report** pops up after every rebuild: what to check in
   VTPro-e (page, the object's text and position, what to look at), errors on
   a red label, warnings on a yellow one, harmless notes plain and last; then
   what no rebuild brings back, and every line of the rebuild in small print
   for debugging (each line labelled in words: *Error* red, *Warning* yellow, *Info* blue, *Note* and *Detail* plain). The paths are
   links: the `.vtp` opens in VTPro-e, *open folder* and the `.vtz` / `.sgd`
   paths open their folder. *Save .md...* keeps it; **Done** shows the rebuilt
   `.vtp` in Explorer (after a pull: the root folder); *...* > *Results report...*
   shows the last one again.
5. **A custom theme comes back with the panel.** When the panel uses a theme
   that is not installed on this PC (a custom one, or a Crestron theme the
   Crestron Database here lacks), the rebuild takes the theme's files out of
   the compiled panel and saves them beside the `.vtp` as
   `<theme>.core3_theme` (the same file already there is kept; a different one
   of that name is left alone and `<theme> (from <project>).core3_theme`
   written). Then it asks whether to install a copy on this computer: *Yes*
   copies it into VT Pro-e's user themes folder
   (`C:\ProgramData\Crestron\Core3\UserThemes`), so VT Pro-e opens the project
   with its own theme instead of asking for a replacement; *No* leaves it with
   the `.vtp` only. The file beside the `.vtp` stays either way, for the next
   PC: copy it into that folder there. A pull asks once, for every panel it
   rebuilt.

**What the rebuild cannot recover** (from [CLAUDE.md](CLAUDE.md)): the contents of pages that
were marked 'do not compile' (they are not in the compiled panel; a subpage reference to one keeps it as
an empty placeholder of the same name, itself marked 'do not compile', and the report says so); for a native panel or an XPanel 1.x
build, design-only details (object names, groups, hardkey labels, the Project Properties text,
which gateway is the default). Not supported yet: other TPMC models and
firmware panels without a project header here (TPS-12G-QM) -- the refusal asks the user to message
Logan Vaughn with the model number or the results report -- and Protection settings (unchecked).

**Folders** (*...* > *Source folder (compiled panel)* and *Output folder (.vtp)*, set
separately): the source's *Remember the last folder used* and the output's
*The panel's folder* (the defaults), or *Always* one fixed folder, picked with
*Choose the fixed folder...*. A fixed folder that has gone missing falls back
to the default, with a line in the log.

**Theme** (*...* > *Theme*): *System* (the default) follows Windows' light or
dark mode; *Light* or *Dark* keeps that one. It applies at once and is remembered.

The **Log** card collapses to its header (click *Log*); collapsed, it counts
what was logged since. *Copy all*, *Export...*, and Ctrl+C / Ctrl+A or the
right-click menu for selected lines.

## Settings file

Settings live in `CTI-CrestronRecovery.ini` in `%LOCALAPPDATA%\CTI-CrestronRecovery\`,
one per Windows user. (An exe run from outside Program Files, such as a
development build, keeps the file next to itself instead, unless that folder
is read-only.) The log's first lines show which file is in use. The file is
UTF-16, so any folder name survives. The first run under this name copies
System Saver's `CTI-SystemSaver.ini` (in `%LOCALAPPDATA%\CTI-SystemSaver\`, or
beside the exe) to the new name when there is no new file yet; the old file
is left where it was.

- `[Settings]`: `SourceFolderMode` / `OutputFolderMode` (`last` or `fixed`),
  `SourceFixedFolder` / `OutputFixedFolder`, `SourceLastFolder` /
  `OutputLastFolder`, `CheckUpdates` (default 1; 0 skips the start-up check;
  the menu item still works).
- `[Window]`: `Position`, `Maximized`, `LogCollapsed`, `Tab` (`system`, `program`
  or `panel`: the tab it opens on; `panel` when unset).
- `[System]`: `User`, `Password` (the password as DPAPI-protected hex,
  readable only by the same Windows user on the same PC), `Host` (the address
  last typed for *Add*), `Root` (the Project Save Root Folder).
- `[Program]`: `LastFolder` (where the Program tab's Browse and Convert's
  picker open).
- (`[Panel]` and `[Processor]`, written by 0.2.0 and earlier, are no longer
  read.)

## Building

Requires MSYS2 with the UCRT64 gcc toolchain
(`pacman -S mingw-w64-ucrt-x86_64-gcc`) and, for the setup, Inno Setup 6
(`winget install JRSoftware.InnoSetup`). Then run:

```powershell
.\build.ps1
```

This produces `build\Crestron Recovery.exe` and, from it,
`dist\CTI Crestron Recovery Setup v<version>.exe`, e.g. `v1_0_0`
(`installer\Crestron Recovery.iss`; the setup's name carries the version, with
underscores). `dist\` holds only the setup, which is what is committed; the
previous version's is removed. Without Inno Setup the exe is still built, so
the tests can run. `.\build.ps1 -Out <path>` writes the exe elsewhere and
skips the setup. The exe is x64, statically linked, and imports only Windows
system DLLs. A failed compile prints gcc's diagnostics line by line and puts
the `error:` lines in the exception text.

**The version** lives in `src\resource.h`: `APP_VERSION_STR` and
`APP_VERSION_NUM` change together. The installed exe's name never carries
it (`Crestron Recovery.exe`); the setup's name does.

**The engine** is behind one call, `RebuildVtpFromVtz` in `src\engine.h`
(the `.vtz`, the `.sgd` or NULL, the `.vtp`, a log callback; TRUE only when
the `.vtp` was written); the window runs it on a worker thread and logs the
limitations and the `.sgd` warning before calling it. It is the C port of
the Python reference engine in `tools\vtz-to-vtp` (`vtp_build.py --gen`,
`gen_*.py`), file for file: `src\zip_read.c` (the `.vtz`, with its own
inflate), `src\xml_dom.c` (ElementTree's view of XML), `src\cresdb.c`
(control trees and block headers from the Crestron Database:
`xsd_model.py`, `gen_rules.py`, `gen_template.py`, `gen_header.py`),
`src\vtp_node.c` (the property-node record), `src\vtp_rebuild.c`
(`vtp_build.py` and `gen_project.py`: injection, page records, the project
header, the `.sgd`, the OLE file), `src\native.c` (a native panel:
`native_write.py` and what it uses; `src\vtx_read.c`, `native_vtx.py`, hands it an
XPanel 1.x build's entries) with `src\native_data.c`, generated by
`tools\vtz-to-vtp\native_to_c.py` (templates, page template, per-panel
header canons), and `src\canon_headers.c`, generated by
`tools\vtz-to-vtp\canon_to_c.py` from `gen_project.py`'s data (run it after
changing that data). The Crestron Database is found through the registry
(`Crestron Core 3 UI Controls`, `SysCtrls Path`). The `.vtp` is written to a
temporary file beside the target and moved into place when complete. The
Smart Graphics version check (`ReadSmartGraphicsMismatch`) compares the
panel's Project `VersionNumber` in `swf/Environment.xml` with the installed
Smart Graphics (`Crestron Core 3 UI Controls`, `Version`) and refuses a
panel compiled by a newer one, naming both: the window before its save
picker opens (`EnsureSmartGraphics` in `src\main_window.c`), the engine
again before it builds. Native panels and XPanel 1.x builds are not checked.

**Releasing a version:**

1. Bump both version defines and update the download link at the top of
   this README to the new setup's name (it appears twice in that line).
   Add the version's entry at the top of [CHANGELOG.md](CHANGELOG.md): `## <version> -- <date>`,
   then a `- ` line for each thing a user gains. The program shows that file
   rendered, and knows only `#`, `##`, `- ` and `**bold**`.
   Run `.\build.ps1`; it warns if the link does not name the setup it just
   built, if the change log's first heading is not this version, or if it
   uses other markdown. Commit the setup with the change and merge the pull
   request into `main`.
2. **Right after that**, replace `version.json` with the line `build.ps1`
   printed (it carries the setup's `sha256`; without it, installed copies
   fall back to the browser download), in the public repo
   [logan-vaughn-cti/crestron-recovery-version-check](https://github.com/logan-vaughn-cti/crestron-recovery-version-check):
   the new `version`, and the new download link as `url`. Not before the
   merge (users would be sent to a setup that is not there yet), and not much
   after (the merge removes the previous setup, so the old `url` stops working).

```json
{"version": "1.0.1", "url": "<the download link above>", "sha256": "<the setup's SHA-256>", "notes": "One line about what changed."}
```

The program downloads the setup from this repo itself (`APP_DOWNLOAD_URL`),
never from the version file's `url`: the public file can only say which
version, and which hash to insist on. `version` must be digits and dots.
`url` is what the banner opens, and must start with `https://`; without one,
the program opens this repo's page instead. `notes` is optional. The repo
holds only that file and a README: no source and no exe. The address is
`UPDATE_URL` in `src\resource.h`. Until that repo and file exist, the
start-up check logs "the update server could not be reached" and carries on.

### Icons

`assets\make_icon.ps1` builds `assets\app.ico`, the one icon the program
uses everywhere (the exe, Explorer, Alt+Tab, the title bar, the taskbar
button and its thumbnail), as DHCP Server and Repo Check do: the CTI triangle
mark (`assets\cti_mark.png`) bottom-right and the owner's badge
`assets\blue-green-orange.png` (a sheet split System blue / SIMPL green with a
VT Pro-e-orange folded corner and a white `</>`: the three tabs' colours) top-left, under the mark
where the two meet. The tabs carry colour
accents instead of icons -- a swatch before the name and the selected tab's
pill in the colour of the tool each stands in for (Crestron blue for System,
SIMPL's green for Program, VT Pro-e's orange for Touch Panel). No vendor
artwork ships inside the program. `cti_logo.png` (the window header) and
`cti_mark.png` are the same brand files the siblings use.

### Look

The same Windows 11 design as DHCP Server and Repo Check: light and dark
follow the Windows app setting, live; buttons and highlights use the Windows
accent colour; the font is Segoe UI Variable (Segoe UI on Windows 10). It
all lives in `src\theme.c` (DHCP Server's, trimmed to this window).

## Testing

```powershell
python tests\ui_test.py
```

The window end to end, on files in a scratch folder: no menu bar and the
"..." button; Save greyed until a `.vtz` is chosen; Browse through the real
file picker; the same-name `.sgd` picked up whatever its case, a near-miss
name not; a `.sgd` chosen and cleared; dropped files (a `.vtz`, a `.vtz` with
a `.sgd`, anything else refused); Save proposing `<name>.vtp`, the
limitations logged, the exact no-`.sgd` warning only without a `.sgd` (once),
a `.vtz` that is not a ZIP failing cleanly (an error, no file, no temporary
file, Save usable again), and a real panel (`testdata\`, local only; skipped
without it) rebuilt into a `.vtp` with its `.sgd`'s 27 smart-object GUIDs; a native panel (the `.sgd`
row off, rebuilt, the results report with its tiers and its two buttons); the
fixed / last-used folders surviving a restart and deciding where the pickers
open (a bare file name typed into the picker lands in that folder; one fixed
folder has a non-ASCII name); a `.vtz` on the command line; Pull against two
stand-in FTP servers on this PC (`tests\panel_ftp_server.py` over Mike
Krejci's TSW-770 pull in `testdata\FULL TP PULL`, local only: one lists by
NLST only, the other by MLSD; no address, a wrong password, a good pull with
a `.sgd` chosen before it kept, the `.vtz` written beside the `.vtp` byte for
byte and reused on the second pull, the report, the settings surviving a
restart); the update check
against a version file served on this PC (newer: the banner; unreachable:
one quiet log line); the tabs (a page and a log each, the last one
remembered) and Program Recovery (a dropped `.lpz`, Convert, the `.smw`;
needs SIMPL Windows); the processor Pull against a stand-in processor served
the same way (slot 01 = the CP3N pull copy in `testdata\processor`, local
only; slot 02 = a pair's `.lpz` extracted without its archive; slot 03
empty): a wrong password, a good pull with the folder picker, every slot's
`.lpz`, slot 01's archive beside it byte for byte, slot 02 rebuilt into its
own folder, identical files reused on a second pull, the question after each
pull (its rows, none for an archived slot, *Skip all*, an unticked row), the
pull's own log in the root and in the zip,
the settings surviving a restart. The windows and the pickers appear for about
four minutes; nothing is clicked or typed for real, but leave the pickers
alone.

```powershell
.\tests\pull_test.ps1 [-Since <git ref>] [-ReferenceExe <pull_cli.exe>]
```

The pulls without the window: builds `tests\pull_cli.c` (the panel pull) and
`tests\processor_cli.c` (the processor pull) from the working tree, and the
panel pull again from a reference (`HEAD` by default, in a scratch worktree),
starts the stand-in FTP servers (NLST-only and MLSD over the TSW-770 pull; one
over a stand-in processor whose slots are the CP3N copy, a pair without its
archive, an empty folder and a `.bin` cut short), and checks that the panel
pull's `.vtz` is byte-identical to the reference's both ways (the shared
SFTP / FTP client, `src\remote_files.c`, must not change what a pull writes),
and that the processor pull writes every loaded slot with the folder's exact
files, the archive byte for byte, the right warnings, the empty slots named,
and that the engine's command line (`tests\smw_test.ps1` builds it) opens what
it wrote.

SFTP has no stand-in here (it is libssh2 against a real SSH server): it was
proven on a TSW-770 over the VPN (2026-10-29, 18 files in 13 s); a processor
over SFTP is the CP3N at 192.168.4.44 (his pull copy is the fixture). To try
a device without the window, the two command lines:

```powershell
gcc -O2 -std=gnu11 -Isrc -o build\pull_cli.exe tests\pull_cli.c src\panel_pull.c src\remote_files.c src\zip_write.c -static -lssh2 -lz -lws2_32 -lcrypt32 -lbcrypt
.\build\pull_cli.exe 10.1.1.49 admin CTIr0cks! <out folder>
gcc -O2 -std=gnu11 -Isrc -o build\processor_cli.exe tests\processor_cli.c src\processor_pull.c src\remote_files.c src\zip_write.c -static -lssh2 -lz -lws2_32 -lcrypt32 -lbcrypt
.\build\processor_cli.exe 192.168.4.44 admin CTIr0cks! <out folder>
```

```powershell
.\tests\engine_test.ps1
.\tests\engine_test.ps1 -Vtz <any .vtz> [-Sgd <its .sgd>]
```

The engine against its reference: builds `tests\engine_cli.c` (the engine on
the command line) with gcc, then rebuilds the same `.vtz` with the C engine
and with `tools\vtz-to-vtp\vtp_build.py --gen`, GUIDs made deterministic in
both (`VTZ_TEST_GUIDS`), and compares the two Contents streams byte for byte
(`tests\engine_compare.py` names the first difference and its segment).
Cases: UIS SU 226 (TSW-760) with and without its `.sgd`, Hoffman's LG Room
(UC-Engine), the panel-specific headers (DGE-2, TPMC-V12, TST-600, and
VTPro-e 6.2.02 compiles of a test project on the TSW-570, 570P, 770, 770-GV
and 1070), two real job panels with Subpage Reference Lists (Methodist College
E169 TSW-770, Glendale AZ Courtroom 1 TSW-1070; `page_ids.py` checks each
list's target, picker and control header), and a mutant that must be caught
(the C engine without the `.sgd`). Native panels (no `Environment.xml`) are compared with
`tools\vtz-to-vtp\native_write.py` instead (`tests\native_reference.py`):
the stream byte for byte, every results-report line equal, and the page
records tiling under an independent reader (panel-firmware panels with `tests\fw_reference.py`
and `engine_compare.py --pixels`: images by what they draw, the rest byte for byte; twelve corpus
rows); Westlake 310, Lincoln, Meda,
MasterCard TPMC-9, CAT DGE-1, a Caterpillar V15, a join-byte mutant and a
crafted unmapped radius. `-Vtz` takes either kind. The panel files are local (`testdata\`, a Hoffman job repo); a
missing one is skipped. Needs Python 3 with `olefile`.

```powershell
.\tests\smw_test.ps1 [-Since <git ref>] [-ReferenceExe <exe>] [-Show]
```

Program Recovery's regression gate: builds the engine's command line
(`tests\smw_cli.c`) from the working tree and from a reference (`HEAD` by
default, in a scratch worktree), rebuilds each answer-key pair in
`testdata\lpz\pairs` (local only) with both, and compares every output file
byte for byte, GUID lines aside. A change must leave them identical unless it
meant to touch them; `-Show` prints the differing lines.

```powershell
python tests\change_log_test.py
```

"..." > Change log, opened through the system menu: the title, the built
version first, every version heading in order, a bullet for every `- ` line
and no markdown marker showing; the text follows a resize, the window never
shrinks below its first size, and Close closes it.

```powershell
python tests\self_update_test.py
```

Installing an update from the banner, end to end without installing: a bare
repo on disk plays the Azure DevOps repo and a local web server the version
file; the right hash, a wrong hash (deleted unrun), no hash, not newer.
Needs git.

**Proven able to fail (2026-10-23):** `tests\fail_proof.ps1` builds one
mutant per feature (no drop handling, a case-sensitive `.sgd` match, the
warning removed or always shown, no limitations, settings not saved, an ANSI
settings file, the picker's folder not forced, an engine that writes and
succeeds, no update banner, raw markdown in the change log, no hash check,
the `.sgd` row left on for a native panel, notes logged as warnings, no
results report) and runs its suite against it; all 16 were CAUGHT (2026-10-24). Mutants
are built under `%TEMP%\crestron-recovery-fail-proof` with `-Out`, never to `build\`; each
run is limited (`-Minutes`, default 4) and every result is written to a
results file there as it lands. Run `.\tests\fail_proof.ps1` again after
changing a feature or its checks (`-Only <name>` for one).

| Switch | Effect |
|---|---|
| `<file.vtz>` | open this compiled panel at start-up |
| `--ini <file>` | use this settings file |
| `--log <file>` | also write the log to a file (UTF-8) |
| `--theme light` / `--theme dark` | force the light or dark look (screenshots) |
| `--update-url <url>` | read the version file from this address instead |
| `--no-update-check` | skip the start-up update check |
| `--update-repo <url>` | download the setup from this repo instead (tests use a bare repo on disk) |
| `--self-update-test` | take an update without asking, as an installed copy would, but only log the setup's command; then close |

## Source layout

| Path | What |
|---|---|
| `src/main_window.c`, `src/app.rc` | the window: the files, the pickers and folders, drops, the rebuild thread, the update check, the log |
| `src/engine.h`, `src/engine.c` | the decompile engine's seam (one call, the limitations text, the Crestron Database check) |
| `src/vtp_rebuild.c`, `src/cresdb.c`, `src/vtp_node.c`, `src/xml_dom.c`, `src/zip_read.c`, `src/engine_util.c`, `src/canon_headers.c` | the engine: the C port of `tools\vtz-to-vtp` (see *The engine* above) |
| `src/native.c`, `src/native_data.c` | the native (DGE / TPMC) engine: the C port of `native_write.py` (data generated by `native_to_c.py`) |
| `src/vtx_read.c` | an XPanel 1.x build (`.vtx`) as the native engine's entries: the C port of `native_vtx.py` |
| `src/remote_files.c` | reading files off a Crestron device over SFTP (libssh2 on OpenSSL, static: `third_party\`) or FTP (Winsock): the client both pulls share |
| `src/panel_pull.c` | Pull: the project off a touch panel (its display folder), stored as a `.vtz` in memory |
| `src/processor_pull.c` | Pull: every program slot off a processor (`/Program01` .. `/Program10`), each stored as an `.lpz` in memory and checked without a database |
| `src/zip_write.c` | the stored-ZIP writer in memory (both pulls, the packaged SIMPL# libraries) |
| `src/device_console.c` | a Crestron device's console over SSH (display commands only) and its IP tables |
| `src/discovery.c` | Discover: Crestron's UDP 41794 broadcast on every adapter, and (with Npcap) devices in another range by their ARP |
| `src/device_report.c` | the info files (the pulled devices' facts off their consoles, a panel's project information) and `Discovered Devices.txt` (every device found) |
| `tests/system_cli.c` | the console, Discover and the devices' facts on the command line (`facts <host> <user> <password> [slot]...`) |
| `tests/panel_ftp_server.py`, `tests/pull_cli.c`, `tests/processor_cli.c`, `tests/pull_test.ps1` | the stand-in FTP server the window test pulls from; the two pulls on the command line; their gate |
| `src/results_report.c` | the results report window |
| `src/theme.c` | the Windows 11 look (DHCP Server's, trimmed) |
| `src/change_log.c` | the Change log window: CHANGELOG.md rendered (the same file as the siblings') |
| `src/self_update.c` | installing an update from the banner: git download, SHA-256 check, silent setup |
| `installer/Crestron Recovery.iss` | the setup (Inno Setup 6) |
| `tests/ui_test.py` | the window end to end |
| `tests/change_log_test.py` | the Change log window |
| `tests/self_update_test.py` | the self-update path against a stand-in repo and version file |
| `tests/engine_test.ps1`, `tests/engine_cli.c`, `tests/engine_compare.py`, `tests/native_reference.py` | the C engine against the Python reference, byte for byte |
| `tools/` | the Python reference engine, its scorers and probes, and `vtpro_drive.py` (VTPro-e's loader as an unattended check) |

## License

Copyright (c) 2026 Conference Technologies, Inc. All rights reserved
(`LICENSE.md`): a CTI tool, for use within CTI.
