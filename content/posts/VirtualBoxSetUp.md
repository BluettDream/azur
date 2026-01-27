---
title: "VirtualBox最小化安装"
subtitle: ""
date: '2025-05-02T06:50:00+08:00'
lastmod: '2026-01-27T11:30:00+08:00'
draft: false
categories: ['教程']
tags: ['VirtualBox','安装']
lightgallery: true
---



## 一、搭建后台运行的centos7服务器

[VirtualBox官网](https://www.virtualbox.org/wiki/Downloads) ；[centOS7镜像网站列表](https://www.centos.org/download/mirrors/) ；[清华大学镜像网站链接](https://mirrors.tuna.tsinghua.edu.cn/centos-vault/7.9.2009/isos/x86_64/)

首先安装VirtualBox虚拟机，最新版就可以。下载centos7镜像文件，首先打开网站列表，搜索框输入Asia寻找亚洲地区，然后找一个链接点进去，选择版本7，选择isos目录，无脑往下点，然后选择CentOS-7-x86_64-Minimal-2009.iso 镜像文件，下载即可（也可直接点击清华大学的链接，就能直接选对应iso文件下载了）

> minimal是最小体积的centos，无界面，想要有界面下载everything或者dvd都可以

**Warning:** VirtualBox7.0之后的版本安装可能会出现Invalid installation directory错误, 直接给出省流解决办法，管理员打开cmd控制台，一定是cmd不是powershell，然后输入以下命令：

```bash
icacls D:\VBox /reset /t /c
icacls D:\VBox /inheritance:d /t /c
icacls D:\VBox /grant *S-1-5-32-545:(OI)(CI)(RX)
icacls D:\VBox /deny *S-1-5-32-545:(DE,WD,AD,WEA,WA)
icacls D:\VBox /grant *S-1-5-11:(OI)(CI)(RX)
icacls D:\VBox /deny *S-1-5-11:(DE,WD,AD,WEA,WA)
```

D:\VBox是你的VirtualBox安装目录，最好是在盘符根目录下创建安装目录，否则你就要递归所有目录执行一遍上述命令！！！且在执行上述命令时会更改对应目录下的所有文件权限。例如：D:\app\VBox是你的安装目录，则需要执行：

```bash
icacls D:\app /reset /t /c
icacls D:\app\VBox /reset /t /c
```
这两条命令，你的安装目录层级有多深就要幂等执行多少条，所以建议一定要安装在最上层目录下！该解决方案来源于[官网](https://forums.virtualbox.org/viewtopic.php?t=112067)，为什么要这么做里面也有讨论和说明。

> 命令解释说明：
> - icacls: 用于显示或修改访问控制列表 (ACL)。
> - /reset: 用于将 ACL 重置为默认的权限。
> - /t: 递归处理所有子目录和文件。
> - /c: 继续执行命令，即使发生错误。
> - /inheritance:d: 移除继承的权限。
> - /grant: 授予访问权限。
> - *S-1-5-32-545: Users组的SID。
> - (OI)(CI)(RX): 对象继承、容器继承、读取和执行权限。
> - /deny: 拒绝访问权限。
> - *S-1-5-11: Authenticated Users组的SID。
> - (DE,WD,AD,WEA,WA): 删除、写入数据、添加数据、写入扩展属性、写入属性权限。

### 1、新建虚拟电脑

{{< image src="https://raw.githubusercontent.com/BluettDream/ImgBed01/master/learn/image-20230627105021674.png" alt="Network error" title="image" >}}

### 2、配置硬件

{{< image src="https://raw.githubusercontent.com/BluettDream/ImgBed01/master/learn/image-20230627105124138.png" alt="Network error" title="image" >}}

### 3、创建虚拟硬盘

{{< image src="https://raw.githubusercontent.com/BluettDream/ImgBed01/master/learn/image-20230627105155640.png" alt="Network error" title="image" >}}

### 4、点击下一步，点击完成，创建成功

{{< image src="https://raw.githubusercontent.com/BluettDream/ImgBed01/master/learn/image-20230627105235695.png" alt="Network error" title="image" >}}

### 5、配置服务器

> 对新建的服务器右键选择设置打开以下页面

#### 5.1、选择系统，勾选启动顺序中的网络，然后点击光驱，点击右边的向上箭头将光驱放到第一位

{{< image src="https://raw.githubusercontent.com/BluettDream/ImgBed01/master/learn/image-20230627111356990.png" alt="Network error" title="image" >}}

#### 5.2、选择网络，选择网卡2，点击启动网络连接，连接方式选择仅主机Host-only网络

> 这一步如果没有显示其余网卡，则可以先跳过，等安装完系统之后关闭虚拟机，然后再次打开设置，就会显示其余网卡了

{{< image src="https://raw.githubusercontent.com/BluettDream/ImgBed01/master/learn/image-20230627111507975.png" alt="Network error" title="image" >}}

> 注：网卡1也要已经启动网络连接，连接方式默认为网络地址转换NAT即可

#### 5.3、点击右下角确定即配置完毕，配置完成之后的主界面往下滑动如下图

{{< image src="https://raw.githubusercontent.com/BluettDream/ImgBed01/master/learn/image-20230627111819220.png" alt="Network error" title="image" >}}

> 注：马赛克是因为这个地方的教程是后续添加的，so ψ(._. )>

### 6、选中服务器，点击右上角启动，启动服务器(第一次需正常图形化界面启动)

{{< image src="https://raw.githubusercontent.com/BluettDream/ImgBed01/master/learn/image-20230627105855222.png" alt="Network error" title="image" >}}

> 选择第一个，敲击回车

### 7、等待安装的过程，直到出现选择语言界面，如下图

{{< image src="https://raw.githubusercontent.com/BluettDream/ImgBed01/master/learn/image-20230627110120876.png" alt="Network error" title="image" >}}

> 左边滑到最下面选择中文，右边选择简体中文，点击继续(可选，可以直接默认使用英文)

### 8、出现如下界面

{{< image src="https://raw.githubusercontent.com/BluettDream/ImgBed01/master/learn/image-20230627110325509.png" alt="Network error" title="image" >}}

### 9、配置键盘和语言支持(可选，增加英语键盘和语言)

{{< image src="https://raw.githubusercontent.com/BluettDream/ImgBed01/master/learn/image-20230627110625655.png" alt="Network error" title="image" >}}

### 10、配置系统安装位置

{{< image src="https://raw.githubusercontent.com/BluettDream/ImgBed01/master/learn/image-20230627110905248.png" alt="Network error" title="image" >}}

> 选择本地标准磁盘，点击完成即可（外面不再显示黄色警告，如果还出现再次点进来选中再点击完成）

### 11、配置网络

{{< image src="https://raw.githubusercontent.com/BluettDream/ImgBed01/master/learn/image-20230627111052091.png" alt="Network error" title="image" >}}

> 选择第一个网卡enp0s3，点击右边的关闭按钮变成打开，网络显示已连接即成功，点击完成

### 12、配置完成，点击开始安装

{{< image src="https://raw.githubusercontent.com/BluettDream/ImgBed01/master/learn/image-20230627111234695.png" alt="Network error" title="image" >}}

### 13、设置root密码(必须)，添加用户(可选)，等待安装

{{< image src="https://raw.githubusercontent.com/BluettDream/ImgBed01/master/learn/image-20230627112551175.png" alt="Network error" title="image" >}}

### 14、安装完成，点击重启

{{< image src="https://raw.githubusercontent.com/BluettDream/ImgBed01/master/learn/image-20230627113003973.png" alt="Network error" title="image" >}}

### 15、重启之后默认选择第一个进入即可，然后输入root和密码进行登录，如下图即成功

{{< image src="https://raw.githubusercontent.com/BluettDream/ImgBed01/master/learn/image-20230627113201065.png" alt="Network error" title="image" >}}

### 16、检查网卡配置和网络是否连接

{{< image src="https://raw.githubusercontent.com/BluettDream/ImgBed01/master/learn/image-20230627113402821.png" alt="Network error" title="image" >}}

> 输入ip addr查看网卡，然后输入ping www.baidu.com查看能够连接外网(按ctrl+c终止)

### 17、配置静态ip地址

#### 17.1、查看virtualboxip地址

##### 17.1.1、右键电脑右下角wifi标志，点击打开网络设置

{{< image src="https://raw.githubusercontent.com/BluettDream/ImgBed01/master/learn/image-20230627114723088.png" alt="Network error" title="image" >}}

##### 17.1.2、选择更改适配器选项

{{< image src="https://raw.githubusercontent.com/BluettDream/ImgBed01/master/learn/image-20230627114834652.png" alt="Network error" title="image" >}}

##### 17.1.3、找到含有VirtualBox Host-Only Ethernet标志的网络，右键点击属性

{{< image src="https://raw.githubusercontent.com/BluettDream/ImgBed01/master/learn/image-20230627114934438.png" alt="Network error" title="image" >}}

##### 17.1.4、找到ipv4，点击右下方属性

{{< image src="https://raw.githubusercontent.com/BluettDream/ImgBed01/master/learn/image-20230627115055535.png" alt="Network error" title="image" >}}

##### 17.1.5、查看ip地址

{{< image src="https://raw.githubusercontent.com/BluettDream/ImgBed01/master/learn/image-20230627115146818.png" alt="Network error" title="image" >}}

> 这里如果不是这样显示，配置成这样，然后点击确定

#### 17.2、进入服务器，编辑网络地址

##### 17.2.1、查看是否含有网卡配置文件(敲击tab快捷输入)

> ls /etc/sysconfig/network-scripts/

{{< image src="https://raw.githubusercontent.com/BluettDream/ImgBed01/master/learn/image-20230627115451891.png" alt="Network error" title="image" >}}

##### 17.2.2、编辑第二块网卡配置文件(即ifcfg-enp0s8，如果没有请先返回第5条配置服务器)

如果返回之后配置完毕使用ip addr命令能够显示两张网卡，但是net-word-scripts中没有ifcfg-enp0s8文件，那么就使用以下命令生成一个网卡配置文件：

```bash
nmcli con add con-name enp0s8 type ethernet ifname enp0s8
```

这个时候就会出现这个配置文件了，然后进行下列操作。

> vi /etc/sysconfig/network-scripts/ifcfg-enp0s8
>
> 编辑红框区域，如果没有就自己添加（vi编写文件，:wq保存退出）

{{< image src="https://raw.githubusercontent.com/BluettDream/ImgBed01/master/learn/image-20230627120450227.png" alt="Network error" title="image" >}}

##### 17.2.3、重启网络服务

> systemctl restart network

#### 17.3、输入ip addr查看配置是否成功

{{< image src="https://raw.githubusercontent.com/BluettDream/ImgBed01/master/learn/image-20230627120901486.png" alt="Network error" title="image" >}}

> 成功显示ip地址即为成功

### 18、配置xshell远程连接

{{< image src="https://raw.githubusercontent.com/BluettDream/ImgBed01/master/learn/image-20230627121042587.png" alt="Network error" title="image" >}}

{{< image src="https://raw.githubusercontent.com/BluettDream/ImgBed01/master/learn/image-20230627121219491.png" alt="Network error" title="image" >}}

> 点击确定后会弹框，选择接受并保存即可，然后如下图显示就表示远程连接成功

{{< image src="https://raw.githubusercontent.com/BluettDream/ImgBed01/master/learn/image-20230627121323169.png" alt="Network error" title="image" >}}

## 二、配置初始的centos7

### 1、更新yum

> yum update -y

如果命令执行失败，提示yum源错误，则需要下载[yum华为云镜像源](https://mirrors.huaweicloud.com/repository/conf/CentOS-7-anon.repo)，然后备份/etc/yum.repos.d下的所有文件：

```bash
cd /etc/yum.repos.d 
find . -type f exec cp {} {}.bak \;
```

将下载的文件上传到该目录下，原有的CentOS-Base.repo文件删除，上传的文件重命名为CentOS-Base.repo，执行以下命令：

```bash
yum clean all
yum makecache
yum repolist all
```

这时候就可以重新执行yum update -y命令了

### 2、安装vim

> yum install -y vim

### 3、解决xshell连接显示警告(The remote SSH server rejected X11 forwarding request)

#### 3.1、安装xorg-x11-xauth

> yum install -y xorg-x11-xauth

#### 3.2、防火墙开放22端口

> firewall-cmd --zone=public --add-port=22/tcp --permanent

#### 3.3、重新加载防火墙

> firewall-cmd --reload
>
> 这时候你断开xshell连接，然后重新连接就会发现警告消失了，但是会一个文件不存在的语句（/usr/bin/xauth:  file /root/.Xauthority does not exist）

#### 3.4、创建指定文件(解决/usr/bin/xauth:  file /root/.Xauthority does not exist)

> touch ~/.Xauthority

现在重新连接xshell就不会显示任何报错了

## 三、配置windows自启

win11任务计划脚本

```powershell
$VBoxManagePath = "E:\VBox\VBoxManage.exe"
$VMName = "remote1"
$TaskName = "StartVM-$VMName"
if (-not (Test-Path $VBoxManagePath)) {
    Write-Error "VirtualBox not found in: $($VBoxManagePath).please check path or update script!"
    exit 1
}
$ActionArgument = "startvm ""$VMName"" --type headless"
$TaskAction = New-ScheduledTaskAction -Execute $VBoxManagePath -Argument $ActionArgument -WorkingDirectory (Split-Path $VBoxManagePath)
$TaskTrigger = New-ScheduledTaskTrigger -AtLogon
$TaskSettings = New-ScheduledTaskSettingsSet -AllowStartIfOnBatteries:$false -DontStopIfGoingOnBatteries:$false
$TaskPrincipal = New-ScheduledTaskPrincipal -UserId "Blue" -RunLevel Highest
$ScheduledTask = New-ScheduledTask -Action $TaskAction -Trigger $TaskTrigger -Settings $TaskSettings -Principal $TaskPrincipal
try{
	Register-ScheduledTask -TaskName $TaskName -InputObject $ScheduledTask -Force
	Write-Host "create startup task '$TaskName' success!"
} catch {
	Write-Error "create or update task fail:$($_.Exception.Message)"
	exit 1
}
```

参数解释说明：
- $VBoxManagePath: VirtualBoxManager的路径，修改为你自己的路径
- $VMName: 虚拟机的名称，修改为你自己的虚拟机名称
- $TaskName: 任务名称，默认是StartVM-$VMName
- $TaskAction: 任务执行的操作，在目标工作目录启动虚拟机
- $TaskTrigger: 任务触发器，系统启动时触发
- $TaskSettings: 任务设置，默认是使用交流电才启动任务,如果使用电池则不启动
- $TaskPrincipal: 任务主体，UserId是当前登录的用户名(最好具有管理员权限)，RunLevel是最高权限

复制到txt文件中,重命名为startvm.ps1，放在你想要的目录下，然后以管理员身份运行powershell，执行此脚本:脚本所在目录\startvm.ps1直接回车,输出create startup task 'StartVM-remote1' success!即成功