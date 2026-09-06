# 用户个人配置文件:
~/.bash_profile
~/.bashrc

用户登录后先执行.bash_profile，再执行.bashrc
实际上是只执行.bash_profile, .bash_profile里写了执行.bashrc的命令

# 全局配置文件
/etc/profile
/etc/bashrc
系统建议最好创建在/etc/profile.d/, 而非直接修改主文件
修改全局配置文件, 影响所有登录系统的用户

# 检查系统环境变量的命令
set 输出所有变量, 包括全局变量、局部变量
declare 输出所有的变量，如同set
env 只显示全局变量
export 显示和设置环境变量值

# 撤销环境变量
unset 变量名, 删除变量或函数


