---
title: "쉘은 무엇인가?"
teaching: 60
exercises: 2
questions:
- "쉘은 무엇인가?"
- "명령라인은 무엇인가?"
- "왜 쉘, 명령라인을 사용해야 할까?"
objectives:
- "유닉스 쉘 기초를 이해한다."
- "명령라인을 사용하는 이유와 더불어 사용법을 익힌다."
- "쉘명령어를 사용해서 디렉토리와 파일을 다루는 방법을 익힌다."
- "쉘명령어를 사용해서 데이터를 찾고 조작하는 방법을 익힌다."
keypoints:
- "쉘은 강력하다"
- "쉘을 사용해서 다수 파일을 복사, 이동, 조합할 수 있다"
- "`pwd` 명령어를 사용해서 컴퓨터에 본인이 위치한 장소를 파악하게 된다"
- "`ls` 명령어를 사용해서 디렉토리 안에 담긴 내용을 열거한다"
- "`ls` 명령어에 플래그 `-l`, `-lh`를 활용하여 `ls` 명령어 출력결과를 조정한다"
- "`cd` 명령어를 사용해서 컴퓨터 여기저기를 이동한다"
- "`man` 명령어를 사용해서 도움말로 해당 명령어에 대한 메뉴얼 내용을 파악한다"
- "`mv` 명령어를 사용해서 파일명을 바꾸고, 파일을 옮길 수 있다"
- "`cp` 명령어를 사용해서 기존 파일에서 새로운 파일을 생성시킨다"
- "`cat` 명령어를 사용해서 동일한 유형의 파일 하나 이상을 조합한다"
- "`*`, `?` 와일드카드를 사용해서 특정 동작에 제어되는 파일에 제한을 둘 수 있다"
- "`rm` 명령어를 사용해서 불필요한 파일을 삭제한다"
---

## 들어가며

In this session we will introduce programming by looking at how data can be manipulated, counted, and mined using the Unix shell, 
a command line interface to your computer and the files to which it has access.

The shell is one of the most productive programming environments ever created. Its syntax may be cryptic, but people who have mastered it can experiment with different commands interactively, then use what they have learned to automate their work. Graphical user interfaces may be better at the first, but the shell is still unbeaten at the second.

A Unix shell is a command-line interpreter that provides a user interface for the Linux 
operating system and for Unix-like systems (such as iOS). 

For Windows users, popular shells such as Cygwin or Git Bash provide a Unix-like 
interface.  This session will cover a small number of basic commands using Git Bash for Windows users, 
Terminal for iOS. These commands constitute building blocks upon which more 
complex commands can be constructed to fit your data or project.


The motivations for wanting to learn shell commands are many and various. 
What you can quickly learn is how to query lots of data for the information you want super fast. 

To give you some idea of what the shell can do, I will demonstrate how to find the number of articles published in 2009 in academic history journals whose title contains the word ‘International’.

First, let's use a shell command to see how big this file is.

~~~
$ wc -l 2014-01_JA.tsv
~~~
{: .bash}
~~~
507732 2014-01_JA.tsv
~~~
{: .output}

The shell command `wc` with the flag `-l` (for lines) tells us this is a 500,000 line data file. Excel will struggle to manipulate that, but the shell won’t. Let’s look at this shell command:

~~~
$ grep 2009 2014-01_JA.tsv | grep INTERNATIONAL | awk -F'\t' '{print $5}' | sort | uniq -c
~~~
{: .bash}
~~~
40 AFRICA -LONDON- INTERNATIONAL AFRICAN INSTITUTE-
1 ARCHAEOLOGY ETHNOLOGY AND ANTHROPOLOGY OF EURASIA
34 AUSTRALIAN JOURNAL OF INTERNATIONAL AFFAIRS
947 BAR INTERNATIONAL SERIES
105 CHINA REVIEW INTERNATIONAL
70 FOREIGN POLICY -WASHINGTON-
13 GESTA
38 INDONESIAN QUARTERLY
2 INTERNATIONAL AFRICAN LIBRARY
274 INTERNATIONAL HISTORY REVIEW
80 INTERNATIONAL JOURNAL OF AFRICAN HISTORICAL STUDIES
17 INTERNATIONAL JOURNAL OF AFRICAN RENAISSANCE STUDIES
23 INTERNATIONAL JOURNAL OF CONTEMPORARY IRAQI STUDIES
16 INTERNATIONAL JOURNAL OF HISTORICAL ARCHAEOLOGY
13 INTERNATIONAL JOURNAL OF IBERIAN STUDIES
217 INTERNATIONAL JOURNAL OF MARITIME HISTORY
169 INTERNATIONAL JOURNAL OF MIDDLE EAST STUDIES
95 INTERNATIONAL JOURNAL OF NAUTICAL ARCHAEOLOGY
70 INTERNATIONAL JOURNAL OF OSTEOARCHAEOLOGY
25 INTERNATIONAL JOURNAL OF REGIONAL AND LOCAL STUDIES
55 INTERNATIONAL JOURNAL OF THE CLASSICAL TRADITION
46 INTERNATIONAL REVIEW OF SOCIAL HISTORY
42 INTERNATIONALES ASIENFORUM
28 JEUNE AFRIQUE
1 JOURNAL OF AFRICAN AMERICAN HISTORY
31 JOURNAL OF CONTEMPORARY AFRICAN STUDIES
1 JOURNAL OF MODERN AFRICAN STUDIES
2 RESEARCH IN INTERNATIONAL STUDIES SOUTHEAST ASIA SERIES
1 REVOLUTIONARY RUSSIA
~~~
{: .output}

