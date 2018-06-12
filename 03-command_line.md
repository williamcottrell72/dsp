# Learn command line

Please follow and complete the free online [Bash Scripting Tutorial](https://ryanstutorials.net/bash-scripting-tutorial/) or [Codecademy's Learn the Command Line](https://www.codecademy.com/learn/learn-the-command-line). These are helpful tutorials. You should be able to go through these in a couple of hours.

**Note:** Bash is not installed on a PC. Rather, PC users must install Cygwin. Once Cygwin has been installed, the command line interface witll _emulate_ bash. You can find all information regarding Cygwin [here](https://www.cygwin.com/).

---

### Q1.  Cheat Sheet of Commands

Here's a list of items with which you should be familiar:
* show current working directory path
* creating a directory
* deleting a directory
* creating a file using `touch` command
* deleting a file
* renaming a file
* listing hidden files
* copying a file from one directory to another

Make a cheat sheet for yourself: a list of at least **ten** commands and what they do.  (Use the 8 items above and add a couple of your own.)

> > pwd | present working directory
> >---|:---:
> >mkdir | make directory
> >rmdir | remove directory
> >touch path/file | creates 'file'
> >rm path/file | removes 'file'
> >mv source dest | moves 'source' to 'dest'
> >ls -a | lists all Files
> >cp source dest | copies 'source' to 'dest'
> > man cmd | manual for command 'cmd'
> > cd | change directory
> > locate *str | finds files containing 'str'
> >clear | clears terminal
---

### Q2.  List Files in Unix

What do the following commands do:
`ls`
`ls -a`
`ls -l`
`ls -lh`
`ls -lah`
`ls -t`
`ls -Glp`

> > ls | Lists files in directory
> > --- | :---:
> > -a | lists **all** files.
> > -l | **long** listing information
> > -h | **human** readable.
> > -lh | **long and human** readable
> > -lah | **all long human** readable
> > -t |  sort by **time** (newest first)
> > -Glp | no **group**, **long** and a**ppend** slash (/) to directories

---

### Q3.  More List Files in Unix

Explore these other [ls options](http://www.techonthenet.com/unix/basic/ls.php) and pick 5 of your favorites:

> > ls option | meaning
> > :---: | :---:
> > -r | **reverse** order
> > -R | **recursively** list subdirectories
> > -X  | sort alphabetically by **extension**
> > -g | like `-l` but no owner listed
> > -d | just list **directories**
> > -F | Classify (i.e., add */=>@ \|)

---

### Q4.  Xargs

What does `xargs` do? Give an example of how to use it.

> > `xargs` allows one to build and excute command lines from standard input.

>**Example**  Suppose we have a text file separated by spaces 'example.txt' and we want to make a directory for each word in the text file.  Then we can write:
> > **cat example.txt | xargs mkdir**
