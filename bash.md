##Day One Bash##
ctrl+p or up arrow – history
!### - history by number
ctrl+e – end of line
ctrl+b – moves back on line
ctrl+f – moves forward on line
ctrl+l – clears screen



man page
man bash
man find
G – moves to end
g – moves to start
/pattern – search forward
?pattern – search backword
&pattern – matches only matching lines
n or shift+n – move between search results
q - quit



*** explainshell.com ***


**Bash Order of Evaluation**
1. Redirection
2. alias
	setting another name to a command
3. parameter expansion, command substitution, arithmetic expression, and quote removal
4. shell functions
5. built-in commands
6. Hash table
7. Path variables
	where you currently are
8. error



**File system and manipulation**
- home dir = ~ or just cd with nothing after will bring you home
- root dir = /
- cd .. takes you back one directory
- absolute (/etc/passwd) and relative path(./ or ..)
-  touch	change timestamp
- mkdir	makes dir
- rm		removes files
- rmdir		usually have to delete all files in dir first
- ls		listing of contents
- cd		changes dir



**symbolic link**	creates pointer. Can remove and wont impact core file. Creats seperate pointer
hard link		creates second access point.  



**Cat – read files**
more is less, less is more



**finding files**
- locate	can use wildcards,  will not work with something that was just created
- whereis	look through established locations for programs, core file locations
- which	where something is installed and where it is at
- find		searches for files in a directory hierarchy
	-name is case sensitive
	-iname case is ignored
	- / looks through the whole file system
	-type -d looks for directories
	-executable finds executables
	
  

***curl cht.sh/(command)***



-exec commands
find (filename) -exec cat {} \;

mv (filename) (location)

2> dev/null



**grep**
print lines matching a pattern
	- c gets count of the number of times something shows up that you are searching
	- egrep uses regex. Expression take arguments and evaluates themselves
	- -n adds line number in searche
	egrep ‘:[0-9]{5}’



**cut**
remove sections from each line of files
tail etcpasswd | cut -d: -f6
cut -d: -f1         displays only portion of line before 1st instance of delimiter ``:''
cut -d: -f1-        " and any following strings up to the very next instance ``:''
cut -d: -f1- -s     " " but don’t print any lines not containing delimiter ``:''
cut -f3             displays only the 3rd field delimited by space
cut -f2-4           displays only fields 2 through 4 delimited by space
cut -c3-10          displays only the 3rd through 10th characters of each line




###**CTF Challenges**###
**01 Bash activity**
Activity: Using Brace-Expansion, create the following directories within the $HOME directory:
    • 1123
    • 1134
    • 1145
    • 1156
mkdir ~/11{23,34,45,56}



**1.2**
Use Brace-Expansion to create the following files within the $HOME/1123 directory. You may need to create the $HOME/1123 directory. Make the following files, but utilze Brace Expansion to make all nine files with one touch command.
Files to create:
    • 1.txt
    • 2.txt
    • 3.txt
    • 4.txt
    • 5.txt
    • 6~.txt
    • 7~.txt
    • 8~.txt
    • 9~.txt
touch ~/1123/{1..5}.txt ~/1123/{6..9}~.txt




