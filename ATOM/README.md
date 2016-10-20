
# 目次

- [はじめに](#first)
- [コンパイルエラー対処法](#1)
- [実行結果画面がすぐ閉じる現象](#2)

<hr id="first" />
## はじめに

Atomとは拡張機能が豊富なエディターソフトです。　
あらゆる言語が使用可能でプログラムを製作するうえで便利な機能が豊富。




<hr id="1" />
## 下記のコンパイルエラー対処法
*Pthread*関係で次のファイルをダウンロードする．
libpthread-2.8.0-3-mingw32-dll-2.tar.lzma
pthreads-w32-2.8.0-3-mingw32-dev.tar.lzma
lzmaで圧縮されているので，7zipで展開する。

```
libpthread-2.8.0-3-mingw32-dll-2.tar.lzma
> bin > pthreadGC2.dll pthreadGCE2.dll
.dillファイルを
*MinGW\bin\*
に移動もしくわコピペ。

pthreads-w32-2.8.0-3-mingw32-dev
include > pthread.h sched.h semaphore.h
.hファイルを
*MinGW\include\*
に移動。

同じ要領で
libの展開ファイル
 .aファイルも　
*MinGW\lib\*
に移動。
```
コマンドプロンプでcdを使いCファイルのフォルダまで移動する。
以下のコマンドを入力する。

　例としてtest.cというCファイルがあると定義する。
>gcc -o test test.c
同フォルダにtest.exeが生成される。

<hr id="2" />
## 実行結果画面がすぐ閉じる現象

#include <stdlib.h>を読み込ませる
処理の終わりにsystem("pause");入れてあげる
