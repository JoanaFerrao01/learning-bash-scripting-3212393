# 1 
## Pipes 
**Send output of 1 prog to other**  

``cat lorem.txt | less`` - sends lorem.txt to less function to navigate text w/ arrows  

``cat lorem.txt | wc`` - sends lorem.txt to wc function to count words, lines and __

## redirections
**works w/ stdin/out/err . sends input/output from a prog to other**  

**``>`` output redirection (truncate)**   
overwrites the content of the dest file  

**``>>`` output redirection (append)**   
adds the content to the end of dest file 

**``<`` input redirection**   
take output from source file and input it to dest file (through stdin)  

**``<<`` here document**  
specify input up to specified limit string   

### Examples
``ls > list.txt`` - writes the content of ls into list.txt 

``ls >> list.txt`` - adds the context of ls to the existing content of list.txt

###### *diferente between stdin, stdout, stderr - numbers before arrows*

"ls /notreal" will return an **error** (the dir doesnt exist)  

``ls /notreal 1>output.txt 2>error.txt`` - will send the stdout msg of the command to output.txt and the stderr msg to error.txt (output is empty, error has stderr error msg)

``cat < list.txt `` - sends list.txt as input to cat prog 

```
cat << EndOfString
> This is a
> multiline
> text string
> EndOfString
```
\- specify end of string, then send input up until the defined end of string 

## bash commands built-ins and others..
**``echo`` prints/outputs the text given to the cmd line WITH new line at the end**  

**``printf``  prints the text given to the cmd line WITHOUT new line at the end**  

**``command ___`` . explicitly run the command version of ___**  

**``builtin ___ ``. explicitly run the builtin version of ___**   

**``command -V ___`` . see if the command ___ is a shell builtin or not (if not it shows the local of the command)**  

**``help ___`` . see doc of builting command ___ (bash version of man for builtins)**  

**``help`` . list the builtins provided by bash**  


Builtins are the default run, but can be disabled/enabled  

**``enable -n ___`` . disable builtin ___ and allows the command version of ___ to run instead**  
**``enable ___`` . enables the builtin ___ version, instead of the command one**  
**``enable -n`` . shows the builtins that are disabled in the system**  

### Examples
``echo hello world`` - outputs "hello world" to the cmd line and starts a new line

``printf hello world`` - outputs "hello world" to the cmd line and stays on the same line

``command echo hello`` - runs the command named "echo" (with its parameters following) 

``builtin echo hello`` - runs the builtin named "echo" (with its parameters following) 

``help echo`` - shows the documentation of the echo builtin command

``help`` -shows the list of the shell's builtin commands

``command -V echo`` - checks if the echo command is builtin:
- **it is** -> output: ``echo is a shell builtin``  

``command -V df``
- **it isn't** -> output: ``df is /usr/bin/df``

``enable -n echo`` - disables builtin echo command and enables the default to be the command version  
*(command -V echo now returns "echo is /usr/bin/echo")*

## Parentesis (), Braces {}, Brackets []
These may act like functions themselves!

## Expansions & substitutions
### `~`  tilde expansion**  
user's ``$HOME`` environment variable

> **Examples**  
> `echo ~` . outputs `/home/___` with ___ being the user's username (*returned by the command `whoami`*)  
> `echo ~-` . outputs the last directory the user was in


### `{...}`  brace expansion**  
creates sets or ranges
*{a,b,c} items separated by ``,``*  
*{x..y..i} ranges separated by ``..``*

> ---
> **Examples**  
> `echo {1..100}` will output:
> ###### 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50 51 52 53 54 55 56 57 58 59 60 61 62 63 64 65 66 67 68 69 70 71 72 73 74 75 76 77 78 79 80 81 82 83 84 85 86 87 88 89 90 91 92 93 94 95 96 97 98 99 100
>  `echo {a..z}` will output:
> ###### a b c d e f g h i j k l m n o p q r s t u v w x y z
>  `echo {a..z..2}` will output:
> ###### a c e g i k m o q s u w y 
> `touch file_{01..05}{a..c}` will create:
> <h6>
>
> file_01a  file_02a  file_03a  file_04a  file_05a  
file_01b  file_02b  file_03b  file_04b  file_05b  
file_01c  file_02c  file_03c  file_04c  file_05c  
>
> </h6>
>
> *like a nested loop, the second loop nested in the first*   
>
> ###### *you can remove them all by using `rm file_*`*
>
> --- 
> > `echo | ls ~/learning-bash-scripting-3212393/{dir1,dir2,dir3}/` . outputs the contents of the directories *dir1*, *dir2* and *dir3* : 
> <h6>
> /home/joanaferrao/learning-bash-scripting-3212393/dir1/:lorem.txt  
> /home/joanaferrao/learning-bash-scripting-3212393/dir2/:lorem.txt
> /home/joanaferrao/learning-bash-scripting-3212393/dir3/:lorem.txt
> </h6>
>
> so does the ``echo | ls ~/learning-bash-scripting-3212393/dir{1..3}/`` command
>
> ---

### **`${...}`  parameter expansion**  
retrieves and transforms stored values

> ---
> **Examples**    
> 
> Given `greeting = "Hello World"`
>
> - `echo $greeting` or `echo ${greeting}` outputs `Hello World`  
>
> - `echo ${greeting:5}` outputs `World`
>   - ###### *however, `echo $greeting:5` would output `Hello World:5`! The braces are necessary when changing the stored value.* 
> 
>
> - `echo ${greeting:2:9}` outputs `llo Worl`
>
> - `echo ${greeting/World/Everybody\!}` outputs `Hello Everybody!`
>
> - `echo ${greeting//o/_}` outputs `Hell_ W_rld!`
>
> - `echo ${greeting/o/_}` outputs `Hell_ World!`
>
> ---

### **`$(...)`  command substitution**  
puts the output of one command inside another

> ---
> **Examples**
>
> `echo "The kernel is $(uname -r)."` outputs `The kernel is 5.15.153.1-microsoft-standard-WSL2.`
>
>`echo "Result: $(python -c 'print("Hello from python!")' | tr [a-z] [A-Z])"`  outputs `Result: HELLO FROM PYTHON!`
> ###### inside the parentisis there are 2 functions: python run code *and* transforming to upper case (via pipe) 
>
> ---

### **`$((...))`  artimetic expansion**  
does calculations
###### *...only with integers*

> ---
> **Examples**
>
> `echo $((2+2))` outputs `4`
>
> `echo $((4/5))` outputs `0` (*only supports integers, 4/5 is less than 1 so.. 0*)
>
> ---
