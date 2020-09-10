# logbook
A set of Zsh functions for logging tasks to files

This suite includes two tools: One for creating a logbook using the command
`lb`, and one called `slb` that utilizes `gpg` to create an encrypted logbook.

When invoked, both logbook functions will determine if there exists a file for
the day requested (see below for day options), and, if one exists, it will be
opened. If one does not exist, then a new one will be created.

Logbook does not impose any requirement on the formatting or style of the
logbooks themselves - it's simply a tool to create text files in an organized
manner. 

## Setup

For the `lb` function, only `zsh` is required. For `slb`, you will need `zsh`,
as well as the `moreutils` package, and you will need a working installation of
`gpg`.

To install the functions, add the following to your `.zshrc` file:

```
## This specifies which directory logbooks will be written to. For secure logs
## (i.e. the `slb` function), logbooks will be stored in a subdirectory of this
## location called `secure`.
LOGBOOK_PATH=/path/to/logbooks

## This specifies which identity should be used for the secure logbook function.
## The public key corresponding to this email address will be used to encrypt
## logbooks. It must be in your gpg keyring.
SECURE_IDENTITY=someone@somewhere.net

## This adds the directory containing the functions to the function path for
## zsh.
FPATH=${FPATH}:/path/to/functions/directory

## Uncomment either or both of the following function load commands.
## `lb` loads the standard logbook command and `slb` loads the secure logbook
## command.
#autoload lb
#autoload slb
```

## Usage

### Normal Logbook
The `lb` function will create a logbook for the current day and open an editor
so you can enter your log for the day. It can also take a number as a parameter.
If a number, `X`, is specified as a parameter on the command line, `lb` will
open a log for the date X days in the past.

Each logbook is titled `YYYY-MM-DD.md`, where `YYYY` corresponds to the four
digit year of the log date, `MM` to the two digit month of the log date, and
`DD` to the two digit day of the month of the log date. If a log file already
exists for the specified date, it will be opened instead of created.

### Secure Logbook
The `slb` function works the same as `lb`, except that it will not create a file
on the filesystem initially. Instead, it will store the file contents in memory
until you save and quit the editor. At that time, it will pipe the contents of
the file to `gpg` to encrypt the contents using the public key corresponding to
the identity specified in `SECURE_IDENTITY`. The encrypted contents will be
written to a file in the `secure` subdirectory of the `LOGBOOK_PATH` directory.

If the logbook for the specified date already exists, it will attempt to decrypt
the contents of the logbook before opening the editor, so the contents can be
edited.
