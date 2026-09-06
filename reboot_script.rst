reboot script
==============

reboot.sh

::

    #!/bin/bash
    
    HOMEDIR="/home/scm/"
    WIFIINTERFACE="wlan0"
    
    reboot_log_file="${HOMEDIR}reboot_log.txt"
    reboot_times_file="${HOMEDIR}reboot_times.txt"
    if [ ! -e "${reboot_times_file}" ];then
    	echo "1" > ${reboot_times_file}
    fi
    
    i=$(cat ${reboot_times_file})
    
    if [ "$i" -le 500 ];then
            sleep 40
    
            WIFISTATUS=`sudo wpa_cli -i ${WIFIINTERFACE} status`
            BTMAC=`hciconfig`
            if [[ "$WIFISTATUS" == *"COMPLETED"* && "$BTMAC" != *"00:00:00:00:00:00"* && "$BTMAC" != "" ]];then
                    echo "================ This is the $i time test ================" >> ${reboot_log_file}
                    current_time=`date +%Y%m%d-%H%M%S`
                    echo ${current_time} >> ${reboot_log_file}
                    echo >> ${reboot_log_file}
                    echo "$WIFISTATUS" >> ${reboot_log_file}
                    echo >> ${reboot_log_file}
                    echo "$BTMAC" >> ${reboot_log_file}
                    echo >> ${reboot_log_file}
                    echo "wifi and bt are normal, will reboot" >> ${reboot_log_file}
                    
                    let i++
                    echo $i > ${reboot_times_file}
                    
                    sudo reboot
            else
                    echo "wifi or bt may be abnormal, please confirm" >> ${reboot_log_file}
            fi
    fi
    

::

    crontab -e

    @reboot /home/scm/reboot.sh

