# grop
Evaluate a perl re &amp;&amp; print 

`grop [-hir] <regex pattern> [expression to evaluate] [file...]`

-h prints a help message

-i ignores case (adds /i flag to regex)

-r recursively decends into directories like find

This is basically a small script which does `perl -lne`.  It essentially greps through lines in files matching a regex (first arg) and evals the matching lines with the second arg.  The remaining args are file names, or if none given, reads from stdin.  Grop, as in grep + operate: grop.

An idiom I used to type a lot was something like this:

```
perl -lne '/(\w+):[^:]*:\d+:\d+:([^:,]*):/ && print "$2 $1"' /etc/passwd
```

Which read /etc/passwd, read each line, applied the regex and did something with the capture groups.  I decided to turn this into a single command and remove the boilerplate (the perl -lne, the slashes, the && print) and just give me a simple command that focused on what I was trying to do.  

The above perl line becomes a bit shorter:

```
grop '(\w+):[^:]*:\d+:\d+:([^:,]*):' '$2 $1' /etc/passwd
```

Of course you can do this in awk and probably even sed.  But the interesting thing here is that in the second arg, you can do interesting things in perl like do some math on the capture groups or something more complicated.

If you don't specify an operation, the line is printed out.

If you only specify a pattern, then it treats that as a single capture group.  It greps through lines that match the pattern and prints the section of the line that matches the pattern.  

If you specify some capture groups in that pattern, it prints only those lines that match and only the capture groups, each group separated by a space.

```
grop '(?:[^:]*:){4}([^:,]*)[,:]' /etc/passwd
```

Prints the name in the GCOS field.  Note the non-capturing group is ignored.
