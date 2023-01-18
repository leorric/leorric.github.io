---
layout: page
title: The Linux Command Line
permalink: /linux/cmd
---

# The Linux Command Line


## 1.What is the Shell
bells and whistles 附加的修饰物


The shell is a program that takes keyboard commands and passes them to the operating system to carry out. 

some commands:
date, cal , df, free


## 2.Navigation
whim 一时的兴起，突发奇想

Linux always have a single file system tree

command:
pwd, cd , ls

cd -     // to the previous working directory
cd ~user_name

Linux has no concept of “file extension”. The contents or the purpose of a file
is determined by other means.

Do not embed space in filename

## 3.Exploring the System

ls , less, file

ls option:
-a -A -F -S -t -d -h

less:
G // to the end
1G // first line

reset

Linux directories
/bin  
/boot 



Symbolic Links (Soft link)

Hard Links



## 4.Manipulating Files and Directories
indistinguishable 难以区分的
predate 时间上早于


Wildcard

mkdir
mkdir directories…

cp
-a -i -r -u -v

mv
-i -u -v

rm 
-i -r -f -v

tips: 
be careful with rm. test with ls first and then execute rm.


Hard Links
When we create a hard link, we create an additional directory entry for a file
Two limitations

Creating Hard Links
ln <target> <link name>

create hard links actually create additional file name parts that all refer to the same data part. system assign inode which is then associated with the name part.


Symbolic Links
create a special type of file that contains a pointer to the referenced file or directory. (like windows shortcut)

Creating Symbolic Links
ln -s <target> <link name>
Symbolic links are a special type of file that contains a text pointer to the target file or di- rectory. 

ln -s ../fun dir1/fun-sym


## 5.Working with Commands

What exactly are commands:
an executable program , like those we saw in /usr/bin
a command built into shell itself , like cd
a shell function
an alias

identifying command:
1.type - display a command’s type
2.which - display an executable’s location

3.help
Getting a command’s documentation

4.man
the manual “man” displays is broken into sections.
to refer to a specific section we use:
man section search_term
e.g. man 5 passwd

5.apropos
search the manual page names and descriptions

6.whatis
one line description of a man page

man bash (80 pages long)
7. info
alternative to man page

less and zless

8.alias
use type to see if the there is such existing command
alias name=‘string’
unalias
alias // 


Vim

光标移到行首
0


光标移到行尾
shift+4

光标移到第一行
：0或：1

光标移到最后行
:$

删除行
按两次d

linux上按ctrl D可以输入EOF

https://blog.csdn.net/varyall/article/details/79220745

复制多行
nyy

黏贴
p


Part-1


## 6.Redirection

Standard input, standard output & standard error
> rewrite the file from beginning
>> append

file descriptor
shell reference them internally as fd0,fd1 and fd2 (in, out, err)

ls -l /bin/usr 2> ls-error.txt (redirect std err)
ls -l /bin/usr > ls-output.txt 2>&1 (redirect stdout and std err to one file)
equivalent to :
ls -l /bin/usr &> ls-output.txt 
order matters!

Deposing of unwanted output
ls -l /bin/usr 2> /dev/null


cat - concatenates file
reads one or more files and copy them to stdout
if no argument , copy from stdin to stdout

cat ls-out.txt

cat ( read std in from keyboard and copy to std out)
cat > lazy_dog.txt  (read std in and copy to a file)
cat < lazy_dog.txt ( change std in from keyboard to a file) 


Pipeline
ls -l /usr/bin | less

Filters
put several commands together in a pipeline

Uniq
ls /bin /usr/bin | sort | uniq | less 
uniq -d only print duplicate lines

wc - word count
count lines, words and bytes
wc -l , only lines

grep
-i ignore case 
-v print lines do not match the pattern

head/tail
tail -f
ls /bin/usr | head -n 5

tee
read stdin and copy to both stdout and file


## 7.Seeing the world as shell sees it

pathname expansion
echo *

tilde expansion
echo ~

arithmetic expansion
echo $((2+2))

brace expansion
mkdir chap{1..10}
can be nested

parameter expansion
echo $USER

command substitution
ls -l $(which cp)
in older shell, ls -l `which cp`

