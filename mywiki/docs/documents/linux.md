### vim编辑器

包括普通模式、插入模式和命令模式    

普通模式，用户可以使用快捷键进行文件的复制、删除、粘贴等操作    

插入模式，按下 i 键进入插入模式，用户可以编辑文本内容，按 Esc 键退出插入模式回到普通模式  

命令模式，按下 : 或者 / 进入命令模式，如保存文件（:w）、退出不保存（:q）、强制退出不保存（:q!）或保存并退出（:wq）  

x,X : 在一行中，x为向后删除一个字符（相当于del键），X为向前删除一个字符（相当于backspace键）  

dd : 删除光标所在的那一整行  

ndd : n 为数字。从光标开始，删除向下n列  

yy : 复制光标所在的那一行  

nyy : n为数字。复制光标所在的向下n行  

p,P : p 为将已复制的数据粘贴到光标的下一行，P则为贴在光标的上一行  

u : 复原前一个操作  

CTRL + r : 重做上一个操作  

小数点 ‘.’：重复前一个动作  

:set number :在每一行设置行标号  

:n1,n2 m n3 移动n1-n2行(包括n1,n2)到n3行之下  

:n1,n2 co n3 复制n1-n2行(包括n1,n2)到n3行之下  

:n1,n2 d 删除n1-n2行(包括n1,n2)行  

### 文件目录指令
#### cd

cd 文件路径  用于切换当前目录，可以是绝对路径和相对路径  

cd /home 进入 ‘/ home’ 目录  

cd .. 返回上一级目录  

cd ../.. 返回上两级目录  

cd / 返回根目录  

cd - 返回上次所在的目录  

#### pwd

pwd 显示当前工作路径  

#### ls

ls 查看目录中的文件  

ls -l 显示文件和目录的详细信息    

ls -a 列出全部文件，含隐藏文件    

ls -S 按照文件或目录大小排序  

#### tree

tree 查看⽂件和⽬录的树形结构 （如果没有需要先安装 yum install tree）  

#### mkdir

mkdir <目录名> 创建目录  

mkdir dir1 dir2 同时创建两个目录  

mkdir -p /tmp/dir1/dir2 递归创建目录树  

#### rmdir

rmdir dir1 删除’dir1’⽬录，只能删除空目录  

#### rm

-f ：即force强制删除  

-i ：互动模式，在删除前会询问用户是否操作  

-r ：递归删除，常用于目录删除  

rm -f file1 删除’file1’⽂件  

rm -rf dir1 删除’dir1’⽬录和其内容  

rm -rf dir1 dir2 同时删除两个⽬录及其内容  

#### cp 

-a 复制文件，保留文件属性  

-r 递归持续复制，用于目录的复制  

-u 目标文件与源文件有差异时才会复制 

cp -a dir1 dir2 复制目录  

cp -a /temp/dir1 . 复制一个目录至当前目录  

#### scp

用于不同主机之间复制文件

scp local_file remote_username@remote_ip:remote_folder 从本地复制到远程主机  

scp remote_username@remote_ip:remote_file local_folder 从远程主机复制到本地  

scp **-r** local_folder remote_username@remote_ip:remote_folder 拷贝整个目录到远程主机  

#### mv 

-f  也即force强制，如果目标文件已经存在，不会询问而直接覆盖  
-i 若目标文件已经存在，就会询问是否覆盖  
-u 若目标文件已经存在，且比目标文件新，才会更新  
mv old_dir new_dir 重命名/移动⽬录  

### 查看文件内容

#### awk 

对文件进行指定规则浏览和抽取信息，可用于**日志分析 ** 

awk '{print $0}' file.txt 打印文件所有行  

awk '{print $1, $3}' file.txt  打印文件中每行的第一和第三个字段  

awk '$3 > 50 {print $1, $3}' file.txt  基于过滤条件打印，打印第三字段大于50的行的第一和第三字段‌  

awk '{print NR, $0}' file.txt  NR是内置变量表示行号，打印每行的行号和该行的内容‌  

awk -F : '{print $1}' /etc/passwd  使用冒号作为分隔符打印`/etc/passwd`文件中第一个字段  

awk 'NR%2==0' filename 提取偶数行  

awk 'NR%2==1' filename 提取奇数行  

#### grep 

grep ss hello.txt 在⽂件hello.txt中查找关键词 ss  

grep ^s hello.txt 在⽂件hello.txt中查找以 s 开头的内容  

grep [0-9] hello.txt 选择hello.txt⽂件中所有包含数字的⾏    

grep -r "text" /path/to/search  在目录下递归搜索包含text的文件

#### sed

