## 当命令的输出结果看不懂是否有报错时，使用`echo $?`来检测上一条命令是否有报错，返回`0`表示无报错，返回`非0`的数即为有报错

------

## 破解密码（忘记 root 密码处理）

1. 开机（或重启），在 **内核选择界面** 按 **e**

2. 找到 **Linux16** 所在行，光标移动到行末，添加：

   ```
   rd.break console=tty0
   ```

   然后按 **Ctrl+X** 启动

3. 系统进入 **紧急救援模式**，提示符为：

   ```
   switch_root:/#
   ```

4. 重新挂载真实根目录为可读写模式：

   ```
   mount -o remount,rw / /sysroot
   ```

5. 切换根目录环境：

   ```
   chroot /sysroot
   ```

   （提示符会从 `switch_root:/#` 变为 `sh-4.2#`）

6. 修改 root 密码：

   ```
   passwd
   ```

   按提示输入新密码

7. 重置 SELinux 上下文：

   ```
   touch /.autorelabel
   ```

8. 退出当前环境：

   ```
   exit
   ```

9. 重启：

   ```
   reboot
   ```

------

## BASH 常用快捷键

- `Ctrl + c` ：终止当前命令
- `Ctrl + l` ：清屏
- `Ctrl + u` ：删除光标前所有内容
- `Ctrl + w` ：删除光标前一个单词
- `Ctrl + Shift + t` ：新建终端标签页
- `↑ / ↓` ：调取历史命令
- `ESC + .` 或 `Alt + .` ：粘贴上一个命令的参数
- `Ctrl + - / +` ：调整终端字体大小
- `Ctrl + s` ：锁定终端
- `Ctrl + q` ：解锁终端

------

## 时间与日历

#### date

**作用**：查看/修改系统时间
 **常用参数**：

- `+%F` → 显示日期 (YYYY-MM-DD)
- `+%R` → 显示时间 (HH:MM)
- `+"%Y-%m-%d %H:%M:%S"` → 显示完整时间
- `-s "时间"` → 设置系统时间

**示例：**

```bash
date
date +%F
date +%R
date +"%Y-%m-%d %H:%M:%S"
sudo date -s "2025-09-11 10:00:00"
```

------

#### cal

**作用**：显示日历
 **常用参数**：

- `-1` → 当前月（默认）
- `-3` → 上月、本月、下月
- `-y` → 整年
- `-s` → 周日为一周第一天
- `-m` → 周一为一周第一天

示例：

```bash
cal
cal -3
cal -y
cal -m 2025
```

------

## 文件与目录操作

### pwd

**作用**：显示当前目录

示例：

```bash
pwd
```

------

### ls

**作用**：列出目录内容
 **常用参数**：

- `-l` → 显示详细信息
- `-h` → 人类可读方式显示文件大小（需配合 `-l`）
- `-d` → 查看目录本身信息
- `-A` → 显示隐藏文件

示例：

```bash
ls
ls -lh
ls -ld /etc
ls -A ~
```

------

### cd

**作用**：切换目录
 **常用参数/用法**：

- `cd -` → 上一次目录
- `cd ..` → 上一级目录
- `cd ~` → 回到家目录

示例：

```bash
cd /etc
cd ..
cd -
cd ~
```

------

### touch

**作用**：创建空文件

示例：

```bash
touch test.txt
```

------

### mkdir

**作用**：创建目录
 **常用参数**：

- `-p` → 递归创建多级目录

示例：

```bash
mkdir dir1
mkdir -p a/b/c
```

------

### cp

**作用**：复制文件或目录
 **常用参数**：

- `-r` → 递归复制目录
- `-f` → 强制覆盖
- `-p`→ 复制后不改变文件属性 

示例：

```bash
cp file1.txt file2.txt
cp -r dir1 dir2
cp -pf file1.txt /tmp/
```

------

### mv

**作用**：移动/重命名文件

示例：

```bash
mv old.txt new.txt
mv file.txt /tmp/
```

------

### rm

**作用**：删除文件或目录
 **常用参数**：

- `-r` → 递归删除目录
- `-f` → 强制删除（无提示）

示例：

```bash
rm file.txt
rm -rf dir1
```

------

## 文件内容查看

### cat

**作用**：查看小文件内容
 **常用参数**：

- `-n` → 显示行号

示例：

```bash
cat file.txt
cat -n file.txt
```

------

### less

**作用**：分页查看大文件
 **常用操作**：

- `Space` / `PageDown` → 下一页
- `PageUp` → 上一页
- `/关键字` → 搜索
- `n` / `N` → 下一个/上一个匹配
- `q` → 退出

示例：

```bash
less /var/log/messages
```

------

## 文本统计

### wc

**作用**：统计文件信息
 **常用参数**：

- `-l` → 行数
- `-w` → 单词数
- `-c` → 字节数

示例：

```bash
wc file.txt
wc -l file.txt
wc -w file.txt
wc -c file.txt
```

------

## 系统管理

### reboot

**作用**：重启系统

### poweroff

**作用**：关机

### shutdown

**作用**：定时关机/重启
 **常用参数**：

- `-h now` → 立即关机
- `-h +10` → 10 分钟后关机
- `-h 10:10` → 指定时间关机
- `-r now` → 立即重启

------

## 运行级别（RHEL6）

- `0` → 关机
- `1` → 单用户模式
- `2` → 字符界面（无网络）
- `3` → 字符界面（有网络）
- `4` → 未定义
- `5` → 图形界面
- `6` → 重启

切换命令：

```bash
init [运行级别]
```

------

## 文本编辑（vim）

### 模式切换

- `i` → 插入模式
- `:` → 末行模式
- `Esc` → 返回命令模式

### 常用命令

- `:wq` → 保存并退出
- `:q!` → 强制退出不保存
- `u` → 撤销
- `Ctrl+r` → 取消撤销
- `/关键字` → 搜索

### 末行模式参数

- `:set nu` → 显示行号
- `:set nonu` → 取消行号
- `:%s/旧/新/g` → 全文替换
- `:n,m s/旧/新/g` → 指定行替换

------

## 通配符

- `*` → 任意多个字符
- `?` → 单个字符
- `[x-y]` → 匹配范围
- `{a,b,c}` → 多个匹配

------

## 管道符 |

**作用**：将前一命令的输出作为后一命令的输入

------

## grep 文本过滤

**作用**：过滤文本内容
 **常用参数**：

- `-n` → 显示行号
- `-v` → 取反（排除匹配）
- `-i` → 忽略大小写
- `-c` → 统计匹配行数

**常用匹配符**：

- `关键词` → 匹配包含的行
- `^关键词` → 匹配开头
- `关键词$` → 匹配结尾
- `^$` → 匹配空行

示例：

```bash
grep root /etc/passwd
grep -n root /etc/passwd
grep -v root /etc/passwd
grep -i user file.txt
grep -c bash /etc/passwd
```

------

## 重定向

**作用**：改变输入/输出位置

**输出重定向**：

- `>` → 正确输出覆盖
- `>>` → 正确输出追加
- `2>` → 错误输出覆盖
- `2>>` → 错误输出追加
- `&>` → 正确+错误覆盖
- `&>>` → 正确+错误追加

**输入重定向**：

- `< file` → 从文件输入
- `<< EOF` → 从标准输入直到 EOF

------

## 文件搜索

### find

**作用**：递归查找文件

**常用参数**：

- `-a` → 条件 1 **并且** 条件 2（逻辑与，默认可省略）
- `-o` → 条件 1 **或者** 条件 2（逻辑或）
- `-type` → 按类型查找
  - `f` → 普通文件
  - `d` → 目录
  - `l` → 软链接
- `-name "文件名"` → 按名称匹配（支持通配符）
- `-iname "文件名"` → 按名称匹配（忽略大小写）
- `-size [+/-N][kMG]` → 按大小查找
  - `+N` → 大于 N
  - `-N` → 小于 N
  - `N` → 等于 N
  - `k`=KB，`M`=MB，`G`=GB
- `-user 用户名` → 查找属于某用户的文件
- `-group 组名` → 查找属于某组的文件
- `-maxdepth n` → 限制搜索深度（最多搜索 n 层目录）
- `-exec command {} \;` → 对结果执行指定命令
  - `{}` 代表找到的文件
  - `\;` 表示命令结束
- **时间相关**
  - `-atime n` → 访问时间（n 天前访问的文件）
  - `-mtime n` → 修改时间（内容被修改）
  - `-ctime n` → 改变时间（权限/属性被修改）
    - `+n` → 大于 n 天
    - `-n` → 小于 n 天
    - `n` → 正好 n 天
  - `-newer file` → 比指定文件新的文件

- **权限相关**
  - `-perm mode` → 按权限查找（如 `644`）
  - `-perm -mode` → 必须包含所有指定权限
  - `-perm /mode` → 包含任意一个指定权限**逻辑控制**
- **逻辑控制**
  - `! 条件` → 取反（例如：`! -user root` → 非 root 用户的文件）
  - `\( 条件 \)` → 使用括号改变逻辑运算优先级（注意转义）

- **输出与操作**
  - `-print` → 输出匹配的文件（默认行为，常省略）
  - `-ls` → 类似 `ls -l` 显示详细信息
  - `-ok command {} \;` → 执行命令前需确认（比 `-exec` 安全）

**示例**：

```bash
find /etc -type f -name "*.conf"         # 查找 /etc 下的配置文件
find /var -size +100M                    # 查找大于 100M 的文件
find /home -user root                    # 查找 root 用户的文件
find /tmp -maxdepth 1 -type f            # 限制只查找一层
find /var/log -name "*.log" -exec rm {} \;   # 删除所有 .log 文件
find /var/log -mtime -7           # 最近 7 天修改过的文件
find /home -atime +30             # 30 天没访问过的文件
find /etc -perm 644               # 权限为 644 的文件
find / -user root ! -group root   # 属于 root 用户但不属于 root 组的文件
find /tmp \( -name "*.log" -o -name "*.tmp" \) -delete   # 删除日志和临时文件
find . -type f -print | xargs wc -l   # 统计所有文件行数
```

------

### **locate**

- **格式用法**：

  ```
  locate 文件名
  ```

- **作用**：
  根据系统维护的数据库（通常是 `/var/lib/mlocate/mlocate.db`）快速查找文件路径。

  > ⚠️ 数据库不是实时更新的，需要使用 `updatedb` 命令手动或定时更新，否则可能查不到新文件。

------

### **whereis**

- **格式用法**：

  ```
  whereis 命令名
  ```

- **作用**：
  查找命令的二进制文件、源代码和帮助文档的位置。

  > 适用于可执行文件相关资源的定位。

------

## 获取帮助

**作用**：获取命令说明
 **常用方式**：

- `command --help` → 查看帮助
- `man command` → 手册页
- `pinfo command` → 类似 man，有超链接

**快捷操作**：

- `/关键词` → 搜索
- `n / N` → 下一个/上一个匹配
- `q` → 退出

------

## 查找命令所在位置

**which**

- **格式用法**：

  ```
  which 命令名
  ```

- **作用**：
   显示 **可执行命令** 的绝对路径（即用户在当前 PATH 环境变量下能找到的实际路径）。

  > 如果命令是 shell 内置命令，`which` 可能不会显示结果。

------

## 别名

**alias**

**功能**：为常用命令创建快捷方式，提高操作效率

- 设置别名

  ```bash
  alias ll='ls -lh --color=auto'  # 常用列表显示
  alias la='ls -lha --color=auto' # 显示所有文件（包括隐藏文件）
  alias grep='grep --color=auto'  # 搜索结果高亮显示
  ```

  > 注：临时别名重启终端后失效，永久生效需写入`~/.bashrc`或`~/.zshrc`

- 查看所有别名

  ```bash
  alias  # 列出当前系统中所有已定义的别名
  ```

- 取消别名

  ```bash
  unalias ll  # 取消指定别名
  unalias -a  # 取消所有别名
  ```

------

## 文件权限与用户

### 1. 用户管理基础

- **登录系统**：用户通过账号密码登录，不同用户拥有不同身份和权限。
- **用户与组关系**：每个用户至少属于一个组（基本组），可加入多个附加组。

------

### 2. 用户操作命令

添加用户（`useradd`）

**功能**：创建新用户账号
 **语法**：

```bash
useradd [选项] 用户名
```

**常用选项**：

- `-u UID`：指定用户的 UID
- `-d /家目录路径`：指定用户家目录
- `-s /shell路径`：指定登录 shell（如 `/bin/bash` 可登录，`/sbin/nologin` 禁止登录）
- `-G 附加组名`：指定用户的附加组

修改用户属性（`usermod`）

**功能**：修改用户信息
 **语法**：

```bash
usermod [选项] 用户名
```

**常用选项**：

- `-u UID`：修改用户 UID
- `-d /家目录路径`：修改用户家目录（仅修改配置，不会自动创建）
- `-s /shell路径`：修改默认登录 shell
- `-G 附加组名`：修改用户的附加组

删除用户（`userdel`）

**功能**：删除用户账号
 **语法**：

```bash
userdel [选项] 用户名
```

**常用选项**：

- `-r`：同时删除用户的家目录

密码管理（`passwd`）

**功能**：管理用户密码
 **语法**：

```bash
passwd [用户名]
```

**常用方式**：

- `passwd test01` → 交互式修改 test01 密码
- `echo 123456 | passwd --stdin test01` → 非交互式修改密码
- `passwd` → 修改当前用户密码

用户信息查询

- `head -1 /etc/passwd` → 查看用户信息（格式：用户名:x:UID:GID:描述:家目录:shell）
- `id 用户名` → 查看用户 UID、GID 和组信息

临时切换用户（`su`）

**功能**：切换用户身份
 **语法**：

```bash
su - 用户名   # 切换并加载环境变量
```

------

### 3. 组管理

组的分类

- **基本组**：与用户名同名，用户创建时自动生成。
- **附加组（从属组）**：用户可以加入多个附加组，获取额外权限。

组操作命令

创建组（`groupadd`）

```bash
groupadd 组名          # 创建组
groupadd -g GID 组名   # 指定 GID 创建组
```

删除组（`groupdel`）

```bash
groupdel 组名   # 组内无用户才能删除
```

组成员管理（`gpasswd`）

```bash
gpasswd -a 用户名 组名   # 添加用户到组
gpasswd -d 用户名 组名   # 从组中移除用户
```

组信息查询

```bash
tail -1 /etc/group   # 查看组信息（格式：组名:x:GID:成员列表）
```

------

### 4. 权限类型

- **r（read）读**
  - 文件：允许查看内容
  - 目录：允许查看目录结构（`ls`）
- **w（write）写**
  - 文件：允许修改文件内容
  - 目录：允许更改目录内容（创建、删除、移动文件等）
- **x（execute）执行**
  - 文件：允许运行（如脚本、二进制程序）
  - 目录：允许进入目录（`cd`）

------

### 5. 权限作用对象

- **u（user）所有者**：文件的属主
- **g（group）所属组**：文件的属组，一般是所有者的基本组
- **o（others）其他用户**：除所有者和所属组外的用户

------

### 6. 权限在不同对象上的意义

**文本文件**

- `r` → 可读：`cat`、`less`、`head`、`tail`
- `w` → 可写：`vim`、`>`、`>>`
- `x` → 可执行：脚本/程序可以直接运行

**目录**

- `r` → 可列出：`ls`
- `w` → 可修改：`rm`、`mv`、`cp`、`mkdir`、`touch`
- `x` → 可进入：`cd`

⚠ 注意：

- 如果**父目录没有执行权限 `x`**，即使子文件有权限也无法访问。

------

### 7. 查看权限

**查看文件或目录权限**

```bash
ls -l 文件名
ls -ld 目录名
```

**示例**

```bash
[root@localhost ~]# ls -ld /etc/
drwxr-xr-x. 133 root root 8192 Sep  7 18:37 /etc/
```

- `d` → 目录
- `rwxr-xr-x` → 权限串
- `133` → 子目录/硬链接数
- `root root` → 所有者和所属组
- `8192` → 目录大小（字节）

```bash
[root@localhost ~]# ls -l /etc/passwd
-rw-r--r--. 1 root root 2037 Sep  7 18:27 /etc/passwd
```

- `-` → 普通文件
- `rw-r--r--` → 权限串
- `1` → 硬链接数
- `root root` → 所有者和所属组
- `2037` → 文件大小（字节）

------

### 8. 文件类型标识

- `-` → 普通文件
- `d` → 目录
- `l` → 软链接（快捷方式）
- `c` → 字符设备文件（/dev/tty）
- `b` → 块设备文件（/dev/sda）
- `s` → 套接字文件
- `p` → 管道文件

------

### 9. 判断用户是否有某个文件权限的步骤

**第一步：查看文件的权限归属关系**

```bash
ls -l /etc/passwd
-rw-r--r--. 1 root root 2037 Sep  7 18:27 /etc/passwd
```

解析结果：

- **所有者**：`root`
- **所属组**：`root`
- **权限串**：
  - 所有者：`rw-`
  - 所属组：`r--`
  - 其他用户：`r--`

**第二步：判断访问者的角色**

判断当前用户相对于该文件的角色：

- 如果用户是 `root` → 属于 **所有者**
- 如果用户在文件的所属组里 → 属于 **组**
- 如果都不是 → 属于 **其他用户**

**优先级**：所有者 > 所属组 > 其他用户
 **规则**：匹配即停止，不会继续往下判断。

**第三步：命令权限（执行权限 vs 系统角色）**

- **普通用户**可以使用 `chmod` 修改**自己拥有的文件**的权限。
- **root 用户**可以修改**系统中任意文件**的权限。

```bash
# 普通用户创建一个文件
touch test.txt
chmod 600 test.txt   # ✅ 可以改权限
chmod 600 /etc/passwd  # ❌ 不行，没权限
```

------

### 10. chmod：修改权限

**语法：**

```bash
chmod [选项] {u,g,o,a} {+,-,=} {r,w,x} 文件/目录路径
```

角色

- **u**：所有者 (user)
- **g**：所属组 (group)
- **o**：其他用户 (others)
- **a**：所有用户 (all，相当于 `ugo`)

权限

- **r**：读 (read)
- **w**：写 (write)
- **x**：执行 (execute)

操作符

- **+**：添加权限
- **-**：去掉权限
- **=**：直接设置权限（会覆盖原有的权限）

------

**常用选项**

- **-R**：递归修改目录及其下所有文件权限
- **-v**：显示每个文件权限更改的详细信息 (verbose)
- **-c**：仅在权限实际改变时显示信息 (changes)
- **--reference=文件**：将目标文件权限修改为和参考文件相同

```bash
chmod --reference=ref.txt target.txt   # 设置 target.txt 权限与 ref.txt 相同
chmod -R 755 /var/www/html        # 递归修改目录及文件权限
chmod -v 644 *.txt                # 修改多个 txt 文件的权限并显示过程
chmod -c u-w,g+w data.log         # 仅在权限改变时提示
chmod a=rw shared.txt             # 所有人只能读写，不能执行
```

------

**使用方式**

符号模式（u/g/o/a + r/w/x）

```bash
chmod u+x file.sh       # 给文件所有者增加执行权限
chmod g-w file.txt      # 移除组用户的写权限
chmod o=r file.txt      # 设置其他用户只有读权限
chmod a+r file.txt      # 给所有用户添加读权限
chmod ug+rw dir/ -R     # 递归地给所有者和组添加读写权限
```

数字模式（八进制表示法）

权限数值对应关系：

- **r=4，w=2，x=1**
- 三位数字分别表示 **u, g, o** 的权限

```bash
chmod 644 file.txt      # rw-r--r-- (所有者读写，组用户只读，其他用户只读)
chmod 755 script.sh     # rwxr-xr-x (所有者可读写执行，其他人只读+执行)
chmod 700 private.txt   # rwx------ (仅所有者可读写执行)
chmod 777 test.sh       # rwxrwxrwx (所有人完全权限，通常不推荐)
```

------

### 11. chown：修改文件/目录的所有者和所属组

**语法：**

```bash
chown [选项] 所有者:所属组 文件/目录路径
chown [选项] 所有者 文件/目录路径
chown [选项] :所属组 文件/目录路径
```

参数说明

- **所有者**：新的文件所有者 (user)
- **所属组**：新的用户组 (group)
- **所有者:所属组**：同时修改文件的所有者和组

------

常用选项

- **-R**：递归修改目录及其下所有文件和子目录的归属
- **-v**：显示每个文件归属更改的详细信息 (verbose)
- **-c**：仅在归属实际改变时显示信息 (changes)
- **--reference=文件**：将目标文件的归属修改为和参考文件相同

```bash
chown -v user2 test.log           # 修改并显示过程
chown -c root:root *.conf         # 批量修改配置文件的所有者和组
chown -R www-data:www-data /var/www/html   # 网站目录归属修改
chown --reference=old.sh new.sh   # 让新文件的归属与旧文件保持一致
```

------

**使用方式**

1. 修改所有者

```bash
chown root file.txt        # 修改文件所有者为 root
chown user1 mydoc.txt      # 修改文件所有者为 user1
```

2. 修改所属组

```bash
chown :staff file.txt      # 修改文件所属组为 staff
chown :developers *.c      # 修改所有 .c 文件的所属组为 developers
```

3. 修改所有者和所属组

