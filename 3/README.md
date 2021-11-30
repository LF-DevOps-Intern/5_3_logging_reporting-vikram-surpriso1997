# Assignment 3:

Create a file in your system. Whenever a someone performs some action(read, write, execute) on that file, the event should be logged somewhere

1.  Install auditd:
    There is also a toocal called iwatch but auditd is used in this assignment.
    A tool called `Auditd` is used to track and monitor changes to a file. It can be installed in ubuntu with the command:

            sudo apt install auditd

2.  Monitor a file:
    Create a file m`sample-log.txt` which audtid is going to monitor.

    To monitor this file, auditd's configuration file `etc/audit.rules.d/audit.rules` is edited and rule is added for monitoring this file.

        -w ~/Documents/sample-log.txt -p rwx -k monitor-log-file

        > where rwx= read write exeute permissions
        > -p= indicates the pemissions
        > -k=key name

3.  Any new modifications to the file like read or write actions are now logged into `/var/log/audit/audit.log`

4.  Searching for the log
    The saved logs can be searched using `ausearch` with the key of the file which was set earlier:

        ausearch -k monitor-log-file
