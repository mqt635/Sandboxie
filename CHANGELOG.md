# Changelog
All notable changes to this project will be documented in this file.
This project adheres to [Semantic Versioning](http://semver.org/).


## [0.8.5 / 5.50.5] - 2021-07-05

### Added
- added global hotkey to terminate all boxed processes (by default Ctrl+Break)
- the Run Sandboxed dialog can now be handled by the Sandman UI.
- added "AllowBoxedJobs=y" allowing boxed processes to use nested jobs on Windows 8 and later
-- note: this allows Chrome and other programs to use the job system for additional isolation
- added librewolf.exe to the list of Firefox derivatives [#927](https://github.com/sandboxie-plus/Sandboxie/issues/927)
- added run regedit sandboxed menu command
- added new support settings tab to Sandman UI for updates and stuff
- added code integrity verification to Sbie service and UI
- added template for Vivaldi Notes (by isaak654) [#948](https://github.com/sandboxie-plus/Sandboxie/issues/948)

### Changed
- Replaced the Process List used by the driver with a much faster Hash Map implementation
-- Note: this change provides an almost static system call speed of 1.2us irregardless of the running process count
-- The old list, with 100 programs running required: 4.5µs; with 200: 12µs; and with 300: 18µs per syscall
-- Note: some of the slowdown was affecting also non sandboxed applications due to how the driver handles certain callbacks
- Replaced the per-process Thread List used by the driver with a much faster Hash Map implementation
- Replaced configuration section list with a hash map to improve configuration performance, and increased line limit to 100000
-- not yet enabled in production build
- the presence of default box is only checked on connect
- the portable dir dialog now shows the directory [#924](https://github.com/sandboxie-plus/Sandboxie/issues/924)
- when terminated boxed processes now we first try doing that by terminating the job object
- the driver now by default can terminate problematic processes without the help of the service
- box delete routine now retries up to 10 times to fix [#954](https://github.com/sandboxie-plus/Sandboxie/issues/954)
- Replaced the Process List used by the service with a much faster Hash Map implementation
- Replaced the per-process Thread List used by the service with a much faster Hash Map implementation

### Fixed
- fixed faulty initialization in SetServiceStatus (by flamencist) [#921](https://github.com/sandboxie-plus/Sandboxie/issues/921)
- fixed buttons position in Classic UI settings (by isaak654) [#914](https://github.com/sandboxie-plus/Sandboxie/issues/914)
- fixed missing password length check in the Sandman UI [#925](https://github.com/sandboxie-plus/Sandboxie/issues/925)
- fixed issues opening job objects by name
- fixed missing permission check when reopening job object handles (thanks Diversenok)
- fixed issue with some Chromium 90+ hooks affecting PDF plugin in derived browsers [#930](https://github.com/sandboxie-plus/Sandboxie/issues/930) [#817](https://github.com/sandboxie-plus/Sandboxie/issues/817)
- fixed isues with reconnecting broken LPC ports used for communication with SbieSvc
- fixed minor setting issue [#957](https://github.com/sandboxie-plus/Sandboxie/issues/957)
- fixed minor UI issue with resource access COM settings [#958](https://github.com/sandboxie-plus/Sandboxie/issues/958)
- fixed an issue with NtQueryKey using NtQueryObject instead [#951](https://github.com/sandboxie-plus/Sandboxie/issues/951)
- fixed crash in key.c when failing to resolve key paths
- added workaround for topmost modality issue [#873](https://github.com/sandboxie-plus/Sandboxie/issues/873)
-- the notification window is not only topmost for 5 seconds
- fixed an issue deleting directories introduced in 5.49.5
- fixed an issue with box copies

### Removed
- removed switch for "BlockPassword=n" as it does not seem to be working [#938](https://github.com/sandboxie-plus/Sandboxie/issues/938)
-- it's recommended to use "OpenSamEndpoint=y" to allow for password change in windows 10



## [0.8.2 / 5.50.2] - 2021-06-15

### Changed
- Split anti-phishing rules per browser (by isaak654) [#910](https://github.com/sandboxie-plus/Sandboxie/pull/910)

### Fixed
- properly fixed an issue with Driver Verifier and user handles [#906](https://github.com/sandboxie-plus/Sandboxie/issues/906)
- fixed an issue with CreateWindow function introduced with 0.8.0
- fixed issue with outdated BoxDisplayOrder entries being retained [#900](https://github.com/sandboxie-plus/Sandboxie/issues/900)



## [0.8.1 / 5.50.1] - 2021-06-14

### Fixed
- fixed an issue with Driver Verifier and user handles
- fixed driver memory leak of FLT_FILE_NAME_INFORMATION objects
- fixed broken clipboard introduced in 5.50.0 [#899](https://github.com/sandboxie-plus/Sandboxie/issues/899)
- fixed DcomLaunch issue on Windows 7 32 bit introduced in 5.50.0 [#898](https://github.com/sandboxie-plus/Sandboxie/issues/898)



## [0.8.0 / 5.50.0] - 2021-06-13

### Added
- Sandboxie now applies by default "Close...=!<program>,..." directives to non-excluded images if they are located in a sandbox
-- added 'AlwaysCloseForBoxed=n' to disable this behaviour as it may not be always desired, and it doesn't provide extra security
- added process image information to Sandman UI
- localized template categories in the Plus UI [#727](https://github.com/sandboxie-plus/Sandboxie/issues/727)
- added "DisableResourceMonitor=y" to disable resource access monitor for selected boxes [#886](https://github.com/sandboxie-plus/Sandboxie/issues/886)
- added option to show trace entries only for the selected sandbox [#886](https://github.com/sandboxie-plus/Sandboxie/issues/886)
- added "UseVolumeSerialNumbers=y" that allows drive letters to be suffixed with the volume SN in the \drive\ sandbox location
-- it helps to avoid files mixed together on multiple pendrives using the same letter
-- note: this option is not compatible with the recovery function of the Classic UI, only SandMan UI is fully compatible
- added "ForceRestart=PicoTorrent.exe" to the PicoTorrent template in order to fix a compatibility issue [#720](https://github.com/sandboxie-plus/Sandboxie/issues/720)
- added localization support for RPC templates (by isaak654) [#736](https://github.com/sandboxie-plus/Sandboxie/issues/736)

### Changed
- portable cleanup message now has yes/no/cancel options [#874](https://github.com/sandboxie-plus/Sandboxie/issues/874)
- consolidated Proc_CreateProcessInternalW and Proc_CreateProcessInternalW_RS5 to remove duplicate code
- the ElevateCreateProcess fix, as sometimes applied by the Program Compatibility Assistant, will no longer be emulated by default [#858](https://github.com/sandboxie-plus/Sandboxie/issues/858)
-- use 'ApplyElevateCreateProcessFix=y' or 'ApplyElevateCreateProcessFix=program.exe,y' to enable it
- trace log gets disabled only when it has no entries and the logging is stopped

### Fixed
- fixed APC issue with the new global hook emulation mechanism and WoW64 processes [#780](https://github.com/sandboxie-plus/Sandboxie/issues/780) [#779](https://github.com/sandboxie-plus/Sandboxie/issues/779)
- fixed IPv6 issues with BlockPort options
- fixed an issue with CheatEngine when "OpenWinClass=*" was specified [#786](https://github.com/sandboxie-plus/Sandboxie/issues/786)
- fixed memory corruption in SbieDrv [#838](https://github.com/sandboxie-plus/Sandboxie/issues/838)
- fixed crash issue with process elevation on CreateProcess calls [#858](https://github.com/sandboxie-plus/Sandboxie/issues/858)
- fixed process elevation when running in the built-in administrator account [#3](https://github.com/sandboxie-plus/Sandboxie/issues/3)
- fixed template preview resetting unsaved entries in box options window [#621](https://github.com/sandboxie-plus/Sandboxie/issues/621)



## [0.7.5 / 5.49.8] - 2021-06-05

### Added
- clipboard access for a sandbox can now be disabled with "OpenClipboard=n" [#794](https://github.com/sandboxie-plus/Sandboxie/issues/794)

### Changed
- now the OpenBluetooth template is enabled by default for compatibility with Unity games [#799](https://github.com/sandboxie-plus/Sandboxie/issues/799)
- "PreferExternalManifest=program.exe,y" can now be set on a per-process basis

### Fixed
- fixed compiler issues with the most recent VS2019 update
- fixed issue with Vivaldi browser [#821](https://github.com/sandboxie-plus/Sandboxie/issues/821)
- fixed some issues with box options in the Plus UI [#879](https://github.com/sandboxie-plus/Sandboxie/issues/879)
- fixed some issues with hardware acceleration in Chromium based browsers [#795](https://github.com/sandboxie-plus/Sandboxie/issues/795)
- the "Stop All" command now issues "kmdutill scandll" first to solve issues when the SbieDll.dll is in use
- workaround for Electron apps, by forcing an additional command line argument on the GPU renderer process [#547](https://github.com/sandboxie-plus/Sandboxie/issues/547) [#310](https://github.com/sandboxie-plus/Sandboxie/issues/310) [#215](https://github.com/sandboxie-plus/Sandboxie/issues/215)
- fixed issue with Software Compatibility tab that doesn't always show template names correctly [#774](https://github.com/sandboxie-plus/Sandboxie/issues/774)


## [0.7.4 / 5.49.7] - 2021-04-11

### Added
- added option to disable file migration prompt in the Plus UI with PromptForFileMigration=n [#643](https://github.com/sandboxie-plus/Sandboxie/issues/643)
- added UI options for various security isolation features
- added missing functionality to set template values in the Plus UI
- added templates for Popcorn-Time, Clementine Music Player, Strawberry Music Player, 32-bit MPC-HC (by Dyras) [#726](https://github.com/sandboxie-plus/Sandboxie/pull/726) [#737](https://github.com/sandboxie-plus/Sandboxie/pull/737)

### Changed
- align default settings of AutoRecover and Favourites to the Plus version (thanks isaak654) [#747](https://github.com/sandboxie-plus/Sandboxie/pull/747)
- list of email clients and browsers is now centralized in Dll_GetImageType
- localstore.rdf reference in Templates.ini was replaced with xulstore.json (by isaak654) [#751](https://github.com/sandboxie-plus/Sandboxie/pull/751)

### Fixed
- fixed minor issue with logging internet blocks
- fixed issue with file recovery when located on a network share [#711](https://github.com/sandboxie-plus/Sandboxie/issues/711)
- fixed UI issue with CallTrace [#769](https://github.com/sandboxie-plus/Sandboxie/issues/769)
- fixed sandbox shortcuts receiving double extension upon creation [#770](https://github.com/sandboxie-plus/Sandboxie/issues/770)
- fixed misplaced labels in the Classic UI (thanks isaak654) [#759](https://github.com/sandboxie-plus/Sandboxie/pull/759)
- fixed separator line in SbieCtrl (thanks isaak654) [#761](https://github.com/sandboxie-plus/Sandboxie/pull/761)
- fixed broken paths in The Bat! template (by isaak654) [#756](https://github.com/sandboxie-plus/Sandboxie/pull/756)
- fixed issue about media players that attempt to write unneeded media files inside the box (by Dyras) [#743](https://github.com/sandboxie-plus/Sandboxie/pull/743) [#536](https://github.com/sandboxie-plus/Sandboxie/issues/536)


## [0.7.3 / 5.49.5] - 2021-03-27

### Added
- added "UseSbieWndStation=y" to emulate CreateDesktop for selected processes, not only Firefox and Chrome [#635](https://github.com/sandboxie-plus/Sandboxie/issues/635)
- added option to drop the console host process integrity, now you can use "DropConHostIntegrity=y" [#678](https://github.com/sandboxie-plus/Sandboxie/issues/678)
- added option to easily add local templates
- added new torrent clients and media players templates (by Dyras) [#719](https://github.com/sandboxie-plus/Sandboxie/pull/719)

### Changed
- reworked window hooking mechanism to improve performance [#697](https://github.com/sandboxie-plus/Sandboxie/issues/697) [#519](https://github.com/sandboxie-plus/Sandboxie/issues/519) [#662](https://github.com/sandboxie-plus/Sandboxie/issues/662) [#69](https://github.com/sandboxie-plus/Sandboxie/issues/69) [#109](https://github.com/sandboxie-plus/Sandboxie/issues/109) [#193](https://github.com/sandboxie-plus/Sandboxie/issues/193)
-- resolves issues with file save dialogues taking 30+ seconds to open
-- this fix greatly improves the win32 GUI performance of sandboxed processes
- reworked RPC resolver to be ini-configurable
-- the following options are now deprecated:
--- "UseRpcMgmtSetComTimeout=some.dll,n", so use "RpcPortBinding=some.dll,*,TimeOut=y"
--- "OpenUPnP=y", "OpenBluetooth=y", "OpenSmartCard=n", so use the new RPC templates instead
-- See Templates.ini for usage examples

### Fixed
- fixed process-specific hooks being applied to all processes in a given sandbox
- fixed issue with messages and templates sometimes not being properly displayed in the SandMan UI
- fixed issue with compatibility settings not being applied properly
- fixed auto delete issue that got introduced with 0.7.1 [#637](https://github.com/sandboxie-plus/Sandboxie/issues/637)
- fixed issue with NtSetInformationFile, FileDispositionInformation resulting in Opera installer failing
- fixed issue with MacType introduced in the 0.7.2 build [#676](https://github.com/sandboxie-plus/Sandboxie/issues/676)
- fixed global sandboxed windows hooks not working when window rename option is disabled
- fixed issue with saving local templates
- fixed issue when using runas to start a process that was created outside of the Sandboxie supervision [#688](https://github.com/sandboxie-plus/Sandboxie/issues/688)
-- since the runas facility is not accessible by default, this did not constitute a security issue
-- to enable runas functionality, add "OpenIpcPath=\RPC Control\SECLOGON" to your Sandboxie.ini
-- please take note that doing so may open other yet unknown issues
- fixed a driver compatibility issue with Windows 10 32 bit Insider Preview Build 21337
- fixed issues with driver signature for Windows 7



## [0.7.2 / 5.49.0] - 2021-03-04

### Added
- added option to alter reported Windows version "OverrideOsBuild=7601" for Windows 7 SP1 [#605](https://github.com/sandboxie-plus/Sandboxie/issues/605)
- the trace log can now be structured like a tree with processes as root items and threads as branches

### Changed
- SandboxieCrypto now always migrates the CatRoot2 files in order to prevent locking of real files
- greatly improved trace log performance
- MSI Server can now run with the "FakeAdminRights=y" and "DropAdminRights=y" options [#600](https://github.com/sandboxie-plus/Sandboxie/issues/600)
-- special service allowance for the MSI Server can be disabled with "MsiInstallerExemptions=n"
- changed SCM access check behaviour; non elevated users can now start services with a user token
-- elevation is now only required to start services with a system token
- reworked the trace log mechanism to be more verbose
- reworked RPC mechanism to be more flexible

### Fixed
- fixed issues with some installers introduced in 5.48.0 [#595](https://github.com/sandboxie-plus/Sandboxie/issues/595)
- fixed "add user to sandbox" in the Plus UI [#597](https://github.com/sandboxie-plus/Sandboxie/issues/597)
- FIXED SECURITY ISSUE: the HostInjectDll mechanism allowed for local privilege escalation (thanks hg421)
- Classic UI no longer allows to create a sandbox with an invalid or reserved device name [#649](https://github.com/sandboxie-plus/Sandboxie/issues/649)



## [0.7.1 / 5.48.5] - 2021-02-21

### Added
- enhanced RpcMgmtSetComTimeout handling with "UseRpcMgmtSetComTimeout=some.dll,n"
-- this option allows to specify if RpcMgmtSetComTimeout should be used or not for each individual dll
-- this setting takes precedence over hard-coded and per-process presets
-- "UseRpcMgmtSetComTimeout=some.dll" and "UseRpcMgmtSetComTimeout=some.dll,y" are equivalent
- added "FakeAdminRights=y" option that makes processes think they have admin permissions in a given box
-- this option is recommended to be used in combination with "DropAdminRights=y" to improve security
-- with "FakeAdminRights=y" and "DropAdminRights=y" installers should still work
- added RPC support for SSDP API (the Simple Service Discovery Protocol), you can enable it with "OpenUPnP=y"


### Changed
- SbieCrypto no longer triggers message 1313
- changed enum process API; now more than 511 processes per box can be enumerated (no limit)
- reorganized box settings a bit
- made COM tracing more verbose
- "RpcMgmtSetComTimeout=y" is now again the default behaviour, it seems to cause less issues overall

### Fixed
- fixed issues with webcam access when the DevCMApi filtering is in place
- fixed issue with free download manager for 'AppXDeploymentClient.dll', so RpcMgmtSetComTimeout=y will be used by default for this one [#573](https://github.com/sandboxie-plus/Sandboxie/issues/573)
- fixed not all WinRM files were blocked by the driver, with "BlockWinRM=n" this file block can be disabled
- fixed Sandboxie Classic crash when saving any option in Sandbox Settings -> Appearance (by typpos) [#586](https://github.com/sandboxie-plus/Sandboxie/issues/586)



## [0.7.0 / 5.48.0] - 2021-02-14

### Added
- sandboxed indicator for tray icons, the tooltip now contains [#] if enabled
- the trace log buffer can now be adjusted with "TraceBufferPages=2560"
-- the value denotes the count of 4K large pages to be used; here for a total of 10 MB
- new functionality for the list finder

### Changed
- improved RPC debugging
- improved IPC handling around RpcMgmtSetComTimeout; "RpcMgmtSetComTimeout=n" is now the default behaviour
-- required exceptions have been hard-coded for specific calling DLLs
- the LogApi dll is now using Sbie's tracing facility to log events instead of its own pipe server

### Fixed
- FIXED SECURITY ISSUE: elevated sandboxed processes could access volumes/disks for reading (thanks hg421)
-- this protection option can be disabled by using "AllowRawDiskRead=y"
- fixed crash issue around SetCurrentProcessExplicitAppUserModelID observed with GoogleUpdate.exe
- fixed issue with Resource Monitor sort by timestamp
- fixed invalid Opera bookmarks path (by isaak654) [#542](https://github.com/sandboxie-plus/Sandboxie/pull/542)
- FIXED SECURITY ISSUE: a race condition in the driver allowed to obtain an elevated rights handle to a process (thanks typpos) [#549](https://github.com/sandboxie-plus/Sandboxie/pull/549)
- FIXED SECURITY ISSUE: "\RPC Control\samss lpc" is now filtered by the driver (thanks hg421) [#553](https://github.com/sandboxie-plus/Sandboxie/issues/553)
-- this allowed elevated processes to change passwords, delete users and alike; to disable filtering use "OpenSamEndpoint=y"
- FIXED SECURITY ISSUE: "\Device\DeviceApi\CMApi" is now filtered by the driver (thanks hg421) [#552](https://github.com/sandboxie-plus/Sandboxie/issues/552)
-- this allowed elevated processes to change hardware configuration; to disable filtering use "OpenDevCMApi=y"



## [0.6.7 / 5.47.1] - 2021-02-01

### Added
- added UI language auto-detection

### Fixed
- fixed Brave.exe now being properly recognized as Chrome-, not Firefox-based 
- fixed issue introduced in 0.6.5 with recent Edge builds
-- the 0.6.5 behaviour can be set on a per-process basis using "RpcMgmtSetComTimeout=POPPeeper.exe,n"
- fixed grouping issues [#445](https://github.com/sandboxie-plus/Sandboxie/issues/445)
- fixed main window restore state from tray [#288](https://github.com/sandboxie-plus/Sandboxie/issues/288)



## [0.6.5 / 5.47.0] - 2021-01-31

### Added
- added detection for Waterfox.exe, Palemoon.exe and Basilisk.exe Firefox forks as well as Brave.exe [#468](https://github.com/sandboxie-plus/Sandboxie/issues/468)
- added Bluetooth API support, IPC port can be opened with "OpenBluetooth=y" [#319](https://github.com/sandboxie-plus/Sandboxie/issues/319)
-- this should resolve issues with many Unity games hanging on startup for a long time
- added enhanced RPC/IPC interface tracing
- when DefaultBox is not found by the SandMan UI, it will be recreated
- "Disable Forced Programs" time is now saved and reloaded

### Changed
- reduced SandMan CPU usage
- Sandboxie.ini and Templates.ini can now be UTF8 encoded [#461](https://github.com/sandboxie-plus/Sandboxie/issues/461) [#197](https://github.com/sandboxie-plus/Sandboxie/issues/197)
-- this feature is experimental, files without a UTF-8 Signature should be recognized also
-- "ByteOrderMark=yes" is obsolete, Sandboxie.ini is now always saved with a BOM/Signature
- legacy language files can now be UTF8 encoded
- reworked file migration behaviour, removed hardcoded lists in favour of templates [#441](https://github.com/sandboxie-plus/Sandboxie/issues/441)
-- you can now use "CopyAlways=", "DontCopy=" and "CopyEmpty=" that support the same syntax as "OpenFilePath="
-- "CopyBlockDenyWrite=program.exe,y" makes a write open call to a file that won't be copied fail instead of turning it read-only
- removed hardcoded SkipHook list in favour of templates

### Fixed
- fixed old memory pool leak in the Sbie driver [#444](https://github.com/sandboxie-plus/Sandboxie/issues/444)
- fixed issue with item selection in the access restrictions UI
- fixed updater crash in Sbiectrl.exe [#450](https://github.com/sandboxie-plus/Sandboxie/issues/450)
- fixed issues with RPC calls introduced in Sbie 5.33.1
- fixed recently broken 'terminate all' command
- fixed a couple minor UI issues with SandMan UI
- fixed IPC issue with Windows 7 and 8 resulting in process termination
- fixed "recover to" functionality



## [0.6.0 / 5.46.5] - 2021-01-25

### Added
- added confirmation prompts to terminate all commands
- added window title to boxed process info [#360](https://github.com/sandboxie-plus/Sandboxie/issues/360)
- added WinSpy based sandboxed window finder [#351](https://github.com/sandboxie-plus/Sandboxie/issues/351)
- added option to view disabled boxes and double click on box to enable it

### Changed
- "Reset Columns" now resizes them to fit the content, and it can now be localized [#426](https://github.com/sandboxie-plus/Sandboxie/issues/426)
- modal windows are now centered to the parent [#417](https://github.com/sandboxie-plus/Sandboxie/issues/417)
- improved new box window [#417](https://github.com/sandboxie-plus/Sandboxie/issues/417)

### Fixed
- fixed issues with window modality [#409](https://github.com/sandboxie-plus/Sandboxie/issues/409)
- fixed issues when main window was set to be always on top [#417](https://github.com/sandboxie-plus/Sandboxie/issues/417)
- fixed a driver issue with Windows 10 insider build 21286
- fixed issues with snapshot dialog [#416](https://github.com/sandboxie-plus/Sandboxie/issues/416)
- fixed an issue when writing to a path that already exists in the snapshot but not outside [#415](https://github.com/sandboxie-plus/Sandboxie/issues/415)



## [0.5.5 / 5.46.4] - 2021-01-17

### Added
- added "SandboxService=..." to force selected services to be started in the sandbox
- added template clean-up functionality to Plus UI
- added internet prompt to now also allow internet access permanently
- added browse button for box root folder in the SandMan UI [#382](https://github.com/sandboxie-plus/Sandboxie/issues/382)
- added explorer info message [#352](https://github.com/sandboxie-plus/Sandboxie/issues/352)
- added option to keep the SandMan UI always on top
- allow drag and drop file onto Sandman.exe to run it sandboxed [#355](https://github.com/sandboxie-plus/Sandboxie/issues/355)
- added start SandMan UI when a sandboxed application starts [#367](https://github.com/sandboxie-plus/Sandboxie/issues/367)
- recovery window can now list all files
- added file counter to recovery window
- when "NoAddProcessToJob=y" is specified, Chrome and related browsers now can fully use the job system
-- Note: "NoAddProcessToJob=y" reduces the box isolation, but the affected functions are mostly covered by UIPI anyway
- added optimized default column widths to Sbie view
- added template support for Yandex and Ungoogled Chromium browsers (by isaak654)

### Changed
- updated templates with multiple browsers fixes (thanks isaak654)
- when trying to take a snapshot of an empty sandbox a proper error message is displayed [#381](https://github.com/sandboxie-plus/Sandboxie/issues/381)
- new layout for the recovery window
- Sbie view sorting is now case insensitive

### Fixed
- fixed issue child window closing terminating application when main was hidden [#349](https://github.com/sandboxie-plus/Sandboxie/issues/349)
- fixed issues with non modal windows [#349](https://github.com/sandboxie-plus/Sandboxie/issues/349)
- fixed issues connecting to driver in portable mode
- fixed minor issues with snapshot window
- fixed missing error message when attempting to create an already existing sandbox [#359](https://github.com/sandboxie-plus/Sandboxie/issues/359)
- fixed issue allowing to save setting when a sandbox was already deleted [#359](https://github.com/sandboxie-plus/Sandboxie/issues/359)
- fixed issues with disabled items in dark mode [#359](https://github.com/sandboxie-plus/Sandboxie/issues/359)
- fixed some dialogues not closing when pressing Esc [#359](https://github.com/sandboxie-plus/Sandboxie/issues/359)
- fixed tab stops on many windows



## [0.5.4d / 5.46.3] - 2021-01-11

### Changed
- improved access tracing, removed redundant entries
- OpenIpcPath=\BaseNamedObjects\[CoreUI]-* is now hardcoded in the driver no need for the template entry 
- WindowsFontCache is now open by default
- refactored some IPC code in the driver

### Fixed
- FIXED SECURITY ISSUE: the registry isolation could be bypassed, present since Windows 10 Creators Update
- fixed creation time not always being properly updated in the SandMan UI



## [0.5.4c / 5.46.2] - 2021-01-10

### Added
- added "CallTrace=*" to log all system calls to the access log

### Changed
- improved IPC logging code
- improved MSG_2101 logging

### Fixed
- fixed more issues with IPC tracing
- fixed SBIE2101 issue with Chrome and derivatives



## [0.5.4b / 5.46.1] - 2021-01-08

### Added
- added "RunServiceAsSystem=..." allows specific named services to be run as system

### Changed
- refactored some code around SCM access

### Fixed
- fixed a crash issue in SbieSvc.exe introduced with the last build
- fixed issue with SandMan UI update check
- FIXED SECURITY ISSUE: a Sandboxed process could start sandboxed as system even with DropAdminRights in place

### Removed
- removed "ProtectRpcSs=y" due to incompatibility with new isolation defaults



## [0.5.4 / 5.46.0] - 2021-01-06

### Added
- FIXED SECURITY ISSUE: Sandboxie now strips particularly problematic privileges from sandboxed system tokens
-- with those a process could attempt to bypass the sandbox isolation (thanks Diversenok)
-- old legacy behaviour can be enabled with "StripSystemPrivileges=n" (absolutely NOT Recommended) 
- added new isolation options "ClosePrintSpooler=y" and "OpenSmartCard=n" 
-- those resources are open by default, but for a hardened box it is desired to close them
- FIXED SECURITY ISSUE: added print spooler filter to prevent printers from being set up outside the sandbox
-- the filter can be disabled with "OpenPrintSpooler=y"
- added overwrite prompt when recovering an already existing file
- added "StartProgram=", "StartService=" and "AutoExec=" options to the SandMan UI
- added more compatibility templates (thanks isaak654) [#294](https://github.com/sandboxie-plus/Sandboxie/pull/294)

### Changed
- Changed Emulated SCM behaviour, boxed services are no longer by default started as boxed system
-- use "RunServicesAsSystem=y" to enable the old legacy behaviour
-- Note: sandboxed services with a system token are still sandboxed and restricted 
-- However not granting them a system token in the first place removes possible exploit vectors
-- Note: this option is not compatible with "ProtectRpcSs=y" and takes precedence!
- reworked dynamic IPC port handling
- improved Resource Monitor status strings

### Fixed
- FIXED SECURITY ISSUE: processes could spawn processes outside the sandbox (thanks Diversenok)
- FIXED SECURITY ISSUE: bug in the dynamic IPC port handling allowed to bypass IPC isolation
- fixed issue with IPC tracing
- FIXED SECURITY ISSUE: CVE-2019-13502 "\RPC Control\LSARPC_ENDPOINT" is now filtered by the driver (thanks Diversenok)
-- this allowed some system options to be changed, to disable filtering use "OpenLsaEndpoint=y"
- fixed hooking issues SBIE2303 with Chrome, Edge and possibly others [#68](https://github.com/sandboxie-plus/Sandboxie/issues/68) [#166](https://github.com/sandboxie-plus/Sandboxie/issues/166)
- fixed failed check for running processes when performing snapshot operations
- fixed some box option checkboxes were not properly initialized
- fixed unavailable options are not properly disabled when SandMan is not connected to the driver
- fixed MSI installer issue, not being able to create "C:\Config.msi" folder on Windows 20H2 [#219](https://github.com/sandboxie-plus/Sandboxie/issues/219)
- added missing localization to generic list commands
- fixed issue with "iconcache_*" when running sandboxed explorer
- fixed more issues with groups



## [0.5.3b / 5.45.2] - 2021-01-02

### Added
- added settings for the portable boxed root folder option
- added process name to resource log
- added command line column to the process view in the SandMan UI

### Fixed
- fixed a few issues with group handling [#262](https://github.com/sandboxie-plus/Sandboxie/issues/262) 
- fixed issue with GetRawInputDeviceInfo when running a 32 bit program on a 64 bit system
- fixed issue when pressing apply in the "Resource Access" tab; the last edited value was not always applied
- fixed issue merging entries in Resource Access Monitor



## [0.5.3a / 5.45.2] - 2020-12-29

### Added
- added prompt to choose if links in the SandMan UI should be opened in a sandboxed or unsandboxed browser [#273](https://github.com/sandboxie-plus/Sandboxie/issues/273)
- added more recovery options
- added "ClosedClsid=" to block COM objects from being used when they cause compatibility issues
- added "ClsidTrace=*" option to trace COM usage
- added "ClosedRT=" option to block access to problematic Windows RT interfaces
- added option to make a link for any selected process to SandMan UI
- added option to reset all hidden messages
- added more process presets "force program" and "allow internet access"
- added "SpecialImage=chrome,some_electron_app.exe" option to Sandboxie.ini, valid image types "chrome", "firefox"
-- with this option you can enable special hardcoded workarounds to new obscure forks of those browsers
- added German translation (thanks bastik-1001) to the SandMan UI
- added Russian translation (thanks lufog) to the SandMan UI
- added Portuguese translation (thanks JNylson ) to the SandMan UI

### Changed
- changed docs and update URLs to the new sandboxie-plus.com domain
- greatly improved the setup script (thanks mpheath)
- "OpenClsid=" and "ClosedClsid=" now support specifying a program or group name
- by default, when started in portable mode, the sandbox folder will be located in the parent directory of the Sandboxie instance

### Fixed
- grouping menu not fully working in the new SandMan UI [#277](https://github.com/sandboxie-plus/Sandboxie/issues/277)
- fixed not being able to set quick recovery in SandMan UI
- fixed resource leak when loading process icons in SandMan UI
- fixed issue with OpenToken debug options
- fixed Chrome crashing on websites that cause the invocation of "FindAppUriHandlersAsync" [#198](https://github.com/sandboxie-plus/Sandboxie/issues/198)
- fixed issue connecting to the driver when starting in portable mode
- fixed missing template setup when creating new boxes

### removed
- removed obsolete "OpenDefaultClsid=n" use "ClosedClsid=" with the appropriate values instead
- removed suspend/resume menu entry, pooling that state wastes substantial CPU cycles; use task explorer for that functionality



## [0.5.2a / 5.45.1] - 2020-12-23

### Fixed
- fixed translation support in the SandMan UI
- fixed sandboxed explorer issue [#289](https://github.com/sandboxie-plus/Sandboxie/issues/289)
- fixed simplified Chinese localization



## [0.5.2 / 5.45.1] - 2020-12-23

### Added
- added advanced new box creation dialogue to SandMan UI
- added show/hide tray context menu entry
- added refresh button to file recovery dialogue
- added mechanism to load icons from {install-dir}/Icons/{icon}.png for UI customization
- added tray indicator to show disabled forced program status in the SandMan UI
- added program name suggestions to box options in SandMan UI
- added saving of column sizes in the options window

### Changed
- reorganized the advanced box options a bit
- changed icons (thanks Valinwolf for picking the new ones) [#235](https://github.com/sandboxie-plus/Sandboxie/issues/235)
- updated Templates.ini (thanks isaak654) [#256](https://github.com/sandboxie-plus/Sandboxie/pull/256) [#258](https://github.com/sandboxie-plus/Sandboxie/pull/258)
- increased max value for disable forced process time in SandMan UI

### Fixed
- fixed BSOD introduced in 5.45.0 when using Windows 10 "core isolation" [#221](https://github.com/sandboxie-plus/Sandboxie/issues/221)
- fixed minor issue with lingering/leader processes
- fixed menu issue in SandMan UI
- fixed issue with stop behaviour page in SandMan UI
- fixed issue with Plus installer not displaying kmdutil window
- fixed SandMan UI saving UI settings on Windows shutdown
- fixed issue with Plus installer autorun [#247](https://github.com/sandboxie-plus/Sandboxie/issues/247)
- fixed issue with legacy installer not removing all files
- fixed a driver compatibility issue with Windows 20H1 and later [#228](https://github.com/sandboxie-plus/Sandboxie/issues/228)
-- this solves "stop pending", LINE messenger hanging and other issues...
- fixed quick recovery issue in SbieCtrl.exe introduced in 5.45.0 [#224](https://github.com/sandboxie-plus/Sandboxie/issues/224)
- fixed issue advanced hide process settings not saving
- fixed some typos in the UI (thanks isaak654) [#252](https://github.com/sandboxie-plus/Sandboxie/pull/252) [#253](https://github.com/sandboxie-plus/Sandboxie/pull/253) [#254](https://github.com/sandboxie-plus/Sandboxie/pull/254)
- fixed issue with GetRawInputDeviceInfo failing when boxed processes are put in a job object [#176](https://github.com/sandboxie-plus/Sandboxie/issues/176) [#233](https://github.com/sandboxie-plus/Sandboxie/issues/233)
-- this fix resolves issues with CP2077 and other games not getting keyboard input (thanks Rostok)
- fixed failing ClipCursor won't longer span the message log
- fixed issue with adding recovery folders in SandMan UI
- fixed issue with Office 2019 template when using a non-default Sbie install location
- fixed issue setting last access attribute on sandboxed folders [#218](https://github.com/sandboxie-plus/Sandboxie/issues/218)
- fixed issue with process start signal



## [0.5.1 / 5.45.0] - 2020-12-12

### Added
- added simple view mode

### Changed
- updated SandMan UI to use Qt5.15.1

### Fixed
- fixed crash issue with progress dialogue
- fixed progress dialogue cancel button not working for update checker
- fixed issue around NtQueryDirectoryFile when deleting sandbox content
- fixed dark theme in the notification window
- fixed issue with disable force programs tray menu



## [0.5.0 / 5.45.0] - 2020-12-06

### Added
- added new notification window
- added user interactive control mechanism when using the new SandMan UI
-- when a file exceeds the copy limit instead of failing, the user is prompted if the file should be copied or not
-- when internet access is blocked it now can be exempted in real time by the user
- added missing file recovery and auto/quick recovery functionality [#188](https://github.com/sandboxie-plus/Sandboxie/issues/188) [#178](https://github.com/sandboxie-plus/Sandboxie/issues/178)
- added silent MSG_1399 boxed process start notification to keep track of short lived boxed processes
- added ability to prevent system wide process starts, Sandboxie can now instead of just alerting also block processed on the alert list
-- set "StartRunAlertDenied=y" to enable process blocking
- the process start alert/block mechanism can now also handle folders use "AlertFolder=..." 
- added ability to merge snapshots [#151](https://github.com/sandboxie-plus/Sandboxie/issues/151)
- added icons to the sandbox context menu in the new UI
- added more advanced options to the sandbox options window
- added file migration progress indicator
- added more run commands and custom run commands per sandbox
-- the box settings users can now specify programs to be available from the box run menu
-- also processes can be pinned to that list from the presets menu
- added more Windows 10 specific template presets
- added ability to create desktop shortcuts to sandboxed items
- added icons to box option tabs
- added box grouping
- added new debug option "DebugTrace=y" to log debug output to the trace log
- added check for updates to the new SandMan UI
- added check for updates to the legacy SbieCtrl UI

### Changed
- File migration limit can now be disabled by specifying "CopyLimitKb=-1" [#526](https://github.com/sandboxie-plus/Sandboxie/issues/526)
- improved and refactored message logging mechanism, reducing memory usage by factor of 2
- terminated boxed processes are now kept listed for a couple of seconds
- reworked sandbox deletion mechanism of the new UI
- restructured sandbox options window
- SbieDLL.dll can now be compiled with an up to date ntdll.lib (Thanks to TechLord from Team-IRA for help)
- improved automated driver self repair

### Fixed
- fixed issues migrating files > 4GB
- fixed an issue that would allow a malicious application to bypass the internet blockade
- fixed issue when logging messages from a non-sandboxed process, added process_id parameter to API_LOG_MESSAGE_ARGS
- fixed issues with localization
- fixed issue using file recovery in legacy UI SbieCtrl.exe when "SeparateUserFolders=n" is set
- when a program is blocked from starting due to restrictions no redundant messages are issues anymore
- fixed UI not properly displaying async errors
- fixed issues when a snapshot operation failed
- fixed some special cases of IpcPath and WinClass in the new UI
- fixed driver issues with WHQL passing compatibility testing
- fixed issues with classical installer



## [0.4.5 / 5.44.1] - 2020-11-16

### Added
- added "Terminate all processes" and "disable forced programs" commands to tray menu in SandMan UI
- program start restrictions settings now can be switched between a white list and a black list
-- programs can be terminated and blacklisted from the context menu
- added additional process context menu options, lingering and leader process can be now set from menu
- added option to view template presets for any given box
- added text filter to template view
- added new compatibility templates:
-- Windows 10 core UI component: OpenIpcPath=\BaseNamedObjects\[CoreUI]-* solving issues with Chinese Input and Emojis [#120](https://github.com/sandboxie-plus/Sandboxie/issues/120) [#88](https://github.com/sandboxie-plus/Sandboxie/issues/88)
-- Firefox Quantum, access to Windows’ FontCachePort for compatibility with Windows 7
- added experimental debug option "OriginalToken=y" which lets sandboxed processes retain their original unrestricted token
-- This option is comparable with "OpenToken=y" and is intended only for testing and debugging, it BREAKS most SECURITY guarantees (!)
- added debug option "NoSandboxieDesktop=y" it disables the desktop proxy mechanism
-- Note: without an unrestricted token with this option applications won't be able to start
- added debug option "NoSysCallHooks=y" it disables the sys call processing by the driver
-- Note: without an unrestricted token with this option applications won't be able to start
- added ability to record verbose access traces to the Resource Monitor
-- use ini options "FileTrace=*", "PipeTrace=*", "KeyTrace=*", "IpcTrace=*", "GuiTrace=*" to record all events
-- replace "*" to log only: "A" - allowed, "D" - denied, or "I" - ignore events
- added ability to record debug output strings to the Resource Monitor
-- use ini option DebugTrace=y to enable

### Changed
- AppUserModelID sting no longer contains Sandboxie version string
- now by default Sbie's application manifest hack is disabled, as it causes problems with version checking on Windows 10
-- to enable old behaviour add "PreferExternalManifest=y" to the global or the box specific ini section
- the resource log mechanism can now handle multiple strings to reduce on string copy operations

### Fixed
- fixed issue with disabling some restriction settings failed
- fixed disabling of internet block from the presets menu sometimes failed
- the software compatibility list in the SandMan UI now shows the proper template names
- fixed use of freed memory in the driver
- replaced swprintf with snwprintf to prevent potential buffer overflow in SbieDll.dll
- fixed bad list performance with resource log and API log in SandMan UI



## [0.4.4 / 5.44.0] - 2020-11-03

### Added
- added SbieLdr (experimental)

### Changed
- moved code injection mechanism from SbieSvc to SbieDll
- moved function hooking mechanism from SbieDrv to SbieDll
- introduced a new driverless method to resolve wow64 ntdll base address

### removed
- removed support for Windows Vista x64



## [0.4.3 / 5.43.7] - 2020-11-03

### Added
- added disable forced programs menu command to the SandMan UI

### Fixed
- fixed file rename bug introduced with an earlier Driver Verifier fix [#174](https://github.com/sandboxie-plus/Sandboxie/issues/174) [#153](https://github.com/sandboxie-plus/Sandboxie/issues/153)
- fixed issue saving access lists
- fixed issue with program groups parsing in the SandMan UI 
- fixed issue with internet access restriction options [#177](https://github.com/sandboxie-plus/Sandboxie/issues/177) [#185](https://github.com/sandboxie-plus/Sandboxie/issues/185)
- fixed issue deleting sandbox when located on a drive directly [#139](https://github.com/sandboxie-plus/Sandboxie/issues/139)


## [0.4.2 / 5.43.6] - 2020-10-10

### Added
- added explore box content menu option

### Fixed
- fixed thread handle leak in SbieSvc and other components [#144](https://github.com/sandboxie-plus/Sandboxie/issues/144)
- msedge.exe is now categorized as a Chromium derivate
- fixed Chrome 86+ compatibility bug with Chrome's own sandbox [#149](https://github.com/sandboxie-plus/Sandboxie/issues/149)


## [0.4.1 / 5.43.5] - 2020-09-12

### Added
- added core version compatibility check to SandMan UI
- added shell integration options to SbiePlus

### Changed
- SbieCtrl does not longer auto show the tutorial on first start
- when hooking to the trampoline, the migrated section of the original function is no longer noped out
-- it caused issues with unity games, will be investigated and re-enabled later

### Fixed
- fixed colour issue with vertical tabs in dark mode
- fixed wrong path separators when adding new forced folders
- fixed directory listing bug introduced in 5.43
- fixed issues with settings window when not being connected to driver
- fixed issue when starting SandMan UI as admin
- fixed auto content delete not working with SandMan UI



## [0.4.0 / 5.43] - 2020-09-05

### Added
- added a proper custom installer to the Plus release
- added sandbox snapshot functionality to Sbie core
-- filesystem is saved incrementally, the snapshots built upon each other
-- each snapshot gets a full copy of the box registry for now
-- each snapshot can have multiple children snapshots
- added access status to Resource Monitor
- added setting to change border width [#113](https://github.com/sandboxie-plus/Sandboxie/issues/113)
- added snapshot manager UI to SandMan
- added template to enable authentication with an Yubikey or comparable 2FA device
- added UI for program alert
- added software compatibility options to the UI

### Changed
- SandMan UI now handles deletion of sandbox content on its own
- no longer adding redundant resource accesses as new events

### Fixed
- fixed issues when hooking functions from delay loaded libraries
- fixed issues when hooking an already hooked function 
- fixed issues with the new box settings editor

### Removed
- removes deprecated workaround in the hooking mechanism for an obsolete anti-malware product



## [0.3.5 / 5.42.1] - 2020-07-19

### Added
- added settings window
- added translation support
- added dark theme
- added auto start option
- added sandbox options
- added debug option "NoAddProcessToJob=y"

### Changed
- improved empty sandbox tray icon
- improved message parsing
- updated homepage links

### Fixed
- fixed ini issue with SandMan.exe when renaming sandboxes
- fixed ini auto reload bug introduced in the last build
- fixed issue when hooking delayed loaded libraries



## [0.3 / 5.42] - 2020-07-04

### Added
- API_QUERY_PROCESS_INFO can be now used to get the original process token of sandboxed processes
-- Note: this capability is used by TaskExplorer to allow inspecting sandbox internal tokens
- added option "KeepTokenIntegrity=y" to make the Sbie token keep its initial integrity level (debug option)
-- Note: Do NOT USE Debug Options if you don't know their security implications (!)
- added process id to log messages very useful for debugging
- added finder to resource log
- added option to hide host processes "HideHostProcess=[name]"
-- Note: Sbie hides by default processes from other boxes, this behaviour can now be controlled with "HideOtherBoxes=n"
- Sandboxed RpcSs and DcomLaunch can now be run as system with the option "ProtectRpcSs=y" however this breaks sandboxed explorer and other
- Built In Clsid whitelist can now be disabled with "OpenDefaultClsid=n"
- Processes can be now terminated with the del key, and require a confirmation
- added sandboxed window border display to SandMan.exe
- added notification for Sbie log messages
- added Sandbox Presets sub menu allowing to quickly change some settings
-- Enable/Disable API logging, logapi_dll's are now distributed with SbiePlus
-- And other: Drop admin rights; Block/Allow internet access; Block/Allow access to files on the network
- added more info to the sandbox status column
- added path column to SbieModel
- added info tooltips in SbieView

### Changed
- reworked ApiLog, added PID and PID filter
- auto config reload on in change is now delayed by 500ms to not reload multiple times on incremental changes
- Sandbox names now replace "_" with " " for display allowing to use names that are made of separated words

### Fixed
- added missing PreferExternalManifest initialization to portable mode
- FIXED SECURITY ISSUE: fixed permission issues with sandboxed system processes
-- Note: you can use "ExposeBoxedSystem=y" for the old behaviour (debug option)
- FIXED SECURITY ISSUE: fixed missing SCM access check for sandboxed services (thanks Diversenok)
-- Note: to disable the access check use "UnrestrictedSCM=y" (debug option)
- fixed missing initialization in service server that caused sandboxed programs to crash when querying service status
- fixed many bugs that caused the SbieDrv.sys to BSOD when running with Driver Verifier enabled [#57](https://github.com/sandboxie-plus/Sandboxie/issues/57)
-- 0xF6 in GetThreadTokenOwnerPid and File_Api_Rename
-- missing non optional parameter for FltGetFileNameInformation in File_PreOperation
-- 0xE3 in Key_StoreValue and Key_PreDataInject



## [0.2.2 / 5.41.2] - 2020-06-19

### Added
- added option SeparateUserFolders=n to no longer have the user profile files stored separately in the sandbox
- added SandboxieLogon=y it makes processes run under the SID of the "Sandboxie" user instead of the Anonymous user
-- Note: the global option AllowSandboxieLogon=y must be enabled, the "Sandboxie" user account must be manually created first and the driver reloaded, else process start will fail
- improved debugging around process creation errors in the driver

### Fixed
- fixed some log messages going lost after driver reload
- found a workable fix for the MSI installer issue, see Proc_CreateProcessInternalW_RS5



## [0.2.1 / 5.41.1] - 2020-06-18

### Added
- added different sandbox icons for different types
-- Red LogAPI/BSA enabled
-- more to come :D
- added progress window for async operations that take time
- added DPI awareness [#56](https://github.com/sandboxie-plus/Sandboxie/issues/56)
- the driver file is now obfuscated to avoid false positives
- additional debug options to Sandboxie.ini OpenToken=y that combines UnrestrictedToken=y and UnfilteredToken=y
-- Note: using these options weakens the sandboxing, they are intended for debugging and may be used for better application virtualization later

### Changed
- SbieDll.dll when processing InjectDll now looks in the SbieHome folder for the DLLs if the entered path starts with a backslash
-- i.e. "InjectDll=\LogAPI\i386\logapi32v.dll" or "InjectDll64=\LogAPI\amd64\logapi64v.dll"

### Fixed
- IniWatcher did not work in portable mode
- service path fix broke other services, now properly fixed, maybe
- found workaround for the MSI installer issue



## [0.2 / 5.41.0] - 2020-06-08

### Added
- IniWatcher, no more clicking reload, the ini is now reloaded automatically every time it changes
- added Maintenance menu to the Sandbox menu, allowing to install/uninstall and start/stop Sandboxie driver, service
- SandMan.exe now is packed with Sbie files and when no Sbie is installed acts as a portable installation
- added option to clean up logs

### Changed
- Sbie driver now first checks the home path for the Sbie ini before checking SystemRoot

### Fixed
- FIXED SECURITY ISSUE: sandboxed processes could obtain a write handle on non sandboxed processes (thanks Diversenok)
-- this allowed to inject code in non sandboxed processes
- fixed issue boxed services not starting when the path contained a space
- NtQueryInformationProcess now returns the proper sandboxed path for sandboxed processes



## [0.1 / 5.40.2] - 2020-06-01

### Added
- created a new Qt based UI names SandMan (Sandboxie Manager)
- Resource Monitor now shows the PID
- added basic API call log using updated BSA LogApiDll


### Changed
- reworked Resource Monitor to work with multiple event consumers
- reworked log to work with multiple event consumers



## [5.40.1] - 2020-04-10

### Added
- "Other" type for the Resource Access Monitor
-- added call to StartService to the logged Resources

### Fixed
- fixed "Windows Installer Service could not be accessed" that got introduced with Windows 1903

