# bash

## note

```
1. bash script要注意关标签，例如if fi, do done
2. if和then搭配使用, for, while, until和do..done搭配使用
3. 如果有些系统用sh test.sh报错，检查sh是不是指向了dash, bash和dash不兼容
which sh
/bin/sh

ls -l /bin/sh
ubuntu上sh指向了dash: /bin/sh -> dash
```

## 为什么shell脚本会有赋值语句的等号两边不能有空格

```
https://www.zhihu.com/question/411917655

呃题主问的是“为什么”，下面基本都是在吐槽或者“习惯就好”是什么鬼，不知道可以不答...

这个问题是个好问题，每个初学 shell 的人应该都会有这个疑问。
这个规定是为了防止歧义，因为 bash和其他语言的不同点在于，bash可以在脚本中直接运行命令，
而且其中空格最重要的作用就是将参数列表中的参数区分开。
```

## 变量前的$

```
变量前加$表示对变量进行取值

$((...)) and ((...)) are very similar. Both do only integer calculations. 
The difference is that $((...)) returns the result of the calculation and ((...)) does not. 

Thus $((...)) is useful in echo statements:
$ a=2; b=3; echo "$((a*b))"
6

((...)) is useful when you want to assign a variable or set an exit code:
$ a=3; b=3; ((a==b)) && echo yes
yes
```

## special variables

```
$0 - The name of the Bash script.
$1 - $9 - The first 9 arguments to the Bash script. (As mentioned above.)
$# - How many arguments were passed to the Bash script.
$@ - All the arguments supplied to the Bash script.
$? - The exit status of the most recently run process.
$$ - The process ID of the current script.
$USER - The username of the user running the script.
$HOSTNAME - The hostname of the machine the script is running on.
$SECONDS - The number of seconds since the script was started.
$RANDOM - Returns a different random number each time is it referred to.
$LINENO - Returns the current line number in the Bash script.
```

## What is the difference between =, == and -eq in shell scripting?

```
https://stackoverflow.com/questions/20449543/shell-equality-operators-eq

= and == are for string comparisons
-eq is for numeric comparisons
-eq is in the same family as -lt, -le, -gt, -ge, and -ne

== is specific to bash (not present in sh (Bourne shell), ...). 
Using POSIX = is preferred for compatibility. 
In bash the two are equivalent, and in sh = is the only one that will work.

$ a=foo
$ [ "$a" = foo ]; echo "$?"       # POSIX sh
0
$ [ "$a" == foo ]; echo "$?"      # bash-specific
0
$ [ "$a" -eq foo ]; echo "$?"     # wrong
-bash: [: foo: integer expression expected
2

(Note: make sure to quote the variable expansions. Do not leave out the double-quotes above.)

If you're writing a #!/bin/bash script then I recommend using [[ instead. 
The double square-brackets [[...]] form has more features, a more natural syntax, and fewer gotchas that will trip you up. 
For example, double quotes are no longer required around $a:

$ [[ $a == foo ]]; echo "$?"      # bash-specific
0

See also:
What's the difference between [ and [[ in Bash?
```

## partial string matching

```
https://www.namehero.com/blog/bash-string-comparison-the-comprehensive-guide/

if [[ "$str" == *"pattern"* ]]; then echo "str contains pattern"; fi

-----------------------

str="abc1def"
if [[ "$str" == abc?def ]]; then
    echo "String matches the pattern 'abc?def'"
fi

----------------------

str="Apple"
if [[ "$str" == [Aa]pple ]]; then
    echo "String starts with 'A' or 'a', followed by 'pple'"
fi

Usage of double square brackets
Note that in each of the above examples, the string matching happens between double square brackets [[ ]]. 
That’s because partial string comparisons aren’t possible if you use ordinary conditional square brackets. 
This makes particular sense, in the third case of partial string matching, since the range square brackets [ ] would conflict with the conditional ones.
```


## until

```
    i=1
    until [ $i -gt 10 ]
    do
    	echo $i
    	let i++
    done
```

## if elif else

```
#!/bin/bash

read -p "a or b: " OPT

if [ "$OPT" = "a" ]; then
  echo "a"
elif [ "$OPT" = "b" ]; then
  echo "b"
else
  echo "not in list"
fi
```

## case

```
case $1 in
	a)
		echo "a"
		;;
	b)
		echo "b"
		;;
	*)
		echo "not in list"
esac
-----------------------------
case的选项和;;字符可以写到同一行
case $1 in 
	a)echo "a";;
	b)echo "b";;
	*)echo "not in list";;
esac
```


