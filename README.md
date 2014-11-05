fileexists -- check if file exists

Synopsis
--------

**fileexists** [options] *filename*

Description
-----------

**fileexists** is a small utility to check whether the *filename* provided exists.
By default it will print out the filename preceeded by either a plus or
minus symbol, where plus indicates the file does exist and minus indicates
the file does not exist. For example:

    + fileexists

or alternatively

    - filexists

**fileexists** can also output the results in color using the **-c**/**--color**
flag. In this form, the result output will appear as the following:

![Colorized fileexists Output for the Terminal](http://d.pr/i/1adEM+)

Regardless of the **-q**/**--quiet** setting, **fileexists** will always produce an exit
status based on whether the *filename* exists. See **EXIT STATUS** below for
more details.

Note that **fileexists** checks for the existence of a file based on the
supplied *filename*, using the current working directory as the relative
point to look for that *filename*. If you need to check on a file outside
the working directory, you can either supply it as a relative path or as
an absolute path.

Options
-------

### -c, --color

Specifies that the output should be colorized. Output will be in green
for a file that exists and red for a file that does not exist. This
option is especially useful when you are checking for the existence of
a large number of files at once.

This requires a terminal that supports output in color using ANSI SGR
escape sequences. If the words green and red in the paragraph above
are colorized, your terminal supports this.

### -h, --help

Prints the help text (which is nearly identical to this README)

### -q, --quiet

Specifies that utiltity should not output anything. This is useful if
you are using the exit code as a conditional determinant in another
script. See EXIT STATUS below.

Recommended Usage
-----------------

**fileexists** was originally written as a quick and dirty way to
test if all the files installed with a package had been removed. Since
GNU Parallel required some hoop-jumping to make it work with the Zsh
built-in compound command conditional (e.g., **[[ -e *filename* ]]**)
it was easier to just write a simple Ruby script that could be reused.

The original usecase was uninstalling the Graphviz Mac OS binary package
in preparation for installing Graphviz from homebrew. After removing most
of the files for installed by the binary package, the following commands
(knowing that Graphviz installs under prefix /usr/local/) allows verification
of any files that are still left around:

    cd /usr/local
    pkgutil --only-files --files com.att.graphviz.cli.pkg | parallel fileexists -c

Since this usecase is for removing files, a quick scan of which lines are
green shows those files that still need to be removed.

Exit Status
-----------

**fileexists** shines when called from other systems, and to that end a number
of exit status codes are provided to help the caller determine whether the
*filename* provided exists or not. Most callers will want to pay attention
to exit codes **0** (*filename* exists) and **1** (*filename* does not exist). All
other exit codes specify that the options provided (or not) did not result
in a file existence check. The full list of codes is as follows:

###0

**fileexists** will exit with a status of **0** to mean the *filename* exists.

###1

**fileexists** will exit with a status of **1** to mean the *filename* does
not exist.

###2

**fileexists** will exit with a status of **2** when the help flag is set
via the command line option **-h**/**--help**.

###64

**fileexists** will exit with a status of **64** when there are no arguments
(in correlation to the <*sysexits.h*> **EX_USAGE** exit code).