double quotes
Most are treated as ordinary characters. some are suppressed , others are not. 
$, \ , ` is not suppressed

single quotes
all expansions are suppressed. Treat as ordinary characters

Escaping Characters
\ quote a single character, preventing expansion.
to represent certain special characters called control nodes. \a, \t, \n

echo -e // enable interpretation of backslash escape

## 8.Advanced Keyboard Tricks
clear, history

Command line editing
cursor movement
ctrl-a // to beginning
ctrl-e // to end
ctrl-b // same as left arrow key
ctrl-f // same as right arrow key
alt-f //  forward one word
alt-b // backward one word
ctrl-l // same as clear command
Modifying Text
ctrl-d  //delete character at the cursor location
ctrl-t  // transpose current char with the one preceding it 
alt-t   // transpose word with the one preceding it 
alt-l   //  convert the chars from cursor location to end of the word to lower case 
alt-u //  ….to upper case
ctrl-w // remove current word


Cutting and Pasting
ctrl-k    // kill text from the cursor location to the end of the line
ctrl-u    // kill …. to the beginning of the line
alt-d     // kill … to the end of current word
alt-backspace // kill … to the beginning of the current word. If cursor is at the beginning of the word , kill previous word
ctrl-y   // yank from the killing ring and insert it at the current cursor location.



Meta key


Completion
tab

Alt-? // tab twice
Alt-*  // insert all possible completions

Using History
hitstory expansion
!88 // execute history command with line number 88


ctrl-r // search the history list incrementally


history commands:
ctrl r 
ctrl o







History Expansion

script [file] // record entire shell session and store it in a file

## 9.Permissions
id - display user identity
chmod - change file mode
umask - set default file permissions
su - run a shell as another user
sudo - execute a command as another user
chown - chang a file’s owner
chgrp - change a file’s group ownership
passwd - change a user’s password






User, Group, and Owners

Users can, in turn, belong to a group consisting of one or more users who are given access to files and directories by their owners 




Read, Writing, and Executing

-rw-rw-r-- 

The first character represents file type. And then comes owner permissions, group permissions and world permissions.

file types:
regular file , d directory , l symbolic link, c a character special file(like terminal or /dev/null), b a block special file(like hard drive or DVD)

chmod 
1.octal number
2.symbolic notation. Consists three parts.
1). Who the change will affect  (u,g,a,o)
2).Which operation will be performed (+,-,=)
3).What permission will be set (r,w,x)

chmod u+x

umask
set default permissions

umask default value is normally 0002(could also be 0022)
It is four digits instead of 3 because there are additional permissions settings

the first is setuid bit (4000) , sets the effective user id from the real user to that of program’s owner.

the second is setgid bit(2000)

the third is sticky bit(1000)
For files, linux ignores the sticky bit.
For directories, it prevents user from deleting or rename files unless the user is either the owner of the directory , the owner of the file or the superuser.


Change Identities

su command
assume the identity of another user , and either start a new shell session with that user’s ID, or to issue a single command as that user


su - [user]
su - // switch to superuser
su -c // execute single command in a new shell


sudo command
similar to su , but there are some differences.
Administrator can configure sudo to allow an ordinary user to execute commands as a different user in a controlled way. 
sudo does not require access to superuser’s password. To execute sudo command , requires the user’s own password.
Sudo does not start a new shell, nor does it load another user’s environment.


/etc/sudoers




chown
change file owner and group owner

chgrp
change only group owner

passwd
set passwd. if superuser , can also set for other user.
see man passwd.


## 10.Processes

Viewing processes
ps, top
tty is controlling terminal 控制终端
当tty的值为pts/0 时，表示伪终端或虚拟终端，简单来说代表了远程终端
第一个是pts/0,第二个就是pts/1，以此类推

STAT field
short for “state”
S sleeping
s a session leader

ps aux
show process belonging to every user

see man page for ps

top - is a more dynamic view compared to ps


Controlling processes
interrupt (ctrl c)   // politely ask to terminate 

put process in background
[command] & // e.g. xlogo & 

return to  foreground
jobs // to get job number
fg %[jobspec]   // fg %1

stopping( pausing)  a process
ctrl z // stop and put it to background if it is previously running foreground
can continue either in foreground or in background with fg or bg


Signals
kill [signal] PID


Shutdown the system
halt, poweroff, reboot, shutdown

https://www.zhihu.com/question/22060662/answer/2286701882

Processes, like files, have owners, and you must be the owner of a process (or the supe- ruser) to send it signals with kill. 
More Process-Related Commands 
pstree
vmstat 
load
told

## 11.The Enviroment

printenv - only display env variables. 
set - when used without options or arguments, will display both shell variables and env variables
export - export environment to subsequently executed programs
alias


Shell stores two types of data in environment - shell variables and environment variables. Shell also stores programmatic data, alias and shell functions.

Shell variables are bits of data placed there by bash.
Environment variables are everything else.



Some interesting env variables
DISPLAY EDITOR SHELL HOME PWD…



How is the environment established
bash starts -> read startup files shared by all users -> more startup files in home directory that define our personal environment



login shell // prompt for username and password
non-login shell // normally from GUI. It inherits env var from parent process

for login shell:
/etc/profile   //global
~/.bash_profile // personal
~/.bash_login
~/.profile


for non-login:
/etc/bash.bashrc   //global
~/.bashrc   //personal


Text editors to modify env var (nano)
ctrl+x // exit
ctrl+o // save

## 12.A Gentle Introduction to vi

command mode

modal editor
:q  exit
:w save 

Moving the Cursor Around
l h j k // move one character for different direction
w    // next character(including punctuation character)
W   // next character(exclude punctuation character)
b    // previous character(including punctuation character)
B
^
0
$

numberG // to line number
G // to last line
many commands in vi can be prefixed with a number.
e.g. 5j means moving the cursor down five lines. similar for 5h

i command // insert
a // append, insert at next position
A command // insert at the end of the line

o // opening a line below
O // above

x
d (delete) ,
d$
dW
d0
d^
dG
d20G

u // undo
 p(paste)
y (yank)
J(join lines) // seems does not work on raspberry pi

f(find/search)  // search within a line for a specified character, can use semicolon to repeat search

/(search word or phrase) // search the entire file. to repeat use ’n'

Global search and replace
in ex command mode, e.g.
:%s/Line/line/g 
:%s/Line/line/gc (confirm)

Editing Multiple Files
vi file1 file2 file3
:bn // buffer next
 :bp // ! can be added to force switch
:buffer :buffer 2

Opening Additional Files For Editing
:e

Copy Content from One File into Another

Inserting an Entire File into Another 
:r foo.txt

Saving Our Work 
ex command
:w 
:w foo1.txt


13.Customizing the prompt
prompt is defined by a env variable PS1

non printing characters encapsulated by \[ and \] 

##14.Package Management

*words*
- proprietary 所有的，所有权的，专卖的
- entail 使必要，需要
- circumvention 规避


generally two camps of packaging techonologies:
- Debian .deb camp  // Debian, Ubuntu, Linux Mint, Raspbian
- Red Hat .rpm camp // Fedora,  CentOS, Red Hat Enterprise Linux, OpenSUSE

### How a package system works
Virtually all software for a Linux system will be found on the Internet. Most of it will be provided by the distribution vendor in the form of package files, and the rest will be available in source code form that can be installed manually.

#### Package Files
The basic unit of software in a package system. 
Consists of 
- programs and data files to be installed
- metadata about the package, description of the package and its content 
- pre and post installation scripts

Package files are created by package maintainers.
- get source code from author
- compile it
- create package metadata and installation scripts

#### Repositories
While some software projects choose to perform their own packaging and distribution, most packages today are created by the distribution vendors and interested third parties. Packages are made available to the users of a distribution in central repositories.

A distribution may maintain several different repositories for different stages of the software development life cycle.
- testing
- development
- etc(like third party repository)

#### Dependencies
*shared libraries*
provide essential services to more than one program

*dependency*
If a package requires a shared resources, it is said to have a dependency.

Most package management systems provide some method of dependency resolution to ensure package and its dependency are installed.

#### High and Low-level Package Tools
Package management systems usually consist of two types of tools.
- Low-level tools which handle tasks such as installing and removing package files
- High-level tools that perform metadata searching and dependency resolution

## Common Package Management Tasks

### Finding a Package in a Repository

Installing package from a repository with high level tools




Installing package from a package file with low level tools
e.g rmp -i  // Red Hat

### Installing a Package from a Repository

**Debian**

apt-get update

更新软件列表，会访问软件源列表里的每个网址，并读取软件列表信息，然后保存在本地主机

apt-get install *package_name*

**Redhat**

yum install *package_name*

### Installing a Package from a Package File (Low level)
Will not resolve dependency.

**Debian**
dkpg -i *package_file*


**Redhat**
rpm -i *package_file*


### Removing a package:
**Debian**

apt-get remove *package_name*

**Redhat**
yum erase *package_name*

### Updating Packages from a Repository
**Debian**
apt-get update
apt-get upgrade

**Redhat**
yum update

to apply all available updates to the installed packages on a Debian-style system, we can use this command:
`apt-get update; apt-get upgrade`

### Upgrading a package from a package file

If an updated version of a package has been downloaded from a non-repository source, it can be installed, replacing the previous version 

**Debian**

dpkg -i

**Redhat**

rpm -U

### List installed packages
**Debian**

dpkg -l

**Redhat**

rpm -qa

### Determine whether a package is installed
**Debian**

dpkg -s *package_name*

**Redhat**

rpm -q *package_name*

Display information
apt-cache show package_name
yum info

Finding Which Package Installed a File 
dpkg -S file_name
rpm -qf

### Displaying Information About an Installed Package
**Debian**

apt-cache show *package_name*

**Redhat**

yum info *package_name*

### Finding Which Package Installed a File
**Debian**

dpkg -S *file_name*

**Redhat**

rpm -qf *file_name*

##15.Storage Media

Modern Linux distributions will automatically mount the CD-ROM after insertion




create new file system:
partition (if needed)
create file system

Moving data directly to and from device

dd if=input_file of=output_file [bs=block_size [count=blocks]] 


Writing CD-ROM images
wodim


checksum, md5sum

##16. Networking
local to remote
scp local_file remote_username@remote_ip:remote_folder


remote to local
scp root@www.runoob.com:/home/root/others/music /home/space/music/1.mp3 


##17. Searching for Files

locate command
performs rapid database search of pathnames

note:
if locate command returns: can not stat () `/var/lib/mlocate/mlocate.db’ No such file or directory
Execute : sudo updatedb
To update the database.