## bash array

```
https://www.freecodecamp.org/news/bash-array-how-to-declare-an-array-of-strings-in-a-bash-script/
Bash Array – How to Declare an Array of Strings in a Bash Script

https://opensource.com/article/18/5/you-dont-know-bash-intro-bash-arrays
You don't know Bash: An introduction to Bash arrays
```

```
myArray=("a" "b" "c" "d")

for str in ${myArray[@]}; do
  echo $str
done

Note: The @ symbol in the square brackets indicates that you are looping through all of the elements in the array. 
If you were to leave that out and just write for str in ${myArray}, only the first string in the array would be printed.
```


## 算术运算

```
a=1;b=3

echo "$[$a+$b]"
echo "$(($a+$b))"
let c=$a+$b ; echo $c
echo `expr $a + $b`
```

## 求100以内整数之和

```
#!/bin/bash

sum=0
for i in {1..100};do
	sum=$(($sum+$i))
done

echo "$sum"
```

## 求100以内偶数之和

```
#!/bin/bash

sum=0
for i in $(seq 2 2 100);do
	sum=$(($sum+$i))
done

echo "$sum"
```

## 打印99乘法表

```
#!/bin/bash

for ((i=1;i<10;i++));do
	for ((j=1;j<=i;j++));do
		echo -n -e "$i × $j = $[ $i * $j ]\t"
		# 或者echo -n -e "$i × $j = $(($i * $j))\t"
            # 或者echo -n -e "$i * $j = `expr $i \* $j`\t"，expr要对乘法符号*转义
	done
	echo
done

注意:
echo -n是不换行
-e是启用转义的\
echo不加参数是默认跳下一行

windows乘法字符输入
Character Map:Press Win + R, type charmap, and press Enter.
Find and copy the multiplication symbol from the Character Map.
charmap -> Advanced view-> Search for: multiplication
```

## A simple script with help menu

```
#!/bin/bash

# A simple script with help menu

function show_help() {
	echo "Usage: script.sh [OPTIONS]"
	echo "Options:"
	echo "  --help       Display this help message"
	echo "  --version    Show the script version"
}

# Check if no arguments are provided
if [ "$#" -eq 0 ];then
	show_help
	exit 1
fi

while [[ "$1" != "" ]];do
	case $1 in
		--help)
			show_help
			exit
			;;
		--version)
			echo "Script Version 1.0"
			exit
			;;
		*)
			echo "Invalid option: $1"
			show_help
			exit 1
	esac
	shift
done
```

## Testing your network connection using a bash script

https://www.joemore.com/blogs/testing-your-internet-connection-using-a-bash-script

::

    #!/bin/bash
    
    # Host to ping
    HOST="google.com"  # Replace with the IP or hostname you want to ping
    
    # Path to the success sound file (sound when connection is alive)
    SUCCESS_SOUND="/System/Library/Sounds/Glass.aiff"
    
    # Path to the error sound file (sound when connection is dropped)
    ERROR_SOUND="/System/Library/Sounds/Basso.aiff"
    
    # Variable to track connection status
    CONNECTION_ALIVE=0
    
    while true; do
        # Ping the host and check if successful
        if ping -c 1 $HOST > /dev/null 2>&1; then
            # If previously disconnected, play reconnect sound and update status
            if [ $CONNECTION_ALIVE -eq 0 ]; then
                echo "Connection restored"
                afplay $SUCCESS_SOUND
                CONNECTION_ALIVE=1
            else
                # Keep playing the success sound every 1 second
                afplay $SUCCESS_SOUND
            fi
        else
            # If disconnected, keep playing the error sound every second
            echo "Connection lost"
            afplay $ERROR_SOUND
            CONNECTION_ALIVE=0
        fi
        sleep 1  # Wait 1 second before the next check
    done

加法计算
--------

::

    #!/bin/bash

    while true
    do
    	num1=$((RANDOM%26))
    	num2=$((RANDOM%26))
    
        echo -e "\n\n$num1 + $num2 = "
        read result
        right=`expr $num1 + $num2`
    
        if [ $result -eq $right ]; then
            echo "Correct" 
    	    continue
    	    #exit 0
        else
           echo "Wrong"
           echo "The correct result is: $right"
           continue
        fi
    done

减法计算
--------

