# *backup2l* - A Low-Maintenance Backup/Restore Tool

by Gundolf Kiefer, 2001-2018

## Description

*backup2l* is a lightweight command line tool for generating, maintaining and restoring backups on a mountable file system (e. g. hard disk). The main design goals are are low maintenance effort, efficiency, transparency and robustness. In a default installation, backups are created autonomously by a cron script.

*backup2l* supports hierarchical differential backups with a user-specified number of levels and backups per level. With this scheme, the total number of archives that have to be stored only increases logarithmically with the number of differential backups since the last full backup. Hence, small incremental backups can be generated at short intervals while time- and space-consuming full backups are only sparsely needed.

The restore function allows to easily restore the state of the file system or arbitrary directories/files of previous points in time. The ownership and permission attributes of files and directories are correctly restored.

An open driver architecture allows to use virtually any archiving program as a backend. Built-in drivers support .tar.gz, .tar.bz2, or .afioz files. Further user-defined drivers can be added as described in the supplied sample configuration file 'first-time.conf'.

An integrated split-and-collect function allows to comfortably transfer all or selected archives to a set of CDs or other removable media.

All control files are stored together with the archives on the backup device, and their contents are mostly self-explaining. Hence, in the case of an emergency, a user does not only have to rely on the restore functionality of *backup2l*, but can - if necessary - browse the files and extract archives manually.

For deciding whether a file is new or modified, *backup2l* looks at its name, modification time, size, ownership and permissions. Unlike other backup tools, the i-node is not considered in order to avoid problems with non-Unix file systems like FAT32.

## Screenshots

**a) Generating a backup: mail received from cron daemon**

The monitored area covers 26803 (=23733+3053) files and directories and over 2.2 GB of data. Look at the time stamps!

```
backup2l v0.9 by Gundolf Kiefer

Tue Nov  6 07:58:00 CET 2001

Mounting /disk2...

Running pre-backup procedure...
  writing dpkg selections to /root/getselections.log...

Removing old backups...

Preparing differential level-3 backup <all.1104> based on <all.1103>...
  657 / 23745 file(s), 94 / 3058 dir(s), 63945 / 2332639 KB (uncompressed)
  skipping: 498 file(s), 14 dir(s), 3144527 KB (uncompressed)

Creating archive...
Checking TOC of tar file (< real file, > archive entry)...
Creating check file for <all.1104>...

Tue Nov  6 07:58:41 CET 2001


Summary
=======

Archive     Date       | Size (KB) | Skipped  Files+Dirs |  New  Obs. | Errors
------------------------------------------------------------------------------
all.1       2001-07-15 |   1475248 |     620       17795 |17795     0 |     45
all.101     2001-07-27 |    235720 |     617       17002 |  812  1605 |      2
all.102     2001-08-19 |    210648 |     626       23446 | 7077   633 |      1
all.103     2001-08-30 |    125396 |     670       23796 | 1963  1613 |      0
all.104     2001-09-07 |    203880 |     669       25597 | 5353  3552 |      1
all.105     2001-09-20 |     87076 |     409       24333 | 2192  3456 |      6
all.106     2001-10-01 |     88492 |     409       27611 | 4591  1313 |      1
all.107     2001-10-12 |     67624 |     409       26589 | 1194  2216 |      0
all.108     2001-10-21 |    111580 |     506       26553 | 1351  1387 |      0
all.1081    2001-10-21 |       948 |     506       26554 |   70    69 |      0
all.1082    2001-10-22 |       164 |     506       26554 |   90    90 |      0
all.1083    2001-10-23 |     46680 |     506       27814 | 1465   205 |      0
all.1084    2001-10-24 |     29672 |     506       28578 | 1111   347 |      0
all.1085    2001-10-25 |     28272 |     506       26869 |  282  1991 |      0
all.1086    2001-10-26 |     45000 |     506       26866 |  314   317 |      0
all.1087    2001-10-27 |     33136 |     506       27162 |  610   314 |      0
all.1088    2001-10-28 |     12280 |     506       27645 |  673   190 |      0
all.11      2001-10-29 |    822056 |     506       27725 |13582  3652 |      8
all.1101    2001-10-30 |     20888 |     506       23707 |  858  4876 |      0
all.1102    2001-10-31 |     11336 |     512       26807 | 3372   272 |      0
all.1103    2001-11-05 |       312 |     512       26786 |  173   194 |      0
all.1104    2001-11-06 |     49592 |     512       26803 |  751   734 |      0

Filesystem            Size  Used Avail Use% Mounted on
/dev/hde1             5.8G  3.5G  2.3G  61% /disk2

Unmounting /disk2...
```