Find 
-type -file -size …

by default there is an implied -and relationship between tests and action

find ~ -type d -name “*.bak” -ls


User-Defined Action
-exec 
xargs


18. Archiving and Backup

Data compression:
Remove redundancy from data

Compression algorithm:
Lossless
Lossy


gzip, gunzip
zcat = gunzip -c

bzip2


19. Regular Expressions

grep


meta characters
^ $ . [ ]{}-?*+() |\ 


The Any Character
.  // one or more character

Anchors
^ // beginning of the line
$ // end of the line

use anchor to ensure no extra character at the either end

Bracket Expressions and Character classes
match a single character from a specified set of characters

POSIX Character Classes



POSIX BRE and ERE

egrep or grep -E

alternation
 |

Quatifier 
? 
match an element zero or one time

*
match an element zero or more times

+
match an element one or more times

{}
match an element specific number of times


find . -regex

locate - - regexp (basic) 
locate - - regex (extended)


Searching for Text with less and vim

less , support extended reg
vim , support basic reg

:hl // try activate highlighting 





20. Text Processing

typographical 印刷上的，排字上的
tabular 扁平的，列成表格的
transliteration 音译，直译
adjoining 邻接的
prowess 英勇；超凡技术

cat 
-A  show all (including hidden) characters
-n  numbers lines
-s  suppress multiple blank lines


