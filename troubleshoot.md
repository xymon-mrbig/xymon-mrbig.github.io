# Troubleshoot

OK, you have [installed according to the instructions](install.md), you have [configured](configure.md) until you're blue but the piece of XXX won't work.

## Has the service been installed and started?
Type "net start mrbig". The reply will look different depending on whether the service has been installed and/or started:
The service name is invalid 	Not installed
The Mr Big Monitoring Agent service was started successfully 	Installed, not started
The requested service has already been started 	Installed and started

##Won't start
Since Windows services don't make a lot of noise, it can be difficult to see what's going on. For this reason, Mr Big can be started as an ordinary console application. This is done with the -t option:

    mrbig -t -d -m

It will run until you press Ctrl-C. The -d option gives some debugging information of dubious value. The -m option activates memory debugging by keeping track of all memory allocations and deallocations, and detecting if the program attempts to write outside allocated memory.

Repeating -d will increase the amount of debugging information. Three or more -d will produce so much "information" that it will be very difficult to see the needle for all the hay, and server load will rise.

##Starts, nothing happens
First of all: make sure that the file mrbig.cfg exists and has exactly that name, not mrbig.cfg.txt or some other such Windows weirdness.

Also make sure that the client is configured on the BB server. Otherwise the reports may not be accepted.

Verify the spelling of everything in mrbig.cfg, especially the lines for display and machine. If possible, use minicfg to get this information automatically.

##Logging
Mr Big can log some information to a file. Add to mrbig.cfg:

    logfile C:\temp\mrbig.log

The file can be anywhere, but make sure that SYSTEM has permission to write there.

If looking for some specific information, it can be a good idea to build a custom version of mrbig.exe with extra debugging. Use the source, Luke!

Running Mr Big with logging enabled permanently is not recommended since it will increase load and take up disk space.

## Minibbd
The program minibbd is used to see what the client is trying to send. Start minibbd in a dos window. It listens to port 1984 just like a real BB server, but just logs everything it receives to the console. Add the line "display 127.0.0.1" to mrbig.cfg to send all reports to minibbd. It is possible to have several display lines in mrbig.cfg and the reports will be sent to all of them.

##Missing performance objects
Mr Big works by reading performance data from the Registry. Mr Big can't monitor an object unless it can be read.

The cause of the problem can be a disabled dll. Check this like this:

1. Install exctrlst, which comes with the Windows Resource Kit.
1. Run exctrlst.exe and go through the list of "Extensible Performance Counters". 
1. Check the square for "Performance Counters Enabled" everywhere. 

Reading the objects should now be possible.
