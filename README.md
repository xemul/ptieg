ptieg
=====

Process ties graph

This tool shows the process tree starting from given point
and tries to demonstrate how process in it are tied to each
other with FDs.

* How to read its cryptic output.

The tool shows process in blockes. Firt line in a block is

  1234: my_process

i.e. it's PID then the comm of the task.

Then there goes several lines of file descriptors owned by
task, lines look like

  1:            <_1 +2 /foo/bar
  2:            >^1

for files that are used by several tasks or just

  3: </foo/bar

for those owned by a single task.

The first column is FD number, the < or > sign means that
the file is read-only or write-only (if RW there will be
a space). The "_1 +2" part means that the file is shared,
it's name is shown in the end of the line, it's ID is 1
and it's referenced by 2 more lines somewhere below. The
"^1" is one of those lines -- it says that the file is
shared with some other one described by the record with
id 1 above.

E.g. the following lines not necesserily go one-by-one
and describe a file owned by differend tasks

  1:                >_3 +2 /bar/foo
  ...
  3:                 ^3
  ...
  2:                <^3
