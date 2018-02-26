# Configuration

The file mrbig.cfg must exist in the same directory as mrbig.exe. It must contain at least the lines

    machine our.name.in.bb-hosts
    display 10.0.4.5

Alternatively if we use minicfg:

    .config 10.0.4.5 27016

Other available options in mrbig.cfg can be seen in the file mrbig.cfg.dist, which comes with Mr Big.

[Full list of options](options.md)

It is not necessary to restart the service after changing the configuration.

## Minicfg
The program minicfg is run on the BB server and automatically sends a suitable configuration to the client requesting it.

Each client can have its own configuration to be sent by minicfg. In that case, there is a file in the minicfg directory on the BB server with the same name as the client's IP address. The file can contain a complete configuration or just a part thereof.

Clients without unique configuration get the contents of the file 0.0.0.0.

Configuration retrieved from minicfg can be combined with local settings in mrbig.cfg. The last match applies, with the exception of messages which trigger on the first match à la access lists.

Example:

    [msgs]
    ignore source Dhcp
    green type warning
    
    .config 10.0.4.5 27016
    
    [disk]
    C	75	95 

## Services
A client which has been installed with a minimal configuration, whether using minicfg or local, will monitor neither processes nor services. Processes are rarely useful to monitor on Windows, since services are The Good Stuff [tm]. To monitor services, list these either in mrbig.cfg under the [svcs] heading or in the separate file services.cfg.

The services that are running can be seen by typing "net start". The program svcscfg can be used to create a basic service monitoring configuration in a simple way:

    svcscfg > services.cfg

To use service names rather than display names:

    svcscfg -s > services.cfg

Please note that this will not work with services that use bogus service names. Yes, this happens. Using display names is recommended.

To get all services, not just the ones that are running now:

    svcscfg -a > services.cfg

An alert will then be generated if a service that was previously stopped suddenly starts.

Svcscfg has no idea what services are interesting to monitor, so trimming down services.cfg with a text editor can be a good idea. Wordpad is good, because it handles lines without carriage returns reasonably. 
