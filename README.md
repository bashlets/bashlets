# bashlets

1. [Introduction](#introduction)
2. [Examples](#examples)
3. [Design Notes](#design-notes)
4. [Style Guide](#style-guide)
5. [Bibliography](#bibliography)


## Introduction

Bashlets is a modular extensible toolbox for Bash.

A reasonably sane GNU/Linux environment is assumed to be at hand; support for FreeBSD and Cygwin is available (in _homeopathic dilution_), yet highly experimental.


## Examples

### Data types: generating a random printable string of arbitrary length

	$ source bashlet datatype/string

	$ echo $(string generate_random 32)
	6J-%.v(M)`N_de&2fvdVd;yy R;FRt=[

### Data types: generating and validating UUIDs

	$ source bashlet datatype/uuid

	$ uuid create
	2fc83419-df73-4cfd-bf9d-85634c8370fd

	$ uuid validate 2fc83419-df73-4cfd-bf9d-85634c8370fd || echo INVALID
	$ uuid validate 2fc83419-df73-4cfd-_f9d-85634c8370fd || echo INVALID
	INVALID

### Data types: handling intervals of timestamps

	$ export TZ=UTC
	$ date
	Sat Nov 28 14:05:21 UTC 2015

	$ source datatype/tsinterval

	$ delta=$(tsinterval create yesterday)

	$ date -d @$(tsinterval get_t0 $delta)
	Fri Nov 27 00:00:00 UTC 2015
	$ date -d @$(tsinterval get_t1 $delta)
	Sat Nov 28 00:00:00 UTC 2015

### Data types: comparing version strings

	$ source bashlet datatype/version

	$ version sort 1.44 1.4 1.4.4 1.4.4a 1.4-1234
	1.4
	1.4-1234
	1.4.4
	1.4.4a
	1.44

### Cache: passing data around

	$ source bashlet cache/fs

	$ cache=$(fs create)

	$ # set key/value in a subshell...
	$ ( fs set $cache question 'To be, or not to be...' )

	$ # read value in the parent shell
	$ fs get $cache question
	To be, or not to be...

	$ fs destroy $cache

### Character streams: converting formats without external tools

	$ source bashlet charstream/convert

	$ cat << ??? | convert unix2dos | file -
	> Shall I compare thee to a summer's day?
	> Thou art more lovely and more temperate.
	> Rough winds do shake the darling buds of May,
	> And summer's lease hath all too short a date...
	> ???
	/dev/stdin: ASCII text, with CRLF line terminators

### Math: manipulating complex numbers

	$ source bashlet math/icomplex

	$ i=$(icomplex create 0 1)
	$ icomplex to_real $(icomplex square $i)
	-1

### Math: calculating basic constants to (almost) arbitrary precision

	$ source bashlet math/constants

	$ constants calculate phi 60
	1.618033988749894848204586834365638117720309179805762862135448

### Parsers: interpreting YAML documents in na(t)ive Bash code

	$ # from http://yaml.org/spec/1.2/spec.html
	$ cat sample.yaml
	Date: 2001-11-23 15:03:17 -5
	User: ed
	Fatal:
	  Unknown variable "bar"
	Stack:
	  - file: TopClass.py
	    line: 23
	    code: |
	      x = MoreObject("345\n")
	  - file: MoreClass.py
	    line: 58
	    code: |-
	      foo = bar

	$ source bashlet parsers/yaml

	$ yaml get_by_key sample.yaml Stack__line
	23
	58

### User eXperience: enjoying a basic REPL cycle

	$ source bashlet ux/repl

	$ repl start
	> quit
	$


## Design Notes

TODO

## Style Guide

TODO

## Bibliography

### Bash Hacking

- [Advanced Bash-Scripting Guide](http://www.tldp.org/LDP/abs/html/index.html)
- [The Bash Hackers Wiki](http://wiki.bash-hackers.org/)
- [Bash Design Patterns](http://stackoverflow.com/questions/78497/design-patterns-or-best-practices-for-shell-scripts)

### Bash Criticism

- [Bash Weaknesses](http://mywiki.wooledge.org/BashWeaknesses)

### Collections of scripts

- [Bash One-Liners](http://bashoneliners.com/)
- [Awesome Shell](https://github.com/alebcay/awesome-shell)

### Frameworks

- [bang.sh](https://github.com/bashlets/bangsh)
- [bashinator](https://github.com/wschlich/bashinator)
- [bashworks](https://github.com/jpic/bashworks)
- [log4sh](https://sites.google.com/a/forestent.com/projects/log4sh)