```bash
chown root:root file.txt   # 修改所有者和所属组为 root
chown user1:staff report   # 修改所有者为 user1，组为 staff
```

4. 递归修改

```bash
chown -R user1:staff /project   # 修改目录及其下所有文件的所有者和所属组
```

5. 参考模式

```bash
chown --reference=ref.txt target.txt   # 设置 target.txt 的归属与 ref.txt 相同
```

------

### 12. 特殊权限（附加权限）

#### Set GID (SGID)

作用

- 在 **目录** 上：让该目录下新建的文件或子目录 **自动继承父目录的所属组**。

使用范围

- 目录

设置命令

```bash
chmod g+s /目录路径    # 开启
chmod g-s /目录路径    # 关闭
```

特征

- SGID 附加在 **所属组执行位 (g+x)** 上
- **显示效果**：
  - `s` → 组原本有执行权限 (x)
  - `S` → 组原本没有执行权限 (-)

示例

```bash
chmod g-x /java/
ls -ld /java/
# drwxr-S---. 2 java1 java 6 Sep  9 17:34 /java/

chmod g+x /java/
ls -ld /java/
# drwxr-s---. 2 java1 java 6 Sep  9 17:34 /java/
```

------

#### Set UID (SUID)

作用

- 在 **可执行文件** 上：执行该文件时，用户会临时获得该文件 **所有者的权限**。

使用范围

- 可执行文件

设置命令

```bash
chmod u+s /路径/可执行文件    # 开启
chmod u-s /路径/可执行文件    # 关闭
```

特征

- SUID 附加在 **所有者执行位 (u+x)** 上
- **显示效果**：
  - `s` → 所有者原本有执行权限 (x)
  - `S` → 所有者原本没有执行权限 (-)

------

#### Sticky Bit (SBIT)

作用

- 适用于 **所有人可写的目录**，防止用户删除或修改别人文件。
- 即：目录里，**只有文件的所有者才能删除/修改自己的文件**。

使用范围

- 目录（典型目录：`/tmp`）

设置命令

```bash
chmod o+t /目录路径    # 开启
chmod o-t /目录路径    # 关闭
```

特征

- Sticky Bit 附加在 **其他用户执行位 (o+x)** 上
- **显示效果**：
  - `t` → 其他用户原本有执行权限 (x)
  - `T` → 其他用户原本没有执行权限 (-)

| 权限类型 | 用途                             | 使用范围   | 设置方式    | 权限显示标志   |
| -------- | -------------------------------- | ---------- | ----------- | -------------- |
| **SUID** | 用户执行文件时临时获得所有者权限 | 可执行文件 | `chmod u+s` | `s / S` (u 位) |
| **SGID** | 子文件继承目录所属组             | 目录       | `chmod g+s` | `s / S` (g 位) |
| **SBIT** | 防止删除别人文件（仅能删自己）   | 开放写目录 | `chmod o+t` | `t / T` (o 位) |

## ACL（Access Control List）权限管理

### 1. ACL 简介

- **ACL（访问控制列表）** 用于在传统 `rwx` 权限之外，提供更精细的文件权限控制。
- 可以针对 **个别用户** 或 **个别组** 设置独立权限，而不局限于文件的 **所有者、所属组、others** 三类。
- 常见应用场景：
  - 某目录对大多数人只读，但需要额外给某个用户写权限。
  - 多个项目成员在同一目录下，需要不同的访问权限。

------

### 2. ACL 命令工具

Linux 常用命令：

- **设置 ACL** → `setfacl`
- **查看 ACL** → `getfacl`

------

### 3. setfacl 用法

格式：

```bash
setfacl [选项] [u|g]:用户名或组名:权限 /路径/文件或目录
```

#### 常用选项：

- `-m` → 修改或添加 ACL 权限
- `-x` → 删除指定用户或组的 ACL 权限
- `-b` → 删除所有 ACL 权限
- `-R` → 递归修改目录及子文件

#### 示例：

```bash
# 给用户 alice 添加读写权限
setfacl -m u:alice:rw /data/file

# 给组 dev 添加读写执行权限
setfacl -m g:dev:rwx /data/dir

# 删除用户 bob 的 ACL
setfacl -x u:bob /data/file

# 删除所有 ACL（恢复到传统权限模式）
setfacl -b /data/file

# 递归为目录及子目录文件设置 ACL
setfacl -R -m u:charlie:rwx /data/project
```

------

### 4. 查看 ACL 权限

```bash
getfacl /路径/文件
```

示例输出：

```
# file: test.txt
# owner: root
# group: root
user::rw-
user:alice:rwx
group::r--
mask::rwx
other::r--
```

字段说明：

- `user::rw-` → 文件所有者的基本权限
- `user:alice:rwx` → 用户 alice 的独立 ACL
- `group::r--` → 所属组权限
- `mask::rwx` → ACL 权限的最大值（mask 会限制 user/group 的实际权限）
- `other::r--` → 其他用户权限

------

### 5. 注意事项

1. **mask 的作用**：

   - mask 定义了所有 ACL 权限的上限。
   - 如果 ACL 给了用户 `rw-`，但 mask 是 `r--`，最终该用户实际权限是 `r--`。
   - 修改 ACL 后，系统会自动提示 mask 是否被覆盖。

   更新 mask 的方法：

   ```bash
   setfacl -m m:rw /data/file
   ```

   > 在 ACL 里，**mask** 表示 **用户和组的最大有效权限上限**。
   >  它不会影响 **owner**（文件所有者）和 **others**，只会限制：
   >
   > - 所有 **ACL 用户条目**（`user:xxx`）
   > - 所有 **ACL 组条目**（`group:xxx`）
   > - 文件的 **所属组权限**（`group::`）

2. **继承规则**：

   - 新创建文件/目录不会自动继承 ACL，除非在目录上设置了 **默认 ACL**。

   设置默认 ACL：

   ```bash
   setfacl -m d:u:alice:rwx /data/project
   ```

   - `d:` 表示 default ACL，目录下新建的文件/目录会继承该 ACL。

3. **系统兼容性**：

   - EXT3/EXT4、XFS 文件系统默认支持 ACL。
   - 某些系统可能需要在 `/etc/fstab` 中挂载时加 `acl` 参数。

------

### 6. 优势总结

- 解决传统权限模式（owner / group / others）粒度不够的问题。
- 可以为不同用户/组灵活分配权限，适用于协作开发、多用户共享目录等场景。
- 默认 ACL 提高自动化管理效率。

------

## 系统配置与进程管理

### 1. 查看系统信息

- **系统版本**

  ```bash
  cat /etc/redhat-release
  ```

  示例输出：

  ```
  Red Hat Enterprise Linux Server release 7.0 (Maipo)
  ```

- **内核版本**

  ```bash
  uname -r
  ```

  示例输出：

  ```
  3.10.0-123.1.2.el7.x86_64
  ```

------

### 2. 程序与进程

- **程序 (Program)**
   静态代码，仅占用磁盘存储空间。
- **进程 (Process)**
   动态运行的程序，占用 CPU 和内存。
- **进程标识**
   每个进程都有唯一 **PID** (Process ID)。

------

### 3. top 命令

**用法**

```bash
top [选项]
```

**常用选项**

- `-d n`  设置刷新时间间隔（秒，默认 3 秒）
- `-u 用户名`  按指定用户显示进程（默认 root）

**交互快捷键**

- **P**：按 CPU 使用率排序
- **M**：按内存使用率排序
- **q**：退出 

**top 命令输出**

第一行：系统整体情况

```
top - 18:04:21 up  1:33,  2 users,  load average: 0.00, 0.01, 0.05
```

- **18:04:21** → 当前系统时间
- **up 1:33** → 系统已运行时间（这里是 1 小时 33 分钟）
- **2 users** → 当前登录用户数
- **load average: 0.00, 0.01, 0.05** → 系统平均负载，分别对应 **过去 1 分钟、5 分钟、15 分钟** 的平均运行队列长度
  - 小于 CPU 核数 → 系统空闲
  - 接近 CPU 核数 → 系统繁忙
  - 远大于 CPU 核数 → 系统过载

第二行：进程（Tasks）

```
Tasks: 495 total,   1 running, 494 sleeping,   0 stopped,   0 zombie
```

- **total** → 进程总数
- **running** → 正在运行的进程数（消耗 CPU）
- **sleeping** → 睡眠状态的进程数（等待资源或事件）
- **stopped** → 被停止的进程（例如通过 `Ctrl+Z` 挂起）
- **zombie** → 僵尸进程数（父进程未回收子进程资源）

第三行：CPU 使用情况

```
%Cpu(s):  0.0 us,  0.0 sy,  0.0 ni, 99.9 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
```

- **us (user)** → 用户空间占用 CPU 百分比（应用程序运行时消耗）
- **sy (system)** → 内核空间占用 CPU 百分比（系统调用/驱动程序消耗）
- **ni (nice)** → 改变过优先级的进程占用 CPU 百分比
- **id (idle)** → CPU 空闲百分比
- **wa (wait)** → CPU 等待 I/O 的百分比（磁盘/网络响应慢时升高）
- **hi (hardware interrupt)** → 硬中断占用的 CPU 百分比（硬件请求，例如网卡中断）
- **si (software interrupt)** → 软中断占用的 CPU 百分比（内核触发的软件中断，例如网络包处理）
- **st (steal time)** → 虚拟机环境下被“偷走”的 CPU 时间（宿主机占用）

第四行：物理内存（Mem）

```
KiB Mem:   3869044 total,   699244 used,  3169800 free,      812 buffers
```

- **total** → 物理内存总量（这里约 3.8 GB）
- **used** → 已使用的内存
- **free** → 空闲的内存
- **buffers** → 缓冲区（Buffer），内核用于存放数据块（如磁盘块）的缓存

> 注意：Linux 的“free”并不是实际可用内存，还要考虑 buffers 和 cache。

第五行：交换分区（Swap）

```
KiB Swap:        0 total,        0 used,        0 free.   246332 cached Mem
```

- **total** → 交换分区总量（swap）
- **used** → 已使用的交换空间
- **free** → 空闲交换空间
- **cached Mem** → 已缓存的内存，主要是磁盘文件缓存，可以随时释放给应用使用

------

### 4. 进程优先级

- **公式**：优先级 = 优先系数 + NICE 值
- **范围**：NICE 值 -20 ~ 19
  - 值越小，优先级越高；默认 0。

修改优先级

- **已有进程**

  ```bash
  renice -n NICE值 PID
  ```

- **新建进程**

  ```bash
  nice -n NICE值 命令
  ```

------

### 5. 常用进程控制

- **暂停程序**

  ```bash
  Ctrl + C
  ```

  中断当前命令。

- **杀死进程**

  ```bash
  kill [-9] PID
  ```

  - `-9` 表示强制终止。

- **按进程名杀死**

  ```bash
  killall [-9] 进程名
  ```

- **模糊搜索并杀死**

  ```bash
  pkill 条件
  ```

- **强制踢出用户**

  ```bash
  killall -9 -u 用户名
  ```

------

## 日志分析与存储

### 1. 日志系统组成

- **systemd-journald**
  - 随系统启动，日志默认存放在 **RAM** 中（`/run/log/journal`），重启后会清除。
- **rsyslog**
  - 从 journald 读取日志，按优先级分类，写入 **/var/log**，永久保存。
  - 配置文件：`/etc/rsyslog.conf`

> **日志流转过程**
>  `systemd → systemd-journald(RAM) → rsyslog → /var/log`

------

### 2. 日志作用

- **记录**：系统与应用运行中发生的事件
- **应用**：信息安全控制、故障排查、审计

------

### 3. 常见日志文件

| 日志文件            | 说明                                       |
| ------------------- | ------------------------------------------ |
| `/var/log/messages` | 内核消息，各类服务的公共日志               |
| `/var/log/dmesg`    | 系统启动过程消息                           |
| `/var/log/cron`     | 定时任务（cron）相关消息                   |
| `/var/log/maillog`  | 邮件收发相关消息                           |
| `/var/log/secure`   | 与安全、访问限制相关的消息（如登录、认证） |

------

### 4. 日志优先级

Linux 内核定义了 **0-7 共 8 个优先级**（数值越小越紧急）：

| 数字 | 名称    | 说明             |
| ---- | ------- | ---------------- |
| 0    | EMERG   | 紧急，系统不可用 |
| 1    | ALERT   | 必须立即处理     |
| 2    | CRIT    | 严重错误         |
| 3    | ERR     | 一般错误         |
| 4    | WARNING | 警告提示         |
| 5    | NOTICE  | 需要注意         |
| 6    | INFO    | 普通信息         |
| 7    | DEBUG   | 调试信息         |

------

### 5. 日志分析方法

**(1) 文本日志分析**

- **tail -f 文件** → 实时追踪日志

  ```bash
  # 实时追踪系统日志
  tail -f /var/log/messages
  
  # 查看 nginx 错误日志，最后 50 行开始追踪
  tail -n 50 -f /var/log/nginx/error.log
  ```

- **less 文件** → 分页查看日志

  ```bash
  # 分页查看安全日志
  less /var/log/secure
  
  # 在 less 中常用快捷键：
  # 	/关键词 → 向下搜索
  # 	?关键词 → 向上搜索
  # 	n → 跳到下一个匹配
  ```

- **grep 关键词 文件** → 搜索关键字

  ```bash
  # 查找登录失败记录
  grep "Failed" /var/log/secure
  
  # 搜索 error（忽略大小写）
  grep -i "error" /var/log/messages
  
  # 实时监控 ssh 日志
  tail -f /var/log/secure | grep ssh
  ```

- **awk/sed** → 格式化过滤

  ```bash
  # 提取登录失败用户名（第 9 列）
  grep "Failed password" /var/log/secure | awk '{print $9}'
  
  # 统计各用户失败次数（排序）
  grep "Failed password" /var/log/secure | awk '{print $9}' | sort | uniq -c | sort -nr
  
  # 显示 secure 日志第 100~110 行
  sed -n '100,110p' /var/log/secure
  
  # 删除所有包含 "session closed" 的行
  sed '/session closed/d' /var/log/secure
  ```

**(2) systemd 日志分析（journalctl）**

journalctl 提取 **systemd-journald** 收集的日志（包括内核、系统、服务日志）

常用命令：

```bash
journalctl | grep 关键词         # 关键字搜索
journalctl -u 服务名             # 查看某服务日志
journalctl -p 优先级             # 按优先级过滤
journalctl -f                    # 实时查看最新日志（类似 tail -f）
journalctl --since="2025-09-10 08:00:00" --until="2025-09-10 10:00:00"
```

------

好👌 我帮你把 **NTP 时间同步（chrony）配置与验证** 整理成一份清晰的笔记：

------

## NTP 时间同步（Network Time Protocol）

### 1. 功能

- 保持服务器时间与标准时间源一致
- 常用工具：**chrony**（RHEL7 默认）

------

### 2. 配置流程

第一步：确认 chrony 是否安装

```bash
rpm -q chrony
```

- 已安装 → 显示版本号
- 未安装 → 使用 `yum install chrony -y` 安装

------

第二步：修改配置文件，指定 NTP 服务器

```bash
vim /etc/chrony.conf
```

示例：

```ini
# 官方公共服务器（注释掉）
#server 0.rhel.pool.ntp.org iburst
#server 1.rhel.pool.ntp.org iburst
#server 2.rhel.pool.ntp.org iburst

# 指定实验环境/内网 NTP 服务器
server classroom.example.com iburst
```

📌 **参数说明**：

- `server` → 指定上游 NTP 服务器
- `iburst` → 启动时快速发送请求，加快首次同步速度

------

第三步：重启服务并设置开机自启

```bash
systemctl restart chronyd
systemctl enable chronyd
```

------

第四步：验证同步状态

执行：

```bash
chronyc sources -v
```

示例输出：

```
MS Name/IP address         Stratum Poll Reach LastRx Last sample
===============================================================================
^* classroom.example.com         0   6     0   10y     +0ns[   +0ns] +/-    0ns
```

------

### 3. 输出解析

**符号含义（第一列 MS）**

- `^*` → 当前已同步的时间源
- `^+` → 备用时间源（可用但未被选中）
- `^-` → 不推荐使用
- `^?` → 不可达（unreachable）
- `^x` → 时间可能有错误
- `^~` → 时间波动过大

**字段说明**

- **Stratum**：层级（1 表示直接与原子钟同步，数值越小越精准；0 为无效）
- **Poll**：轮询间隔（以 2 的幂表示，比如 6 → 2^6 = 64 秒）
- **Reach**：可达性（8 位二进制 → 八次请求的应答情况，0 表示未成功）
- **LastRx**：上一次接收响应的时间
- **Last sample**：本机时间与时间源的偏移值（offset）

------

### 4. 常见问题

- `^?` → 服务器不可达，检查 **网络/DNS/防火墙**
- `Stratum = 0` → 时间源无效，可能是配置错了
- `Reach = 0` → 本机没有收到任何应答，可能被 UDP 123 端口阻止

------

## 周期性计划任务（crontab）

### 1. 基本用法

```bash
crontab -e -u 用户名   # 编辑某个用户的计划任务
crontab -l -u 用户名   # 查看某个用户的计划任务
crontab -r -u 用户名   # 删除某个用户的计划任务
```

如果省略 `-u 用户名`，表示操作当前用户的任务。

------

### 2. 时间格式

```
分 时 日 月 周  命令(绝对路径)
```

- **分**：0–59
- **时**：0–23
- **日**：1–31
- **月**：1–12
- **周**：0–7 （0 和 7 都表示星期日）

例子：

```
* * * * *    每分钟执行一次
30 2 * * *   每天 2:30 执行
0 */6 * * *  每隔 6 小时执行一次
```

------

### 3. 符号含义

| 符号 | 说明                 | 示例                                   |
| ---- | -------------------- | -------------------------------------- |
| 数字 | 指定具体时间点       | `5 * * * *` → 每小时第 5 分钟          |
| `*`  | 匹配范围内任意值     | `* 1 * * *` → 每天 1 点的每分钟        |
| `,`  | 指定多个不连续时间点 | `0 8,20 * * *` → 每天 8 点和 20 点     |
| `-`  | 指定连续时间范围     | `0 9-17 * * *` → 每天 9 点到 17 点整点 |
| `/n` | 指定间隔频率         | `*/10 * * * *` → 每 10 分钟一次        |

------

### 4. 注意事项

1. **命令必须使用绝对路径**
    例如：`/usr/bin/python3 /mnt/test.py`
    （因为 crontab 的环境变量较少，可能找不到命令）

2. **脚本要有执行权限**

   ```bash
   chmod +x /mnt/1.sh
   ```

3. **环境变量**
    crontab 执行时不会加载用户的 `~/.bash_profile`，如果脚本依赖环境变量，最好写在脚本内或 crontab 中：

   ```bash
   PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
   ```

4. **日志输出**
    建议加日志，方便排查：

   ```bash
   0 3 * * * /mnt/backup.sh >> /var/log/backup.log 2>&1
   ```

------

### 5. 示例

1. 每天凌晨 2 点执行备份：

   ```bash
   0 2 * * * /mnt/backup.sh
   ```

2. 每 5 分钟检查一次服务状态：

   ```bash
   */5 * * * * /usr/bin/systemctl status nginx
   ```

3. 每周一凌晨 1 点清理日志：

   ```bash
   0 1 * * 1 /mnt/clean_logs.sh
   ```

------

## 网络管理

### 1. 两台计算机通信的必要条件

1. **IP 地址 / 子网掩码**
   - IP 地址：唯一标识主机
   - 子网掩码：区分网络位和主机位
2. **网关地址**
   - 不同网段之间通信时需要网关（通常是路由器）

------

### 2. 网络基础知识（IPv4）

1. **IP 地址**
   - 32 位二进制组成，点分十进制表示（如：192.168.1.1）
   - 特殊地址：
     - `255.255.255.255` → 广播地址
     - `0.0.0.0` → 未指定地址
   - 分类（传统 A/B/C 类）：
     - **A 类**：1–127（网.主.主.主）
     - **B 类**：128–191（网.网.主.主）
     - **C 类**：192–223（网.网.网.主）
     - **D 类**：224–239（组播地址）
     - **E 类**：240–254（科研保留）
2. **子网掩码**
   - 用来区分网络位与主机位，例如：
     - `255.255.255.0` → 前 3 段为网络位，最后 1 段为主机位
3. **DHCP**
   - 自动分配 IP、网关、DNS 等信息
   - 同一网络中只允许有一个 DHCP 服务器
4. **网关地址**
   - 主机访问不同网段时，通过网关转发数据
5. **DNS 服务器**
   - 把域名解析为 IP 地址（如 `www.baidu.com` → `220.181.38.148`）

> ⚠️ 一般情况下，只有同一网段的主机才能直接通信，不同网段需要网关（路由器）转发。

------

### 3. 主机名管理

**查看主机名**

```bash
hostname
```

- 输出短主机名（如 `server0`）
- 如果配置了域名，可能显示完整主机名（如 `server0.example.com`）

**临时修改主机名（重启后失效）**

```bash
hostname 新主机名
```

👉 修改后需 `bash` 刷新或重新登录终端生效。

**永久修改主机名**

方法一：修改配置文件

```bash
vi /etc/hostname
bash
cat /etc/hostname
```

方法二：使用 `hostnamectl`