::

    while :
    do
    	a=$(($RANDOM%26))
    	b=$(($RANDOM%26))
    
    	if [ $a -lt $b ];then
    		read -p "$b - $a = " result
    		c=$(($b-$a))
    	else
    		read -p "$a - $b = " result
    		c=$(($a-$b))
            fi	
    	
    	if [ "$result" -eq "$c" ];then
    		echo -e "回答正确\n\n"
    	else
    		echo -e "回答错误\n\n"
    	fi
    
    done

simple_http_server.sh
----------------------

https://gist.github.com/tdpreece/91c6b0305cc7a151e03f

Running a Python SimpleHTTPServer in the background and killing it when doneSimpleHTTPServer

::

    #!/usr/bin/env bash
    
    # Create a page in the current dir
    echo "My Test Page" > test.html
    
    # Start server
    python -m SimpleHTTPServer 8000 &> /dev/null &
    pid=$!
    
    # Give server time to start up
    sleep 1
    
    # request page and print to stdout
    wget -O - http://0.0.0.0:8000/test.html 2> /dev/null
    
    # Stop server
    kill "${pid}"
    
    # Output on running script:
    # My Test Page

start-stop-server-in-background.sh
-----------------------------------

https://gist.github.com/pwittchen/531c0bed8a20c1f06231
Starting and stopping simple HTTP server in background on Linux. After starting server and closing terminal, server should keep running

::

    # starting simple HTTP server with Python in background
    screen -d -m python -m SimpleHTTPServer 7777
    
    # killing process running with screen in background
    kill -9 `top -n 1 | pgrep screen`

命令行下打开文件夹
------------------

::

    打开当前文件夹
    open .

    gnome desktop还可以用
    nautilus .

gnome terminal keyboard shortcuts
---------------------------------

::

    https://help.gnome.org/users/gnome-terminal/stable/adv-keyboard-shortcuts.html.en
    
    File shortcuts
    New Tab
    Shift+Ctrl+T
    
    New Window
    Shift+Ctrl+N
    ------------------------
    Tab shortcuts
    Switch to Previous Tab
    Ctrl+Page Up
    
    Switch to Next Tab
    Ctrl+Page Down
    
    Switch to Tab 1
    Alt+1
    
    Switch to Tab 2
    Alt+2

bash keyboard shortcuts
------------------------

http://blog.chinaunix.net/uid-21782158-id-20019.html  
使用bind -P命令可以查看所有键盘绑定;Alt快捷键较少使用，因为常常和编辑器冲突.  

http://ss64.com/bash/syntax-keyboard.html  

::

    Moving the cursor:
    
      Ctrl + a   Go to the beginning of the line (Home)
      Ctrl + e   Go to the End of the line (End)
      Ctrl + xx  Toggle between the start of line and current cursor position

    Editing:
    
     Ctrl + L   Clear the Screen, similar to the clear command
     TAB        Tab completion for file/directory names
    For example, to move to a directory 'sample1'; Type cd sam ; then press TAB and ENTER. 
    type just enough characters to uniquely identify the directory you wish to open.
    
    History:
    
      Ctrl + r   Recall the last command including the specified character(s)
                 searches the command history as you type.
                 Equivalent to : vim ~/.bash_history. 
      Ctrl + o   Execute the command found via Ctrl+r or Ctrl+s
      Ctrl + g   Escape from history searching mode
            !!   Repeat last command
          !abc   Run last command starting with abc
        !abc:p   Print last command starting with abc
            !$   Last argument of previous command
       ALT + .   Last argument of previous command
            !*   All arguments of previous command
    ^abc­^­def   Run previous command, replacing abc with def
    Process control:
    
     Ctrl + C   Interrupt/Kill whatever you are running (SIGINT)
     Ctrl + l   Clear the screen
     Ctrl + s   Stop output to the screen (for long running verbose commands)
                Then use PgUp/PgDn for navigation
     Ctrl + q   Allow output to the screen (if previously stopped using command above)
     Ctrl + D   Send an EOF marker, unless disabled by an option, this will close the current shell (EXIT)
     Ctrl + Z   Send the signal SIGTSTP to the current task, which suspends it.
                To return to it later enter fg 'process name' (foreground).
    Emacs mode vs Vi Mode
    
    All the above assume that bash is running in the default Emacs setting, if you prefer this can be switched to Vi shortcuts instead.
    
    Set Vi Mode in bash:
    
    $ set -o vi 
    Set Emacs Mode in bash:
    
    $ set -o emacs 
    “...emacs, which might be thought of as a thermonuclear word processor” ~ Emacs vs. Vi Wiki
    
    Related:
    
    fg - Bring a command to the foreground.
    vi editor - A one page reference to the vi editor.
    ~./.bash_history - Text file with command history.
    Terminals Are Weird - How and why of terminal keybindings.
    Equivalent Windows Keyboard shortcuts

