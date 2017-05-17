# Script

# 目次

- [はじめに](#first)
- [プリンター関係](#printscript)
- [ネットワークスクリプト](#netsc)
- [サーバースクリプト](#serversc)
- [クライアントスクリプト](#rename)
- [ATOMでC言語を使い開発する環境構築](ATOM)
- [Github文書の書き方](Gitwrite)
<hr id="first" />

## はじめに

1. Githubには書き方がある**Markdown**と調べればいくらでも出てくる。[Markdownでいこう](https://gist.github.com/wate/7072365)
- [Markdown文書](http://kojika17.com/2013/01/starting-markdown.html)

2. スクリプトには書き方がある、私はクックパッド社_CoffeeScrip_のスタイルガイドを基準として書くようにしている。


<hr id="printscript" />
## プリンター関係
準備中

```rb
num = 0
while num < 2 do
   print("num = ", num)
end
print("End")
```

<hr id="netsc" />

## ネットワークスクリプト

netshコマンドでTCP/IPパラメータを設定
PSを開いて以下のコマンドを打つ


1. C:\>netsh  ……netshコマンドの起動
2. netsh>interface  ……interfaceコンテキストへ移動
3. netsh interface>ip  ……ip（ipv4）コンテキストへ移動
4. netsh interface ipv4>  ……Windows Vista／Server 2008以降ではip→ipv4となっている
5. netsh interface ipv4>?  ...コマンド一覧表示 参考までに
6. netsh interface ipv4>set address "name＝お名前"static 192.###.###.### 255.##.###.###

以下結果出力
```rb
PS C:\Windows\system32> cd ../
PS C:\Windows> cd ../
PS C:\> netsh
netsh interface>ip
netsh interface ipv4>set address "ローカルエリア接続"static 192.168.1.100 255.255.255.0
```

<hr id="serversc" />

## サーバースクリプト

クライアントのWindowsUpdateの向き先を変えるバッチ
下記のHKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Nodeは64bitのレジストリキーであるため32bitは別のレジストリキーなので注意！
クライアント端末側で
HKEY_LOCAL_MACHINEのSOFTWARE\Microsoft\Windows\CurrentVersion\WindowsUpdate
キーにある以下の3つの値を消す必要がある場合がある。　なぜ必要かはGOOGLE先生へ
・AccountDomainSid
・PingID
・SusClientId

```rb
@echo off
rem WSUSサーバのURL
set THE_WSUS=http://192.168.???.???:8530
rem 自動更新オプション　2:DL前通知、3:自動DL＆インストール前通知、4:自動DL＆スケジュールインストール
set AUOptions=4
rem 自動インストールする時間（24時間）
set ScheduledInstallTime=17
rem 自動インストールする曜日　0:毎日、1:日曜、2:月曜、……、7:土曜
set ScheduledInstallDay=3

set /P STR_INPUT="OSに合わせて1～4の値を入力してください。1.Windows7  2.Windows8  3.Windows8.1  4.Windows10 :"

if "%STR_INPUT%" equ "1" (
set ComGroup=win7
) else if "%STR_INPUT%" equ "2" (
set ComGroup=win8
) else if "%STR_INPUT%" equ "3" (
set ComGroup=win8.1
) else if "%STR_INPUT%" equ "4" (
set ComGroup=win10
)


cls
echo WSUSサーバを使うよう設定します。
echo  [ %THE_WSUS% ]
pause
reg add "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Internet Settings" /f /v ProxyOverride /t reg_sz /d "192.168.???.???;<local>"

reg.exe delete "HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Policies\Microsoft\Windows\WindowsUpdate" /f

reg.exe add "HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Policies\Microsoft\Windows\WindowsUpdate\AU" /v NoAutoUpdate /t REG_DWORD /d 0 /f
reg.exe add "HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Policies\Microsoft\Windows\WindowsUpdate\AU" /v UseWUServer /t REG_DWORD /d 1 /f

reg.exe add "HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Policies\Microsoft\Windows\WindowsUpdate\AU" /v AUOptions /t REG_DWORD /d "%AUOptions%" /f
reg.exe add "HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Policies\Microsoft\Windows\WindowsUpdate\AU" /v NoAutoRebootWithLoggedOnUsers /t REG_DWORD /d 1 /f


reg.exe add "HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Policies\Microsoft\Windows\WindowsUpdate\AU" /v ScheduledInstallDay /t REG_DWORD /d "%ScheduledInstallDay%" /f
reg.exe add "HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Policies\Microsoft\Windows\WindowsUpdate\AU" /v ScheduledInstallTime /t REG_DWORD /d "%ScheduledInstallTime%" /f

reg.exe add "HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Policies\Microsoft\Windows\WindowsUpdate" /v DoNotConnectToWindowsUpdateInternetLocations /t REG_DWORD /d 1 /f
reg.exe add "HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Policies\Microsoft\Windows\WindowsUpdate" /v TargetGroup /t REG_SZ /d "%ComGroup%" /f
reg.exe add "HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Policies\Microsoft\Windows\WindowsUpdate" /v TargetGroupEnabled /t REG_DWORD /d 1 /f
reg.exe add "HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Policies\Microsoft\Windows\WindowsUpdate" /v WUServer /t REG_SZ /d "%THE_WSUS%" /f
reg.exe add "HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Policies\Microsoft\Windows\WindowsUpdate" /v WUStatusServer /t REG_SZ /d "%THE_WSUS%" /f


net stop bits
net stop wuauserv
net start wuauserv
net start bits
wuapp.exe
echo.
echo 設定が完了しました。
pause


```

<hr id="rename" />

## クライアントスクリプト

+ DHCPに設定すバッチ。
****.bat　←形式で保存すること。

```rb
@echo off
pause

set IFNAME="ローカル エリア接続"

netsh interface ipv4 set address name=%IFNAME% dhcp
netsh interface ipv4 set dnsservers name=%IFNAME% dhcp

ipconfig /all

pause
```
============

+  固定IP設定するバッチ。

```rb
@echo off
pause

set IFNAME="ローカル エリア接続"
set IPADDR=xxx.xxx.xxx.xxx
set MASK=xxx.xxx.xxx.xxx
set GW=xxx.xxx.xxx.xxx
set DNS1=xxx.xxx.xxx.xxx
set DNS2=xxx.xxx.xxx.xxx

netsh interface ipv4 set address name=%IFNAME% static %IPADDR% %MASK% %GW% 1
netsh interface ipv4 set dnsservers name=%IFNAME% static %DNS1% primary validate=no
netsh interface ipv4 add dnsservers name=%IFNAME% %DNS2% index=2 validate=no

ipconfig /all

pause
```
## ATOMで開発
ATOMというエディタは様々な言語をサポートしている。  
プラグインを導入することにより開発補助をしてくれる。  
以下に導入環境を記す。　　　　　　　　　　　　　　　　　　　　　
 


[ATOMでC言語を使い開発する環境構築](ATOM)