```bash
hostnamectl set-hostname server0.example.com
```

👉 推荐方法，RHEL7 及之后版本通用。

------

### 4. 修改 IP 地址 / 子网掩码 / 网关 / DNS

#### 方法一：nmcli

使用 **nmcli（Network Manager client）** 工具。

**第一步：查看连接名称**

```bash
nmcli connection show
```

示例输出：

```
NAME  UUID                                  TYPE            DEVICE 
eth0  5fb06bd0-0bb0-7ffb-45f1-d6edd65f3e03  802-3-ethernet  eth0
```

👉 这里的 `eth0` 就是连接名。

**第二步：修改连接配置**

```bash
nmcli connection modify eth0 ipv4.method manual ipv4.addresses '172.25.0.11/24 172.25.0.254' ipv4.dns 172.25.0.254 connection.autoconnect yes
```

解释：

- `ipv4.method manual` → 手动配置 IPv4
- `ipv4.addresses '172.25.0.11/24 172.25.0.254'`
  - `172.25.0.11/24` → IP 地址和子网掩码
  - `172.25.0.254` → 网关地址
- `ipv4.dns 172.25.0.254` → 设置 DNS 服务器
- `connection.autoconnect yes` → 开机自动启用

> ⚠️ 以上配置方法仅适用于RHEL7系列的7.2以下的系统

```
nmcli connection modify eth0 ipv4.method manual ipv4.addresses 172.25.0.11/24 ipv4.gateway 172.25.0.254 ipv4.dns 172.25.0.254 connection.autoconnect yes
```

- `ipv4.method manual` → 手动配置 IPv4
- `ipv4.addresses 172.25.0.11/24`
  - `172.25.0.11/24` → IP 地址和子网掩码
- `ipv4.gateway 172.25.0.254`
  - `172.25.0.254` → 网关地址
- `ipv4.dns 172.25.0.254` → 设置 DNS 服务器
- `connection.autoconnect yes` → 开机自动启用

> ⚠️ 以上配置方法仅适用于RHEL7系列的7.2以上的系统

**第三步：激活连接**

```bash
nmcli connection up eth0
```

👉 激活后配置立即生效。

------

**第四步：验证网络配置**

```bash
ifconfig
```

👉 查看网卡信息（需要安装 `net-tools`），或用：

```bash
ip addr
```

------

#### 方法二：配置文件

修改配置文件

```bash
vim /etc/sysconfig/network-scripts/ifcfg-eth0 
```

**支持 IPv4 静态 IP 且禁用 IPv6** 的网卡配置文件示例：

```ini
TYPE=Ethernet              # 网络接口类型（以太网）
BOOTPROTO=static           # 启动协议：static 表示使用静态 IP
DEFROUTE=yes               # 是否作为默认网关
IPV4_FAILURE_FATAL=yes     # IPv4 配置失败时是否致命
IPV6INIT=no                # 禁用 IPv6
NAME=eno16777736           # 网卡名称（根据实际情况修改）
UUID=72cf53fd-534c-4765-912b-3df575a10a7d  # 网卡唯一标识符（自动生成）
ONBOOT=yes                 # 系统启动时是否自动启用该网卡
IPADDR0=192.168.1.20       # 静态 IPv4 地址
GATEWAY0=192.168.1.1       # 默认网关
PREFIX0=24                 # 子网掩码长度（24 表示 255.255.255.0）
DNS1=192.168.1.1           # 主 DNS 服务器
DNS2=8.8.8.8               # 备用 DNS（可选，谷歌公共 DNS）
HWADDR=00:0C:29:B8:2B:07   # 网卡物理地址（MAC 地址，可选）
PEERDNS=no                 # 不从 DHCP 获取 DNS（因为是静态配置）
PEERROUTES=no              # 不从 DHCP 获取路由（因为是静态配置）
```

配置文件详解：

```ini
TYPE=Ethernet              # 网络接口类型（Ethernet 表示以太网）
BOOTPROTO=static           # 启动协议：static 表示静态 IP，dhcp 表示自动获取
DEFROUTE=yes               # 是否作为默认网关路由（yes 表示启用）
IPV4_FAILURE_FATAL=yes     # IPv4 配置失败时是否导致网络启动失败（yes 表示失败则停止）
IPV6INIT=yes               # 是否启用 IPv6（yes 表示启用）
IPV6_AUTOCONF=yes          # 是否自动配置 IPv6 地址（yes 表示自动获取）
IPV6_DEFROUTE=yes          # 是否允许 IPv6 默认路由
IPV6_FAILURE_FATAL=no      # IPv6 配置失败时是否致命（no 表示失败也继续）
NAME=eno16777736           # 网卡名称（设备名）
UUID=72cf53fd-534c-4765-912b-3df575a10a7d  # 网卡唯一标识符（自动生成）
ONBOOT=yes                 # 系统启动时是否自动启用该网卡（yes 表示自动启动）
IPADDR0=192.168.1.20       # 静态 IPv4 地址（注意你写成 IPADDRO，应该是 IPADDR0）
GATEWAY0=192.168.1.1       # 网关地址
PREFIX0=24                 # 子网掩码长度（24 表示 255.255.255.0）
DNS1=192.168.1.1           # 主 DNS 服务器
HWADDR=00:0C:29:B8:2B:07   # 网卡物理地址（MAC 地址）
PEERDNS=yes                # 是否从 DHCP 获取并配置 DNS（静态时一般无效）
PEERROUTES=yes             # 是否从 DHCP 获取并配置路由（静态时一般无效）
IPV6_PEERDNS=yes           # 是否从 IPv6 DHCP 获取 DNS
IPV6_PEERROUTES=yes        # 是否从 IPv6 DHCP 获取路由
```

**重启网卡服务：**

1. 重启整个 **network 服务**

```bash
# 适用于 RHEL/CentOS 6 及兼容系统
service network restart  

# 适用于 RHEL/CentOS 7+ 或 systemd 系统
systemctl restart network
```

2. 单独重启某个网卡

```bash
# 只影响指定的网卡，不会影响其他网卡。
ifdown eth0   # 关闭网卡 eth0
ifup eth0     # 启用网卡 eth0
```

3. 使用 **NetworkManager 工具**

```bash
nmtui  # 打开图形化配置界面（文本界面）
# 进入后可以选择 Activate a connection / Deactivate a connection 来启用或关闭网卡。

nmcli connection reload     # 重新加载配置
nmcli connection up eth0    # 启动 eth0
nmcli connection down eth0  # 停止 eth0
```

#### 方法三：nmtui

1、使用`nmtui`命令进入伪图形化配置界面

