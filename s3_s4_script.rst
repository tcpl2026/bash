s3 s4 script
=============

s3.sh

::

    #!/usr/bin/bash
    
    NUM=$1
    if [ "x$1x" = "xx" ]
    then 
    	echo "Error: please enter s3 test times ......"
    	exit 1
    fi
    
    echo "System will run s3 test $1 times"
    
    while [ $NUM -gt 0 ]
    do
    	cnum=`expr $1 - $NUM + 1`
    	echo "This is the $cnum time" >> log_s3
    	sleep 40
        rtcwake -m mem -s 40
    	NUM=`expr $NUM - 1`
    done

run

::

    sudo su
    chmod +x s3.sh
    ./s3.sh

s3.sh: v2

::

    #/usr/bin/bash
    
    if [ "x$1x" = "xx" ]
    then
    	echo "Please input test times ......"
    	exit 1
    fi
    
    echo "System will run s3 test $1 times"

    i=1
    while [ $i -le $1 ]
    do
    	echo "This is $i time" >> log_s3
    	sleep 40
    	rtcwake -m mem -s 40
    	let i++
    done

s3.sh: v3

::

    #/usr/bin/bash
    
    if [ "x$1x" = "xx" ]
    then
    	echo "Please input test times ......"
    	exit 1
    fi
    
    for ((i=1; i<=$1; i++))
    do
    	echo "This is $i time" >> log_s3
    	sleep 40
    	rtcwake -m mem -s 40
    done

s3 script check bluetooth interface
------------------------------------

::

    当BT异常时(MAC地址全0)或者没有BT interface，停止运行
    
    for i in {1..500};do
            echo "================ This is the $i time test ================"
            sleep 40
    	    BTMAC=`hciconfig`
    	    echo $BTMAC
    	    if [[ $BTMAC == *"00:00:00:00:00:00"* || $BTMAC == "" ]];then
    	    	    echo "bt mac matched or no bt device, break"
    	    	    break
    	    fi
            /opt/GWRD_ATP/testcases/tools/S3S4S5Tool_EC 40
            systemctl suspend
    done
    
    ---------------------------------------------
    
    for i in {1..500};do
            echo "================ This is the $i time test ================"
            sleep 40
    	    BTMAC=`hciconfig`
    	    echo $BTMAC
    	    if [[ $BTMAC == *"00:00:00:00:00:00"* ]];then
    	    	    echo "bt mac matched, break"
    	    	    break
    	    elif [[ $BTMAC == "" ]];then
    	    	    echo "no bt device, break"
    	    	    break
    	    fi
            /opt/GWRD_ATP/testcases/tools/S3S4S5Tool_EC 40
            systemctl suspend
    done

s3 script check wifi connection and bluetooth
----------------------------------------------

::

    #!/bin/bash

    for ((i=1;i<=500;i++));do
            echo "================ This is the $i time test ================"
            sleep 40

            WIFISTATUS=`sudo wpa_cli -i wlan0 status`
            echo "$WIFISTATUS"
            if [[ "$WIFISTATUS" == *"COMPLETED"* ]];then
                    echo "wifi connected, continue test"
            else
                    echo "wifi disconnected, break"
                    break
            fi

    	    BTMAC=`hciconfig`
    	    echo "$BTMAC"
    	    if [[ "$BTMAC" == *"00:00:00:00:00:00"* ]];then
    	    	    echo "bt mac matched, break"
    	    	    break
    	    elif [[ "$BTMAC" == "" ]];then
    	    	    echo "no bt device, break"
    	    	    break
    	    fi

            sudo rtcwake -m mem -s 40
    done

s4 script
---------------
::

    只要修改rtcwake -m mem -s 40为rtcwake -m disk -s 40


s3 s4放到同一个脚本
-------------------

s3s4.sh

::

    #!/bin/bash
    
    if [ $# -ne 3 ];then
    	echo "usage:"
    	echo "s3: ./s3s4.sh s3 count wlan0"
    	echo "s4: ./s3s4.sh s4 count wlan0"
    	exit 1
    fi
    
    opt=$1
    COUNT=$2
    interval=10
    s3timer=10
    s4timer=10
    
    mkdir -p log
    DATE=$(date +%Y-%m-%d)
    LOG=log/${opt}_${DATE}.log
    cat /dev/null > ${LOG}
    
    echo "=============================== $opt test start ===============================" |tee -a ${LOG}
    
    for (( i=1; i<=$COUNT; i++ ))
    do 
    	if [ $opt == "s3" ];then
    		echo "************************* S3 Cycle: $i start *************************" |tee -a ${LOG}
    		echo `date +%Y-%m-%d' '%H:%M:%S` "Going to S3, Duration "$s3timer" sec" |tee -a ${LOG}
    		sudo rtcwake -m mem -s $s3timer >> ${LOG} 2>&1
    		echo `date +%Y-%m-%d' '%H:%M:%S`" Waitable timer triggered." |tee -a ${LOG}
    		echo `date +%Y-%m-%d' '%H:%M:%S`" Wake up from S3, Cycle "$i"" |tee -a ${LOG}
    		echo `date +%Y-%m-%d' '%H:%M:%S`" Successfully left sleep state S3..." |tee -a ${LOG}
    	elif [ $opt == "s4" ];then
    		echo "************************* S4 Cycle: $i start *************************" |tee -a ${LOG}
    		echo `date +%Y-%m-%d' '%H:%M:%S` "Going to S4, Duration "$s4timer" sec" |tee -a ${LOG}
    		sudo rtcwake -m disk -s $s4timer >> ${LOG} 2>&1
    		echo `date +%Y-%m-%d' '%H:%M:%S`" Waitable timer triggered." |tee -a ${LOG}
    		echo `date +%Y-%m-%d' '%H:%M:%S`" Wake up from S4, Cycle "$i"" |tee -a ${LOG}
    		echo `date +%Y-%m-%d' '%H:%M:%S`" Successfully left sleep state S4..." |tee -a ${LOG}
    	else
    		echo "error input, use s3 or s4 as input"
    	fi
    	a="$(ifconfig -a | grep -A1 $3 | grep -w ether)"
    	if [ -n "$a" ];then
    		echo $i ${a} >> ${LOG}
    	else
    		echo "no device" >> ${LOG}
    	fi
    	echo `date +%Y-%m-%d' '%H:%M:%S` "wake up for $interval seconds" |tee -a ${LOG}
    	echo "************************* $opt Cycle: $i finish *************************" |tee -a ${LOG}
    	#keep wake up time
    	if [ $i -eq $COUNT ];then
    		break;
    	fi
    	sleep $interval
    done
    
    echo "=============================== $opt test finished =============================== " |tee -a ${LOG}

