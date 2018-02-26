# Install

The first step is to add the client to bb-hosts on the BB server. Make sure that the spelling and layout are correct.

Use the Control Panel to uninstall Quest's Big Brother client if it is already installed.

Get the latest zip archive and unpack it. Copy mrbig.exe into a directory on the client, for example C:\mrbig.

Run these commands in a CMD window in the same directory:

    echo .config 10.0.4.5 27016 > mrbig.cfg
    mrbig -i
    net start mrbig

This assumes that minicfg is used for centralized configuration, and that minicfg is running on host 10.0.4.5, port 27016. This port is used because 27015 was taken by Counterstrike. If minicfg is not used, every client must be configured locally; keep reading.

Reports from the new client should start appearing within 5-10 minutes. No processes or services are monitored until this has been [configured.](configure.md)