![image-20250915171046978](https://raw.githubusercontent.com/JHgdaldwa/Picture/master/image-20250915171046978.png)

2、选择`Edit a connection`选项并回车

![image-20250915171500545](https://raw.githubusercontent.com/JHgdaldwa/Picture/master/image-20250915171500545.png)

3、选择对应的连接名称并回车，进入该界面后进行修改，修改完成后，选择`OK`并回车即可完成修改,完成修改后直接退出即可

![image-20250915171634043](https://raw.githubusercontent.com/JHgdaldwa/Picture/master/image-20250915171634043.png)

若是使用`DHCP`规则进入后界面如下，选择`IPv4`进行对应修改即可

![image-20250915171928559](https://raw.githubusercontent.com/JHgdaldwa/Picture/master/image-20250915171928559.png)

4、重启网卡

```bash
systemctl restart network
```

------

## 文件压缩、传输与远程控制

### tar 归档与压缩

1. 常见压缩格式

- **.gz** → gzip / gunzip （速度快，压缩率低）
- **.bz2** → bzip2 / bunzip2
- **.xz** → xz / unxz （速度慢，压缩率高）

2. 制作压缩包

```bash
tar -cf /保存路径/压缩包名.tar[.格式] /源文件路径
```

- **必用选项**
  - `-c`：创建归档
  - `-f`：指定归档文件名（必须在选项最后）
- **常用压缩选项**
  - `-z`：压缩为 .gz
  - `-j`：压缩为 .bz2
  - `-J`：压缩为 .xz
- **常用辅助选项**
  - `-v`：显示压缩过程
  - `-P`：保留源路径（解压时也需加 `-P`）

示例：

```bash
tar -zcvf /root/etc.tar.gz /etc
```

3. 解压压缩包

```bash
tar -xf /压缩包路径/压缩包名.tar[.格式] [-C /目标路径]
```

- **必用选项**
  - `-x`：解压归档
  - `-f`：指定归档文件名
- **常用选项**
  - `-v`：显示解压过程
  - `-P`：解压到原路径

示例：

```bash
tar -xvf etc.tar.gz -C /tmp
```

4. 查看压缩包内容

```bash
tar -tf 压缩包.tar[.格式]
```

------

### ssh 远程控制

### 用途

远程安全连接与管理服务器。

### 使用格式

```bash
ssh [选项] 用户名@IP
```

- **常用选项**
  - `-X`：支持远程运行图形程序
  - `-p 端口号`：指定端口

示例：

```bash
ssh -p 2222 root@192.168.1.100
```

------

### scp 远程传输

**用途**

基于 ssh 的安全文件拷贝工具。

**使用格式**

1. **本地 → 远程**

```bash
scp [选项] /本地路径 用户名@IP:/远程路径
```

1. **远程 → 本地**

```bash
scp [选项] 用户名@IP:/远程路径 /本地路径
```

- **常用选项**
  - `-r`：递归复制目录

示例：

```bash
scp -r /etc root@192.168.1.100:/tmp
scp root@192.168.1.100:/etc/passwd /root/
```

------

## 软链接与硬链接（ln）

### 软链接（Symbolic Link）

创建：

```bash
ln -s /源文件 /目标路径/软链接名
```

### 特点

- 类似 **Windows 快捷方式**
- **i 节点不同**，只是一个特殊文件，存放了源文件路径
- 可以 **跨分区、跨文件系统**
- **文件大小**：软链接文件本身大小 = 源文件路径的字符串长度
- **权限**：访问时会解析到源文件的权限
- **依赖源文件**：
  - 如果源文件删除 → 软链接失效（显示为红色，提示“悬挂的链接”）
  - 如果源文件改名 → 链接失效

------

### 硬链接（Hard Link）

创建：

```bash
ln /源文件 /目标路径/硬链接名
```

### 特点

- 类似 **同一个文件的多个名字**
- **i 节点相同**，指向同一个数据块
- **不能跨分区/文件系统**
- **文件大小**：和源文件完全一致
- **权限**：和源文件完全一致（修改任意一方，另一方同步变化）
- **不依赖源文件**：
  - 删除源文件，硬链接依旧可用
  - 删除一个名字不会影响数据，只有 **引用计数（link count）为 0** 时，文件数据才会被释放

------

### i 节点与引用计数

- 每个文件有一个 i 节点（inode），存放文件的元数据（大小、权限、时间戳、磁盘位置等）
- 硬链接与源文件共享 i 节点，引用计数 +1
- 删除文件时，引用计数 -1，只有当计数 = 0 时，数据块才真正被释放
- 软链接有自己的 i 节点，指向目标路径，不增加引用计数

查看引用计数：

```bash
ls -l
```

输出的第 2 列数字就是引用计数。

------

### 对比总结

| 特性           | 软链接（Symbolic Link）      | 硬链接（Hard Link）            |
| -------------- | ---------------------------- | ------------------------------ |
| i 节点         | 不同                         | 相同                           |
| 跨分区         | 可以                         | 不可以                         |
| 文件系统       | 可以跨                       | 必须同一文件系统               |
| 占用空间       | 仅存路径字符串               | 与源文件一致                   |
| 文件大小       | 很小（路径长度）             | 与源文件相同                   |
| 源文件删除     | 链接失效                     | 仍可访问                       |
| 是否依赖源文件 | 依赖                         | 不依赖                         |
| 应用场景       | 跨分区共享文件，类似快捷方式 | 本地冗余访问路径，保证文件可用 |

------

### 示例

```bash
# 创建源文件
echo "hello" > file.txt

# 创建硬链接
ln file.txt file_hard

# 创建软链接
ln -s file.txt file_soft

# 查看信息
ls -li
```

```
12345 -rw-r--r-- 2 root root  6  9月 16 18:00 file.txt
12345 -rw-r--r-- 2 root root  6  9月 16 18:00 file_hard
12346 lrwxrwxrwx 1 root root  8  9月 16 18:00 file_soft -> file.txt
```

- `file.txt` 和 `file_hard` 共享 inode **12345**（硬链接）
- `file_soft` 有自己的 inode **12346**，内容是字符串 `"file.txt"`

------

## 安装和更新软件包

### 一、软件包来源

- **主要来源**：官方镜像光盘
- **其他来源**：软件官网下载的 rpm 包

------

### 二、RPM

1. 概念

- **RPM 包**：红帽系列 Linux 使用的软件包格式
- **数字签名验证**：用于确认软件包是否被篡改

2. 常用操作

- **下载软件包**：

  ```bash
  wget [-O /保存路径/文件名] 链接地址
  ```

- **安装软件包**：

  ```bash
  rpm -i 软件包名.rpm
  ```

- **查询是否已安装**：

  ```bash
  rpm -q 软件包名
  ```

- **卸载软件包（只卸载本体，不卸载依赖）**：

  ```bash
  rpm -e 软件包名
  ```

3. 缺点

- 手动安装时可能遇到 **依赖关系问题**，需要用户自己解决。

------

### 三、YUM

1. 作用

- 自动解决依赖关系的软件包管理工具。

2. 依赖条件

- **软件包（rpm 包）**
- **软件清单（repo 配置）**

------

### 四、配置 YUM 源

> 注：
>
> - **YUM 源可以存在多个 repo 文件**。
> - **一个 repo 文件中也可以配置多个源**。
> - 配置文件必须存放在 `/etc/yum.repos.d/`，后缀为 `.repo`。

#### 方法一：手动创建 repo 文件

```bash
vim /etc/yum.repos.d/rhel_dvd.repo
```

内容示例：

```ini
[rhel_dvd]                          # 仓库标识符
name=Remote classroom copy of dvd   # 描述信息
baseurl=http://content.example.com/rhel7.0/x86_64/dvd   # 仓库地址
gpgcheck=0                          # 是否进行数字签名验证 (0=关闭, 1=开启)
enabled=1                           # 是否启用该仓库
```

------

#### 方法二：命令行生成 repo 文件

```bash
yum-config-manager --add-repo http://172.25.0.254/content/rhel7.0/x86_64/dvd/
```

生成的文件示例：

```ini
[172.25.0.254_content_rhel7.0_x86_64_dvd_]
name=added from: http://172.25.0.254/content/rhel7.0/x86_64/dvd/
baseurl=http://172.25.0.254/content/rhel7.0/x86_64/dvd/
enabled=1
```

修改文件，添加关闭签名验证：

```ini
gpgcheck=0
```

------

#### 验证配置

```bash
yum clean all     # 清空缓存
yum repolist      # 列出仓库清单
```

------

### 五、YUM 常用操作

- **安装软件包**：

  ```bash
  yum -y install 软件包名
  ```

- **卸载软件包（推荐，不删除依赖）**：

  ```bash
  yum remove 软件包名
  ```

- **搜索软件包**：

  ```bash
  yum search 关键词
  yum list | grep 关键词
  ```

- **查找命令属于哪个软件包**：

  ```bash
  yum provides 命令
  ```

------

## 用户集群管理LDAP

### 一、LDAP 认证的作用

- **集中管理**：用户账号统一存放在 LDAP 服务器上，便于维护。
- **客户端认证**：用户登录客户端时，账号密码信息由 LDAP 验证。

------

### 二、服务端与客户端角色

- **LDAP 服务端**：存储用户信息（用户名、密码、UID、GID、家目录等）。
- **客户端**：通过 `sssd` 与 LDAP 服务端通信，实现统一用户认证。

------

### 三、客户端配置流程

1. 安装 SSSD

```bash
yum -y install sssd
```

------

2. 配置 LDAP 认证

使用图形工具 `authconfig-tui`：

```bash
authconfig-tui
```

选择以下选项：

- [*] Use LDAP
- [*] Use LDAP Authentication
- [*] Use TLS

配置：

- Server：`classroom.example.com`
- Base DN：`dc=example,dc=com`

**下载 CA 证书：**

```bash
cd /etc/openldap/cacerts
wget http://classroom.example.com/pub/example-ca.crt
```

------

3. 启动 SSSD

```bash
systemctl restart sssd
systemctl enable sssd
```

------

4. 验证用户

```bash
# 检测 LDAP 用户是否存在
id ldapuser0

# 本地 /etc/passwd 中不应该存在
grep ldapuser0 /etc/passwd
```

如果 `id ldapuser0` 有输出，说明认证成功。

------

5. 登录测试

```bash
su - ldapuser0
```

若提示 **找不到家目录**，需要挂载远程家目录。

------

### 四、挂载家目录（NFS）

1. 临时挂载

先创建挂载点：

```bash
mkdir /home/guests
```

挂载 NFS 共享：

```bash
mount classroom.example.com:/home/guests /home/guests
```

验证登录：

```bash
su - ldapuser0
```

------

### 五、自动挂载（autofs）

1. 安装 autofs

```bash
yum -y install autofs
```

2. 修改主配置文件

```bash
vim /etc/auto.master
```

添加：

```ini
/home/guests   /etc/auto.rule
```

3. 创建规则文件

```bash
vim /etc/auto.rule
```

示例 1：单用户

```ini
ldapuser0   -rw   classroom.example.com:/home/guests/ldapuser0
```

示例 2：多用户（推荐）

```ini
*   -rw   classroom.example.com:/home/guests/&
```

- `*`：匹配所有用户
- `&`：匹配用户名对应的子目录

4. 启动 autofs

```bash
systemctl restart autofs
systemctl enable autofs
```

------

## 系统服务管理

### 一、/etc/fstab 常见错误

- **文件系统损坏** → 系统自动进入紧急救援模式
- **挂载的设备不存在** → 等待超时后，进入紧急救援模式
- **挂载点不存在** → 系统尝试自动创建，若失败则进入紧急救援模式
- **挂载点错误** → 系统进入紧急救援模式

------

### 二、systemctl 命令

**格式：**

```bash
systemctl 参数 服务
```

**常用参数**

- `start` —— 启动服务
- `restart` —— 重启服务（或启动）
- `enable` —— 设置开机自启
- `stop` —— 停止服务
- `status` —— 查看服务状态
  - **绿色**：运行正常
  - **红色**：启动报错
  - **无颜色**：未启动
- `disable` —— 取消开机自启
- `mask` —— 屏蔽服务（彻底禁用）
- `unmask` —— 取消屏蔽

------

### 三、systemd 目标 (Target)

> systemd 目标类似于 **运行级别**，但更灵活。

**常见目标**

| 目标                | 用途                            |
| ------------------- | ------------------------------- |
| `graphical.target`  | 多用户模式，图形界面            |
| `multi-user.target` | 多用户模式，文本界面            |
| `rescue.target`     | 单用户救援模式                  |
| `emergency.target`  | 紧急 Shell 模式（最低级别维护） |

------

**常用命令**

- **临时切换目标：**

  ```bash
  systemctl isolate 运行模式
  ```

  例：切换到文本界面

  ```bash
  systemctl isolate multi-user.target
  ```

- **查看默认目标：**

  ```bash
  systemctl get-default
  ```

- **设置默认目标：**

  ```bash
  systemctl set-default 运行模式
  ```

  例：设置开机进入图形界面

  ```bash
  systemctl set-default graphical.target
  ```

------

## 存储管理

### 一、文件系统基本概念

- **格式化**：给分区建立文件系统。
- **文件系统**：存储数据的规则（EXT2/3/4、XFS、SWAP）。
- **常见文件系统**：
  - **EXT2/3/4**：Linux 常用，RHEL6 默认 EXT4。
  - **XFS**：高性能日志文件系统，RHEL7/8 默认。
  - **SWAP**：交换分区，用磁盘当虚拟内存。

------

### 二、分区类型

**1. MBR（主引导记录）**

- **限制**：最大支持 2.2TB。
- **方案**：
  1. 1–4 个主分区
  2. 0–3 个主分区 + 1 个扩展分区（扩展分区可再划逻辑分区）
- **特点**：
  - 扩展分区仅能包含逻辑分区，不能直接存数据。

**2. GPT（全局唯一标识分区表）**

- 支持更大磁盘（常用于新系统）。
- 没有主/扩展/逻辑分区的限制。

------

### 三、磁盘操作流程

📌 操作流程：
 **识别硬盘 → 分区规划 → 格式化 → 挂载使用 → 配置开机自动挂载**

------

#### 1. 识别硬盘

- 系统开机后会自动识别新硬盘。

- 查看磁盘和分区情况：

  ```bash
  lsblk
  fdisk -l
  ```

- 未分区的磁盘显示为整块 `disk`，有分区的会显示 `part`。

------

#### 2. 分区规划

**查看分区表**

```bash
fdisk -l /dev/sdb
```

- **未分区**：只显示磁盘基本信息。
- **已分区**：显示分区号、起止扇区、大小和类型。

##### 使用 `fdisk` 修改分区表

```bash
fdisk /dev/sdb
```

常用交互命令：

- `m`：帮助
- `p`：查看分区表
- `n`：新建分区（主/扩展/逻辑）
- `d`：删除分区
- `w`：保存退出
- `q`：放弃退出

```ini
进入交互式的分区页面
输入n
Command (m for help): n							//添加一个分区
Partition type:									//分区类型
   p   primary (0 primary, 0 extended, 4 free)	//主分区（4个空闲位置，0个主分区，0扩展分区）
   e   extended									//扩展分区
Select (default p):								//进行选择（不需要改变，按照默认配置即可，前三个为主分区，第四个为扩展分区，第五个是逻辑分区）
Using default response p						//使用了默认值，主分区
Partition number (1-4, default 1):				//分区号码（不需要更改，按照默认配置即可）
First sector (2048-20971519, default 2048):		//起始扇区（默认预留2048扇区用于存放元数据，按照默认配置即可）
Using default value 2048						//使用默认值
Last sector, +sectors or +size{K,M,G} (2048-20971519, default 20971519): +1G	//终点扇区（为分区分配空间）
Partition 1 of type Linux and of size 1 GiB is set	//分区1大小1G已经设置完成
```

**创建主分区**

- 最多 4 个主分区。
- 分区时指定大小（如 `+1G`）。

**创建扩展分区**

- 用来容纳逻辑分区，不能直接格式化或挂载。
- 整个扩展分区最多一个。

**创建逻辑分区**

- 在扩展分区内部，编号从 `5` 开始（`sdb5、sdb6...`）。

完成分区后：

```bash
w   # 保存并退出
partprobe   # 通知内核重新读取分区表（避免重启）
```

------

#### 3. 格式化分区

对新建分区进行文件系统初始化：

```bash
mkfs.ext4 /dev/sdb1
mkfs.xfs  /dev/sdb2
mkfs.ext3 /dev/sdb3
```

⚠️ 注意事项：

- 不要格式化扩展分区，只能格式化主/逻辑分区。
- 如果分区已有文件系统，会提示，可用 `-f` 强制覆盖（需谨慎）。

查看文件系统 UUID：

```bash
blkid /dev/sdb1
```

------

#### 4. 挂载使用

##### 临时挂载

1. 创建挂载点：

   ```bash
   mkdir /part1
   ```

2. 挂载：

   ```bash
   mount /dev/sdb1 /part1
   ```

3. 查看挂载情况：

   ```bash
   df -Th
   ```

##### 卸载挂载

```bash
umount /part1
```

##### 开机自动挂载

编辑 `/etc/fstab`，添加：

```
/dev/sdb1   /part1   ext4   defaults   0 0
```

- `defaults`：常用挂载参数（读写、自动挂载等）。
- `0 0`：是否参与 dump 备份和 fsck 检查。

应用修改：

```bash
mount -a
```

💡 建议用 **UUID** 替代设备名，避免设备顺序变化导致挂载错误：

```
UUID=2f1ba73b-7ffc-45a8-b6b0-2ed4d63e200a   /part1   ext4   defaults   0 0
```

------

#### 5. 常见问题及解决方法

- **分区后报错：设备或资源繁忙**

  ```sql
  WARNING: Re-reading the partition table failed with error 16: Device or resource busy.	//重新读取分区列表失败：设备或资源繁忙
  The kernel still uses the old table. The new table will be used at
  the next reboot or after you run partprobe(8) or kpartx(8)
  Syncing disks.
  ```

  解决方法：

  ```bash
  partprobe
  ```

- **扩展分区不可格式化/挂载**

  - 扩展分区只作为逻辑分区的“容器”，不可直接使用。

- **挂载点不为空**

  - 挂载时目录原有文件会“被隐藏”，卸载后恢复。

------

#### 6. 设备命名规则

- `sda`：第一块 SCSI/SATA 磁盘
- `sdb`：第二块磁盘
- `sda1`：第一块磁盘的第一个分区
- `sdb5`：第二块磁盘的第一个逻辑分区

------

#### 7. 其他命令

- 查看磁盘空间使用情况：`df -h`
- 查看分区详细信息：`lsblk -f`
- 查看文件系统使用率（inode）：`df -i`
- 分区时，建议：
  - `/boot`：1G 左右
  - `swap`：内存的 1–2 倍
  - `/`（根目录）：20–50G
  - 其他分区按需求分配（如 `/home`、`/data`）

------

### 四、交换分区（Swap）

#### 1. 概念

- **交换空间（Swap Space）**：类似于 Windows 的虚拟内存，把一部分硬盘空间作为内存临时使用。
- **交换分区（Swap Partition）**：通过分区创建的交换空间。
- 作用：当物理内存不足时，将数据临时转移到交换空间，避免系统崩溃。

💡 **最佳实践**：

- 交换空间大小：
  - 内存 ≤ 2GB → swap = 2 × RAM
  - 内存 2–8GB → swap = RAM
  - 内存 ≥ 16GB → swap 可设置 2–4GB

------

#### 2. 创建交换分区

① 分区

```bash
fdisk /dev/sdb
```

> 分区大小决定交换空间大小。

② 格式化为交换分区

```bash
mkswap /dev/sdb6
```

③ 查看 UUID 和类型

```bash
blkid /dev/sdb6
```

④ 启用交换分区

```bash
swapon /dev/sdb6
```

⑤ 查看是否启用成功

```bash
swapon -s
```

输出示例：

```
Filename        Type      Size     Used Priority
/dev/sdb6       partition 524284   0    -1
```

- **Priority（优先级）**：数值越大，优先使用该 swap。

------

#### 3. 停用交换分区

```bash
swapoff /dev/sdb6
```

------

#### 4. 开机自动启用

编辑 `/etc/fstab`：

```
设备路径    挂载点   文件系统类型  参数       备份  检查
/dev/sdb6   swap    swap          defaults   0     0
```

推荐使用 **UUID**：

```
UUID=xxxx-xxxx   swap   swap   defaults   0 0
```

------

#### 5. 检查和启用所有配置的 swap

```bash
swapon -a    # 挂载 fstab 中所有未启用的 swap
swapon -s    # 查看状态
```

------

#### 6. 其他方式（了解）

除了分区，还可以用 **文件** 创建 swap：

1. **创建一个空文件**（大小 = 2G）：

   ```bash
   dd if=/dev/zero of=/swapfile bs=1M count=2048
   ```

   - `if=/dev/zero`：输入源是无限的零数据流。
   - `of=/swapfile`：输出文件名。
   - `bs=1M`：块大小 1MB。
   - `count=2048`：写 2048 个块 → 文件大小 = 2GB。

2. **格式化为 swap 类型**

   ```bash
   mkswap /swapfile
   ```

3. **修改权限（安全要求，避免被普通用户读写）**

   ```bash
   chmod 600 /swapfile
   ```

4. **启用 swap 文件**

   ```bash
   swapon /swapfile
   ```

5. **永久生效**（写入 `/etc/fstab`）

   ```
   /swapfile   swap   swap   defaults   0 0
   ```

------

### 五、加密分区（LUKS）

#### 1. 概念

- **LUKS（Linux Unified Key Setup）**：Linux 下标准的磁盘加密方式。
- 使用加密分区时必须输入密码解锁，**不支持开机自动挂载**（因为需要手动输入密码）。

------

#### 2. 配置流程

**① 分区**

```bash
fdisk /dev/sdb
```

**② 对分区加密并设置密码**

```bash
cryptsetup luksFormat /dev/sdb7
```

- 确认时必须输入 **YES（大写）**
- 设置并确认密码（需符合安全要求）

**③ 解锁分区**

```bash
cryptsetup luksOpen /dev/sdb7 mydisk
```

- `mydisk`：自定义名字
- 输入密码后，系统会生成设备文件 `/dev/mapper/mydisk`

**④ 格式化解锁后的设备**

```bash
mkfs.ext4 /dev/mapper/mydisk
```

**⑤ 挂载使用**

```bash
mkdir /part7
mount /dev/mapper/mydisk /part7
```

⚠️ **不开机自动挂载**（因为重启后必须重新解锁）

**⑥ 卸载并上锁**

```bash
umount /part7
cryptsetup luksClose mydisk
```

**⑦ 验证上锁是否成功**

```bash
mount /dev/mapper/mydisk /part7
# 报错：device does not exist

mount /dev/sdb7 /part7
# 报错：unknown filesystem type 'crypto_LUKS'
```

------

### 六、LVM 分区（Logical Volume Manager）

#### 1. 概念

- **LVM（Logical Volume Manager）**：逻辑卷管理工具，将众多零散的物理卷PV整合组成卷组VG，再从卷组中划分逻辑卷LV
- 优点：
  1. 扩展空间大小（动态调整，不需重新分区）
  2. 整合零散空间（多个物理卷合成一个卷组）

结构关系：

```
物理卷 PV  ---->  卷组 VG  ---->  逻辑卷 LV
```

示意：

```
/dev/vdb1 10G
/dev/vdc1 10G   ----> VG (30G) ----> LV1 15G
/dev/vdc2 10G
```

------

#### 2. 常用命令

| 功能        | 物理卷管理 | 卷组管理  | 逻辑卷管理 |
| ----------- | ---------- | --------- | ---------- |
| scan扫描    | pvs        | vgs       | lvs        |
| create创建  | pvcreate   | vgcreate  | lvcreate   |
| display显示 | pvdisplay  | vgdisplay | lvdisplay  |
| remove删除  | pvremove   | vgremove  | lvremove   |
| extend扩展  |            | vgextend  | lvextend   |

p：physical 物理

v：volume 卷

g：group 组

l：logical 逻辑

s：scan 扫描

------

#### 3. 创建 LVM 分区流程

**1. 准备分区（空闲分区，例如 `/dev/sdb8` 200M）**

**2. 创建卷组**

> 命令格式：vgcreate 卷组名称 闲置分区路径

```bash
vgcreate myvg /dev/sdb8
```

查看：

```bash
pvs
vgs
```

**3. 创建逻辑卷**

> 命令格式：lvcreate -L 逻辑卷大小 -n 逻辑卷名称 卷组名称

```bash
lvcreate -L 10M -n lv1 myvg
```

查看：

```bash
lvs
```

> 逻辑卷创建成功后，会自动生成设备路径`/dev/卷组名称/逻辑卷名称`

**4. 格式化**

```bash
mkfs.ext3 /dev/myvg/lv1
```

**5. 挂载使用**

```bash
mkdir /lvm
vim /etc/fstab
/dev/myvg/lv1   /lvm   ext3   defaults 0 0
mount -a
```

------

#### 4. 扩展逻辑卷

情况 1：VG 有足够剩余空间

> 命令格式：lvextend -L +n{G,M}或n{G,M} /dev/卷组名称/逻辑卷名称
>
> 扩大n{G,M}或扩大成n{G,M}

```bash
lvextend -L +10M /dev/myvg/lv1    # 扩大10M
```

刷新文件系统：

- **ext 系列**：

  ```bash
  resize2fs /dev/myvg/lv1
  ```

- **xfs**：

  ```
  xfs_growfs /dev/myvg/lv1
  ```

情况 2：VG 空间不足

1. 扩展卷组：

   > 注：扩展时需要用空闲的磁盘分区进行扩展，即未进行格式化的分区

   ```bash
   vgextend myvg /dev/sdb9
   ```

2. 再扩展 LV：

   ```bash
   lvextend -L +20M /dev/myvg/lv1
   ```

3. 刷新文件系统：

   ```bash
   resize2fs /dev/myvg/lv1
   ```

------

#### 5. PE（Physical Extent）

- **PE**：卷组划分空间的最小单位（默认 4M）

- 查看 PE 大小：

  ```bash
  vgdisplay myvg
  ```

- 创建 VG 时指定 PE：

  ```bash
  vgcreate -s 2M vg1 /dev/sdb10
  ```

- 修改已有 VG 的 PE：

  ```bash
  vgchange -s 8M vg1
  ```

  ⚠️ 必须为 **2 的次幂**，且已有 LV 大小必须能整除新的 PE

------

#### 6. 删除 LVM

1. 卸载逻辑卷：

   ```bash
   umount /lvm
   ```

2. 删除 `/etc/fstab` 中对应行

3. 删除逻辑卷：

   ```bash
   lvremove /dev/myvg/lv1
   ```

4. 删除卷组：

   ```bash
   vgremove myvg
   ```

5. 删除物理卷：

   ```bash
   pvremove /dev/sdb8
   ```

------

#### 7. 注意事项 & 补充

- **LV 扩容**：安全且常用
- **LV 缩容**：不推荐（容易数据丢失，ext4 可缩小，xfs 不支持缩小）
- **在线扩容**：文件系统挂载时也能扩容（需用对应命令刷新 FS）
- **应用场景**：
  - 数据库、文件服务器，需动态扩容
  - 多块小磁盘整合成大空间

------

## 安全设置

### iptables



------

### Firewalld

基于区域（zone）的动态防火墙，支持运行时和永久配置。

管理工具：

- **firewall-cmd**（命令行）：学习时间稍长，但熟练后更高效、容错率高。
- **firewall-config**（图形）：操作简单，学习成本低，但容错率较低。

#### 一、预设安全区域（Zone）

| 区域        | 功能说明                                    |
| ----------- | ------------------------------------------- |
| **public**  | 默认区域，仅允许 ssh、dhcp、ping 等少量服务 |
| **trusted** | 信任区域，允许所有访问                      |
| **block**   | 阻塞来访请求，明确拒绝                      |
| **drop**    | 丢弃来访请求，不返回任何响应                |

⚙️ **防火墙进入区域判定规则（匹配即停止）：**

1. 根据源 IP 匹配已有区域规则，匹配成功则进入该区域。
2. 若未匹配，进入默认区域（默认是 `public`）。

------

#### 二、常用防火墙操作（firewall-cmd）

1. 重载防火墙

修改配置后需执行：

```bash
firewall-cmd --reload
```

------

2. 默认区域查看与修改（永久）

```bash
firewall-cmd --get-default-zone
firewall-cmd --set-default-zone=区域名称
```

------

3. 服务管理

添加服务到区域（无需重载）：

```bash
firewall-cmd --zone=public --add-service=http
```

查看区域详细信息：

```bash
firewall-cmd --zone=public --list-all
```

------

4. IP / 网段绑定到区域

添加源 IP：

```bash
firewall-cmd --permanent --zone=public --add-source=192.168.1.1
firewall-cmd --permanent --zone=public --add-source=192.168.2.0/24
```

封禁 IP / 网段（加入 block 或 drop 区域）：

```bash
firewall-cmd --permanent --zone=block --add-source=192.168.222.0/24
```

------

5. 永久配置策略

在命令中添加 `--permanent`：

```bash
firewall-cmd --zone=public --add-service=http --permanent
```

------

6. 端口映射（端口转发）

**本地端口映射**（端口1 → 端口2）：

```bash
firewall-cmd --permanent --zone=public --add-forward-port=port=5423:proto=tcp:toport=80
```

**转发到其他主机**：

```bash
firewall-cmd --permanent --zone=public --add-forward-port=port=5423:proto=tcp:toport=80:toaddr=192.168.1.100
```

⚠️ 需要开启伪装（NAT/Masquerade）：

```bash
firewall-cmd --permanent --zone=public --add-masquerade
firewall-cmd --reload
```

------

#### 三、常见服务与默认端口号

| 服务  | 协议  | 默认端口 |
| ----- | ----- | -------- |
| HTTP  | http  | 80       |
| HTTPS | https | 443      |
| FTP   | ftp   | 21       |
| SSH   | ssh   | 22       |
| TFTP  | tftp  | 69       |
| DNS   | dns   | 53       |
| SMTP  | smtp  | 25       |
| POP3  | pop3  | 110      |

------

### SELinux（Security-Enhanced Linux）

#### 一、概念

- **SELinux**：Linux 系统的一套 **强制访问控制（MAC）** 安全机制。
- **作用**：禁止一切 SELinux 认为不安全的系统服务访问行为，从而增强系统安全性。
- **特点**：强制性，非自愿。
- **实现方式**：访问控制策略。

**SElinux安全策略：**

1. 波尔值

2. 安全上下文值

   查看安全上下文值

   ```bash
   ls -Zd 文档路径
   ```

3. 非默认端口开放

------

#### 二、运行模式

| 模式     | 英文关键字 | 特点                                  |
| -------- | ---------- | ------------------------------------- |
| **强制** | enforcing  | 启用 SELinux 策略，直接禁止不安全行为 |
| **宽松** | permissive | 不禁止，发出警告并记录日志            |
| **禁用** | disabled   | 完全关闭，不记录警告也不执行策略      |

------

#### 三、模式切换

1. 启用状态下的临时切换（即时生效，重启失效）

```bash
setenforce 1    # 切换到 enforcing（强制模式）
setenforce 0    # 切换到 permissive（宽松模式）
```

查看当前模式：

```bash
getenforce
```

------

2. 永久配置（需重启生效）

编辑配置文件：

```bash
vim /etc/selinux/config
```

修改参数：

```conf
SELINUX=enforcing   # 强制
SELINUX=permissive  # 宽松
SELINUX=disabled    # 禁用
```

保存后，**重启系统**，配置才会生效。

------

## 简单文件共享服务

### NFS 文件共享服务（Network File System）

#### 一、概念

- **用途**：为客户端提供共享文件目录
- **协议端口**：
  - **NFS**：2049
  - **RPC**：111
- **适用范围**：仅支持 Linux 系统
- **环境准备**：防火墙默认区域设为 `trusted`（允许所有访问）

------

#### 二、服务端配置

1. **设置防火墙区域**

   ```bash
   firewall-cmd --set-default-zone=trusted
   
   # 更安全的方式，只放行 NFS 所需的端口
   firewall-cmd --permanent --zone=public --add-service=nfs
   firewall-cmd --permanent --zone=public --add-service=rpc-bind
   firewall-cmd --permanent --zone=public --add-service=mountd
   
   firewall-cmd --reload
   ```

2. **安装 NFS 服务**

   ```bash
   yum -y install nfs-utils
   ```

3. **创建共享目录**

   ```bash
   mkdir /nfs_share
   ```

4. **修改配置文件**

   ```bash
   vim /etc/exports
   ```

   添加内容：

   ```
   /nfs_share 172.25.0.254(ro)
   ```

   - `/nfs_share`：共享目录路径
   - `172.25.0.254`：允许访问的客户端 IP
   - `(ro)`：只读权限（可改为 `(rw)` 读写权限）

5. **启动 NFS 服务并设置开机自启**

   ```bash
   systemctl status nfs-server    # 查看服务状态
   systemctl start nfs-server     # 启动服务
   systemctl enable nfs-server    # 设置开机自启
   ```

6. **测试共享目录**

   ```bash
   echo hhhh > /nfs_share/1.txt
   ```

------

#### 三、客户端配置

1. **创建挂载点**

   ```bash
   mkdir /nfs
   ```

2. **设置开机自动挂载**
    编辑 `/etc/fstab`：

   ```
   服务器IP:/共享目录路径   /挂载点   nfs   defaults,_netdev   0 0
   ```

   示例：

   ```
   172.25.0.11:/nfs_share   /nfs   nfs   defaults,_netdev   0 0
   ```

3. **挂载共享目录**

   ```bash
   mount -a       # 挂载 /etc/fstab 中的配置
   df -h          # 查看挂载是否成功
   ```

4. **验证共享文件**

   ```bash
   ls /nfs/
   cat /nfs/1.txt
   ```

   输出：

   ```
   1.txt
   hhhh
   ```

------

### Samba 文件共享服务

#### 一、概念

- **适用范围**：Windows & Linux 跨平台共享目录

- **作用**：为客户端提供共享使用的目录

- **协议端口**：

  - **SMB**：139
  - **CIFS**：445

- **特点**：相对 NFS 更安全

- **环境准备**：防火墙区域设为 `trusted`（允许所有访问）或直接关闭防火墙

  ```bash
  firewall-cmd --set-default-zone=trusted
  systemctl stop firewall
  
  # 更安全的方式，只放行 Samba 服务需要的端口（139、445）
  firewall-cmd --permanent --zone=public --add-service=samba
  firewall-cmd --reload
  ```

------

#### 二、服务端配置

1. **安装 Samba 服务**

   ```bash
   yum -y install samba
   ```

2. **创建共享目录**

   ```bash
   mkdir /smb_share
   ```

3. **建立 Samba 共享账号**

   - 必须先有一个同名系统用户
   - 禁止系统登录，采用 `/sbin/nologin`
   - Samba 账号独立设置密码

   ```bash
   # 创建系统用户
   useradd -s /sbin/nologin gx
   
   # 将该用户加入 samba 账号数据库（独立设置密码）
   pdbedit -a gx
   
   # 删除 samba 账号
   pdbedit -x 用户名
   
   # 查看所有 samba 账号
   pdbedit -L
   ```

4. **修改配置文件**

   ```bash
   vim /etc/samba/smb.conf
   ```

   添加：

   ```
   [gx]                     # 共享名称
   path = /smb_share        # 共享目录路径
   ```

   **常用配置项说明**：

   ```
   [共享名]               # 自定义，不可用特殊字符
   path = 绝对路径        # 必填
   public = yes|no        # 是否公共（默认 no）
   browseable = yes|no    # 是否可见（默认 yes）
   read only = yes|no     # 是否只读（默认 yes）
   write list = 用户1...  # 可写用户列表（默认无）
   host allow = IP/网段   # 允许访问（默认空）
   host deny = IP/网段    # 拒绝访问（默认空）
   ```

5. **启动并设置开机自启**

   ```bash
   systemctl status smb    # 查看状态
   systemctl start smb     # 启动服务
   systemctl enable smb    # 开机自启
   ```

6. **测试文件**

   ```bash
   echo samba > /smb_share/1.txt
   ```

------

#### 三、客户端配置

1. **安装 Samba 客户端工具**

   ```bash
   yum -y install samba-client
   ```

2. **发现服务端共享内容**

   ```bash
   smbclient -L //服务端IP   # 默认无密码时可直接查看
   ```

3. **安装 CIFS 支持工具**

   ```bash
   yum -y install cifs-utils
   ```

4. **创建挂载点**

   ```bash
   mkdir /smb_mount
   ```

5. **设置开机自动挂载**
    编辑 `/etc/fstab`：

   ```
   //服务器IP/共享名   /挂载点   cifs   defaults,user=账号,pass=密码,_netdev   0 0
   ```

   示例：

   ```
   //172.25.0.11/gx   /smb_mount   cifs   defaults,user=gx,pass=0000,_netdev   0 0
   ```

   > 把账号密码写到单独文件 `/etc/samba/credentials`：
   >
   > ```
   > username=gx
   > password=0000
   > ```
   >
   > 修改权限：
   >
   > ```
   > chmod 600 /etc/samba/credentials
   > ```
   >
   > `/etc/fstab` 改写为：
   >
   > ```
   > //172.25.0.11/gx /smb_mount cifs credentials=/etc/samba/credentials,_netdev 0 0
   > ```

6. **挂载并验证**

   ```bash
   mount -a
   cat /smb_mount/1.txt
   ```

------

| 对比项           | **NFS**                                      | **Samba**                               |
| ---------------- | -------------------------------------------- | --------------------------------------- |
| **适用范围**     | Linux ↔ Linux                                | Linux ↔ Windows / Linux                 |
| **协议端口**     | NFS：2049RPC：111                            | SMB：139CIFS：445                       |
| **安全性**       | 相对较低（主要依赖防火墙/IP 限制）           | 相对更高（支持用户认证、权限控制）      |
| **认证方式**     | 基于 IP 地址控制                             | 基于用户账号 + 密码                     |
| **配置复杂度**   | 较简单                                       | 相对复杂                                |
| **典型应用场景** | Linux 内网共享（如实验环境、集群节点间共享） | 跨平台文件共享（Windows 与 Linux 互访） |

------

## 虚拟化系统

### 一、KVM 简介

- **KVM**（Kernel-based Virtual Machine）：基于 Linux **内核**的虚拟化技术。
- 作用：在宿主机上运行多个虚拟机，提供虚拟计算机的硬件环境。

------

### 二、管理工具

| 工具             | 类型     | 功能                                   |
| ---------------- | -------- | -------------------------------------- |
| **virt-manager** | 图形界面 | 提供直观的虚拟机管理（适合桌面环境）   |
| **virsh**        | 字符界面 | 通过命令行管理虚拟机（适合服务器环境） |

------

### 三、VMware 常见网络模式

| 模式                    | 特点                                                         | 应用场景                               |
| ----------------------- | ------------------------------------------------------------ | -------------------------------------- |
| **桥接（Bridged）**     | 虚拟机与物理机处于同一网络段，需要自己配置 IP（可静态、可动态） | 虚拟机需要与外网设备通信，独立使用网络 |
| **NAT（网络地址转换）** | 虚拟机借用物理机的网络，通过 NAT 转换访问外网                | 一般办公或上网使用，配置简单           |
| **仅主机（Host-only）** | 虚拟机仅能与本地虚拟机和宿主机通信，无法访问外网             | 内网测试、实验环境                     |

------

## 系统自动安装

### PXE装机

PXE(Pre-boot eXecution Environment)预启动执行环境：

在操作系统之前运行

ISO镜像：压缩包

作用：用于安装操作系统

优点：

- 规模化：同时装配多台主机
- 自动化：装系统，配置各种服务
- 远程实现：不需要光盘，U盘等物理安装介质

工作模式：

`PXE Client`集成在网卡的启动芯片中

当计算机引导时，从网卡芯片中把`PXE Client`调入内存执行

`PXE Client`获取`PXE server`配置，显示菜单,根据应答文件完成装机

根据用户选择将远程引导程序下载到本机运行

服务组件：

DHCP：提供网络

TFTP：共享内核、引导、驱动

HTTP(FTP或NFS)：提供YUM源，应答文件

#### 部署PXE

##### 一、准备工作：

1. 配置IP地址

   ```bash
   nmcli connection modify eth0 ipv4.method manual ipv4.addresses 172.25.0.12/24 connection.autoconnect yes
   
   systemctl restart network
   ```

2. 配置防火墙

   ```bash
   systemctl stop firewalld
   
   systemctl disable firewalld
   ```

3. 修改主机名（也可以不修改）

   ```bash
   hostnamectl set-hostname server1.com
   ```

4. 配置本地YUM源

   1. 使用ISO映像

      ![img](https://raw.githubusercontent.com/JHgdaldwa/Picture/master/image-20250926175657074.png))

   2. 挂载光盘

      ```bash
      mkdir /dvd
      
      vim /etc/fstab
      # 添加
      /dev/cdrom /dvd iso9660 defaults 0 0
      
      mount -a
      # mount: /dev/sr0 is write-protected, mounting read-only	光盘设备写保护，以只读方式挂载
      
      ls /dvd
      ```
   
   3. 创建repo文件

      ```bash
      rm -rf /etc/yum.repos.d/*
      
      vim /etc/yum.repos.d/rhel_dvd.repo
      # 添加
      [rhel7]
      name=repo
      baseurl=file:///dvd
      enabled=1
      gpgcheck=0
      
      
      yum  clean all
      yum repolist
      ```
   
5. 创建一台待安装虚拟机进行测试

##### 二、配置DHCP服务器：

1. 安装软件包

   ```bash
   yum -y install dhcp
   ```

2. 修改配置文件

   ```bash
   vim /etc/dhcp/dhcpd.conf
   ```

   ```ini
   subnet 172.25.0.0 netmask 255.255.255.0 {
      range 172.25.0.100 172.25.0.200;
      default-lease-time 600;
      max-lease-time 7200; 
      next-server 172.25.0.12;
      filename "pxelinux.0";
   }
   ```

   > pxelinux.0：网络装机说明书，二进制文件，指导服务器自动完成装机
   >
   > pxelinux.0文件获取方式：通过安装一个软件包获取

   **更多配置说明**

   ```ini
   # 全局配置
   ddns-update-style none;                      # 不启用 DDNS 动态更新
   ignore client-updates;                       # 忽略客户端主机名更新
   authoritative;                               # 设置为权威 DHCP 服务器（避免冲突）
   
   # 定义默认网关、DNS 等全局选项（如果每个子网不同，可以在 subnet 内单独设）
   option domain-name "example.local";          # 默认域名
   option domain-name-servers 10.0.0.12;        # DNS 服务器
   option subnet-mask 255.0.0.0;                # 默认子网掩码
   option routers 10.0.0.2;                     # 默认网关
   
   default-lease-time 600;                      # 默认租期
   max-lease-time 7200;                         # 最大租期
   
   # 子网配置
   subnet 10.0.0.0 netmask 255.0.0.0 {
      range 10.0.0.50 10.0.0.100;               # 动态分配地址池
      option broadcast-address 10.255.255.255;  # 广播地址（自动计算也可以）
      
      # PXE 引导配置
      next-server 10.0.0.12;                    # 指定 TFTP 服务器地址
      filename "pxelinux.0";                    # 指定 PXE 引导文件
   }
   
   # 针对特定主机（静态分配）示例
   host node01 {
      hardware ethernet 00:11:22:33:44:55;      # 网卡 MAC 地址
      fixed-address 10.0.0.101;                 # 分配固定 IP
      option host-name "node01";                # 主机名
   }
   ```

3. 重启服务，设置开机自启后，查看状态

   ```bash
   systemctl restart dhcpd
   
   systemctl enable dhcpd
   
   systemctl status dhcpd
   ```

4. 测试DHCP

   启动待安装的虚拟机，若DHCP配置成功，则可以看到获取的IP地址等信息

##### 三、搭建TFTP服务：

**默认共享路径：/var/lib/tftpboot**

1. 安装软件包

   ```bash
   yum -y install tftp* xinetd
   ```

2. 修改配置文件

   ```bash
   vim /etc/xinetd.d/tftp
   # 把yes改为no
   disable                 = no
   ```

3. 重启服务，设置开机自启后，查看状态

   ```bash
   systemctl restart xinetd.service
   
   systemctl enable xinetd.service
   
   systemctl status xinetd.service
   ```

##### 四、获取pxelinux.0

1. 查询pxelinux.0由哪个软件包提供

   ```bash
   yum provides */pxelinux.0
   
   # 查询结果
   Loaded plugins: langpacks
   syslinux-4.05-8.el7.x86_64 : Simple kernel loader which boots from a FAT filesystem		# 提供文件的软件包
   Repo        : rhel7
   Matched from:
   Filename    : /usr/share/syslinux/pxelinux.0		# 下载后文件存放路径
   
   ```

2. 安装软件包

   ```bash
   yum -y install syslinux-4.05-8.el7.x86_64
   ```

3. 查看安装软件包后文件是否存在

   ```bash
   ls /usr/share/syslinux/ |grep pxelinux.0
   ```

4. 部署装机说明书（即pxelinux.0文件）

   ```bash
   cp /usr/share/syslinux/pxelinux.0 /var/lib/tftpboot/
   ```

##### 五、部署菜单文件

###pxelinux.0装机说明书中已经规定了菜单文件必须放在指定目录并且指定的命名：

菜单文件的存放路径：`/var/lib/tftpboot/pxelinux.cfg`

菜单文件必须命名为：`default`

1. 在tftp默认共享李静霞创建存放菜单文件的子目录

   ```bash
   mkdir /var/lib/tftpboot/pxelinux.cfg
   ```

2. 从光盘中部署菜单文件复制到指定位置并且重命名为`default`

   ```bash
   cp /dvd/isolinux/isolinux.cfg /var/lib/tftpboot/pxelinux.cfg/default
   ```

3. 附加`default`文件写权限

   ```bash
   chmod u+w /var/lib/tftpboot/pxelinux.cfg/default
   ```

4. 测试

   启动待安装的虚拟机，看是否能连接上TFTP并且获取到菜单文件，若TFTP连接成功，则会获取菜单文件，若获取菜单文件成功，则在菜单文件旁边显示OK

   ![image-20250926220653255](https://raw.githubusercontent.com/JHgdaldwa/Picture/master/image-20250926220653255.png)

##### 六、部署菜单图形模块、背景图片、启动内核、驱动程序

1. 把对应文件复制到tftp共享目录

   ```bash
   cp /pxe_mount/isolinux/{vesamenu.c32,splash.png,vmlinuz,initrd.img} /var/lib/tftpboot/
   ```

   > vesamenu.c32：菜单风格文件
   >
   > vmlinuz：Linux系统的内核文件
   >
   > initrd.img：初始化镜像文件（Linux引导加载模块）

2. 查看文件是否齐全

   ```bash
   ls /var/lib/tftpboot/
   
   ls /var/lib/tftpboot/pxelinux.cfg/
   ```

##### 七、修改菜单文件

```bash
vim /var/lib/tftpboot/pxelinux.cfg/default
```

```ini
2 timeout 600 改成 timeout 60
11 menu title Red Hat Enterprise Linux 7.0 改成 menu title hhhhhhhhhhhh	（也可以不改）
跳转到65行，把65行之后的内容全部删除，再修改61-65行：
将原来的：
 61 label linux
 62   menu label ^Install Red Hat Enterprise Linux 7.0
 63   kernel vmlinuz
 64   append initrd=initrd.img inst.stage2=hd:LABEL=RHEL-7.0\x20Server.x86_64 quiet
改成：
 61 label linux
 62   menu label Install RHEL7.0（也可以不改）
 63   menu default
 64   kernel vmlinuz
 65   append initrd=initrd.img
```

##### 八、部署HTTP服务，共享光盘的YUM源

1. 安装提供HTTP服务的软件包

   ```
   yum -y install httpd
   ```

2. 把光盘挂载到HTTP的默认网页文件目录（/var/www/html）

   1. 在网页文件目录创建挂载点

      ```bash
      mkdir /var/www/html/rhel7
      ```

   2. 修改`/etc/fstab`文件中光驱设备的挂载点

      ```bash
      vim /etc/fstab
      
      /dev/cdrom /var/www/html/rhel7 iso9660 defaults 0 0
      ```

   3. 卸载原有的光驱挂载

      ```bash
      umount /dev/cdrom
      ```

   4. 重新把光驱设备挂载到新的挂载点

      ```bash
      mount -a
      ```

   5. 修改YUM源

      ```bash
      vim /etc/yum.repos.d/rhel7.repo
      ```

      ```bash
      [rhel7]
      name=repo
      baseurl=file:///var/www/html/rhel7
      enabled=1
      gpgcheck=0
      ```

      

3. 重启HTTP服务，并设置开机自启

   ``` bash
   systemctl restart httpd
   
   systemctl enable httpd
   ```

##### 九、生成应答文件

1. 安装一个辅助图形工具

   ```bash
   yum -y install system-config-kickstart.noarch
   ```

2. 图形工具中软件包选择提示：由于下载软件包信息失败，软件包选择被禁止

   原因：这个辅助图形工具有bug

   解决方法：将YUM源配置文件的标志名改成`[development]`

   ```
   vim /etc/yum.repos.d/rhel7.repo
   ```

   ```bash
   [development]
   name=repo
   baseurl=file:///var/www/html/rhel7
   enabled=1
   gpgcheck=0
   ```

3. 运行图形工具

   ```
   # 可以临时把系统语言调成中文
   export LANG=zh_CN.UTF-8
   ```

   ```
   system-config-kickstart
   ```

   配置

   ![image-20250927165226347](https://raw.githubusercontent.com/JHgdaldwa/Picture/master/image-20250927165226347.png)

   ![image-20250927165303574](https://raw.githubusercontent.com/JHgdaldwa/Picture/master/image-20250927165303574.png)

   ![image-20250927165339799](https://raw.githubusercontent.com/JHgdaldwa/Picture/master/image-20250927165339799.png)

   ![image-20250927165442623](https://raw.githubusercontent.com/JHgdaldwa/Picture/master/image-20250927165442623.png)

   ![image-20250927165507926](https://raw.githubusercontent.com/JHgdaldwa/Picture/master/image-20250927165507926.png)

   ![image-20250927165523979](https://raw.githubusercontent.com/JHgdaldwa/Picture/master/image-20250927165523979.png)

   ![image-20250927165549890](https://raw.githubusercontent.com/JHgdaldwa/Picture/master/image-20250927165549890.png)

   ![image-20250927165609021](https://raw.githubusercontent.com/JHgdaldwa/Picture/master/image-20250927165609021.png)

   ![image-20250927165803564](https://raw.githubusercontent.com/JHgdaldwa/Picture/master/image-20250927165803564.png)

   **其余配置默认即可，也可以做更改，例如：创建一个用户：hhh，密码：123**

   ![image-20250927170000334](https://raw.githubusercontent.com/JHgdaldwa/Picture/master/image-20250927170000334.png)

   配置完成后点击`文件-->保存`，选择路径保存后，保存的路径会生成一个`ks.cfg`的文件

4. 查看应答文件

   ```bash
   ls -l /root/ks.cfg
   
   vim /root/ks.cfg
   ```

5. 通过HTTP服务共享kiskstart文件

   ```bash
   cp /root/ks.cfg /var/www/html/
   ```

6. 修改菜单文件，把kiskstart链接地址写到配置文件

   ```bash
   vim /var/lib/tftpboot/pxelinux.cfg/default
   
   65   append initrd=initrd.img ks=http://172.25.0.12/ks.cfg
   ```

7. 测试

   启动待安装的虚拟机，若DHCP配置成功，则可以看到获取的IP地址等信息

#### 排错思路

DHCP没有获取到IP地址：DHCP服务配置是否正确，是否是仅主机网络模式，VMware默认DHCP功能是否关闭

TFTP超时：TFTP服务配置，防火墙

缺少某一个文件：检查`/var/lib/tftpboot`目录文件是否齐全

KS：default有没有写`ks=http://172.25.0.12/ks.cfg`，或者地址有没有写错，可以访问文件链接看看有没有内容

`找不到http://172.25.0.12/......`：应答文件中的`url --url="http://172.25.0.12/rhel7"`是否正确

`connect to 172.25.0.12:Network is unreachable \ faild to fetch kickstart from http://172.25.0.12/ks.cfg`：VMware默认DHCP功能没有关闭

## 系统管理高级设置

### 一、Firewall 富规则（Rich Rules）

**查看所有富规则**

```bash
firewall-cmd --list-rich-rules
```

------

**创建富规则模板**

```bash
firewall-cmd --permanent --zone=<zone_name> --add-rich-rule "
rule
 family=ipv4
 source address=<source_ip_or_network>
 service name=<service_name>
 accept
"
```

> ⚠️ 注意：
>
> - 每个 `rule` 只能指定一个 service / port / protocol。
> - 不能写成 `service name=ssh,samba`，必须分开写两条。

**示例：允许 172.25.0.0/24 网络访问 ssh 和 samba 服务：**

```bash
firewall-cmd --permanent --zone=public --add-rich-rule='rule family=ipv4 source address=172.25.0.0/24 service name=ssh accept'
firewall-cmd --permanent --zone=public --add-rich-rule='rule family=ipv4 source address=172.25.0.0/24 service name=samba accept'
```

------

**移除富规则**

```bash
firewall-cmd --permanent --remove-rich-rule='rule family=ipv4 source address=172.25.0.0/24 service name=ssh accept'
```

------

**应用更改**

```bash
firewall-cmd --reload
```

------

**验证**

```bash
firewall-cmd --zone=public --list-rich-rules
```

应看到：

```
rule family="ipv4" source address="172.25.0.0/24" service name="ssh" accept
rule family="ipv4" source address="172.25.0.0/24" service name="samba" accept
```

------

### 二、SELinux + Samba 文件共享配置与排错

#### 服务端配置步骤

（1）安装 Samba 软件包

```bash
yum -y install samba
```

（2）创建共享目录

```bash
mkdir /share_samba
```

（3）修改 Samba 主配置文件

```bash
vim /etc/samba/smb.conf
```

在文件末尾添加：

```ini
[hhh]
path = /share_samba
writable = yes
write list = yz
```

------

（4）创建 Samba 用户

```bash
useradd -s /sbin/nologin yz
pdbedit -a yz
```

------

（5）赋予共享目录权限

```bash
setfacl -m u:yz:rwx /share_samba/
```

------

（6）启动并设置开机自启

```bash
systemctl enable --now smb
systemctl status smb
```

------

（7）防火墙放行 http 与 samba 服务

（如果此机作为 YUM 源仓库）

```bash
firewall-cmd --permanent --zone=public --add-service=http
firewall-cmd --permanent --zone=public --add-service=samba
firewall-cmd --reload
```

------

（8）启用 SELinux

```bash
setenforce 1
```

（如需永久启用，修改 `/etc/selinux/config` 文件中）

```
SELINUX=enforcing
```

------

#### 客户端配置步骤

（1）配置 YUM 源（若有 http 仓库）

编辑 `/etc/yum.repos.d/local.repo`：

```ini
[rhel_dvd]
name=Remote classroom copy of dvd
baseurl=http://172.25.0.12/rhel7
enabled=1
gpgcheck=0
```

测试：

```bash
yum repolist
```

------

（2）安装客户端工具

```bash
yum -y install samba-client cifs-utils
```

------

（3）测试连接

```bash
smbclient -L //172.25.0.12
```

> 若能列出共享 `[hhh]`，说明服务端配置正确。

------

（4）编辑挂载配置

```bash
vim /etc/fstab
```

添加：

```bash
//172.25.0.12/hhh /mnt cifs defaults,user=yz,pass=0000,_netdev 0 0
```

挂载：

```bash
mount -a
```

------

#### SELinux 相关问题处理（重点）

**检测写入时的报错日志**

在服务端执行：

```bash
tailf /var/log/messages
```

客户端执行：

```bash
echo 123 > /mnt/2.txt
```

查看日志末尾是否含有：

```
setroubleshoot: SELinux is preventing /usr/sbin/smbd from write access on the directory ...
```

------

**查看建议的修复命令**

```bash
grep setroubleshoot /var/log/messages | tail -1
```

复制日志中的 ID，然后运行：

```bash
sealert -l <UUID>
```

> 示例：
>
> ```bash
> sealert -l 6b661980-e3a1-4249-b991-e0d67b4a5e3f
> ```
>
> 若与正确输出不同，则在服务端查看samba相关权限，查看是否开启只读`samba_export_all_ro`
>
> ```
> # 查看
> getsebool -a | grep samba
> 
> # 设置开启
> setsebool samba_export_all_ro on
> ```

------

**根据建议修改 SELinux 上下文**

```bash
semanage fcontext -a -t samba_share_t '/share_samba'
restorecon  -v '/share_samba'
```

输出示例：

```
restorecon reset /share_samba context unconfined_u:object_r:default_t:s0 -> unconfined_u:object_r:samba_share_t:s0
```

------

**验证最终结果**

客户端：

```bash
echo 1234 > /mnt/2.txt
```

服务端：

```bash
ls -l /share_samba/
```

------

### 三、sudo 超级权限指令

#### `su` 切换用户身份 (Substitute User)

**1. 作用**

用于切换到指定的其他用户身份。

- 普通用户执行时：需要输入目标用户的密码
- root 用户执行时：无需验证密码

**2. 命令格式**

```bash
su [-] 用户名
```

- 有 `-`：切换到目标用户的身份和环境变量
- 无 `-`：只切换到目标用户的身份，不加载其环境变量

执行单条命令：

```bash
su [-] -c "命令" [目标用户]
```

> 若省略目标用户，默认切换到 root。

------

#### `sudo` 提升权限 (Super or another Do)

**1. 作用**

让管理员授权普通用户临时以 root 权限执行特定命令。

**2. 基本格式**

```bash
sudo 特权命令
```

------

#### 为用户添加 sudo 特权权限

1. 创建用户并设置密码

```bash
useradd hhh
echo 0000 | passwd --stdin hhh
```

2. 编辑 `/etc/sudoers`

使用 `visudo` 或 `vim` 打开：

```bash
vim /etc/sudoers
```

在第 98 行附近添加：

```bash
hhh ALL=(ALL) /usr/bin/systemctl * httpd, /usr/bin/vim /etc/httpd/conf/httpd.conf
```

> 注意拼写：`/etx` 应为 `/etc`

3. 测试提权

```bash
# 切换到 hhh 用户
su - hhh

# 查看 hhh 可执行的 sudo 命令
sudo -l

# 测试执行
sudo systemctl stop httpd
systemctl status httpd
```

------

#### 为用户组添加 sudo 特权权限

1. 创建用户组与成员

```bash
groupadd webadmin
useradd web1
useradd web2
gpasswd -a web1 webadmin
gpasswd -a web2 webadmin
echo 0000 | passwd --stdin web1
echo 0000 | passwd --stdin web2
```

2. 编辑 `/etc/sudoers`

```bash
vim /etc/sudoers
```

在第 106 行附近添加：

```bash
%webadmin ALL=(ALL) /usr/bin/systemctl * httpd, /usr/bin/vim /etc/httpd/conf/httpd.conf
```

3. 测试

```bash
su - web1
sudo -l
sudo systemctl stop httpd
systemctl status httpd
```

------

#### sudo 别名设置（命令分组）

在 `/etc/sudoers` 文件中可以设置命令别名，以便批量授权。

示例

```bash
# 去掉注释启用默认别名
Cmnd_Alias SOFTWARE = /bin/rpm, /usr/bin/up2date, /usr/bin/yum

# 授权 webadmin 组执行软件管理相关命令
%webadmin ALL=(ALL) /usr/bin/systemctl * httpd, /usr/bin/vim /etc/httpd/conf/httpd.conf, SOFTWARE
```

------

## 高级网络管理

### 一、IPv6 基础知识

**1. IPv6 简介**

IPv6（Internet Protocol version 6）是互联网协议的第六版，用于替代 IPv4，以解决地址枯竭等问题。
 IPv6 具有更大的地址空间、更高的安全性以及更高效的路由机制。

**2. IPv6 地址格式**

- IPv6 地址长度为 **128 位（二进制位）**。

- 使用 **冒号分隔的 8 组 16 进制数** 表示。

- 每组为 **16 位**（4 个十六进制数字）。

- 例如：

  ```
  2001:0db8:0000:0000:0000:ff00:0042:8329
  ```

- 可简化为：

  - 省略每组前导零：

    ```
    2001:db8:0:0:0:ff00:42:8329
    ```

  - 连续的零组可用 `::` 代替（但只能出现一次）：

    ```
    2001:db8::ff00:42:8329
    ```

**3. IPv6 地址类型**

| 类型                           | 前缀示例  | 说明                                 |
| ------------------------------ | --------- | ------------------------------------ |
| **单播（Unicast）**            | 2001::/16 | 唯一标识一个接口（常用）             |
| **组播（Multicast）**          | ff00::/8  | 一组接口共享的地址（取代 IPv4 广播） |
| **任播（Anycast）**            | N/A       | 发送到距离最近的一个成员接口         |
| **链路本地地址（Link-local）** | fe80::/10 | 仅在本地链路中有效，自动分配         |

------

### 网卡桥链路聚合（聚合链路，网卡绑定，网卡桥，聚合连接）

#### 一、概念解析

**网卡桥链路聚合（Bridge + Team/Bond）** 是将两种网络技术结合起来的高级网络配置：

- **链路聚合（Link Aggregation）**：把多块物理网卡合成为一条逻辑链路，实现带宽叠加与冗余备份。实现方式主要有：**Bonding（传统方式）Teaming（新一代方式）**
- **网卡绑定（Bonding）**：Linux 系统中最早实现链路聚合的技术。它通过内核模块 `bonding` 将多块物理网卡（如 `eth0`, `eth1`）绑定为一个逻辑接口（如 `bond0`）。
- **网卡桥**：在逻辑接口之间建立二层转发，实现虚拟机、容器与物理网络的互通。
- **聚合连接（Aggregated Connection）** ：指通过链路聚合技术（如 **bonding** 或 **teaming**）形成的逻辑网络连接。



**简单理解：**

> 先将多块物理网卡聚合成一个逻辑接口（如 `team0`），
>  再将该逻辑接口加入网桥（如 `br0`），
>  这样虚拟机或容器就可以通过 `br0` 访问外网，同时享受链路聚合带来的冗余与高带宽优势。

------

#### 二、组成结构与关系

```
[eth1] \
         \                +-------------+
[eth2] ---->  [team0] --->|     br0     | ---> 虚拟机 / 容器 / 上层网络
                          +-------------+
```

- **eth1、eth2**：物理网卡。
- **team0**：链路聚合接口（负责负载均衡、冗余）。
- **br0**：桥接接口（负责虚拟机、容器通信和外网连接）。

------

#### 三、技术原理

**1. 链路聚合（Team / Bond）**

- 将多块物理网卡绑定为一个逻辑设备（如 `team0`）。
- 由 `teamd` 守护进程负责监控链路状态与流量分配。
- 支持多种运行模式：

| 模式             | 功能说明                               |
| ---------------- | -------------------------------------- |
| **activebackup** | 主备切换，冗余优先                     |
| **roundrobin**   | 轮询转发，带宽叠加                     |
| **lacp**         | 与交换机协商的标准聚合（IEEE 802.3ad） |

------

**2. 网卡桥（Bridge）**

- 工作在二层（数据链路层），模拟交换机功能。
- 将物理或虚拟接口连接起来，使以太网帧在接口间自由转发。

**常见用途：**

- 虚拟机接入外网；
- 容器与主机网络通信；
- 多接口间二层互通。

**3. Teaming 的工作模式（Runner）**

| 模式名称         | JSON 配置参数                      | 功能说明                                   |
| ---------------- | ---------------------------------- | ------------------------------------------ |
| **activebackup** | `"runner":{"name":"activebackup"}` | 主备模式，只有一块网卡工作，另一块作为备用 |
| **roundrobin**   | `"runner":{"name":"roundrobin"}`   | 轮询方式，将流量平均分配到所有接口上       |
| **loadbalance**  | `"runner":{"name":"loadbalance"}`  | 根据负载情况分配流量                       |
| **broadcast**    | `"runner":{"name":"broadcast"}`    | 将数据同时发送到所有接口（多播）           |
| **lacp**         | `"runner":{"name":"lacp"}`         | IEEE 802.3ad 动态链路聚合协议              |

------

#### 四、配置 Team 网卡组（ActiveBackup 模式）

以下以 `team0` 为例，配置一个 **activebackup 模式** 的链路聚合。

------

**第一步：创建网卡组 team0**

```bash
nmcli connection add type team con-name team0 ifname team0 config '{"runner":{"name":"activebackup"}}'
```

**参数说明：**

- `type team`：指定连接类型为 Team。
- `con-name team0`：NetworkManager 中的连接名称。
- `ifname team0`：系统中将创建的接口名称。
- `config`：使用 JSON 格式定义工作模式（此处为 activebackup）。

> 可通过命令查看更多配置项：
>
> ```bash
> man teamd.conf
> ```

**删除网卡组：**

```bash
nmcli connection delete team0
```

------

**第二步：添加物理网卡为成员接口**

```bash
nmcli connection add type team-slave con-name team0-1 ifname eth1 master team0
nmcli connection add type team-slave con-name team0-2 ifname eth2 master team0
```

每个成员网卡通过 `team-slave` 类型加入主接口 `team0`。

------

**第三步：为 team0 配置 IPv4 / IPv6 地址**

```bash
nmcli connection modify team0 ipv4.method manual ipv4.addresses 192.168.0.1/24 connection.autoconnect yes
nmcli connection modify team0 ipv6.method manual ipv6.addresses 2001:db8::1/64
```

------

**第四步：设置成员接口开机自动连接**

```bash
nmcli connection modify team0-1 connection.autoconnect yes
nmcli connection modify team0-2 connection.autoconnect yes
```

------

**第五步：激活网卡组**

```bash
nmcli connection up team0
```

------

**第六步：查看接口状态**

```bash
ifconfig team0
```

输出示例：

```
team0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.0.1  netmask 255.255.255.0  broadcast 192.168.0.255
        inet6 fe80::8c9d:19ff:fe3f:3476  prefixlen 64  scopeid 0x20<link>
        ether 8e:9d:19:3f:34:76  txqueuelen 0  (Ethernet)
```

查看 Team 状态：

```bash
teamdctl team0 state
```

输出示例：

```
setup:
  runner: activebackup
ports:
  eth1
    link summary: up
  eth2
    link summary: up
runner:
  active port: eth2
```

------

**第七步：测试冗余切换**

禁用当前活跃网卡：

```bash
ifdown eth2
```

再查看状态：

```bash
teamdctl team0 state
```

输出：

```
runner:
  active port: eth1
```

重新启用 eth2：

```bash
ifup eth2
```

可以看到，`eth1` 继续保持活跃（不会自动切回），证明 **activebackup 模式切换成功**。

> #### 配置链路桥接
>
> 假设服务器上有两块物理网卡：`eth1`、`eth2`。
>
> ##### 1. 创建聚合接口 team0
>
> ```
> nmcli connection add type team con-name team0 ifname team0 config '{"runner":{"name":"activebackup"}}'
> ```
>
> ##### 2. 添加物理成员
>
> ```
> nmcli connection add type team-slave ifname eth1 master team0
> nmcli connection add type team-slave ifname eth2 master team0
> ```
>
> ##### 3. 创建桥接接口 br0
>
> ```
> nmcli connection add type bridge con-name br0 ifname br0
> ```
>
> ##### 4. 将 team0 加入桥
>
> ```
> nmcli connection add type bridge-slave ifname team0 master br0
> ```
>
> ##### 5. 配置 IP 地址（桥接口上）
>
> ```
> nmcli connection modify br0 ipv4.method manual ipv4.addresses 192.168.0.10/24
> nmcli connection modify br0 ipv6.method manual ipv6.addresses 2001:db8::10/64
> ```
>
> ##### 6. 启动连接
>
> ```
> nmcli connection up br0
> ```
>
> ##### 7. 验证配置
>
> ###### 查看接口状态
>
> ```
> ip link show
> ```
>
> ##### 查看 team 聚合状态
>
> ```
> teamdctl team0 state
> ```
>
> ##### 查看桥接状态
>
> ```
> brctl show
> ```
>
> 或：
>
> ```
> nmcli device show br0
> ```

------

#### 五、验证与维护命令

| 功能               | 命令示例                                 |
| ------------------ | ---------------------------------------- |
| 查看所有连接       | `nmcli connection show`                  |
| 查看接口状态       | `nmcli device status`                    |
| 查看 team 配置详情 | `teamdctl team0 config dump`             |
| 动态修改 team 参数 | `teamdctl team0 config update file.json` |
| 监控实时状态       | `teamdctl team0 state view`              |

------

## 服务器搭建

------

### 邮件服务器（Mail Server）

#### 1. 基本概念

电子邮件服务通常包含以下两个核心协议：

- **SMTP（Simple Mail Transfer Protocol）**：邮件发送协议，默认端口为 **25**。
- **POP3（Post Office Protocol 3）**：邮件接收协议，默认端口为 **110**。

此外，还有 IMAP（Internet Message Access Protocol，默认端口 143），支持邮件同步与多设备访问（本实验不涉及）。

------

#### 2. 搭建 Postfix 邮件服务器

（1）安装邮件服务软件

```bash
yum -y install postfix
```

（2）修改配置文件

编辑 Postfix 主配置文件：

```bash
vim /etc/postfix/main.cf
```

修改以下配置项：

| 行号 | 参数            | 修改前                                        | 修改后                | 说明                       |
| ---- | --------------- | --------------------------------------------- | --------------------- | -------------------------- |
| 99   | myorigin        | `$mydomain`                                   | `server0.example.com` | 设置默认发信域名后缀       |
| 116  | inet_interfaces | `localhost`                                   | `all`                 | 开放所有网络接口供外部访问 |
| 164  | mydestination   | `$myhostname, localhost.$mydomain, localhost` | `server0.example.com` | 设置邮件接收域名           |

（3）启动并设置开机自启

```bash
systemctl restart postfix
systemctl enable postfix
```

（4）本地账户测试

创建测试用户：

```bash
useradd test1
useradd test2
```

发送邮件（交互式）：

```bash
mail -s 'test' -r test1 test2
```

输入邮件正文内容后，单独输入一行 `.` 表示结束输入并发送。

例如：

```
this is test mail.
.
EOT
```

查看收件人邮箱：

```bash
mail -u test2
```

输出示例：

```
"/var/mail/test2": 1 message 1 new
>N  1 test1@server0.exampl  Mon Oct 13 17:50  18/598   "test"
& 1
Message  1:
From test1@server0.example.com  Mon Oct 13 17:50:27 2025
To: test2@server0.example.com
Subject: test

this is test mail.
```

输入 `q` 退出查看。

------

#### 3. 邮件服务器常见问题

| 问题           | 原因                      | 解决方法                               |
| -------------- | ------------------------- | -------------------------------------- |
| 无法发送邮件   | Postfix 服务未启动        | 执行 `systemctl start postfix`         |
| 无法接收邮件   | 配置项 mydestination 错误 | 检查 `/etc/postfix/main.cf`            |
| 邮件丢失或退信 | 主机名未解析              | 确保 `/etc/hosts` 中有正确的主机名映射 |

------

### FTP 文件服务器

FTP（File Transfer Protocol）是一种文件传输协议，常用于文件共享与远程下载。

------

#### 1. 服务端配置

（1）安装 vsftpd

```bash
yum -y install vsftpd
```

（2）启动服务并设置开机自启

```bash
systemctl start vsftpd
systemctl enable vsftpd
```

（3）默认共享目录

vsftpd 默认共享目录为：

```
/var/ftp
```

如果希望客户端通过 FTP 下载系统光盘内容，可将光盘挂载到该目录下：

```bash
vim /etc/fstab
/dev/cdrom /var/ftp/rhel7 iso9660 defaults 0 0

mount -a
```

此时，客户端访问 ftp://服务器IP/rhel7 即可浏览内容。

------

#### 2. 客户端配置与访问

在客户端配置 YUM 仓库：

```bash
rm -rf /etc/yum.repos.d/*

vim /etc/yum.repos.d/ftp.repo
```

添加以下内容：

```
[rhel7]
name=test ftp
baseurl=ftp://172.25.0.11/rhel7
enabled=1
gpgcheck=0
```

安装 FTP 客户端：

```bash
yum -y install ftp
```

连接服务器：

```bash
ftp 172.25.0.11
```

登录时用户名输入 `anonymous`，密码可留空。

常用命令：

| 命令           | 功能说明     |
| -------------- | ------------ |
| `ls`           | 查看目录内容 |
| `cd`           | 切换目录     |
| `get <文件名>` | 下载文件     |
| `put <文件名>` | 上传文件     |
| `bye`          | 退出登录     |

------

#### 3. 启用匿名上传功能

（1）配置共享目录权限

```bash
mkdir -p /var/ftp/pub
chmod 777 /var/ftp/pub
chown ftp:ftp /var/ftp/pub
```

（2）修改配置文件

编辑：

```bash
vim /etc/vsftpd/vsftpd.conf
```

确认或修改以下内容：

```
anonymous_enable=YES
anon_upload_enable=YES
anon_mkdir_write_enable=YES
write_enable=YES
listen=YES
listen_ipv6=NO
```

（3）重启服务

```bash
systemctl restart vsftpd.service
```

（4）上传测试

在客户端执行：

```bash
ftp 172.25.0.11
cd pub
put anaconda-ks.cfg
```

若显示 `Transfer complete`，表示上传成功。

------

#### 4. SELinux 配置与故障排查

若开启 SELinux 后出现上传失败，提示：

```
553 Could not create file.
```

可查看系统日志定位问题：

```bash
grep setroubleshoot /var/log/messages | tail -1
```

根据提示设置布尔值：

```bash
setsebool -P allow_ftpd_anon_write 1
setsebool -P ftpd_full_access 1
```

重新测试上传，问题应可解决。

------

### NFS服务器

#### 一、NFS 概述

1. 什么是 NFS

NFS（Network File System，网络文件系统）是一种允许不同主机之间共享目录和文件的协议。
 通过 NFS，Linux 客户端可以像访问本地磁盘一样访问远程服务器上的共享目录。

2. 常见用途

- 集中存放共享资源（如软件包、日志、备份文件）
- 提供团队共享空间
- 为集群或云主机提供统一数据访问

------

#### 二、NFS 基础知识

| 类型         | 示例            | 说明                     |
| ------------ | --------------- | ------------------------ |
| 本地文件系统 | EXT4, XFS, SWAP | 存储在本地磁盘上         |
| 伪文件系统   | /proc, /sys     | 内存映射信息，非物理存储 |
| 网络文件系统 | NFS, CIFS       | 远程服务器共享的目录     |

协议与端口

| 协议             | 端口         | 说明                        |
| ---------------- | ------------ | --------------------------- |
| NFS              | TCP/UDP 2049 | 实际文件共享协议            |
| RPC (portmapper) | TCP/UDP 111  | 协助客户端找到 NFS 服务端口 |

------

#### 三、NFS 基础搭建（Linux ↔ Linux）

**前提条件**

防火墙需要放行 NFS 相关服务：

```
firewall-cmd --permanent --zone=public --add-service=nfs
firewall-cmd --reload
```

------

##### 服务端配置（以 172.25.0.11 为例）

1. 安装 NFS 服务

```
yum -y install nfs-utils
```

2. 创建共享目录

```
mkdir /nfs
```

3. 编辑共享配置文件

```
vim /etc/exports
```

内容：

```
/nfs 172.25.0.10(ro)
```

含义说明：

- `/nfs`：要共享的目录
- `172.25.0.10`：允许访问的客户端 IP
- `(ro)`：只读权限

4. 启动服务并开机自启

```
systemctl restart nfs-server
systemctl enable nfs-server
```

------

##### 客户端配置（以 172.25.0.10 为例）

1. 创建挂载点

```
mkdir /nfs_mount
```

2. 设置自动挂载
    编辑 `/etc/fstab`：

```
172.25.0.11:/nfs /nfs_mount nfs defaults 0 0
```

立即生效：

```
mount -a
```

3. 验证共享

```
df -h /nfs_mount/
cat /nfs_mount/1.txt
```

------

#### 四、保护 NFS 文件共享（Kerberos 加密）

**为什么要保护 NFS**

默认的 NFS 是明文传输，没有身份认证。
 任何能访问服务器的客户端都能读取共享数据。
 为保证安全，可使用 Kerberos（krb5）提供：

- 身份认证（Authentication）
- 数据完整性校验（Integrity）
- 数据加密传输（Privacy）

------

#### 五、安全 NFS 环境配置（带 Kerberos）

**前提条件**

放行所有 NFS 服务端口：

```
firewall-cmd --permanent --zone=public --add-service=nfs
firewall-cmd --permanent --zone=public --add-service=mountd
firewall-cmd --permanent --zone=public --add-service=rpc-bind
firewall-cmd --reload
```

------

##### 服务端配置（172.25.0.11）

1. 安装 NFS 服务包

```
yum -y install nfs-utils
```

2. 创建共享目录

```
mkdir -p /public /protected/project
chown test1 /protected/project
```

说明：

- `/public` 对所有人开放（只读）
- `/protected/project` 属于用户 test1，可读写

3. 编辑 `/etc/exports`

```
/public    *(ro,sync)
/protected *(rw,sync,sec=krb5p)
```

参数说明：

- `ro`：只读访问
- `rw`：读写访问
- `sync`：实时写入磁盘，保证数据安全
- `sec=krb5p`：启用 Kerberos 加密认证访问

4. 下载 Kerberos 密钥文件

```
wget -O /etc/krb5.keytab http://172.25.0.254/pub/keytabs/server0.keytab
```

5. 启动并设置开机自启

```
systemctl restart nfs-server nfs-secure-server
systemctl enable nfs-server nfs-secure-server
```

------

##### 客户端配置（desktop0）

1. 创建挂载点

```
mkdir -p /mnt/nfsmount /mnt/nfssecure
```

2. 下载客户端密钥文件

```
wget -O /etc/krb5.keytab http://172.25.0.254/pub/keytabs/desktop0.keytab
```

3. 启动 Kerberos 服务

```
systemctl restart nfs-secure
systemctl enable nfs-secure
```

4. 查看服务器共享

```
showmount -e 172.25.0.11
```

输出示例：

```
/public    *
/protected *
```

5. 设置开机自动挂载
    编辑 `/etc/fstab`：

```
172.25.0.11:/public    /mnt/nfsmount   nfs4  defaults,_netdev 0 0
172.25.0.11:/protected /mnt/nfssecure  nfs4  defaults,_netdev,sec=krb5p 0 0
```

立即挂载：

```
mount -a
```

注意：
 使用 `nfs4` 类型，因为 Kerberos 加密（krb5p）仅支持 NFSv4。

6. 验证

```
df -h | grep nfs
su - test1
cd /mnt/nfssecure/project
touch testfile.txt
ls -l
```

用户 test1 能正常创建文件说明配置成功。

------

#### 六、常见问题与排查

| 问题                                                 | 可能原因                                     | 解决方法                                  |
| ---------------------------------------------------- | -------------------------------------------- | ----------------------------------------- |
| `mount.nfs: an incorrect mount option was specified` | 没安装 Kerberos 支持或使用 NFSv3             | 安装 `krb5-workstation`，使用 `nfs4` 挂载 |
| 无法连接服务端                                       | 防火墙或 rpcbind 未开放                      | 放行 111/2049 端口                        |
| 挂载超时                                             | 服务端未启动 nfs-server 或 nfs-secure-server | 重启相关服务                              |
| test1 无法写入 `/protected/project`                  | 目录属主错误                                 | `chown test1 /protected/project`          |

------

### SMB服务器

#### 开启samba可写

前提条件：搭建samba服务器步骤同上，共享用户：gx1，服务端共享目录：/smb_share，共享名：public，客户端共享目录：/smb_mount

1.  为服务添加写列表

   ```
   [public]
   path = /smb_share
   write list = gx1
   ```

2. 重启服务

   ```bash
   systemctl restart smb
   ```

3. 为共享用户添加写权限

   ```bash
   setfacl -m u:gx1:rwx /smb_share
   ```

4. 先设置SELinux为强制模式

   ```
   setenforce 1
   ```

5. 在强制模式下还需要修改SELinux的samba服务的波尔值

   ```bash
   getsebool -a | grep samba
   ```

   ```
   setsebool -P samba_export_all_rw on
   ```

#### 配置多用户SMB服务器

> 在server0通过SMB共享目录/devops，要求如下：
>
> 共享名为devops
>
> 共享目录devops必须可以被浏览
>
> 用户kenji能以读的方式访问此共享，访问密码是0000
>
> 用户chihiro能以读写的方式访问此共享，访问密码是0000
>
> 此共享永久挂载在desktop上的/mnt/dev目录
>
> 并使用用户可kenji作为认证
>
> 任何用户可以通过用户chihiro来临时获取写的权限

服务端：

1. 安装软件包

   ```
   yum -y install samba
   ```

2. 创建挂载点

   ```
   mkdir /devops
   ```

3. 修改配置文件

   ```
   vim /etc/samba/smb.conf
   
   [devops]
   path = /devops
   write list = chihiro
   ```

4. 创建samba账号，并设置密码

   ```
   useradd -s /sbin/nologin kenji
   useradd -s /sbin/nologin chihiro
   
   pdbedit -a kenji
   pdbedit -a chihiro
   ```

5. 重启服务，并设置开机自启

   ```bash
   systemctl restart smb.service
   systemctl enable smb.service
   ```

6. 在强制模式下还需要修改SELinux的samba服务的波尔值

   ```bash
   getsebool -a | grep samba
   ```

   ```
   setsebool -P samba_export_all_rw on
   ```

7. 给chihiro用户赋予读写执行权限acl

   ```bash
   setfacl -m u:chihiro:rwx /devops/
   ```

8. 创建一个文件到共享目录

   ```
   echo 123 > /devops/1.txt
   ```

客户端：

1. 创建挂载点

   ```
   mkdir /mnt/dev
   ```

2. 安装支持cifs文件系统的软件包

   ```bash
   yum -y install cifs-utils
   ```

3. 设置开机自动挂载

   ```bash
   vim /etc/fstab
   
   //172.25.0.11/devops   /mnt/dev   cifs   defaults,user=kenji,pass=0000,_netdev,multiuser,sec=ntlmssp   0 0
   ```

4. 验证与检验

   ```
   cat /mnt/dev/1.txt
   
   # 提示无权限
   echo 1234 > /mnt/dev/2.txt
   ```

   测试写权限

   ```bash
   # 先切换到普通用户，客户端才会发起向服务端提交凭证的过程
   su - test
   
   # 以test普通用户身份，雇佣chihiro作为cifs文件系统的临时身份，就可以拥有权限
   cifscreds add -u chihiro 172.25.0.11
   
   echo 1234 > /mnt/dev/2.txt
   ```

### 网站服务器

Web通信基本概念

基于Browser/Server架构的网页服务

服务端完成网站代码翻译，呈现页面提供给客户端

客户端通过浏览器下载并显示网页内容

超文本标记语言：Hyper Text Markup Language（HTML）

超文本传输协议：Hyper Text Transfer Protocol（HTTP）

常用Web服务器软件：Apache-httpd，Nginx，Tomcat

#### 搭建及管理WEB服务器(Apache-httpd)

准备工作：关闭防火墙，或者放行httpd服务，或者放行80端口。把SELinux设置为强制模式：`setenforce 1`

1. 安装软件包

   ```bash
   yum -y install httpd
   ```

2. 重启服务并设置开机自启

   ```bash
   systemctl restart httpd
   systemctl enable httpd
   ```

3. 访问测试，直接访问http://172.25.0.11/

4. 书写一个自己的主页页面文件

   httpd默认网页文件存放路径：/var/www/html/

   默认主页文件名称：index.html

   ```
   vim /var/www/html/index.html
   
   <h1>test
   ```

5. 再次访问测试，直接访问http://172.25.0.11/

------

##### 主配置文件

> **配置文件路径：**
>
> **主配置文件：/etc/httpd/conf/httpd.conf**
>
> **调用配置文件：/etc/httpd/conf.d/*.conf**
>
> 
>
> Apache-httpd网站服务引擎常用配置：
>
> ServerName：本站点注册的DNS域名
>
> Listen：监听端口（默认：80）
>
> DocumentRoot：网页文件根目录（默认/var/www/html）
>
> DirectorIndex：首页文件名（默认：index.html，一般不作更改）

1. 修改访问域名

   - 备份配置文件


   ```
   cp /etc/httpd/conf/httpd.conf {,./backup}
   ```

   2. 修改配置文件

   ```bash
   vim /etc/httpd/conf/httpd.conf
   
   # ServerName www.example.com:80 --> ServerName server0.example.com:80
   ```

   3. 重启服务

   ```bash
   systemctl restart httpd
   ```

   4. 修改hosts文件

   ```bash
   C:\Windows\System32\drivers\etc\hosts
   
   # 添加内容
   172.25.0.11	server0.example.com www0.example.com webapp0.example.com
   172.25.0.10	desktop0.example.com desktop0
   172.25.0.254 classroom.example.com classroom
   172.25.254.254 classroom.example.com
   172.25.254.254 content.example.com
   ```


2. 自定义网页文件根目录

   1. 修改配置文件内的配置项

      ```bash
      vim /etc/httpd/conf/httpd.conf
      
      # DocumentRoot "/var/www/html" --> DocumentRoot "/var/www/test"
      ```

##### 虚拟web主机

在一台服务器上部署多个站点

作用：提高服务器资源的利用率

构建方法：

基于域名的虚拟web主机（一个IP绑定多个域名）

基于端口的虚拟web主机（基于IP或者域名，将服务分开多个端口号）

基于IP地址的虚拟web主机（已被淘汰，使用多个IP地址成本高）

------

###### 基于域名的虚拟web主机

参数说明：

ServerName：域名



1. 把原有站点迁移为虚拟web主机（一旦使用了虚拟web主机功能，该服务器上所有站点都必须使用虚拟web主机）

   ```bash
   vim /etc/httpd/conf.d/web1.conf
   
   # 为虚拟web主机添加以下配置（代表1个虚拟web主机）
   <VirtualHost *:80>
   ServerName server0.example.com
   DocumentRoot /var/www/web1
   </VirtualHost>
   <VirtualHost *:80>
   ServerName www0.example.com
   DocumentRoot /var/www/web2
   </VirtualHost>
   ```

2. 创建文件夹，并在文件夹内创建一个index.html测试页面

   ```bash
   mkdir /var/www/web1 /var/www/web2
   
   echo '<h1>web1' > /var/www/web1/index.html
   echo '<h1>web2' > /var/www/web2/index.html
   ```

3. 重启服务

   ```bash
   systemctl restart httpd
   ```

4. 访问测试

   ```bash
   curl server0.example.com
   
   curl www0.example.com
   ```

##### httpd的访问控制

使用`<Directory>`配置区段

每个文件夹自动继承其父目录的ACL访问控制，除非针对子目录有明确设置

参数说明：

Directory：

RequireAll：

Require：

1. 在/var/www/之外创建一个目录及文件

   ```
   mkdir /webroot
   echo test > /webroot/index.html
   ```

2. 修改配置文件（可以创建一个新的文件，也可以写到web1.conf，或者把网页配置单独放一起）

   ```
   vim /etc/httpd/conf.d/web1.conf
   
   <VirtualHost *:80>
   ServerName server0.example.com
   DocumentRoot /var/www/web1
   </VirtualHost>
   <VirtualHost *:80>
   ServerName www0.example.com
   DocumentRoot /var/www/web2
   </VirtualHost>
   <VirtualHost *:80>
   ServerName webapp0.example.com
   DocumentRoot /webroot
   </VirtualHost>
   ```

   ```
   vim /etc/httpd/conf.d/acl.conf
   
   <Directory "/webroot">
       Require all granted
   </Directory>
   
   <Directory "/var/www/web1">
       Require all granted
   </Directory>
   
   <Directory "/var/www/web2">
       Require all granted
   </Directory>
   ```

3. 重启服务

   ```
   systemctl restart httpd
   ```

4. 修改安全上下文值

   ```
   chcon -R -t httpd_sys_rw_content_t /webroot
   ```

   或使用以下命令直接复制其他文档的安全上下文到该目录

   ```bash
   chcon -R --reference=/var/www/web1 /webroot
   ```

   查看安全上下文

   ```bash
   ls -Zd /webroot/index.html
   
   -rw-r--r--. root root unconfined_u:object_r:httpd_sys_content_t:s0 /webroot/index.html
   ```

5. 访问测试

   ```
   curl webapp0.example.com
   ```

#### 静态与动态

静态页面：服务端的原始网页 = 浏览器访问到的页面（文本txt/html，图片，视频，音频），由WEB服务软件处理所有请求

动态页面：服务端的原始网页 ≠ 浏览器访问到的页面（PHP网页，Python网页，JSP网页），由WEB服务软件接受请求，动态程序转交由后端模块处理

1. 获取一个Python

   ```bash
   wget -O /var/www/web1/webinfo.wsgi http://172.25.0.254/materials/webinfo.wsgi
   ```

2. 安装处理Python代码的后端模块

   ```bash
   yum -y install mod_wsgi
   ```

3. 修改配置文件，把server0.example.com的页面重定向为/var/www/web1/webinfo.wsgi

   ```bash
   vim /etc/httpd/conf.d/web1.conf
   
   # <VirtualHost *:80>
   # ServerName server0.example.com
   # DocumentRoot /var/www/web1
   # </VirtualHost>
   <VirtualHost *:80>
   ServerName server0.example.com
   DocumentRoot /var/www/web1
   WsgiScriptAlias / /var/www/web1/webinfo.wsgi
   </VirtualHost>
   <VirtualHost *:80>
   ServerName www0.example.com
   DocumentRoot /var/www/web2
   </VirtualHost>
   <VirtualHost *:80>
   ServerName webapp0.example.com
   DocumentRoot /webroot
   </VirtualHost>
   ```

4. 重启服务

   ```
   systemctl restart httpd
   ```

5. 访问测试

   ```
   curl server0.example.com
   ```

##### 拓展

额外要求：此虚拟WEB主机侦听的端口为8909

1. 修改配置文件，将`webinfo.wsgi`的端口设置为非默认端口8909

   ```bash
   vim /etc/httpd/conf.d/web1.conf
   
   <VirtualHost *:80>
   ServerName server0.example.com
   DocumentRoot /var/www/web1
   </VirtualHost>
   
   Listen 8909
   <VirtualHost *:8909>
   ServerName server0.example.com
   DocumentRoot /var/www/web1
   WsgiScriptAlias / /var/www/web1/webinfo.wsgi
   </VirtualHost>
   
   <VirtualHost *:80>
   ServerName www0.example.com
   DocumentRoot /var/www/web2
   </VirtualHost>
   <VirtualHost *:80>
   ServerName webapp0.example.com
   DocumentRoot /webroot
   </VirtualHost>
   ```

2. 开放非默认端口

   ```
   semanage port -l | grep http
   
   http_cache_port_t              tcp      8080, 8118, 8123, 10001-10010
   http_cache_port_t              udp      3130
   http_port_t                    tcp      80, 81, 443, 488, 8008, 8009, 8443, 9000
   pegasus_http_port_t            tcp      5988
   pegasus_https_port_t           tcp      5989
   
   
   semanage port -a -t http_port_t -p tcp 8909
   ```

3. 访问测试

   ```bash
   curl server0.example.com:8909
   ```

#### 部署安全Web服务(https)

使用https协议，默认端口号：443

公钥：用来加密数据

私钥：用来解密数据（与相应的公钥1对1的匹配）

数字证书：证明拥有者的合法性、权威性

数字证书授权中心（CA机构）：负责证书申请/颁发/审核/鉴定/撤销等管理工作

**配置https：**

1. 安装软件包

   ```
   yum -y install mod_ssl
   ```

2. 修改配置文件

   ```
   vim /etc/httpd/conf.d/ssl.conf
   
   <VirtualHost _default_:443>		# 在该行下面添加内容
   ServerName server0.example.com
   DocumentRoot /var/www/web1
   
   ### 指定网站证书位置
   SSLCertificateFile /etc/pki/tls/certs/localhost.crt
   改成
   SSLCertificateFile /etc/pki/tls/certs/server0.crt
   
   ### 指定私钥位置
   SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
   改成
   SSLCertificateKeyFile /etc/pki/tls/private/server0.key
   
   ### 指定根证书位置（先打开注释）
   SSLCACertificateFile /etc/pki/tls/certs/ca-bundle.crt
   改成
   SSLCACertificateFile /etc/pki/tls/certs/example-ca.crt
   ```

3. 下载证书及私钥到对应位置

   ```
   wget -O /etc/pki/tls/certs/server0.crt http://172.25.0.254/pub/tls/certs/server0.crt
   wget -O /etc/pki/tls/private/server0.key http://172.25.0.254/pub/tls/private/server0.key
   wget -O /etc/pki/tls/certs/example-ca.crt http://172.25.0.254/pub/example-ca.crt
   ```

4. 重启服务

   ```bash
   systemctl restart httpd
   ```

5. 访问测试

   ```
   firefox https://server0.example.com
   ```

**httpd服务排错方法：**

1. systemctl status httpd
2. httpd -t

### ISCSI服务器

ISCSI 主要是透过TCP/IP的技术，将存储设备端透过ISCSI target (ISCSI目标）功能，做成可以提供磁盘的服务器端，再透过ISCSI initator（ISCSI初始化用户）功能，做成能够挂载使用ISCSI target的客户端，如此便能透过ISCSI协议来进行磁盘的应用了。

ISCSI 这个架构主要将存储装置与使用的主机分别为两部分，分别是：

- ISCSI target ：就是存储设备端，存放磁盘或RAID的设备，目前也能够将Linux主机仿真成ISCSI target了，目的在提供其他主机使用的磁盘。

- ISCSI inITiator： 就是能够使用target的客户端，通常是服务器，只有装有iscsi initiator的相关功能后才能使用ISCSI target 提供的磁盘。

iSCSI技术在工作形式上分为服务端（target）与客户端（initiator）。iSCSI服务端即用于存放硬盘存储资源的服务器，它作为前面创建的RAID磁盘阵列的存储端，能够为用户提供可用的存储资源。iSCSI客户端则是用户使用的软件，用于访问远程服务端的存储资源。

iscsi server被称为target server，模拟scsi设备，后端存储设备可以使用文件/LVM/磁盘/RAID等不同类型的设备；启动设备（initiator）：发起I/O请求的设备，需要提供iscsi服务，比如PC机安装iscsi-initiator-utils软件实现，或者通过网卡自带的PXE启动。esp+ip+scsi

iscsi再传输数据的时候考虑了安全性，可以通过IPSEC 对流量加密，并且iscsi提供CHAP认证机制以及ACL访问控制，但是在访问iscsi-target的时候需要IQN（iscsi完全名称，区分唯一的initiator和target设备），格式iqn.年月.域名后缀(反着写)：[target服务器的名称或IP地址]

iscsi使用TCP端口3260提供服务



IscsiQualifiedName（IQN名称）：有专门的规范要求target磁盘组命名必须按照以下格式：

iqn.yyyy-mm.倒序域名:自定义名称

作用：用来标识target磁盘组，也用来识别客户机的身份

示例：

iqn.2025-10.com.example:server0

iqn.2025-10.com.example:desktop0

#### 服务端配置

前提条件：有一块空磁盘，没有的话就需要自行添加

1. 查看磁盘

   ```bash
   lsblk
   ```

2. 对sdb进行分区，只需要分一个主分区即可

   ```
   fdisk /dev/sdb
   ```

   ```bash
   [root@server0 ~]# lsblk
   NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
   sda      8:0    0   10G  0 disk
   └─sda1   8:1    0   10G  0 part /
   sdb      8:16   0   10G  0 disk
   └─sdb1   8:17   0   10G  0 part
   sr0     11:0    1 1024M  0 rom
   ```

3. 安装软件包

   ```bash
   yum -y install targetcli
   ```

4. 使用 targetcli工具(交互式)配置 iscsi服务端

   ```
   targetcli
   ```

   1. 查看iscsi的结构

      ```
      ls
      
      # 结果如下
      o- / ................................................................................................................... [...]
        o- backstores ........................................................................................................ [...]
        | o- block ............................................................................................ [Storage Objects: 0]
        | o- fileio ........................................................................................... [Storage Objects: 0]
        | o- pscsi ............................................................................................ [Storage Objects: 0]
        | o- ramdisk .......................................................................................... [Storage Objects: 0]
        o- iscsi ...................................................................................................... [Targets: 0]
        o- loopback ................................................................................................... [Targets: 0]
      ```

   2. 建立后端存储（指定物理磁盘）

      命令格式：`backstores/block create name=后端存储名称 dev=物理设备路径`

      ```
      backstores/block create name=hhh dev=/dev/sdb1
      ```

      ```
      ls
      
      # 结果如下
      o- / ................................................................................................................... [...]
        o- backstores ........................................................................................................ [...]
        | o- block ............................................................................................ [Storage Objects: 1]
        | | o- hhh .................................................................... [/dev/sdb1 (10.0GiB) write-thru deactivated]
        | o- fileio ........................................................................................... [Storage Objects: 0]
        | o- pscsi ............................................................................................ [Storage Objects: 0]
        | o- ramdisk .......................................................................................... [Storage Objects: 0]
        o- iscsi ...................................................................................................... [Targets: 0]
        o- loopback ................................................................................................... [Targets: 0]
      ```

   3. 建立target磁盘组

      命令格式：iscsi/ create wwn=iqn.yyyy-mm.倒序域名:自定义名称

      ```
      iscsi/ create wwn=iqn.2025-10.com.example:server0
      ```

      ```
      ls
      
      # 结果如下
      o- / ................................................................................................................... [...]
        o- backstores ........................................................................................................ [...]
        | o- block ............................................................................................ [Storage Objects: 1]
        | | o- hhh .................................................................... [/dev/sdb1 (10.0GiB) write-thru deactivated]
        | o- fileio ........................................................................................... [Storage Objects: 0]
        | o- pscsi ............................................................................................ [Storage Objects: 0]
        | o- ramdisk .......................................................................................... [Storage Objects: 0]
        o- iscsi ...................................................................................................... [Targets: 1]
        | o- iqn.2025-10.com.example:server0 ............................................................................. [TPGs: 1]
        |   o- tpg1 ......................................................................................... [no-gen-acls, no-auth]
        |     o- acls .................................................................................................... [ACLs: 0]
        |     o- luns .................................................................................................... [LUNs: 0]
        |     o- portals .............................................................................................. [Portals: 0]
        o- loopback ................................................................................................... [Targets: 0]
      ```

   4. 创建客户端访问的acl（指定客户端的iqn名称）

      命令格式：iscsi/磁盘组名称/tpg1/acls create wwn=iqn.yyyy-mm.倒序域名:自定义名称(客户端的iqn名称)

      ```
      iscsi/iqn.2025-10.com.example:server0/tpg1/acls create wwn=iqn.2025-10.com.example:desktop0
      ```

      ```
      /> ls
      o- / ................................................................................................................... [...]
        o- backstores ........................................................................................................ [...]
        | o- block ............................................................................................ [Storage Objects: 1]
        | | o- hhh .................................................................... [/dev/sdb1 (10.0GiB) write-thru deactivated]
        | o- fileio ........................................................................................... [Storage Objects: 0]
        | o- pscsi ............................................................................................ [Storage Objects: 0]
        | o- ramdisk .......................................................................................... [Storage Objects: 0]
        o- iscsi ...................................................................................................... [Targets: 1]
        | o- iqn.2025-10.com.example:server0 ............................................................................. [TPGs: 1]
        |   o- tpg1 ......................................................................................... [no-gen-acls, no-auth]
        |     o- acls .................................................................................................... [ACLs: 1]
        |     | o- iqn.2025-10.com.example:desktop0 ............................................................... [Mapped LUNs: 0]
        |     o- luns .................................................................................................... [LUNs: 0]
        |     o- portals .............................................................................................. [Portals: 0]
        o- loopback ................................................................................................... [Targets: 0]
      
      ```

   5. 创建luns（把后端存储放到磁盘组）

      命令格式：iscsi/磁盘组名称/tpg1/luns create 后端存储路径

      ```bash
      iscsi/iqn.2025-10.com.example:server0/tpg1/luns create /backstores/block/hhh
      ```

      ```
      ls
      
      o- / ................................................................................................................... [...]
        o- backstores ........................................................................................................ [...]
        | o- block ............................................................................................ [Storage Objects: 1]
        | | o- hhh ...................................................................... [/dev/sdb1 (10.0GiB) write-thru activated]
        | o- fileio ........................................................................................... [Storage Objects: 0]
        | o- pscsi ............................................................................................ [Storage Objects: 0]
        | o- ramdisk .......................................................................................... [Storage Objects: 0]
        o- iscsi ...................................................................................................... [Targets: 1]
        | o- iqn.2025-10.com.example:server0 ............................................................................. [TPGs: 1]
        |   o- tpg1 ......................................................................................... [no-gen-acls, no-auth]
        |     o- acls .................................................................................................... [ACLs: 1]
        |     | o- iqn.2025-10.com.example:desktop0 ............................................................... [Mapped LUNs: 1]
        |     |   o- mapped_lun0 ............................................................................. [lun0 block/hhh (rw)]
        |     o- luns .................................................................................................... [LUNs: 1]
        |     | o- lun0 .................................................................................... [block/hhh (/dev/sdb1)]
        |     o- portals .............................................................................................. [Portals: 0]
        o- loopback ................................................................................................... [Targets: 0]
      ```

   6. 创建portals（指定业务网口）

      命令格式：iscsi/磁盘组名称/tpg1/portals create 172.25.0.11	# 不需要指定端口号，会自动分配默认端口号：3260

      ```bash
      iscsi/iqn.2025-10.com.example:server0/tpg1/portals create 172.25.0.11
      ```

      ```
      ls
      
      o- / ................................................................................................................... [...]
        o- backstores ........................................................................................................ [...]
        | o- block ............................................................................................ [Storage Objects: 1]
        | | o- hhh ...................................................................... [/dev/sdb1 (10.0GiB) write-thru activated]
        | o- fileio ........................................................................................... [Storage Objects: 0]
        | o- pscsi ............................................................................................ [Storage Objects: 0]
        | o- ramdisk .......................................................................................... [Storage Objects: 0]
        o- iscsi ...................................................................................................... [Targets: 1]
        | o- iqn.2025-10.com.example:server0 ............................................................................. [TPGs: 1]
        |   o- tpg1 ......................................................................................... [no-gen-acls, no-auth]
        |     o- acls .................................................................................................... [ACLs: 1]
        |     | o- iqn.2025-10.com.example:desktop0 ............................................................... [Mapped LUNs: 1]
        |     |   o- mapped_lun0 ............................................................................. [lun0 block/hhh (rw)]
        |     o- luns .................................................................................................... [LUNs: 1]
        |     | o- lun0 .................................................................................... [block/hhh (/dev/sdb1)]
        |     o- portals .............................................................................................. [Portals: 1]
        |       o- 172.25.0.11:3260 ........................................................................................... [OK]
        o- loopback ................................................................................................... [Targets: 0]
      ```

   7. 保存并退出

      ```
      exit
      
      
      Global pref auto_save_on_exit=true
      Last 10 configs saved in /etc/target/backup.		# 自动备份
      Configuration saved to /etc/target/saveconfig.json	# 自动保存
      ```

5. 启动服务，并设置开机自启

   ```bash
   systemctl restart target
   systemctl enable target
   ```

#### 客户端配置

1. 检测磁盘

   1. 安装软件包

      ```
      yum -y install iscsi-initiator-utils
      ```

   2. 修改配置文件

      ```bash
      vim /etc/iscsi/initiatorname.iscsi
      
      # 将这个名称修改为在服务端配置的名称
      InitiatorName=iqn.2025-10.com.example:desktop0
      ```

   3. 重启服务，刷新客户端声明的iqn别名，并设置服务开机自启

      ```bash
      systemctl restart iscsid
      systemctl enable iscsid
      ```

   4. 发现服务端的共享磁盘

      可以使用`man iscsiadm`去看该命令的帮助手册

      ```bash
      iscsiadm --mode discoverydb --type sendtargets --portal 172.25.0.11 --discover
      
      172.25.0.11:3260,1 iqn.2025-10.com.example:server0
      ```

      > 若输入命令后无类似输出原因可能有以下几点
      >
      > 1. 在targetcli中配置出错
      > 2. 配置文件中的名称错误
      > 3. 客户端或服务端的iscsi服务未启动

   5. 初次验证（此时还未重启iscsi服务，是还没有共享到磁盘的）

      ```
      [root@desktop0 ~]# lsblk
      NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
      sda      8:0    0   10G  0 disk
      └─sda1   8:1    0   10G  0 part /
      sdb      8:16   0   10G  0 disk
      sr0     11:0    1 1024M  0 rom
      ```

   6. 重启iscsi服务并设置服务开机自启

      ```
      systemctl restart iscsi
      systemctl enable iscsi
      ```

   7. 再次验证（此时就可以看到共享的磁盘了）

      ```
      [root@desktop0 ~]# lsblk
      NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
      sda      8:0    0   10G  0 disk
      └─sda1   8:1    0   10G  0 part /
      sdb      8:16   0   10G  0 disk
      sdc      8:32   0   10G  0 disk
      sr0     11:0    1 1024M  0 rom
      ```

2. 设置iscsi客户端开机自动启动配置

   ```
   vim /var/lib/iscsi/nodes/iqn.2025-10.com.example\:server0/172.25.0.11\,3260\,1/default
   
   node.conn[0].startup = manual
   改为
   node.conn[0].startup = automatic
   ```

3. 使用磁盘

   1. 分区（与物理磁盘一样分区）

      ```bash
      fdisk /dev/sdc
      ```

   2. 格式化

      ```bash
      mkfs.ext4 /dev/sdc1
      
      blkid /dev/sdc1
      
      /dev/sdc1: UUID="40eb2461-ad5c-4350-a957-9e2e6ffa7bdd" TYPE="ext4"
      ```

   3. 设置开机自动挂载

      由于ISCSI是网络磁盘，所有每次开机的时候盘符号有可能会发生改变

      ```bash
      vim /etc/fstab
      
      UUID=40eb2461-ad5c-4350-a957-9e2e6ffa7bdd /iscsi0 ext4 defaults,_netdev 0 0
      ```

   4. 创建挂载点

      ```bash
      mkdir /iscsi0
      ```

   5. 挂载，并查看是否成功挂载

      ```
      mount -a
      
      df -Th /dev/sdc1
      ```

### MariaDB数据库

什么是数据库？

数据库DB：一些数据的集合，一般是存放关系型数据的表格

关系型数据：表格中同一行的数据之间有一定的关联关系

数据库管理系统DBMS：用来操作和管理数据库的软件平台

#### 数据库搭建

1. 安装软件包

   mariadb：提供管理数据库的命令工具

   mariadb-server：提供服务相关的软件程序

   ```bash
   yum -y install mariadb mariadb-server
   ```

2. 查看数据库目录

   通过配置文件可以查看到数据库目录的路径

   ```
   vim /etc/my.cnf
   
   [mysqld]
   datadir=/var/lib/mysql
   socket=/var/lib/mysql/mysql.sock
   ```

   ```
   ls /var/lib/mysql
   
   # 目录为空
   ```

3. 重启数据库服务，并设置服务开机自启

   ```
   systemctl restart mariadb
   systemctl enable mariadb
   ```

4. 再次查看，可以发现此时目录不为空

   ```
   ls /var/lib/mysql
   ```

#### 数据库简单操作

1. 初始化配置，默认mariadb无密码

   ```
   mysql
   ```

2. 简单的数据管理

   - 查看数据库

     ```sql
     show databases;
     ```

   - 创建数据库

     ```sql
     create database aaa
     ```

   - 使用数据库

     ```sql
     use aaa
     ```

   - 删除数据库

     ```
     drop database aaa
     ```

   3. 为数据库设置和修改管理员密码

      **若想采用交互式的方式设置密码，则不在命令中明文输入密码即可**

      ```bash
      mysqladmin -uroot password '0000'
      ```

      ```
      mysqladmin -uroot -p0000 password '1234'
      ```

   4. 再次登录数据库

      ```bash
      mysql -uroot -p0000
      ```

   5. 禁用数据库监听网络，仅服务于本机

      ```
      vim /etc/my.cnf
      
      # 在[mysqld]添加
      skip-networking
      ```

   6. 重启服务

      ```bash
      systemctl restart mariadb
      ```


#### 导入、恢复数据到数据库

1. 导入数据

   ```bash
   mysql -uroot -p0000 aaa < users.sql
   ```

2. 查看是否导入成功

   ```
   mysql -uroot -p0000
   
   show databases;
   
   use aaa
   
   show tables;
   
   select * from base;
   select * from location;
   ```

#### 数据库的管理

表字段：表头

表记录：数据内容

数据库四大操作：增：insert，删：delete，改：update，查：select

1. 添加数据

   ```sql
   INSERT INTO 表名(字段名1,字段名2,...) VALUES (值1,值2,...);
   
   # 示例
   insert into user(id, name, age, gender) values (1, 'Alice', 18, '女');
   ```

   **注意：**插入数据时，指定的字段顺序要与值的顺序一一对应。字符串和日期型数据要包含在引号(`''`)中。插入数据的大小，应该再字段规定范围内。

2. 删除数据

   ```sql
   DELETE FROM 表名 [WHERE 条件]
   
   # 示例
   DELETE FROM user ORDER BY id DESC LIMIT 1; -- 删除最后一行数据
   delete from user where name='Make';
   ```

3. 修改数据

   ```sql
   UPDATE 表名 SET 字段名1=值1, 字段名2=值2,... [WHERE 条件];
   
   # 示例
   update user set age = 19 where name='Alice';
   ```

4. 查询数据

   ```sql
   SELECT DISTINCT 字段列表 FROM 表名;	(DISTINCT：去除重复记录)
   SELECT 字段列表 FROM 表名 WHERE 条件列表;
   ```

#### 权限管理

命令格式：grant 权限列表 on 数据库名.表名 to 用户名@客户机地址 identified by '密码'

权限列表：增：insert，删：delete，改：update，查：select，所有：all



## Shell

Shell的作用：

- 解释执行用户输入的命令或程序等
- 用户输入一条命令，shell就解释一条
- 键盘输入命令，Linux给与响应的方式，称之为交互式

shell是一块包裹着系统核心的壳，处于操作系统的最外层，与用户直接对话，把用户的输入，解释给操作系统，然后处理操作系统的输出结果，输出到屏幕给与用户看到结果

shell脚本语言属于一种弱类型语言，无需声明变量类型，直接定义使用

强类型语言，必须先定义变量类型，确定是数字、字符串等，之后再赋予同类型的值



脚本：一个可执行的可以实现某种预设功能的文本文件

脚本规范：

1. 脚本文件命名：自定义名称.sh
2. 脚本三要素：
   1. 声明环境：#!/bin/bash
   2. 必要的注释：作者信息、代码结构与代码的解释
   3. 可执行代码（命令+逻辑结构）

Shell脚本编码常用工具：

1. 管道符 | ：将管道前面命令的执行结果交由管道后面的命令进行二次处理

2. 输出重定向（>覆盖，>>追加，>:只收集正确输出，2>只收集错误输出，&>收集正确输出与错误输出）

   黑洞设备：/dev/null	###粉碎输出结果

   应用场景：当Shell脚本中执行的命令是不想要的输出结果，可以使用重定向将输出结果重新定向到黑洞设备

3. 整数运算：$[+-*/%]：仅支持整数运算，/向下取整；%取余

4. 取消字符特殊意义（转义）：

   1. 字符串：取消''里的所有字符的特殊意义
   2. \：取消\后面一个字符的特殊意义

5. 将命令的输出作为参数参与其他命令执行：$(命令)或'命令'

6. exit：退出脚本，一般是通过条件判断不符合条件报错后退出；特殊用法：exit 值，可以定义$?的返回值，可以知道有无报错

变量：以不变的名称存放可以变化的量

1. Shell脚本中定义变量并赋值：变量名=变量值
2. Shell脚本中引用变量：$变量名
3. 查看变量值：`echo $变量名` 或 `eho${变量名}`

变量名规范：只能由字母，数字，下划线组成，严格区分大小写，不能以数字开头（除系统自带变量）

四大类变量：

1. 自定义变量：用户自主设置、修改及使用

2. 环境变量：由系统定义并赋值，用户直接调用即可

   常见的环境变量：

   $USER：当前登录的用户

   $LANG：当前终端使用的语言编码

   $HOME：当前登录用户的家目录

   $PATH：系统执行命令的搜索路径

   $RANDOM：随机数

3. 位置变量：由系统定义并赋值，用户直接调用即可

   作用：执行脚本时，可以用来作为参数

   表示为${n}（n为序号），代表执行脚本时的第n个参数

   示例：

4. 预定义变量：由系统定义并 赋值，用户直接调用即可

   $?程序或脚本退出后的状态值，默认为0，代表正常退出，非0为其他异常状态

   $#已加载的位置变量的个数，求和

   $*所有位置变量的值

**检查系统环境变量的命令**

- set，输出所有变量，包括全局变量、局部变量
- env，只显示全局变量
- declare，输出所有的变量，如同set
- export，显示和设置环境变量值

**撤销环境变量**

删除变量或函数

```
unset 变量名
```

**设置只读变量**

```
# 显示当前系统的只读变量
readonly

# 设置只读变量
readonly 变量名=变量值
```

### Shell子串

bash一些基础的内置命令

```
echo
eval
exec
export
read
shift
```

**echo**命令

```
-n 不换行输出
-e 解析字符串中的特殊符号

\n 换行
\r 回车
\t 制表符 四个空格
\b 退格
```

**eval**命令

执行多个命令

```
eval ls;echo 111
```

**exec**

不创建子进程，执行后续命令，且执行完毕后，自动exit

```
exec date
```

#### 概述

Shell 子串操作基于「参数展开（Parameter Expansion）」机制实现，无需依赖外部命令即可快速完成字符串的**引用、长度计算、截取、删除、替换**等操作，是脚本开发中的核心效率工具。

#### 核心用法

以下所有示例均基于测试变量：str="abc123abc123"（字符长度 12）。

##### 1. 基础引用与长度计算

| 语法格式 | 功能说明                           | 实践示例     | 输出结果     |
| -------- | ---------------------------------- | ------------ | ------------ |
| ${变量}  | 基础变量引用，返回变量原始值       | echo ${str}  | abc123abc123 |
| ${#变量} | 计算变量内容的字符长度（效率最高） | echo ${#str} | 12           |

##### 2. 字符串截取（切片）

通过索引定位截取子串，**索引从 0 开始**，支持正向与反向定位。

| 语法格式             | 功能说明                               | 实践示例          | 输出结果  |
| -------------------- | -------------------------------------- | ----------------- | --------- |
| ${变量:start}        | 从 start 位置截取至字符串结尾          | echo ${str:3}     | 123abc123 |
| ${变量:start:length} | 从 start 位置截取 length 个字符        | echo ${str:3:3}   | 123       |
| ${变量: -n}          | 截取最后 n 个字符（注意 “:” 后有空格） | echo ${str: -3}   | 123       |
| ${变量: -n:m}        | 从倒数第 n 个字符开始截取 m 个字符     | echo ${str: -6:3} | abc       |

##### 3. 字符串删除（按匹配规则）

通过#（从开头）、%（从结尾）结合匹配规则删除子串，#/%表示最短匹配，##/%%表示最长匹配。

| 语法格式      | 功能说明                  | 实践示例             | 输出结果  |
| ------------- | ------------------------- | -------------------- | --------- |
| ${变量#word}  | 从开头删除最短匹配的 word | echo ${str#abc}      | 123abc123 |
| ${变量##word} | 从开头删除最长匹配的 word | echo ${str##abc*123} | abc123    |
| ${变量%word}  | 从结尾删除最短匹配的 word | echo ${str%123}      | abc123abc |
| ${变量%%word} | 从结尾删除最长匹配的 word | echo ${str%%123*abc} | abc       |

##### 4. 字符串替换（按匹配规则）

通过/分隔匹配模式与替换内容，支持单次替换与全局替换。

| 语法格式                | 功能说明                           | 实践示例             | 输出结果     |
| ----------------------- | ---------------------------------- | -------------------- | ------------ |
| ${变量/pattern/string}  | 替换第一个匹配的 pattern 为 string | echo ${str/abc/XYZ}  | XYZ123abc123 |
| ${变量//pattern/string} | 替换所有匹配的 pattern 为 string   | echo ${str//abc/XYZ} | XYZ123XYZ123 |

> 1. **索引规则**：正向索引从 0 开始，反向索引需在:后加空格（如${str: -3}），否则会被解析为默认值语法。
>
> 2. **匹配优先级**：#/%（最短匹配）仅删除第一个符合规则的子串，##/%%（最长匹配）会删除到最后一个符合规则的子串，可结合通配符（如*）使用。
>
> 3. **效率优势**：参数展开是 Shell 内置机制，比wc、expr等外部命令的字符串处理效率高 10 倍以上。
>
> 4. **特殊变量支持**：对位置参数（如${10}）、数组元素（如${arr[0]:2:3}）同样适用，需用大括号保护变量名。

##### 示例：批量修改文件名

```bash
touch test{1..5}-www.txt
touch test{1..5}-www.sh

ls -l
# 输出结果
-rw-r--r--. 1 root root    0 Nov 17 16:35 test1-www.sh
-rw-r--r--. 1 root root    0 Nov 17 16:35 test1-www.txt
-rw-r--r--. 1 root root    0 Nov 17 16:35 test2-www.sh
-rw-r--r--. 1 root root    0 Nov 17 16:35 test2-www.txt
-rw-r--r--. 1 root root    0 Nov 17 16:35 test3-www.sh
-rw-r--r--. 1 root root    0 Nov 17 16:35 test3-www.txt
-rw-r--r--. 1 root root    0 Nov 17 16:35 test4-www.sh
-rw-r--r--. 1 root root    0 Nov 17 16:35 test4-www.txt
-rw-r--r--. 1 root root    0 Nov 17 16:35 test5-www.sh
-rw-r--r--. 1 root root    0 Nov 17 16:35 test5-www.txt


for ff in `ls test*-www.*`;do mv $ff `echo ${ff//-www/}`;done
ls -l
# 输出结果
-rw-r--r--. 1 root root    0 Nov 17 16:35 test1.sh
-rw-r--r--. 1 root root    0 Nov 17 16:35 test1.txt
-rw-r--r--. 1 root root    0 Nov 17 16:35 test2.sh
-rw-r--r--. 1 root root    0 Nov 17 16:35 test2.txt
-rw-r--r--. 1 root root    0 Nov 17 16:35 test3.sh
-rw-r--r--. 1 root root    0 Nov 17 16:35 test3.txt
-rw-r--r--. 1 root root    0 Nov 17 16:35 test4.sh
-rw-r--r--. 1 root root    0 Nov 17 16:35 test4.txt
-rw-r--r--. 1 root root    0 Nov 17 16:35 test5.sh
-rw-r--r--. 1 root root    0 Nov 17 16:35 test5.txt
```

### 特殊shell扩展变量

```perl
如果char变量值为空，返回str字符串，赋值给res变量
res=${char:-str}

如果char变量值为空，则str字符串替代变量值，且返回其值
res=${char:=str}

如果char变量值为空，str当做stderr(标准错误信息)输出，否则输出变量值
用于设置变量为空导致错误时，返回的错误信息
${char:?str}

如果char变量值为空，什么都不做，否则str返回
${char:+str}
```

#### 示例：数据备份，删除过期数据的脚本

```bash
find xargs 搜索，且删除
# 删除七天以上的过期数据
find 搜索目录 -name 文件名 -type 文件类型 -mtime +7 | xargs rm -rf

vim remove_file.sh
find ${dir:=/root} -name '*.sh' -type f -mtime +1 | xargs rm -rf
```

