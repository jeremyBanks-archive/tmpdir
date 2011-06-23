tmpdir
======

tmpdir is a Python 2.6/2.7 module and command-line tool for working with
temporary directories. It reads and writes tar, tgz, tbz2 and zip archives.

It's available in PyPi, so you can install with setuptools...

    $ easy_install tmpdir

...or with pip...

    $ pip install tmpdir

**It's not stable yet**, but is mostly working.

Command-line Tool
-----------------

The command line tool initializes a temporary folder, runs a command inside
it, then deletes it. All options/arguments are optional.

    tmpdir
           $ARCHIVE
               loads an archive into the temporary folder.
           
           -o, --out=$ARCHIVE
               dumps folder contents into an archive after command exits.
           
           -c, --command="foo [bar bar bar...]"
           -h, --shell="echo foo > bar"
               specifies the command to run in the directory.
               use --shell to run inside /bin/sh.
           
           -d, --delete=secure
                           uses srm --simple to delete directory.
                           fails if srm command is unavailable.
                       =pseudo-secure (default for empty directories)
                           overwrites and renames files before erasing,
                           providing some security on some platforms.                          
                       =attempt-secure (default for loaded archives)
                           attempts --secure, falls back to --pseudo-secure.
                       =not-secure
                           deletes directory normally.
           
           -r, --on-error=ignore
                             discard subcommand exit status.
                         =fail (default)
                             returns exit status from subcommand.
                         =abort
                             returns exit status from subcommand, and
                             does not archive if it's nonzero.

If a command isn't given, it defaults to `bash --login` if run from a shell
and `read -p "Press enter to delete directory..."` otherwise.

When loading an archive it defaults to `--not-secure`, when creating a new one
it defaults to `--attempt-secure`.

### Example Usage

Run `wget --mirror` in a temporary folder and archive the output files in a
tarball.

    ~ $ tmpdir -c "wget stackoverflow.com --mirror -Q 2M" -o so.tgz

Want to compile and install something, but not need to hold on to the source?

    ~ $ tmpdir
    Initializing temporary directory... (secure delete: True)
    /var/folders/fo/fouMw75NGU0zIHpZkU2RF++++TI/-Tmp-/tmpsDGvow/tmp
    ----------------
    tmp $ wget http://python.org/ftp/python/2.7.2/Python-2.7.2.tar.bz2 
    
    [download, ./configure, make, make test, make install...]
    
    tmp $ logout
    ----------------
    Deleting temporary directory... (secure delete: True)
    ~ $

Python Module
-------------

*This exists too.*

