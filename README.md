
NAME
====

  session ---- run a command with its own session id


SYNOPSIS
========

  session [OPTIONS] FILE COMMAND

  session [OPTIONS] -kill [-SIGNAL] FILE

  session [-status] FILE

  session [-help|-man|-html|-version]


DESCRIPTION
===========

  session is a lightweight daemon controller.
  COMMAND is run under a new session id which is written to FILE.
  The COMMAND, along with any processes it spawned, can be terminated with the -kill option.

  COMMAND may be run with a watchdog so that, if it dies, it can be automatically restarted after a delay.  Alternatively a watchdog command may be run before restart.

  Session ids are sometimes, confusingly, referred to as process group ids (particularly by ps).  Any process spawned will have the same session id as its parent unless it is specifically changed.  Thus, any process spawned can be easily identified even if its original parent has terminated.


OPTIONS
=======

  Most options may be preceded by - or --.
  COMMAND can be multiple arguments.


      -k, -kill [-SIGNAL]

  Terminates all processes with the session id specified in FILE and removes FILE.
  With -SIGNAL, sends each process that signal instead of SIGTERM but doesn't remove FILE.
  Thus, it is possible to do:

    session -k -SIGINT /tmp/mydaemon.pgid
    sleep 2
    session -k /tmp/mydaemon.pgid


      -status

  Lists all processes with that session id.


      -cleanup

  If COMMAND terminates then attempted to kill all other process with the same session id.
  Note that the default is not to terminate other session id processes when the initial COMMAND terminates.


      -watchdog [DELAY]

  If COMMAND terminates for any reason, wait for DELAY (default 5) seconds and respawn the COMMAND.
  Implies -cleanup.


      -watchcmd WATCHCOMMAND

  Instead waiting for DELAY seconds; execute WATCHCOMMAND.
  Implies -cleanup.
  WATCHCOMMAND must be a single or quoted argument.
  It will be called with arguments of FILE and COMMAND.


      -user USER

  Run the command as this user.
  If given, this must be the first option.
  Maybe useful when calling session from rc.local (which is run as root).


      -help, -man, -html

  Gives this documentation in text, man or html format.


FILES
=====
  FILE is the file that session keeps the session id of the COMMAND in.
  Most usefully kept in a directory, such as /tmp, which will not remain over reboots.


CAVEATS
=======
  Option processing is somewhat picky.
  Specifying more than one of -watchdog, -watchcmd, -cleanup will give somewhat confusing error messages.
  If specified, -user must come first and will tend to trash quoted arguments.


AUTHOR
======
  Written by Tarim.
  Error reports to <session-cmd@mediaplaygrounds.co.uk>.


SEE ALSO
========
  pkill(1), setsid(1), signal(7)