**1.3**
Using the find command, list all files in $HOME/1123 that end in .txt.
find ~/1123 -name "*.txt"
another way:
find ~/1123/*.txt'\b(?:[0-9]{1,3}\.){3}[0-9]{1,3}\b'




**1.3 Challenge**
List all files in $HOME/1123 that end in .txt. Omit the files containing a tilde (~) character.
  find ~/1123 -name "*.txt" | grep -v -i "~"
another way:
  find ~/1123/*.txt | grep -v -i "~"
or:
  find ~/1123/*.txt ! iname “*~*”
 
 
 
**02**
Copy all files in the $HOME/1123 directory, that end in ".txt", and omit files containing a tilde "~" character, to directory $HOME/CUT.
Use only the find and cp commands. You will need to utilize the -exec option on find to accomplish this activity.
The find command uses BOOLEAN "!" to designate that it does not want to find any files or directories that follows.
  find ~/1123/*.txt ! -name "*~*" -exec cp {} ~/CUT \;
 another way:
  cp ~/1123/*[^~].txt ~



**03**
Using ONLY the find command, find all empty files/directories in directory /var and print out ONLY the filename (not absolute path), and the inode number, separated by newlines.
  find /var -empty -printf '%i %f\n'



**04**
Using ONLY the find command, find all files on the system with inode 4026532575 and print only the filename to the screen, not the absolute path to the file, separating each filename with a newline. Ensure unneeded output is not visible.
  find /* -inum 4026532575 -printf '%f\n'



**05**
Using only the ls -l and cut Commands, write a BASH script that shows all filenames with extensions ie: 1.txt, etc., but no directories, in $HOME/CUT.
    • Write those to a text file called names in $HOME/CUT directory.
    • Omit the names filename from your output.
  ls -l ~/CUT | cut -d' ' -f10| cut -d"." -f1- -s > ~/CUT/names

Another Way:
  ls -l ~/CUT | cut -d"." -f1- -s | cut -d’:’-f2 | cut -d’ ‘ -f2 > ~/CUT/names



##Day 2 Bash##
  **Awk** -searches for patterns, scans file line by line, best tool for changing info in fields.
awk ‘{print}’ coffeeshop.txt – almost does the same thing as cat
awk ‘/manager/ {print}’ coffeeshop.txt – prints lines only with ‘manager’
awk ‘{print $1,$2}’ coffeeshop.txt – sections 1 and 2 of the text
awk ‘{print NR,$0}’ coffeeshop.txt – nr adds line numbers, $0 prints all lines
awk ‘{print $1,$NF} coffeeshop.txt – nf equals the number of fields 
tail etc/passwd | awk -F: ‘{print $NF}’ coffeeshop.txt - -F sets deliminator



  **sort** – lines with numbers are going to go first, then capital, then lowercase, does not change file
Sort sortcoffee.txt
sort -o newsort.txt sortcoffee.txt - -o creates a new file and you name it. Newsort.txt is the new document
sort -r sortcoffee.txt - -r reverses the order it is sorted in so the end of the alphabet is switched
sort – k 2 sortcoffee.txt - -k 2n sorts on the second field or whatever number replaces 2



  **uniq – filters out repeated lines**
Uniq -c sortcoffee.txt – -c list the number of times something appears in a list, lists everything
uniq -d sortcoffee.txt - -d only shows the things in the list that were repeated 
uniq -D sortcoffee.txt - -D shows all repeated lines and how many times they were repeatedly
uniq -u sortcoffee.txt - -u only shows the unique items that are in a list
uniq -i sortcoffee.txt - -i ignores case



  **conditional operators**
For numbers you want to use the alphabetical  opperators (-eq, -lt, -le, -gt, -ge) , for letters you want to use the signs (==, <, <=, >, >=)



  **Sed- a  stream editor.  A stream editor is used to perform basic text transformations on an
       input stream (a file or input from a pipeline)**
       
Sed ‘s/tea/coffee’ beststuff.txt – s substitutes things out
Sed ‘s/tea/coffee/g’ beststuff.txt > truth.txt – s substitutes things out, g replaces all tea to coffee, > appends to a new document. 
Sed ‘s/tea/coffee/2’ beststuff.txt – 2 changes only the second instance of tea to coffee 
Sed ‘s/tea/coffee/p’ beststuff.txt – prints the lines with the replaced word twice
sed ‘$d’ beststuff.txt – removes the last line of the document
sed ‘1,3 s/tea/coffee’ beststuff.txt – replaces tea with coffee for the first 3 lines 
sed ‘4,$d’ beststuff.txt – removes the 4th line of the document and down
sed '\/bin\/sh\|\/bin\/false/d' $HOME/passwd > $HOME/PASS/passwd.txt – sed /d deletes bin/sh or binfalse if it were to appear, then write a copy to $HOME/PASS/passwd.txt



  **Command substitution** – similar to alias
a=$(tail /etc/passwd)
 	echo $a
alias rm=’rm -i’
unalias rm – removes alias



**o6**
  Write a basic bash script that greps ONLY the IP addresses in the text file provided (named StoryHiddenIPs in the current directory); sort them uniquely by number of times they appear.
egrep -o "([0-9]{1,3}[\.]){3}[0-9]{1,3}" StoryHiddenIPs | sort | uniq -c | sort -nr  
egrep -o – omitting everything but what is being listed
uniq -c - sorts content uniqely with a count reading
sort -nr  - sorts content numerically reversed



**07**
Using ONLY the awk command, write a BASH one-liner script that extracts ONLY the names of all the system and user accounts that are not UIDs 0-3.
    • Only display those that use /bin/bash as their default shell.
    • The input file is named $HOME/passwd and is located in the current directory.
    • Output the results to a file called $HOME/SED/names.txt
awk -F: '(($7 == "/bin/bash") && ($3 > 3)) {print $1}' $HOME/passwd > $HOME/SED/names.txt

awk -F: - delimiter on the “:”



**08** 
  Find all dmesg kernel messages that contain CPU or BIOS (uppercase) in the string, but not usable or reserved (case-insensitive)
    • Print only the msg itself, omitting the bracketed numerical expressions ie: [1.132775]
dmesg | grep 'CPU\|BIOS' | cut -c15- | grep -iv 'usable\|reserved'

cut -c15- - cuts on the 15th character and after
grep -iv – ignore case and inverse match



**09**
Write a Bash script using "Command Substitution" to replace all passwords, using openssl, from the file $HOME/PASS/shadow.txt with the MD5 encrypted password: Password1234, with salt: bad4u
    • Output of this command should go to the screen/standard output.
    • You are not limited to a particular command, however you must use openssl. Type man openssl passwd for more information
a=$(openssl passwd -salt "bad4u" -1 "Password1234")
awk -F: -v "awk_var=$a" 'BEGIN {OFS=":"} {$2=awk_var} {print $0}' $HOME/PASS/shadow.txt

another way:
awk -v a=$(openssl passwd -1 -salt bad4u Pssword1234) -F: ‘{OFS”:”}{$2=a;print}’ ~/PASS/shadow.txt

-1 -  Use the MD5 based BSD password algorithm 1
-salt -  Use the specified salt.  When reading a password from the terminal
-F: - delimiter om “:”



**10**
Using ONLY sed, write all lines from $HOME/passwd into $HOME/PASS/passwd.txt that do not end with either /bin/sh or /bin/false.
sed -n '\/bin\/sh\|\/bin\/false/!p' $HOME/passwd > $HOME/PASS/passwd.txt

sed -n – does not automatically print
/!p – do not print what ever comes before it



##Day 3 Bash##
  **Parameter = entity that stores a value**
- name(variable)
- number (positional parameter)
- special character(special parameter)
variable is a parameter denoted by a name
can not include spaces on either side of the “=”
var_a=”Hello World”
when you reference or call a variable use a “$”
echo $var_a



  **Special Parameters or positional parameters**
built in  positional parameter is a parameter denoted by one or more digits, other than the single digit 0. Positional parameters are assigned from the shell’s arguments when it is invoked, and may be reassigned using the set builtin command
$# number of arguments passed to the script
$0 script name
$2,3,4… the 2nd, 3rd, 4th...arguement passed****
$* arguments passed to the script
$? exit status of last command passed to the script
$_ last argument provided to the previous commands
$$ PID of the shell
$- flags are set in the shell
mynewvar=$1 *** might see on test



**Functions**
allow breaking of code into more managable blocks
series of commands executed as one single command
anything inside of the curly brackets is going to execute
function start(){
echo “Hello”
}

[] test
[[]] expr

read prompts user for input
to close case use esac
function 
parentesis



**Variable Substitution**
substituting a variable with its solution
sub1 () {
name =”John”
echo $name
}

‘’ take everything literally
“” will add the variable in the string
echo “My name is ${name}nny”   brace expansion 

echo ${name1:2}
md5sum SUB – sub is a script, scripts md5 hash
echo “SUB” | md5sum – md5 hash of the string “SUB”



**11**
Using find, find all files under the $HOME directory with a .bin extension ONLY.
    • Once the file(s) and their path(s) have been found, remove the file name from the absolute path output.
    • Ensure there is no trailing / at the end of the directory path when outputting to standard output.
    • You may need to sort the output depending on the command(s) you use. Have each path displayed only once.
    • Utilizing -printf options on find.
    • Utilizing awk to manipulate the fields. This may leave the trailing / if you don't take that into account.
    • Utilizing the rev and cut commands creatively.
find ~/ "*.bin" -type f -printf '%h\n' | sort | uniq 
					
          **sort -u ---- would not need to add uniq**
          
          
          
**12**
Write a script that will do the following.
    • Write a script which will copy the last entry/line in the passwd-like file specified by the $1 positional parameter
    • Modify the copied line to change:
        ◦ User name to the value specified by $2 positional parameter
        ◦ Used id and group id to the value specified by $3 positional parameter
        ◦ Home directory to a directory matching the user name specified by $2 positional parameter under the /home directory (i.e. if the $2 was 'Chris', the new line would have /home/Chris as its home directory)
        ◦ The default shell to `/bin/bash'
    • Append the modified line to the end of the file
file=$1
tail -1 $file | awk -F: -v "username=$2" -v "UIDGID=$3" -v "home=/home/$2" 'BEGIN {OFS=":"} {$1=username} {$3=UIDGID} {$4=UIDGID} {$6=home} {$7="/bin/bash"} {print $0}' >> $file

**{“/home/”username} would get variable to show up w/o having to do extra**



**13**
Find all executable files under the following four directories:
        ◦ /bin
        ◦ /sbin
        ◦ /usr/bin
        ◦ /usr/sbin
    • Sort the filenames with absolute path, and get the md5sum of the 10th file from the top of the list.
md5sum $(find /bin /sbin /usr/bin /usr/sbin -executable -type f| sort | sed '10q;d') | cut -d' ' -f1

another way: md5sum (find /bin /sbin /usr/bin /usr/sbin -executable -type f| sort -u | head | tail -1) | cut -d’ ‘ -f1



**14**
Using any BASH command complete the following:
    • Sort the /etc/passwd file numerically by the GID field.
    • For the 10th entry in the sorted passwd file, get an md5 hash of that entry’s home directory.
    • Output ONLY the MD5 hash of the directory's name to standard output.
sort -t: -k4 -n /etc/passwd | sed '10q;d'| cut -d: -f6 | md5sum | cut -d' ' -f1

| head | tail -1| – replaces sed command only because its only asking for the 10th line
sort -t  sets a delimeter
-k4 gets the 4th field 



**15**
Write a script which will find and hash the contents 3 levels deep from each of these directories: /bin /etc /var
    • Your script should:
        ◦ Exclude file type named pipes. These can break your script.
        ◦ Redirect STDOUT and STDERR to separate files.
        ◦ Determine the count of files hashed in the file with hashes.
        ◦ Determine the count of unsuccessfully hashed directories.
        ◦ Have both counts output to the screen with an appropriate title for each count.

md5sum $(find /bin /etc /var -maxdepth 3 ! -type p ) 2> errors > output
a=$(sed -n '=' output | tail -1)
b=$(grep 'directory' errors | sed -n '=' | tail -1)
echo -e 'Successfully Hashed Files:' $a'\nUnsuccessfully Hashed Directories:' $b

example of -exec **find /bin /etc /var -maxdepth 3 ! -type p -exec md5sum {} > errors > output \;**
sed -n ‘=’ – = gets and prints the current line number



##Day 4 Bash## 

**Loops**
**For Loop**
for <x> in {latte, mocha, ‘black coffee’};
do
	echo $x
done

for x in $(cat menu.txt); do echo $x

for x in $(cat /etc/passwd | awk -F: ‘{print $1}’)
	do echo $x is a user on this system
done


**While Loops**
x=0
while [[$x -le ‘10’]] ; do echo $x ; x=$(($x+1))  – adds 1 to x as long as its <= 10

while true ; do
if [[ $x -gt 10 ]] ; then
break
fi
echo $x
x=$(($x+1))
done



**16**
Design a script that detects the existence of directory: $HOME/.ssh
    • Upon successful detection, copies any and all files from within the directory $HOME/.ssh to directory $HOME/SSH and produce no output. You will need to create $HOME/SSH.
    • Upon un-successful detection, displays the error message "Run ssh-keygen" to the user.
if [[ -d ~/.ssh ]]
then
    cp -r ~/.ssh ~/SSH
else
    echo "Run ssh-keygen"
fi



**17**
Write a script that determines your default gateway ip address. Assign that address to a variable using command substitution.
        ◦ NOTE: Networking on the CTFd is limited for security reasons. ip route and route are emulated. Use either of those with no arguments/switches.
    • Have your script determine the absolute path of the ping application. Assign the absolute path to a variable using command substitution. HINT: man which
    • Have your script send six ping packets to your default gateway, utilizing the path discovered in the previous step, and assign the response to a variable using command substitution.
    • Evaluate the response as being either successful or failure, and print an appropriate message to the screen.
a=$(ip route | head -n 1 | cut -d' ' -f3)
b=$(which ping)
c=$($b -c6 $a | grep received | cut -d',' -f2 | cut -d' ' -f2)

if [[ $c == 6 ]]; then
  echo "successful";
else
  echo "failure";
fi



**18**
Create the following files in a new directory you create $HOME/ZIP:
        ◦ file1 will contain the md5sum of the text 12345
        ◦ file2 will contain the md5sum of the text 6789
        ◦ file3 will contain the md5sum of the text abcdef
    • Create a zip file containing the three files above, without being stored inside a directory in the zip file. Name the zip file $HOME/ZIP/file.zip
        ◦ HINT: "Junk" the paths
    • Utilize tar on $HOME/ZIP/file.zip to archive it into a file called $HOME/ZIP/file.tar.gz which should not include directories. Use the gzip option in tar, DO NOT use a seperate gzip binary.
mkdir ~/ZIP
cd ~/ZIP
echo 12345 | md5sum | cut -d' ' -f1 > file1
echo 6789 | md5sum | cut -d' ' -f1 > file2
echo abcdef | md5sum | cut -d' ' -f1 > file3
zip -j file.zip file1 file2 file3
tar -cvzf file.tar.gz file.zip



**19**
Utilize Bash looping to iteratively create each user account and their directories.
    • Design a basic FOR Loop that iteratively alters the file system and user entries in a passwd-like file for new users: LARRY, CURLY, and MOE.
    • Each user should have an entry in the $HOME/passwd file
    • The userid and groupid will be the same and can be found as the sole contents of a file with the user's name in the $HOME directory (i.e. $HOME/LARRY.txt might contain 123)
    • The home directory will be a directory with the user's name located under the $HOME directory (i.e. $HOME/LARRY)
        ◦ NOTE: Do NOT use shell expansion when specifying this in the $HOME/passwd file.
    • The default shell will be /bin/bash
    • The other fields in the new entries should match root's entry
    • Users should be created in the order specified
for x in LARRY CURLY MOE
do
id=$(cat $HOME/$x.txt)
dire=$(echo -n '$HOME'/$x)
mkdir $HOME/$x
echo $x:x:$id:$id:root:$dire:/bin/bash >> $HOME/passwd
done



**20**
Write a bash script that will find all the files in the /etc directory, and obtains the octal permission of those files. The stat command will be useful for this task.
    • Depending how you go about your script, you may find writing to the local directory useful. What you must seperate out from the initial results are the octal permissions of 0-640 and those equal to and greater than 642, ie 0-640 goes to low.txt, while 642 is sent to high.txt.
    • Have your script uniquely sort the contents of the two files by count, numerically-reversed, and output the results, with applicable titles, to the screen. An example of the desired output is shown below.
        ◦ NOTE: There is a blank line being printed between the two sections of the output below.
touch high.txt
touch low.txt
a=$(stat -c '%a' $(find /etc -type f 2>/dev/null))
for i in $a; do
    if [[ $i -le 641 ]]; then
        echo $i >> low.txt
    elif [[ $i -ge 642 ]]; then
        echo $i >> high.txt
    fi
done

echo 'Files w/ OCTAL Perm Values 642+:'
cat high.txt | sort -nr | uniq -c | sort -nr
echo ''
echo 'Files w/ OCTAL Perm Values 0-640:'
cat low.txt | sort -nr | uniq -c | sort -nr