note :
ls -S  // list and sort by size


sort 
sort lines by fields
field is separated by delimiters.
Delimiter by default is blank space , but can be specified by -t
field is sorted alphabetically by default

-n numeric sort
-k sort based on key (can be a range)

sort -k 1,1 -k 2n distros.txt // first sort by field1 , then sort by field2 numerically

sort -k 3.7nbr -k 3.1nbr -k 3.4nbr bistros.txt // 3.7 means 7th character within the third field


uniq
Remove duplicate lines that are adjacently to each other


cut
remove sections from each line of files

-c extract based on characters
-f based on fields
-d change delimiter
—complement

expand

paste
Add one or more columns of text to a file. It does this by reading multiple files and combining the fields found in each file into a single stream on standard output.


join
join by shared key



tr
translate or delete characters

-d delete
-s squeeze （repeated characters must be adjoining)

ROT13 encoding

sed
stream editor for filtering and transforming text
sed accepts only basic regular expression(BRE)

command (s,a and etc)
address


back reference


sed script


aspell
spell check

aspell check foo.txt
-H // check html


paste 
merge lines of files

Chap 21 Formatting output

nl

fold

fmt
-c crown margin 
-p format only lines begin with the prefix string


Document Formatting Systems

roff - including nroff and troff
TEX

gruff
is a suite of program containing the GNU implementation of troff


Chap 23 Compiling Programs

get source code

configure command
analyze the build environment:
source code undergo slight adjustment
check external tools and component installed
if it execute successfully Makefile will be created

Makefile
describe the relationships and dependencies among the components that comprise the finished program

Make command
use Makefile to guide its action

compile the source code
it is intelligent. Will not rebuild same components unless necessary.
e.g. source code is updated or dependencies are updated

install the program
sudo make install 
normally its installed in /usr/local/bin



Chap 24 Writing Your First Script


Chap 25 Starting a Project


