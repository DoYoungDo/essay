*2025-09-10*

## 修改host并重启DNS

### 修改 hosts 文件

> Windows 系统：C:\Windows\System32\drivers\etc\hosts  
> Mac（苹果电脑）系统：/etc/hosts  
> Linux 系统：/etc/hosts

修改方法

> Windows 的 C:\Windows\System32\drivers\etc\hosts文件没有直接修改的权限，需要将其拷贝到其它有读写权限的位置，修改后再将其替换回原位置  
> Linux、Mac 使用 Root 权限：sudo vi /etc/hosts

### 重启DNS

> Windows：在 CMD 窗口输入：ipconfig /flushdns  
> Mac 命令：sudo killall -HUP mDNSResponder  
> Linux 命令：sudo nscd restart  