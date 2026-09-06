命令行中的双引号
=================

::

    要输入双引号本身，例如
    sudo wpa_cli -i wlan0 set_network 0 ssid "tuf2g"
    需要转义
    sudo wpa_cli -i wlan0 set_network 0 ssid \"tuf2g\", man bash: \"     double quote
    或者
    sudo wpa_cli -i wlan0 set_network 0 ssid '"'tuf2g'"'
    sudo wpa_cli -i wlan0 set_network 0 ssid "\""tuf2g"\""