**b) Restoring the directory */home/home/gundolf/prog/* from snapshot *<all.1102>*:**

```
lilienthal:/scratch# backup2l -t 1102 -r /home/gundolf/prog/
backup2l v0.9 by Gundolf Kiefer

Mounting /disk2...

Active files in <all.1102>: 246
  found in all.1102:     119   (  127 left)
  found in all.1101:       7   (  120 left)
  found in all.11:       120   (    0 left)

Restoring 35 directories...
Restoring files...
  all.11: 120 file(s)
  all.1101: 7 file(s)
  all.1102: 119 file(s)

Unmounting /disk2...
lilienthal:/scratch#
```

## Installation (from main source directory)

* Debian Linux: Type 'debuild' to get a binary deb archive.
* Others: Run './install-sh' or 'sh install-sh' and follow the instructions.


## Release Notes

Old releases prior to 1.5 can be downloaded from http://sourceforge.net/projects/backup2l/.
Releases from 1.5 upwards are git-tagged accordingly.


**Version 1.4**

With some help from Joe Auricchio <avarame@ml1.net>, backup2l should
now run under Mac OS X 10.3 (but may still be unstable!). The following
known restrictions apply:
- installing the GNU findutils, e. g. from fink (fink.sf.net),
  in particular the GNU versions of 'find' and 'xargs'.
- If no GNU version of sed is installed, the FILTER_* commands in the
  beginning of "backup2l" have to be changed to use per for filtering
  (just uncomment the respective lines there).
- In some cases, the printf "%s" command of 'find' always returns 0
  instead of the file size. This leads to incorrect size reports.
- The date in the summary output is broken unless GNU date is installed.

Binaries for the four GNU programs which Macintosh users need to install
before using backup2l (find, xargs, sed and date) may be found at
http://luddite.cst.usyd.edu.au/~jason/macos-gnu-utils-for-backup2l.tgz
(Jason Grossman <Jason.Grossman@staff.usyd.edu.au>).


**Version 1.1**

An new open driver architecture allows to use virtually any archiving
program as a backend. Built-in drivers support .tar.gz, .tar.bz2, or
.afioz files. Further user-defined drivers can be added as described in
the sample configuration file 'first-time.conf'.

Due to major code changes, this release may be unstable. The latest
stable release is 1.01. Archives created by the default driver
("DRIVER_TAR_GZ") are fully compatible with backup2l 1.01.


**Versions 0.9 & 0.91**

Some versions of find (e. g. 4.1.7.) seem to have a bug so that printf does
not produce leading 0's if requested. As a result, the .list.gz, .new.gz and
.obsolete.gz files may contain entries like

  4 11/11/01 02:07:17    0.   0 0777 /var/squid/some_file

instead of

  4 11/11/01 02:07:17 0000.0000 0777 /var/squid/some_file

In backup2l 0.91, a workaround is implemented to guarantee the correct (2nd)
format. Two issues have to be considered:

a) Without leading 0's, a restore operation may fail.

b) Backups are always created correctly. However, when switching to 0.91,
   differential backups may be larger than necessary.

It is recommended either to modify incorrect .list.gz, .new.gz and
.obsolete.gz files manually or to purge the respective archives.
