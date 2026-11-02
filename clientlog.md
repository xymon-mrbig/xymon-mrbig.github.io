# Clientlog

The clientlog module produces a single big report that summarizes the state of the machine. It recreates a number of Linux command line utilities (who, ipconfig, route, netstat, etc.) on Windows, giving a snapshot of the system. It runs on every main loop and is always enabled; there is no option to disable it.

The report starts with the line `client <machine>.windows windows` and is made up of the sections listed below, in this order.

## Report sections

### [date]
The current date and time.

### [startargs]
The arguments Mr Big was started with.

* RawCommandLine - the full command line.
* ParsedArgc - the number of parsed arguments.
* ParsedArgs - each parsed argument.

### [osversion]
The Windows name, version and build number, plus any installed feature suites.

### [winuptime]
Uptime (days and hours:minutes) and the boot time. Uses the same measure as the cpu test.

### [bios]
BIOS information read from the registry key `HKEY_LOCAL_MACHINE\HARDWARE\DESCRIPTION\System\BIOS`.

### [domain]
The machine's domain information.

* PartOfDomain - True or False.
* NetBiosDomain - the NetBIOS domain name.
* UPNDomain - the UPN domain.
* FQDN - the fully qualified domain name.

### [who]
The logged-in users and their sessions.

| USERNAME | SESSIONNAME | ID | STATE | IDLE TIME | LOGON TIME |
|----------|-------------|----|-------|-----------|------------|

STATE values:

* Active
* Up
* Query
* Shadow
* DC
* Idle
* Listen
* Reset
* Down
* Init
* Unknown

### [cpuinfo]
Physical and logical processor information.

* CPU Sockets
* Cores per socket
* cores total
* SMT per core
* SMT total

### [diskinfo]
Per-disk and per-volume information: total disk size, partitioning style (MBR, GPT, Raw), and for each drive or partition the total space, free space, block size and partition type.

Device types: CD-ROM, DVD, Disk, Unknown.

Partition styles: MBR, GPT, Raw, Unknown.

MBR partition types: Unused, Extended, FAT12, FAT16, FAT32, IFS, LDM, NTFT, Valid NTFT, Unknown.

GPT partition types: Unused, Basic, EFI System, Microsoft Reserved, LDM metadata, LDM data, Microsoft Recovery, and others.

### [winmemory]
Memory usage for physical memory, the pagefile and virtual memory.

| | TOTAL | USED | FREE | MEMORY USAGE |
|---|-------|------|------|--------------|

Rows: physical memory, pagefile, virtual memory.

### [ipconfig]
The output of `ipconfig /all`.

### [winroute]
The routing table.

### [winports] / [winportsused]
The listening TCP ports and the processes that own them.

Port states: CLOSED, LISTENING, SYN_SENT, SYN_RECEIVED, ESTABLISHED, FIN_WAIT1, FIN_WAIT2, CLOSE_WAIT, CLOSING, LAST_ACK, TIME_WAIT, DELETE_TCB.

### [tcp_connections_statistics]
A summary of established TCP connections.

* Ephemeral Port Range
* Total Connections
* Incoming Connections
* Outgoing Connections
* Unknown Direction
* Fqdn
* The top ports by usage (Port, Count, Service).

Recognized service names: FTP, SSH, Telnet, SMTP, DNS, HTTP, POP3, RPC, IMAP, LDAP, HTTPS, SMB, SMTPS, SMTP-Submission, LDAPS, IMAPS, POP3S, MS-SQL, MrBig Agent, iscsi-target, MySQL, RDP, MS-SQL-Listener, PostgreSQL, WinRM-HTTP, WinRM-HTTPS, HTTP-Alt, Commvault, HTTPS-Alt, SQL Proxy via LK, Graylog, WMI, Dynamic, Unknown.

### [tcp_connections]
The established TCP connections, one per line.

