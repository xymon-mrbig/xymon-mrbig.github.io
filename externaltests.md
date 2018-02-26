External tests are active if the pickupdir option is set. Programs to be run are listed one per line under the [ext] heading or in the ext.cfg file. They are run after all other tests by the Mr Big main loop.

The command should produce a file which is placed into the pickup directory. The name of the file determines the test name, i.e. the name of the column in the display. The first line in the file should be a single word indicating the status: green, yellow or red. The file name as well as the status is case sensitive.

To perform tests on behalf of other machines, specify the file name as:

    machine,example,com.test

After all commands are run, files in the pickup directory are examined and reports sent to the display. The files are removed.

It is not necessary to use Mr Big to generate the files. They can be produced by another scheduling mechanism, uploading files from another system etc.
