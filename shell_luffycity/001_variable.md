
# 1

# 注意: 变量和值之间不得有空格
# 原因:
# 有空格时shell会认为是一个命令，跟了参数，会执行这个命令，但是这个命令并不存在，所以会报错

# 例如:
# name = "hello"
# Command 'name' not found, did you mean:
# 把name认为是一个命令了

# 2
echo $name # 简写
echo ${name} # 带花括号，完整形式


# 3
# 单引号变量，不识别特殊语法
# 双引号变量，能识别特殊符号

# 4
echo $name
echo $?

返回0 上一条命令执行成功
1-255: 错误码

