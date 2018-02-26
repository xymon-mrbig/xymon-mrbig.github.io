## General format:

	.config hostname port
	.bind address
	.include filename
	# Comment
	directive [arguments]
	[heading]

## [mrbig]
Options under the [mrbig] heading

	[mrbig]
	machine hostname (default: localhost)
	port port (default: 1984)
	display hostname|ipaddress[:port] (no default)
	report_size bytes (default: 16384)
	sleep seconds (default: 300)
	loop seconds (default: INT_MAX)
	bootyellow minutes (default: 60)
	bootred minutes (default: 30)
	debug level (default: 0)
	cpuyellow percent (default: 80)
	cpured percent (default: 90)
	dfyellow percent (default: 90)
	dfred percent (default: 95)
	memyellow percent (default: 100)
	memred percent (default: 100)
	cfgdir directory (default: same as directory with mrbig.exe)
	msgage seconds (default: 3600)
	pickupdir directory (default: none)
	logfile filename (default: none)
	gracetime test seconds (default: 0)
	option name[=value]

### Disabling tests
Individual tests can be completely disabled like this:

	option no_cpu
	option no_disk
	option no_wmi

et al. For all tests except wmi and external scripts, this also automatically gives the tests a clear status.

### Message processing
There are three modes for event log processing. This is controlled by the following option:

    option fastmsgs=on
    option fastmsgs=off
    option fastmsgs=auto

With fastmsgs=on, only the last 64k of each log is scanned. This reduces load with large logs.

With fastmsgs=off, the entire log is always scanned.

With fastmsgs=auto (the default), the behaviour is controlled by the file fastmsgs.cfg. Processing is like off unless the file fastmsgs.cfg exists in the configuration directory. The file can be empty.
Options under the [disk] heading
These options can also be placed in a separate disk.cfg file.

    [disk]
    driveletter yellowpercent redpercent

## [ext]
Options under the [ext] heading
These options can also be placed in a separate ext.cfg file.

    [ext]
    command

## [msgs]
Options under the [msgs] heading
These options can also be placed in a separate msgs.cfg file.

    [msgs]
    action test value

    status ::= green | yellow | red | ignore
    test ::= type | source | message | id
    type ::= error | warning | information | audit_failure | audit_success

Rules are tested from top to bottom. Testing ends after first match.

## [procs]
Options under the [procs] heading
These options can also be placed in a separate procs.cfg file.

    [procs]
    processname [min [max]]

    Default min = 1
    Default max = min

## [svcs]
Options under the [svcs] heading
These options can also be placed in a separate services.cfg file.

    [svcs]
    displayname  [status]
    "service name" [status]

Use only display names or only service names.

    Default status = SERVICE_RUNNING

Available status values:

    0 = Not installed
    1 = Stopped
    2 = Start pending
    3 = Stop pending
    4 = Running
    5 = Continue pending
    6 = Pause pending
    7 = Paused 

## [wmi]
Options under the [wmi] heading

	[wmi]
	begin pagename (starts a new report)
	always color (if present, gives the report this status regardless of individual fields)
	text text to appear in the report
	query any wql query
	field fieldname [operator value]
	go (run the query and collect results)
	end (page is finished, send the report)

The operators =, <, <=, > and >= can be used to compare the fields returned by wmi to predetermined values. If the comparison value starts with a digit, the value is treated as a number and a numeric comparison is performed. Otherwise the value is treated as a string.

The operator contains is valid only for strings and checks if the string is present in the field. Here is an example. We don't care what the AddressWidth is, so it will appear on the report with a blue bullet next to it. The CpuStatus must be 1 and CurrentClockSpeed must be at least 1000. Finally, the Manufacturer must contain the string "Intel". A successful comparison produce a green bullet on the report, while a failure produces a red bullet.

	[wmi]
	begin wmi-cpu
	#always green
	text Processor
	query select * from Win32_Processor
	field AddressWidth
	field CpuStatus = 1
	field CurrentClockSpeed >= 1000
	field Manufacturer contains Intel
	go
	end