Chap 26 Top-Down Design
Top-down design
$() 将括号内执行的结果赋值给变量
<pre> preserve the formatting of the command

define function
function name {
	command 
	return
     }

2.  name () {
	command
	return
     }

define local variable
e.g local foo

Chap 27 Flow Control: Branching with if

if
exit status


“$foo” is a defense against parameter being empty string or containing just while spaces
shell function can return exit status by using return command with an integer

test [ expression ]  or [ expression ]
note here [ is actually a command with expression as its argument
that why whitespace is need in [ expression ] 


note:

在 bash shell 中，$()是将括号内命令的执行结果赋值给变量
$[ ] 和$(( ))可以进行整数运算,bash shell $( )并不可以计算

true; echo $? ; false; echo $?


[[ expression ]]  compound expression
additionally it supports:

string =~ regular expression
pathname expansion


(( ))  compound expression

        [] , [[]] and (())
AND  -a   &&
OR   -o   ||
NOT !     !

Control operators: Another way to branch

mkdir temp && cd temp
[[ -d temp ]] || mkdir temp
[[ -d temp ]] || exit 1


Chap 28 Reading Keyboard Input

read <variable> 
read input to variable, if no variable is provided, default variable to hold the value is REPLY

echo -n // suppress newline

read -i <string>   use string as default reply if user simply press enter



<<< 
here string
read <<< “abcd”
standard input is fed to read command

if we use
echo “abcd” | read 
echo $REPLY // output is empty 



IFS 
Internal Field Separator ,  it is a shell variable	

shell allow one or more variable assignments immediately before command.
The effect of the assignment is temporary changing only the environment for the duration of the command. 

IFS=“:” <command>  

because shell pipelines create sub shells, and copies shell and its environment. But when the sub shells finish, the copy of the environment is destroyed. So that means a subshell can never alter the environment of its parent process

通配符必须跟在[] 后面？


Chap 29 Flow Control
while do done
when exit status is zero, loop continues

until do done
when exit status is zero, loop terminates

Use while to read file 
while do done < file


Chap 30 Troubleshooting

Debugging
Finding the problem area by Isolation
Tracing
use echo to trace
bash provide a method to trace, -x option
is default symbol to indicate the display of trace , to change it set PS4 variable 


Chap 31 Flow Control: Branching with Case
case xxx in
	1) do something
		;; ( & is optional, continue check condition)
esac


Chap 32 Positional Parameters

albeit 尽管
clumsy 笨拙的
convoluted 复杂的

$0 , $1 … 
$0 first appeared in command line , which is the pathname of the command
$# number of arguments.

shift

Using positional parameters with shell functions
$FUNCNAME track currently executed shell function



Handling Positional Parameters en Masse 
$*
$@



Chap 33 Flow Control
test -n length not zero
test -r file exists and granted read permission

strings()

two forms
1.
for i in …; do
	command
done

2. c form 


Chap 34 Strings and Numbers


${#character} 
return length

{$parameter#pattern}
{$parameter##pattern}
remove the portion pattern matches, 
#shortest match
##longest match


${parameter/pattern/string} 
${parameter//pattern/string} 
${parameter/#pattern/string} 
${parameter/%pattern/string} 

search and replace
(notice sed and cut as substitution)

expansions can improve the efficiency of scripts by eliminating the use of external programs 


Case Conversion Parameter Expansion
declare -u
declare -l
${parameter,,pattern} 
${parameter, pattern} 
${parameter^^pattern} 

${parameter^pattern} 

Arithmetic Evaluation
Compound command
(())

in evaluation, 0 considered false, 1 considered true


ternary operator


bc- an arbitrary precision calculator language
bc can take standard input
bc script
interactive bc
bc < script
bc <<<
Need more practice for this chapter


Chap 35 Arrays
[[ -d ]] 目录是否存在
sort // sort lines of text

a[0]=foo
echo ${a[0]}

days=(Sun Mon)
days=([0]=Sun [1]=Mon)

create array by direct accessing it or by declare:
declare -a

Array Operations

Output Entire Contents of an Array

Determine the Number of Array Elements
${#a[@]} // array length
${#a[100]} // length of element 100

Finding the Subscripts Used by an Array 
${!array[*]} 
${!array[@]} 



Adding Elements to the End of an Array 
foo=(a b c)
foo+=(d e f)


Deleting an Array 
foo=(a b)
unset foo
unset ‘foo[1]’
Any reference to an array variable without a subscript refers to element zero of the array. 

 Associative Arrays
Use strings rather than integers as array indexes
declare -A colors
colors[“red”]=“#ff0000”
