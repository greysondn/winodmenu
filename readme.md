WinODMenu
=========
An attempt to bring OpenDesktop menu sorting to Windows

Status
======
There is no executable code ready for public consumption yet. Any attempts to use this codebase and accompanying docs is wholly unsupported.

Todo
====
- [X] Write intents document for basic application design
- [ ] Decide XDG base directories mapping for all relevant directories
- [ ] Open Desktop Files Repo
- [ ] Write some basic code
- [ ] Finish these docs
- [ ] Key-Value for override to not show an application in the menu
- [ ] Config: Alternative Steam install directories
- [ ] Config: Alternative program files directories
- [ ] Docs: Special variables, lazy var parsing notes (scunthorpe)

Usage
=====
Nah, no thanks dawg.

Installation
============
Nah, no thanks dawg.

Requirements
============
* Windows 10 x64
* other requirements TBD

TODO: List other requirements

Differences from OpenDesktop
============================

Generally, we try to mantain parity with OpenDesktop specification, such that you could just pull, for example, the Firefox `.desktop` file, add a few fields, and be good to go. This was decided for ease of adding applications, primarily; where a file already exists, work doesn't need to be duplicated (and, hopefully, we could see inclusion of our data later).

However.

There are certain (intentional or consequential) incompatibilities or oddities. They are documented here.

XDG_DATA_DIRS
-------------

This simply doesn't exist under Windows. Application specific locations are used instead. Specifically, in priority order high-to-low, the list is currently: `$HOME/AppData/Local/WinODMenu`, `$ProgramData/WinODMenu` .

As is likely expected, a directory is skipped if it doesn't exist, and files from higher priority directories "overwrite" lower priority directories for the sake of configuration.

Users who need to "fix" install locations of applications or provide their own are urged (and expected) to place them in `$HOME/AppData/Local/WinODMenu`.

Extra Categories
----------------
It was felt that the category taxonomy ([main][fd-cat-main], [additional][fd-cat-additional], [reserved][fd-cat-reserved]) was incomplete for needs under Windows, so some extra categories are supported.

`Archipelago` - a subcategory of `Game`, things related to the Archipelago randomizer

`AudioAnalysis` - a subcategory of `Audio`, tools like spectrometers.

`Geneology` - a subcategory of `Education`

`Modding` - a subcategory of `Game`, Tools for modding games

`ProgrammingGame` - a subcategory of `Game`, games focused on programming.

Full Category Tree
------------------
```text
Adult
Amusement
Audio
    AudioAnalysis
    Midi
    Mixer
    Sequencer
AudioVideo
    AudioVideoEditing
    DiscBurning
    Player
    Recorder
    Tuner
    TV
ConsoleOnly
Core
Development
    Building
    Database
    Debugger
    GUI Designer
    IDE
    Profiling
    Revision Control
    Translation
    WebDevelopment
Documentation
Education
    Art
    Biology
    Chemisty
    ComputerScience
    Economy
    Geneology
    Geography
    Geology
    History
    Humanities
    Languages
    Literature
    Music
    Spirituality
Electronics
Engineering
Game
    ActionGame
    AdventureGame
    ArcadeGame
    Archipelago
    BoardGame
    BlocksGame
    CardGame
    Emulator
    KidsGame
    LogicGame
    Modding
    ProgrammingGame
    Roleplaying
    Shooter
    Simulation
    SportsGame
    StrategyGame
Graphics
    2DGraphics
        RasterGraphics
        VectorGraphics
    3DGraphics
    Photography
    Scanning
        OCR
    Viewer
GTK
    GNOME
    XFCE
Java
Motif
Network
    Chat
        InstantMessaging
            IRCClient
        VideoConference
    Dialup
    Email
    Feed
    FileTransfer
        P2P
    HamRadio
    News
    RemoteAccess
    Telephony
        TelephonyTools
    WebBrowser
Office
    Calendar
    Chart
    ContactManagement
    Dictionary
    Finance
    Flowchart
    PDA
    Presentation
    ProjectMAnagement
    Publishing
    Spreadsheet
    WordProcessor
QT
    DDE
    KDE
Science
    ArtificialIntelligence
    Astronomy
    DataVisualization
    Electricity
    GeoScience
    ImageProcessing
    Math
        NumericalAnalysis
    MedicalSoftware
    ParallelComputing
    Physics
    Robotics
    Sports
Settings
System
    DesktopSettings
    Filesystem
    HardwareSettings
        Printing
    Monitor
    PackageManager
    Security
    TerminalEmulator
Utiity
    Accessibility
    Archiving
        Compression
    Calculator
    Clock
    FileTools
        FileManager
    Maps
    TextEditor
    TextTools
Video
```