用于文本替换、删除、插入和打印

sed 's/old/new/' file.txt 替换命令s, 将文本中old字符串替换为new字符串  

sed '2d' file.txt 删除命令d，删除文件中第二行

sed '3i\new line' file.txt 插入命令i，第3行之前插入new line

sed -n '3p' /etc/passwd 打印命令p，打印文件第3行

sed -n '2,5p' /etc/passwd 打印文件第2到5行

#### cat 

cat file 查看整个文件的内容  

cat -n file 查看文件内容，并给每一行加上行号  

cat file.txt | awk ‘NR%2==1’ 提取文件奇数行  

#### more

more file   分页显示文件内容  

可以使用空格键或Enter键向下翻页，使用b键向上翻页，但无法使用上下箭头键或Page Up/Down键进行翻页  

q退出

more‌适合于查看较小的文件或不需要频繁翻页的情况，因为它会在启动时加载整个文件  

#### less

less file  类似 more 命令，分页显示文件内容  

可以使用空格键、Enter键向下翻页，使用b键向上翻页，还可以使用上下箭头键或Page Up/Down键进行翻页  

q退出  

‌less‌适合于查看较大的文件或需要频繁翻页和搜索的情况，因为它不需要一次性加载整个文件，加载速度更快  

#### head/tail

head -n 2 file1 查看一个文件的前两行  

tail -n 2 file1 查看一个文件的最后两行  

tail -n +1000 file1 从1000行开始显示，显示1000行以后的  

tail -f /log/msg 实时查看添加到⽂件中的内容  

cat filename | head -n 3000 | tail -n +1000 显示1000行到3000行  

cat filename | tail -n +3000 | head -n 1000 从第3000行开始，显示1000(即显示3000~3999行)  



### 文件搜索

#### find 

find / -name file 从根目录开始搜索文件/目录  
find / -user user1 搜索用户 user1 的文件/目录  
find /dir -name .bin 在目录/dir 中搜索带有 .bin 后缀的文件  
find /usr/bin -type f -atime +100 搜索在过去100天内未被使用过的执行文件  
find /usr/bin -type f -mtime -10 搜索在10天内被创建或者修改过的文件  

#### locate

locate *.mp4 寻找 .mp4结尾的文件 通过索引数据库检索

#### whereis 

whereis 关键词  whereis 主要是针对 /bin /sbin 下面的可执行文件来查找

#### which

which 默认是找 环境变量PATH 内所规范的目录

### 文件权限

#### chmod

chmod 777 文件名 修改文件权限（最高权限）  
chmod ugo+rwx dir 设置目录的所有人(u)、群组(g)以及其他人(o)以读（r，4 ）、写(w，2)和执行(x，1)的权限  
chmod go-rwx dir1 删除群组(g)与其他人(o)对目录的读写执行权限  
chmod +x 文件路径 为所有者、所属组和其他用户添加执行的权限  
chmod -x 文件路径 为所有者、所属组和其他用户删除执行的权限  
chmod u+x 文件路径 为所有者添加执行的权限  
chmod g+x 文件路径 为所属组添加执行的权限  
chmod o+x 文件路径 为其他用户添加执行的权限  
chmod ug+x 文件路径 为所有者、所属组添加执行的权限  
chmod =wx 文件路径 为所有者、所属组和其他用户添加写、执行的权限，取消读权限  
chmod ug=wx 文件路径 为所有者、所属组添加写、执行的权限，取消读权限  

#### chown

chown user file 改变文件所有者  

chown -R user dir 改变目录的所有者并同时改变目录下所有文件的所有者  

chown user1:group1 file1 改变一个文件的所有者和所属组  

#### chgrp 

chgrp group1 file1 改变文件的群组  

### 打包和压缩文件

#### zip/unzip

zip file1.zip file1 创建一个zip格式的压缩包  

unzip file1.zip 解压一个zip格式压缩包  

zip -r file1.zip file1 file2 dir1 将几个文件和目录同时压缩成一个zip格式的压缩包  

### 进程相关

#### ps 

ps -ef  显示所有进程的详细信息  

ps aux 类似ps -ef  

ps -ef | grep 进程名

#### pstree

pstree -aup  查看正在运行的进程，树状结构显示  

#### netstat

netstat -lntp  查看进程和使用的端口号  

netstat -a 列出所有连接

#### pgrep

pgrep  进程名称  专门用于进程查询的grep  

#### kill 

kill pid 正常杀死进程

kill -9 pid   -9表示强制杀死进程 

kill -9 程序的名字  

pkill 程序的名字  pkill=pgrep + kill 找到并杀死进程  