This is simple, powerful, and does what we want. 
It may seem intimidating but it is deeply logical and eminently within your reach. Let us go through each part in turn:

- `grep 2009 2014-01_JA.tsv` `grep` is the command here. It tells the machine to look in the spreadsheet 2014-01_JA.tsv for all the lines that contain the string **2009** and to store those in memory. The pipe symbol `|` then tells the machine to hold those in memory for the minute as we have something else we want to do.

- `grep INTERNATIONAL` This tells the computer to look for the capitalised string **international** on those lines that have **2009** in them. 

The shell is case sensitive by default. Again it holds this subset in memory. 

- `awk -F'\t' '{print $5}'` 

This will not be covered in detail but it identifies `2014-01_JA.tsv` as a tab-separated spreadsheet and instructs the computer to print to the shell the 5th column which contains journal titles of all the lines we’ve queried down to (those with `2009` in them, and then those with `INTERNATIONAL` in them) and to hold that in memory.

- `sort` 

This command will sort that column.

- `uniq -c` The final command will tell the computer to remove duplicates but, as it is doing so, to count those duplicates and hold that data in memory.

As this is the last bit, the shell then - by default - prints the results to the shell (the screen), i.e. the number of articles published in 2009 in academic journals whose title contains the word 'International', with counts separated by journal. 

There are a few false positives, but this is still a good start: from 500,000 lines of journal article metadata to a few numbers and names just by typing one line of code.

## Basics - navigating the shell

We will begin with the basics of navigating the Unix shell.

Start by opening your shell. When you run it, you will likely see a black window with a cursor flashing next to a dollar sign. 
This is our command line, and the `$` is the command **prompt** to show the system is ready for your input.

If, when opening a command window or at any other time today, 
you are unsure of where you are in a computer's file system, 
you can find out what directory you are in by using `pwd` command, 
which stands for "print working directory", and hitting enter - which executes commands in the shell. 

~~~
$ pwd
~~~
{: .bash}
~~~
/Users/riley
~~~
{: .output}

To orient ourselves, let's get a listing of what files are in this directory. 
Type `ls` and you will see a list of every file and directory within your current location.

~~~
$ ls
~~~
{: .bash}
~~~ 
Applications Documents    Library      Music        Public
Desktop      Downloads    Movies       Pictures
~~~
{: .output}

You may want more information than just a list of files. 
You can do this by specifying various **flags** (also known as arguments or parameters) to go with our basic commands. 
These are additions to a command that provide the computer with a bit more guidance 
of what sort of output or manipulation you want.

If you type `ls -l` and hit enter, the computer returns a long list of files that contains 
information similar to what you'd find in your finder or explorer: 
the size of the files in bytes, the date it was created or last modified, and the file name.

~~~
$ ls -l
~~~
{: .bash}
~~~
total 0
drwx------+  6 riley  staff   204 Jul 16 11:50 Desktop
drwx------+  3 riley  staff   102 Jul 16 11:30 Documents
drwx------+  3 riley  staff   102 Jul 16 11:30 Downloads
drwx------@ 46 riley  staff  1564 Jul 16 11:38 Library
drwx------+  3 riley  staff   102 Jul 16 11:30 Movies
drwx------+  3 riley  staff   102 Jul 16 11:30 Music
drwx------+  3 riley  staff   102 Jul 16 11:30 Pictures
drwxr-xr-x+  5 riley  staff   170 Jul 16 11:30 Public
~~~
{: .output}