Spec Fields
-----------

As per usual, this is expected to be under the `[Desktop Entry]` heading.

| Field | Spec Requires | Windows Shortcut Field | Ignored | Comments |
|-------|---------------|------------------------|---------|----------|
| Type | Yes | N/A | Yes | We only support Application. |
| Version | No | N/A | No | We ignore this field if it's not under a WinODMenu heading. |
| Name | Yes | Name | No |  |
| Generic Name | No | N/A | Yes |  |
| NoDisplay | No | N/A | Yes |  |
| Comment | No | Comment | No |  |
| Icon | No | Icon | No | We ignore this field if it's not under a Windows heading. |
| Hidden | No | N/A | Yes |  |
| OnlyShowIn | No | N/A | Yes |  |
| NotShowIn | No | N/A | Yes |  |
| DBusActivatable | No | N/A | Yes |  |
| TryExec | No | N/A | No | We ignore this field if it's not under a Windows heading. |
| Exec | No | Target | No | We ignore this field if it's not under a Windows heading. |
| Path | No | N/A | Yes |  |
| Terminal | No | N/A | Yes |  |
| Actions | No | N/A | Yes | This is handled by installers on Windows |
| MiimeType | No | N/A | Yes | This is handled by installers on Windows |
| Categories | No | N/A | No |  |
| Implements | No | N/A | Yes |  |
| Keywords | No | N/A | Yes |  |
| StartupNotify | No | N/A | Yes |  |
| StartupWMClass | No | N/A | Yes |  |
| URL | Yes, if type is Link | N/A | Yes |  |
| PrefersNonDefaultGPU | No | N/A | Yes |  |
| SingleMainWindow | No | N/A | Yes |  |

New Headings and Fields
-----------------------

These are compatible with the spec but serve as extensions to it.

Heading: `[X-WinODMenu]` (Required)

Field `Version`: The version of WinODMenu's intent spec this was written against. The current intent spec is version `1.5~WinODMenu1`. (Required)

Heading: `[X-Windows]` (Required)

Field `TryExec` (Required) - An executable or file used to verify the application is installed. Will not be executed. Expected fully qualified path.

Field `Exec` (Required) - The actual execution target for the resulting Windows shortcut. This should be something to copy-paste directly into the target field of a Windows shortcut.

Field `Icon` (Not Required) - Windows compatible icon location

Field `IconIndex` (Required only if `Icon` is set) - Index within `Icon` to use.

Minimalistic Example File
-------------------------
```ini
[Desktop Entry]
Type=Application
Name=Test
Comment=Test

[X-WinODMenu]
Version=1.5~WinODMenu1

[X-Windows]
TryExec=Whatever
Exec=Whatever
Icon=Whatever.ico
IconIndex=0
```

[fd-cat-additional]: https://specifications.freedesktop.org/menu-spec/latest/apas02.html

[fd-cat-main]: https://specifications.freedesktop.org/menu-spec/latest/apa.html#main-category-registry

[fd-cat-reserved]: https://specifications.freedesktop.org/menu-spec/latest/apas03.html