| host_name | direction | pid | process_name | source_fqdn | source_ip | source_port | target_fqdn | target_ip | target_port |
|-----------|-----------|-----|--------------|-----------|-----------|-------------|-----------|-----------|-------------|

Direction values: Incoming, Outgoing, Unknown.

### [processes]
All processes, with the following columns:

| PROCESS | PID | PPID | USER | CPU | MEMORY | SERVICES |
|---------|-----|------|------|-----|--------|----------|

The SERVICES column lists the services each process is connected to.

### [topprocessescpu]
The top processes by CPU usage.

### [topprocessesmemory]
The top processes by memory usage.

### [runningservices]
The services, with their start type and status.

| PID | SERVICE | DISPLAY NAME | STARTUP | STATUS |
|-----|---------|--------------|---------|--------|

STARTUP (start type) values:

* Auto
* Boot
* Manual
* None
* System
* AutoDelayed
* Unknown

STATUS values:

* Running
* Stopped
* Starting
* Stopping
* Paused
* Pausing
* Unpausing
* Unknown

### [eventlog_application] / [eventlog_setup] / [eventlog_system]
Recent warnings and errors from the Application, Setup and System event log channels, respectively. Each shows:

| Timestamp | Id | Level | Source | Message |
|-----------|----|-------|--------|---------|

Level values:

* Critical
* Error
* Warning
* Info
* Verbose
* Always

A channel can be disabled; see [Configuration](#configuration) below.

### [applications]
The applications installed on the machine.

| Name | Version | Publisher | Architecture |
|------|---------|-----------|--------------|

Architecture values: x86-32, x64.

### [kbs]
The installed hotfixes (KBs), ordered by install date. Each row shows the KB number and its title.

### [certificates]
The certificates in the store. For each certificate:

* Friendly Name
* Valid from
* Valid to
* Store location
* Subject RDN
* Issuer RDN
* Intended purposes
* Key Size
* Signature Algorithm
* Finger Print
* Serial Number

### [registry]
Registry entries (see [Configuration](#configuration) below).

### [reboots]
Recent reboots.

| Date | User | Reason |
|------|------|--------|

### [clientversion]
The version of Mr Big.

### [clock]
Time information.

* epoch - the Unix timestamp.
* local - the local time.
* ISO - the local time in ISO 8601 format.
* UTC - the UTC time.
* Time Synchronisation type
* NTP server

## Configuration

The clientlog module reads its configuration from the `cfg.cache` file and, for some sections, from the local `mrbig.cfg`. The `cfg.cache` file holds the configuration that Mr Big downloads from minicfg; it is located in `C:\Windows\System32` (with a `C:\Windows\Sysnative` fallback for 32-bit processes running on 64-bit Windows).

### Registry entries

The `[clientlog:registry]` section (or `[registry]`) lists registry keys and values to look up and display. This section is read from `cfg.cache` only, so it must be configured through minicfg.

Each line is a full registry path, written as `root\subkey\value`. The root can be one of `HKLM`, `HKCU`, `HKCR`, `HKU` or `HKCC` (or the full name, e.g. `HKEY_LOCAL_MACHINE`). Only valid keys and values are displayed; if a key or value does not exist it is silently ignored.

Example (in the per-client configuration sent by minicfg):

	[clientlog:registry]
	HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProductName
	HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\CurrentBuild
	HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Domain

The following values are always looked up and displayed, even if the section is empty:

	HKLM\SOFTWARE\Axians\ComputerProperties\Customer
	HKLM\SOFTWARE\Axians\ComputerProperties\CI
	HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Domain

Up to 64 configured entries are allowed.

### Event log channels

The clientlog event log section scans the Application, Setup and System channels for recent warnings and errors. Any of these channels can be disabled with a `disable` directive in the `[clientlog:eventlog]` section (or `[eventlog]`). This section is read from both `cfg.cache` and the local `mrbig.cfg`.

	[clientlog:eventlog]
	disable application
	disable setup
	disable system

A disabled channel is still shown in the report, but with a note that it has been disabled by configuration.
