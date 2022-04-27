# Console Input
> Helper for defining parameters in scripts...

### Example

Let's simulate the `locate` command. The command signature is something like this:

```text
Usage: plocate [OPTION]... PATTERN...

  -b, --basename         search only the file name portion of path names
  -c, --count            print number of matches instead of the matches
  -d, --database DBPATH  search for files in DBPATH (default is /var/lib/plocate/plocate.db)
  -i, --ignore-case      search case-insensitively
  -l, --limit LIMIT      stop after LIMIT matches
  -0, --null             delimit matches by NUL instead of newline
  -r, --regexp           interpret patterns as basic regexps (slow)
      --regex            interpret patterns as extended regexps (slow)
  -w, --wholename        search the entire path name (default; see -b)
      --help             print this help
      --version          print version information
```

In our script file we will define the parameters, like so:

```bash
# my_locate.sh

# clear previous entries
console::input::clear

# argument definitions
console::input::argument_definition pattern array required

# option definitions
console::input::option_definition   basename    b   scalar
console::input::option_definition   count       c   scalar
console::input::option_definition   database    d   scalar /var/lib/plocate/plocate.db
console::input::option_definition   ignore-case i   scalar
console::input::option_definition   limit       l   scalar
console::input::option_definition   null        0   scalar
console::input::option_definition   regexp      r   scalar
console::input::option_definition   regex       -   scalar
console::input::option_definition   wholename   w   scalar
console::input::option_definition   help        -   bool
console::input::option_definition   version     -   bool
```

Now ask the tool to process the input, something like this:

```bash
# my_locate.sh

# DEFINITIONS...

console::input::parse "$@"
```

That easy you will have defined the parameters accepted by your script. Now all you have to do is refer the captured values to your variables like this:

```bash
# my_locate.sh

# DEFINITIONS...

# PARSE...

declare -- locate_count=
declare -A locate_pattern=()

console::input::option_ref count locate_count
console::input::argument_ref pattern locate_pattern
```

and to enjoy...

```bash
bash my_locate.sh --count 3 /to/path

# in your script you will have something like this: 
#     locate_count=3
#     locate_pattern=([0]=/to/path)
```