shell单引号与变量
------------------

[http://www.361way.com/quotation-mark/1166.html](http://www.361way.com/quotation-mark/1166.html)  
shell单引号与变量

::

    [root@test] a=55
    [root@test] echo $a
    55
    [root@test] echo '$a'
    $a
    [root@test] echo ''$a''  #注意此处是两个单引不是一个双引
    55
    
    由上面的例子不难看出，双引号是不会屏蔽对变量和某些特殊符号的转义的，而单引号里的所有内容都会原封不动地输出，
    而单引号里再用单引号将变量引起来，变量就又可以正常的显示，有点像数学里的负负为正。
    这里解释有误，不是负负为正，''$a''是前两个单引号引起来的内容为空，然后是$a，然后又是两个单引号引起空的内容

如何给shell脚本传参数
------------------------------------------------------------------------

[https://jingyan.baidu.com/article/b24f6c822645b786bfe5daff.html](https://jingyan.baidu.com/article/b24f6c822645b786bfe5daff.html)  

::

    ##example
    vi test.sh
    
        #!/usr/bin/bash
        
        name=$1
        age=$2
        
        echo "$1 is $2 years old."
        
    run
    
        chmod a+x test.sh
        ./test.sh tom 23
    
        or 
    
        sh test.sh tom 23


if else语句简单示例
------------------------------------------------------

::

    echo "input 1 if IP address is 10.5.5.135:8080"
    echo "input 2 if IP address is 192.168.43.1:8080"
    read option
    
    if [ $option -eq 1 ]; then
        IPAddress=10.5.5.135:8080
    else	
        IPAddress=192.168.43.1:8080
    fi
    echo $IPAddress

shell将命令执行的结果赋值给变量
------------------------------------------------------------

::

    1.用` `（反引号）把命令括起来，然后赋值给变量
    
    dir=`pwd`
    
    2.采用   变量=$(pwd)
    
    dir=$(pwd)


测试下载文件用时的脚本
------------------------------------------------------------

test.sh

::

    #!/usr/bin/env bash
    
    set -o errexit
    set -o nounset
    set -o pipefail
    
    # echo 'Start'
    declare -A urls=(
        ["a"]="http://download.linksys.com/updates/20200211t010942/FW_MR7350_1.1.2.199896_release.img"
        ["b"]="https://test-20200303.s3-ap-southeast-1.amazonaws.com/FW_MR7350_1.1.2.199896_release.img"
        ["c"]="http://releases.ubuntu.com/18.04.4/ubuntu-18.04.4-desktop-amd64.iso"
    )
    time=$(date +"%Y-%m-%dt%H:%M:%S")
    # echo "time: $time"
    test=$1
    # echo "test: $test"
    url="${urls[$test]}"
    echo "url: $url"
    # speed_download: average download speed in bytes per second
    ! out=$(curl --max-time 60 --silent --write-out "%{speed_download}, %{size_download}" --output /dev/null "$url")
    # Exit code is 28 for operation timeout
    result=${PIPESTATUS[0]}
    # echo "result: $result"
    # echo "out: $out"
    echo "$time: $out"
    # echo 'Finish'


run_tests.sh

::

    ./test.sh a >> test_a.txt &
    ./test.sh b >> test_b.txt &
    ./test.sh c >> test_c.txt &



dirname, $0, $1, function, case示例

# test.sh

```
DIR="$( cd "$( dirname "$0" )" && pwd )"
echo "Current directory is $DIR"
TOPDIR=${DIR%%scm*}

function function_start
{
	echo "function start"
}

function function_stop
{
	echo "function stop"
}
case $1 in
	start)
		function_start
		;;
	stop)
		function_stop
		;;
esac

```

# Run
```
./test.sh start
./test.sh stop

```
Shell中变量的单百分号%和双百分号%%的作用

[https://blog.csdn.net/qq_34988540/article/details/102523619](https://blog.csdn.net/qq_34988540/article/details/102523619)  
Shell中单百分号%和双百分号%%的作用

使用百分号将变量的内容从变量的后面删除，并从变量的尾部删除。  
不同的是，一个%号表示从尾部最近的匹配删除，两个%%从尾部最远的匹配删除。同时支持使用通配符。  

```
filename=aabbccaabbcc

echo "${filename%bb*}"
结果:
aabbccaa
可以看到是截取了最后面的bbcc。

使用两个百分号截取尾部bb*
echo "${filename%%bb*}"
结果:
aa
可以看到是截取的最前面的匹配的到的bb。


```

注意： 如果不使用通配符，那么截取的字符串必须是最尾部的，不能是中间的字符。

bash 例子: cat文件内容，赋给变量；if语句判断相等；break语句中止循环

```
echo "OK" > templog.txt
while true
do
	res=$(cat templog.txt)
	echo "=========>checking templog content"
	echo $res
        if [[ $res == "OK" ]]
	then
	        echo "=========>usb.sh stop and sleep 5s"
                ./usb.sh stop
                sleep 5
	        echo "=========>usb.sh start"
                ./usb.sh start
		echo "=========>start scan"
                sudo /home/scm/hostap/wpa_supplicant/wpa_cli -i wlan0 scan > templog.txt
                sleep 5
		echo "=========>print scan_results"
                sudo /home/scm/hostap/wpa_supplicant/wpa_cli -i wlan0 scan_results
        else
		echo "=========>scan fail, stop test"
                break
        fi	
done

```
bash 例子: Timestamp Conversion，转换wpa_supplicant的时间格式

```
#!/bin/bash

input_file=${1}

while read line
do
    epoch=$(echo ${line} | cut -f1 -d":")
    message="$(cut -d ':' -f 2- <<< "${line}")"
    date=$(date +"%a %d %b %T.%N %Z %Y" -d @${epoch})
    echo "${date}:${message}"
done < ${input_file}
I named the file epoch2date.sh. If I give it as input the wpa_supplicant.log file shown in the “Minimum Logs” section above the output appears as follows:


ubuntu@ip-10-0-0-16:~/wifi$ bash epoch2date.sh wpa_supplicant.log

```
bash [[]]模式匹配

```
#!/bin/bash

i=0
int_name="eth0"
times=0
while [ $i -le $times ]
do
echo 'total times: '$i
let 'i++'
echo 'match int_name test start ....'

ifconfig > int.log
sleep 1
cat int.log | while read line
	do
		echo -e "\n"
		echo -e "\n"
		echo "<<<<<<<<<read a new line"
		echo "===>print line"
		echo $line
		echo "===>print int_name"
		echo $int_name
    		if [[ $line == $int_name* ]];
    		then
        		echo '>>>>>>>>>match'
			break
    		elif [[ "$line" != $int_name* ]];
    		then
        		echo 'Continue Checking  ...'
        	else
        		echo 'interface not found ....'
    	        fi	
        done
done

```

# Note
```
1. 下面这一行$line加不加引号都可以
if [[ "$line" == $int_name* ]];
#if [[ $line == $int_name* ]];

2.
if [[ "$line" == $int_name* ]];
一个等于号和两个等于号都可以，是一样的
The equal sign operator can be a single equal sign or a double equals as we have used here. They are the same semantically. 

注意等号两边有空格，等号两边没有空格表示赋值，赋值的话这里始终返回true，始终会执行

3.
等号的右边是匹配的模式
?匹配单个字符
*匹配任意字符
匹配模式不能用引号括起来，括起来后标识精确匹配
例如:
[[ "ab" == a* ]] && echo "ok" ##返回ok
[[ "ab" == "a*" ]] && echo "ok" ##没有返回值



```

# 实例
```
[[ "ab" == a* && 'cd' == c* ]] && echo match ##返回match
[[ "ab" == a* && 'cd' == d* ]] && echo match ##没有返回值
[[ 1 < 2 && 2 < 3 ]] && echo 'ok' ##返回ok
[[ 1 -lt 2 && 3 -gt 2 ]] && echo 'ok' ##返回ok


```

https://www.baeldung.com/linux/use-command-line-arguments-in-bash-script
How to Use Command Line Arguments in a Bash Script

# Positional Parameters 
userReg-positional-parameter.sh

echo "Username: $1";
echo "Age: $2";
echo "Full Name: $3";

sh userReg-positional-parameter.sh john 25 'John Smith'

# Flags

while getopts u:a:f: flag
do
    case "${flag}" in
        u) username=${OPTARG};;
        a) age=${OPTARG};;
        f) fullname=${OPTARG};;
    esac
done
echo "Username: $username";
echo "Age: $age";
echo "Full Name: $fullname";

sh userReg-flags.sh -f 'John Smith' -a 25 -u john


