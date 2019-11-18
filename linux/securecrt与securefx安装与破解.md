 

本地电脑之前安装的是win10,疲于win10频繁的更新和各种兼容问题，果断放弃win10系统，安装了Ubuntu 16.04系统，现在微信、QQ、钉钉、WPS等都已支持linux版本，所以在Ubuntu下进行日常运维操作完全不是问题。虽然在Ubuntu的terminel终端里可以进行ssh远程连接，但由于之前习惯了secureCRT工具，所以还是决定在Ubuntu下安装secureCRT，下面是操作记录：

1）安装secureCRT和secureFX
官网下载地址：https://www.vandyke.com/download/index.html 
直接到官网上下载就可以，下载过程可能会提示你要先注册，没关系，直接注册，然后再下载。 
我直接下载的是secureCRT+secureFX的合体包：scrt-sfx-8.3.4-1699.ubuntu16-64.x86_64.deb
放到百度云盘里的地址：https://pan.baidu.com/s/1EwVjFlkDdbmKPfQXDgGKEQ
提取密码：mw4c

2）直接进行安装
sudo dpkg -i scrt-sfx-8.3.4-1699.ubuntu16-64.x86_64.deb
安装后，到ubuntu的应用搜索里就能找到secureCRT和secureFX工具了。
安装完成之后，必须保证先安装了openSSH 并启动了ssh服务，才能进行ssh连接上的。 
sudo apt-get install openssh-server

```
graph LR
A-->B
```

```
graph LR
A-->B
```

ps -e |grep ssh

3）secureCRT和secureFX的License破解
3.1) 首先要运行破解secureCRT脚本
下载地址：https://pan.baidu.com/s/1MHsNtCEzTlK8jWXV3nsW4g
提取密码：5tu1

或者直接命令行下载：

    wget http://download.boll.me/securecrt_linux_crack.pl

下载完成后运行破解程序：

    sudo perl securecrt_linux_crack.pl /usr/bin/SecureCRT
    
上面脚本执行后, 就会自动生成license信息，根据生成的信息直接填写secureCRT的License信息了

```
Name: ygeR
Company: TEAM ZWT
Serial Number:03-36-338639
License Key: ADJE19 7U19YF 46RJWC 3CGK73 ADF3GN S66TJJ YU7BJP 6WJF1G
Issue Date: 03-10-2017
```

3.2) 接着运行破解secureFX脚本
下载地址：https://pan.baidu.com/s/1IEkUSimUcz6mzZ1dBdruVw
提取密码：xspg

    sudo perl securefx_linux_crack.pl /usr/bin/SecureFX
同样,上面脚本执行后, 也是按照下面的信息去填写secureFX的License信息了
```
Name: ygeR
Company: TEAM ZWT
Serial Number:06-70-001589
License Key: ACUYJV Q1V2QU 1YWRCN NBYCYK ABU767 D4PQHA S1C4NQ GVZDQF
Issue Date: 03-10-2017
```

破解之后，打开secureCRT和secureFX就可以正常使用了。




或者自己编写破解脚本：

    vi /opt/securecrt_linux_crack.pl

```    
use strict;
use warnings;
use File::Copy qw(move);




sub license {
	print "\n".
	"License:\n\n".
	"\tName:\t\txiaobo_l\n".
	"\tCompany:\twww.boll.me\n".
	"\tSerial Number:\t03-91-324785\n".
	"\tLicense Key:\tAC33SN 4JHKFS 48KYUT MY8F24 AAKC1C HJYFXT 8P6S99 MRAUQ2\n".
	"\tIssue Date:\t02-12-2019\n\n\n";
}

sub usage {
    print "\n".
	"help:\n\n".
	"\tperl securecrt_linux_crack.pl <file>\n\n\n".
	"\tperl securecrt_linux_crack.pl /usr/bin/SecureFX\n\n\n".
    "\n";
	
	&license;

    exit;
}
&usage() if ! defined $ARGV[0] ;


my $file = $ARGV[0];

open FP, $file or die "can not open file $!";
binmode FP;

open TMPFP, '>', '/tmp/.securefx.tmp' or die "can not open file $!";

my $buffer;
my $unpack_data;
my $crack = 0;

while(read(FP, $buffer, 1024)) {
	$unpack_data = unpack('H*', $buffer);
	if ($unpack_data =~ m/785782391ad0b9169f17415dd35f002790175204e3aa65ea10cff20818/) {
		$crack = 1;
		last;
	}
	if ($unpack_data =~ s/6e533e406a45f0b6372f3ea10717000c7120127cd915cef8ed1a3f2c5b/785782391ad0b9169f17415dd35f002790175204e3aa65ea10cff20818/ ){
		$buffer = pack('H*', $unpack_data);
		$crack = 2;
	}
	syswrite(TMPFP, $buffer, length($buffer));
}

close(FP);
close(TMPFP);

if ($crack == 1) {
		unlink '/tmp/.securefx.tmp' or die "can not delete files $!";
		print "It has been cracked\n";
		&license;
		exit 1;
} elsif ($crack == 2) {
		move '/tmp/.securefx.tmp', $file or die 'Insufficient privileges, please switch the root account.';
		chmod 0755, $file or die 'Insufficient privileges, please switch the root account.';
		print "crack successful\n";
		&license;
} else {
	die 'error';
}
```


    vi /opt/securefx_linux_crack.pl
    
```
use strict;
use warnings;
use File::Copy qw(move);




sub license {
	print "\n".
	"License:\n\n".
	"\tName:\t\tygeR\n".
	"\tCompany:\tTEAM ZWT\n".
	"\tSerial Number:\t06-70-001589\n".
	"\tLicense Key:\tACUYJV Q1V2QU 1YWRCN NBYCYK ABU767 D4PQHA S1C4NQ GVZDQF\n".
	"\tIssue Date:\t03-10-2017\n\n\n";
}

sub usage {
    print "\n".
	"help:\n\n".
	"\tperl securefx_linux_crack.pl <file>\n\n\n".
	"\tperl securefx_linux_crack.pl /usr/local/bin/SecureFX\n\n\n".
    "\n";
	
	&license;

    exit;
}
&usage() if ! defined $ARGV[0] ;


my $file = $ARGV[0];

open FP, $file or die "can not open file $!";
binmode FP;

open TMPFP, '>', '/tmp/.securefx.tmp' or die "can not open file $!";

my $buffer;
my $unpack_data;
my $crack = 0;

while(read(FP, $buffer, 1024)) {
	$unpack_data = unpack('H*', $buffer);
	if ($unpack_data =~ m/e02954a71cca592c855c91ecd4170001d6c606d38319cbb0deabebb05126/) {
		$crack = 1;
		last;
	}
	if ($unpack_data =~ s/c847abca184a6c5dfa47dc8efcd700019dc9df3743c640f50be307334fea/e02954a71cca592c855c91ecd4170001d6c606d38319cbb0deabebb05126/ ){
		$buffer = pack('H*', $unpack_data);
		$crack = 2;
	}
	syswrite(TMPFP, $buffer, length($buffer));
}

close(FP);
close(TMPFP);

if ($crack == 1) {
		unlink '/tmp/.securefx.tmp' or die "can not delete files $!";
		print "It has been cracked\n";
		&license;
		exit 1;
} elsif ($crack == 2) {
		move '/tmp/.securefx.tmp', $file or die 'Insufficient privileges, please switch the root account.';
		chmod 0755, $file or die 'Insufficient privileges, please switch the root account.';
		print "crack successful\n";
		&license;
} else {
	die 'error';
}
```


然后同样的方式执行破解脚本，然后讲信息填入软件，完成破解。