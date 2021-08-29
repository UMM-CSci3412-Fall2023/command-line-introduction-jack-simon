# Notes on archive structures <!-- omit in toc -->

Tools like `tar` and `zip` that create and extract files
from archive files contain entire directory structures. Depending
on how those are organized and how you use, say, `tar`, you
can sometimes create a real mess.

We'll try to provide some examples and clarify what this means,
and how it applies to this lab, as well as some of the others.
All this will be done in the context of `tar`, but most of this also
applies to things like `zip` as well.

- [The internal structure of a `tar` archive file](#the-internal-structure-of-a-tar-archive-file)
- ["Nice" archive structures](#nice-archive-structures)
- [What's the "not nice" alternative?](#whats-the-not-nice-alternative)
- [How does this relate to the lab?](#how-does-this-relate-to-the-lab)
- [You should always make "nice" archive files](#you-should-always-make-nice-archive-files)

## The internal structure of a `tar` archive file

A `tar` archive file like `my_data.tar` (or `my_data.tgz` if it's
also compressed with `gzip`) has an internal structure, which you
can see with the command

```bash
tar -tf <your archive file name>`
```

If, for example, you go to the `compiling` directory for this lab you
can look at the structure of `NthPrime.tgz`:

```bash
$ tar -tf NthPrime.tgz 
NthPrime/
NthPrime/main.c
NthPrime/nth_prime.c
NthPrime/nth_prime.h
```

Here `$` is the shell prompt, and the command I entered was
`tar -tf NthPrime.tgz`. The output tells me that this archive
file contains a single directory `NthPrime`, where the `/` at the end
indicates that it's a directory. That directory then contains three
files:

- `main.c`
- `nth_prime.c`
- `nth_prime.h`

## "Nice" archive structures

`NthPrime.tgz` above is an example of a "nice" archive structure _because
everything is in a single top-level directory_ (in this case `NthPrime`).

Why is this a good thing?

Imagine that you're in some directory (e.g., your home directory) that has a
lot of stuff in it.

```bash
$ ls
Courses           Desktop         Documents
Downloads         Public          Research
...
...
```

Now consider extracting the contents:

```bash
$ tar -zxvf NthPrime.tgz
x NthPrime/
x NthPrime/main.c
x NthPrime/nth_prime.c
x NthPrime/nth_prime.h
```

This will create a _single_ new directory wherever you are, and all the other
contents of the archive will be in that directory:

```bash
$ ls
Courses           Desktop         Documents
Downloads         NthPrime        Public
Research
...
...
$ ls NthPrime
main.c            nth_prime.c    nth_prime.h
```

This is really good. Extracting the archive contents was a nice, controlled
operation and the resulting files were all nicely organized into a single
directory.

## What's the "not nice" alternative?

An archive file can just have a bunch of "loose" files that aren't organized into a folder like this:

```bash
$ tar -tf scattered.tgz 
main.c
nth_prime.c
nth_prime.h
```

Here this archive has the same three files, but it was constructed in such a
way that the files are not in a singe top-level directory like it was in
`NthPrime.tgz`. If we expand this archive, it will scatter those individual
files all across your directory structure, which can be a real mess!

```bash
$ tar -zxvf scattered.tgz 
x main.c
x nth_prime.c
x nth_prime.h
McPhee-Gilbert-iMac:demo_dir mcphee$ ls
Courses       Documents     Public        main.c        nth_prime.h
Desktop       Downloads     Research      nth_prime.c   scattered.tgz
```

This was only three files, so it wouldn't be that hard to clean up the mess,
make a new directory by hand, and then extract in that new directory. Imagine,
though, that the archive had dozens or even hundreds of files – it would be a
bit like exploding some huge ball of confetti in your living room, and cleaning
up would be a real nightmare!

## How does this relate to the lab?

[In the "Some non-obvious assumptions that the cleaning test script makes"
section of the lab write-up](README.md#some-non-obvious-assumptions-that-the-cleaning-test-script-makes) when we say:

> You should also assume that if you untar `frogs.tgz`
> that will result in a directory called `frogs` that
> contains the files you need to process.

what we mean is that you can (and should) assume that the the `tgz`
archive you're given as input is a "nice" archive. In particular, it
will create a new directory that has the same name as the archive file,
but without the `.tgz` ending, and that any and all other files will
be inside that directory.

We obviously don't _have_ to structure our archive files that way, but
doing so makes everything simpler and cleaner. So we did, and you can
(and should) assume we did.

## You should always make "nice" archive files

One take-away here is that when you make archive files for others to use
(including your future self!), you should almost always make clean archive
files. I.e., you should gather all the content you want to archive into a
single (reasonably named) directory, and include that directory as the
top-level directory in the archive file.

This will ensure that when someone extracts the contents of the archive,
they won't end up with a living room full of confetti and be quite
annoyed with you as a result. :cry:
