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
- 「markdown文書」(http://kojika17.com/2013/01/starting-markdown.html)

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
準備中

<hr id="serversc" />

## サーバースクリプト
準備中

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
