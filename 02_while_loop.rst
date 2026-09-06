while loop
===========

::

    i=1
    while [ $i -le 10 ]
    do
    	echo $i
    	let i++
    done


while无限循环(infinite loops)

::

    #!/bin/bash
    while :
    do
    	echo "infinite loops [ hit CTRL+C to stop]"
    done

::

    #!/bin/bash
    while true
    do
    	echo "Press CTRL+C to stop the script execution"
    	# Enter your desired command in this block.
    done

::
    
    while true; do iperf -c www.example.com -p 9000 -i 1 -t 5; done;


::

    while true
    do
      echo "This is an infinite while loop. Press CTRL + C to exit out of the loop."
      sleep 1
    done


::

    Historically, Bourne shells didn't have true and false as built-in commands. 
    true was instead simply aliased to :, and false to something like let 0.
    Nowadays (that is: in a modern context) you can usually use either : or true. 
    Both are specified by POSIX, and some find true easier to read. 
    However there is one interesting difference: : is a so-called POSIX special built-in, whereas true is a regular built-in.
    ---------
    
    bash colon. It is a built-in utility which simply exits with 0. 
    In the other words, it is almost equivalent to the true command. For example, we can write a spin loop with:
    
    while :; do
        date -R
        sleep 1
    done
    It is almost equivalent to:
    
    while true; do
        date -R
        sleep 1
    done
    ---------
    等同于true ， while ：就是while true

while loop one-liner syntax:

::

    while [ condition ]; do commands; done
    while control-command; do COMMANDS; done
    
    while true; do echo "test";sleep 2; done


finite loop 

::

    i=1
    times=30
    
    while [ $i -le $times ]
    do
            echo "$i"
            let 'i++'
    done