In everyday usage you will be more used to units of measurement like kilobytes, megabytes, and gigabytes. 
Luckily, there's another flag `-h` that when used with the -l option, use unit suffixes: 
Byte, Kilobyte, Megabyte, Gigabyte, Terabyte and Petabyte in order to reduce the 
number of digits to three or less using base 2 for sizes.

Now `ls -h` won't work on its own. When you want to use two flags, 
you can just run them together. So, by typing `ls -lh` and hitting 
enter you receive an output in a human-readable format (note: that the order here doesn't matter).

~~~
$ ls -lh
~~~
{: .bash}
~~~
total 0
drwx------+  6 riley  staff   204B Jul 16 11:50 Desktop
drwx------+  3 riley  staff   102B Jul 16 11:30 Documents
drwx------+  3 riley  staff   102B Jul 16 11:30 Downloads
drwx------@ 46 riley  staff   1.5K Jul 16 11:38 Library
drwx------+  3 riley  staff   102B Jul 16 11:30 Movies
drwx------+  3 riley  staff   102B Jul 16 11:30 Music
drwx------+  3 riley  staff   102B Jul 16 11:30 Pictures
drwxr-xr-x+  5 riley  staff   170B Jul 16 11:30 Public
~~~
{: .output}

You've now spent a great deal of time in your home directory. 
Let's go somewhere else. You can do that through the `cd` or Change Directory command. 

If you type `cd desktop` you are now on your desktop 
(note: for Windows users, the case of the file/directory doesn't matter. 
For Linux users like me, it does, and I believe that is the same for Mac people). 
To double check, type `pwd` and you should see something that represents your desktop.

~~~
$ cd Desktop
~~~
{: .bash}

You'll note that this only takes you 'down' through your directory structure 
(as in into more nested directories). If you want to go back, you can type `cd ..`. 
This moves us 'up' one directory, putting us back where we started. 
If you ever get completely lost, the command `cd --` (or `cd ~` or even just `cd`) will bring 
you right back to the home directory, right where you started.

> ## Previous Directory
> To switch back and forth between two directories use `cd -`.
{: .callout}

Try exploring: move around the computer, get used to moving in and out of directories, 
see how different file types appear in the Unix shell. 

Being able to navigate your file system using the bash shell is very important for using the Unix shell effectively. 
And as you become more comfortable, you'll soon find yourself skipping directly to the directory that you want.

>## Getting help
> Use the `man` command to invoke the manual page (documentation) for a Shell command. 
> For example, `man ls` displays all the flags/options available to you - which saves 
> you remembering them all! Try this for each command you've learned so far. 
> Use the `spacebar` to navigate the manual pages, and `q` to quit. 
> *Note:* this command is for Mac and Linux users only. It will probably not work for Windows users.
{: .callout}

~~~
$ man ls
~~~
{: .bash}
~~~
LS(1)                     BSD General Commands Manual                    LS(1)

NAME
     ls -- list directory contents

SYNOPSIS
     ls [-ABCFGHLOPRSTUW@abcdefghiklmnopqrstuwx1] [file ...]

DESCRIPTION
     For each operand that names a file of a type other than directory, ls
     displays its name as well as any requested, associated information.  For
     each operand that names a file of type directory, ls displays the names
     of files contained within that directory, as well as any requested, asso-
     ciated information.

     If no operands are given, the contents of the current directory are dis-
     played.  If more than one operand is given, non-directory operands are
     displayed first; directory and non-directory operands are sorted sepa-
     rately and in lexicographical order.

     The following options are available:

     -@      Display extended attribute keys and sizes in long (-l) output.

     -1      (The numeric digit ``one''.)  Force output to be one entry per
             line.  This is the default when output is not to a terminal.

     -A      List all entries except for . and ...  Always set for the super-
             user.

...several more pages...

BUGS
     To maintain backward compatibility, the relationships between the many
     options are quite complex.

BSD                              May 19, 2002                              BSD

~~~
{: .output}

## Basics

As well as navigating directories, you can interact with files on the command line: 
you can read them, open them, run them, and even edit them, often without ever 
having to leave the interface. Sometimes it is easier to do this using a 
Graphical User Interface, such as Word or your normal explorer,
but the more you work here, the more it is useful, and the more you write scripts, the more you'll need this basic knowledge.

Here's a few basic ways to interact with files. 

First, you can create a new directory.
For convenience's sake, we will create it in the directory you extracted the 
data provided in advance. Let's confirm that. 

~~~
$ pwd
~~~
{: .bash}
~~~
/Users/riley
~~~
{: .output}

Now let's create the directory. 

~~~
$ mkdir firstdir
$ cd firstdir
~~~
{: .bash}

This used the `mkdir` command (meaning 'make directories') to create a directory named 'firstdir'. Then move into that directory using the `cd` command.

But wait! There's a trick to make things a bit quicker. Go up one directory.

~~~
$ cd ..
~~~
{: .bash}

To navigate to the `firstdir` directory you could type `cd firstdir`. 
Alternatively, you could type `cd f` and then hit tab
You will notice that the interface completes the line to `cd firstdir`. 

> ## Tab for Auto-complete
> Hitting tab at any time within the shell will prompt it to attempt to auto-complete
> the line based on the files or sub-directories in the current directory. 
> Where two or more files have the same characters, the auto-complete will only fill up to the 
> first point of difference, after which you can add more characters, and 
> try using tab again. We would encourage using this method throughout 
> today to see how it behaves (as it saves loads of time and effort!).
{: .callout}

The next step is to manipulate files.

Navigate to the `data` directory in the pre-circulated data directory. 

~~~
$ cd data
~~~
{: .bash}

In here there is a copy of Jonathan Swift's *Gulliver's Travels* downloaded from 
Project Gutenberg along with other files we will cover later. Type `ls -lh` and hit enter to see details of this file.

~~~
$ ls -lh
~~~
{: .bash}
~~~
total 92040
-rwxr-xr-x  1 riley  staff   3.6M Jul 16 11:50 2014-01-31_JA-africa.tsv
-rwxr-xr-x  1 riley  staff   7.4M Jul 16 11:50 2014-01-31_JA-america.tsv
-rw-r--r--  1 riley  staff    30M Jul 16 12:54 2014-01_JA.tsv.zip
-rwxr-xr-x  1 riley  staff   1.8M Jul 16 11:50 2014-02-01_JA-art .tsv
-rwxr-xr-x  1 riley  staff   1.4M Jul 16 11:50 2014-02-02_JA-britain.tsv
-rwxr-xr-x  1 riley  staff   598K Jul 16 11:50 829-0.txt
-rwxr-xr-x  1 riley  staff    13B Jul 16 11:50 gallic.txt
~~~
{: .output}

The file we are interested in is `820-0.txt`, which corresponds to eBook #829 on Project Gutenberg and is the book *Gulliver's Travels*.
You can read the text on the command line by using the `cat` command. 

~~~
$ cat 820-0.txt
~~~
{: .bash}

The terminal window erupts and *Gulliver's Travels* cascades by: this is what is known as printing to the shell. 
And it is great, in theory, but you can't really make any sense of that amount of text. 

> ## Canceling Commands
> To cancel this print of `829-0.txt`, or indeed any ongoing processes in the Unix shell, hit `ctrl+c`
{: .callout}

Instead, you may want to just look at the first or the last bit of the file. 

~~~
$ head 829-0.txt
~~~
{: .bash}
~~~
The Project Gutenberg eBook, Gulliver's Travels, by Jonathan Swift


This eBook is for the use of anyone anywhere at no cost and with
almost no restrictions whatsoever.  You may copy it, give it away or
re-use it under the terms of the Project Gutenberg License included
with this eBook or online at www.gutenberg.org
~~~
{: .output}

This provides a view of the first ten lines,
whereas `tail 829-0.txt` provides a perspective on the last ten lines. 
This is a good way to quickly determine the contents of the file.

~~~
$ tail 829-0.txt
~~~
{: .bash}
~~~
Most people start at our Web site which has the main PG search facility:

    http://www.gutenberg.org

This Web site includes information about Project Gutenberg-tm,
including how to make donations to the Project Gutenberg Literary
Archive Foundation, how to help produce our new eBooks, and how to
subscribe to our email newsletter to hear about new eBooks.
~~~
{: .output}

Another way to navigate files is to view the contents one screen at a time. 
Type `less 829-0.txt` to see the first screen, `spacebar` to see the 
next screen and so on, then `q` to quit (return to the command prompt).

~~~
$ less 829-0.txt
~~~
{: .bash}

You may also want to change the file name to something more descriptive. 
You can **move** it to a new name by using the `mv` or move command. 

~~~
$ mv 829-0.txt gulliver.txt
~~~
{: .bash}

This is equivalent to the 'rename file' function.

Afterwards, when you perform a `ls` command, you will see that it is now `gulliver.txt`.

~~~
$ ls 
~~~
{: .bash}
~~~
2014-01-31_JA-africa.tsv    2014-02-01_JA-art .tsv      gulliver.txt
2014-01-31_JA-america.tsv   2014-02-02_JA-britain.tsv
2014-01_JA.tsv.zip      gallic.txt
~~~
{: .output}

Had you wanted to duplicate it, you could have used the `cp` or copy command.

~~~
$ cp 829-0.txt gulliver.txt
~~~
{: .bash}

That would have created a a new file with the title `gulliver.txt` while leaving the original file `829-0.txt` intact.

Now that you have seen and used several new commands, it's time for another trick. 
Hit the up arrow twice on your keyboard. Notice that `mv 829-0.txt gulliver.txt` 
appears before your cursor. You can continue pressing the up arrow to cycle 
through your previous commands. The down arrow cycles back toward your most recent command. 
This is another important labour-saving function and something we'll use a lot.

> ## Using `history`
> Use the `history` command to see a list of all the commands 
> you've entered during the current session. You can also use `Ctrl + r` to do 
> a reverse lookup. Hit `Ctrl + r`, then start typing any part of the command you're 
> looking for. The past command will autocomplete. Hit `enter` to run the command again, 
> or press the arrow keys to start editing the command. If you can't find what you're 
> looking for in the reverse lookup, use `Ctrl + c` to return to the prompt.
{: .callout}

> ## Duplicating a File
>
> Use `cp` to duplicate the Gulliver 
> file and give it the filename `gulliver-backup.txt`: any ideas how you do that? 
>
> > ## Answer 
> > ~~~
> > cp gulliver.txt gulliver-backup.txt
> > ~~~
> > {: .bash}
> {: .solution}
{: .challenge}

After having read and renamed several files, you may wish to bring their text together into one file. Let's
let's put them together to make an **even longer** book. 

To combine, or concatenate, two or more files use the `cat` command again. 

~~~
$ cat gulliver.txt gulliver-backup.txt
~~~
{: .bash}

This prints, or displays, the combined files within the shell. 
However, it is too long to read on this window! 
Luckily, by using the `>` redirector, you can send the output to 
a new file, rather than the terminal window. 

Hit up arrow to get to your last command and amend the line to: 

~~~
$ cat gulliver.txt gulliver-backup.txt > gulliver-twice.txt
~~~
{: .bash}

Now, when you type `ls` you'll see `gulliver-twice.txt` appear in your directory.

~~~
$ ls
~~~
{: .bash}
~~~
2014-01-31_JA-africa.tsv    2014-02-01_JA-art .tsv      gulliver-backup.txt
2014-01-31_JA-america.tsv   2014-02-02_JA-britain.tsv   gulliver-twice.txt
2014-01_JA.tsv.zip      gallic.txt          gulliver.txt
~~~
{: .output}

When combining more than two files, using a wildcard can 
help avoid having to write out each filename individually. 
Again, labour saving! A useful wildcard is `*` which is a place holder for zero or 
more characters or numbers (note: this is slightly different from regex...). 
So, if you type:

~~~
$ cat *.txt > everything-together.txt
~~~
{: .bash}

and hit enter, a combination of all the `.txt` files in the current directory 
are combined in alphabetical order as `everything-together.txt`. 
This can be very useful if you need to combine a large number of 
smaller files within a directory so that you can work with them in 
a text analysis program. 

Another wildcard worth remembering is `?` which is a place holder 
for a single character or number. We shall return to shell wildcards later - for 
now, note again that they are similar to but not excatly the same as the Regex we saw in the previous episode.

Finally, onto deleting. We won't use it now, but if you do want to delete a file, 
for whatever reason, the command is `rm`, or remove. 

**Be careful with the `rm` command**, as you don't want to delete files that you do not mean to. 
Unlike deleting from within your Graphical User Interface, there is **no** recycling bin 
or undo options. For that reason, if you are in doubt, you may want to exercise caution 
or maintain a regular backup of your data.

The syntax for `rm` is the same as `cp` and `mv`: 
for example:  

~~~
$ rm gulliver-twice.txt
~~~
{: .bash}
 
We can add wildcards above as appropriate to specify the files to delete.
