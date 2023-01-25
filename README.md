# AiUtil

Ai Akaishi's Utils Library Datapack  
便利ライブラリデータパック

## 動作確認済みバージョン / Verified minecraft versions

- 1.19.4 (snapshot 23w03a)

## 一緒に入れてね / Dependencies

Anywhere Function(<https://github.com/Ai-Akaishi/AnywhereFunction>)  

## 使い方 / How To Use

1. [基本 / Basics](#基本--basics)
2. [関数の上書き / Override functions](#関数の上書き--override-functions)
3. [関数の追加 / Add functions](#関数の追加--add-functions)
4. [Advanced関数 / What are the Advanced Functions?](#advanced関数--what-are-the-advanced-functions)

### 基本 / Basics

1. util: in に入力を設定します。  
data modify storage util: in set value (INPUT/入力)
2. 関数を呼びます。  
function #util:xxx
3. 結果が取得できます。  
data get storage util: out

### 関数の上書き / Override functions

関数は全て#util:xxxの形式でfunctionタグになっています。  
そのためこのデータパックのファイルを直接書き換えなくても、他のデータパックから挙動を上書きすることができます。  
All functions are defined as function tags in the form of #util:xxx.  
Therefore, you can overwrite the behavior from other datapacks without directly rewriting the files in this datapack.  

```json
#util:splitの挙動を変えたい場合  
When you want to override the behavior of #util:split.  
  
util/tags/functions/split.json  
{  
    "replace": true,  
    "values": [  
        "your_datapack:split"  
    ]  
}
```

### 関数の追加 / Add functions

好きな関数をヘルプ(#util:help)に追加表示できます。  
You can add a new line of any function you like to the help(#util:help).  
  
```json
#util:your_functionのヘルプを追加したい場合  
If you want to add help for #util:your_function  

util/tags/functions/help/functions.json  
{  
    "replace": false,  
    "values": [  
        "your_datapack:help/your_function"  
    ]  
}
```

```nim
your_datapack/functions/help/your_function.mcfunction  
tellraw @s {"text":"function #util:your_function","underlined": true,"clickEvent": {"action": "run_command","value": "/function your_datapack:your_function/help"}}
```

### Advanced関数 / What are the Advanced Functions?

ブロックを使用した高度な関数です。  
ブロックが使われる実行地点が安全であることを保証するため、最初は無効化されています。  
使用したい場合は次の手順で有効化してください。  
This is an Advanced Function using some Block.  
To ensure that the execution point where the block will be placed is safe, this feature is initially disabled.  
If you wish to use it, please follow the next steps to activate it.

1. ライブラリが自由に使用しても安全な場所を設定してください。(読み込まれてない場合は自動的にforceloadされます。)  
(次の設定は初期値です。ワールドの状況に応じて好きな場所を設定してください。)  
set up a location where it is safe for the library to use freely. (If it is not loaded, it will be forceloaded automatically.)  
(The following setting is the default value. Please set your favorite location to suit your world situation.)  
```nim
data modify storage util: settings.at {Pos:[0d,-64d,0d],Rotation:[0f,0f],Dimension:"minecraft:overworld"}
```
2. 使われることのないプレイヤーヘッドを設定します。  
(次の設定は初期値です。状況に応じて好きなプレイヤーヘッドを設定してください。)  
Sets the player_head that will never be used.  
(The following setting is the default value. Set any player_head you like to suit the situation.)  
```nim
data modify storage util: settings.unused_player_head set value "MHF_CoconutG"
```
3. Avanced機能を有効化します。  
Enable the Avanced Function.  
```nim
function #util:advanced/on
```
4. Advanced機能を無効化したい場合は次を実行します。  
If you want to disable the Advanced Function, run the following command.  
```nim
function #util:advanced/off
```

## 現在標準で対応している関数一覧 / Default Supported Functions

★:Advanced関数です。Advanced機能が有効化されていないと正常に動きません。  
★:Advanced Function; will not work properly unless the Advanced Functions are enabled.

1. [init(function tag)★](#init)
2. [datetime★](#datetime)
3. [split](#split)

### init

minecraft:loadファンクションタグはワールド読み込み時以外にもreloadなどで実行されますが、  
util:initファンクションタグはワールド読み込み時にのみ実行されます。  
好きなファンクションを登録して使っていいよ。  
The minecraft:load function tag is executed on world loading, reload, etc...,  
but the util:init function tag is only executed on world loading.  
You can register any functions you like and use it.

```json
util/tags/functions/init.json  
{  
    "replace": false,  
    "values": [  
        "your_datapack:your_function"  
    ]  
}
```

### split

文字を分割します。 / Split a string to chars.  
入力(util: in) : 文字列(string)  
出力(util: out): 文字リスト([string,...])

```nim
data modify storage util: in set value "赤石愛のほうそうきょく"
function #util:split
data get storage util: out
-> ["赤", "石", "愛", "の", "ほ", "う", "そ", "う", "き", "ょ", "く"]
```

### datetime

現在の日時を取得します。 / Get the current date and time.  
シングルモード(LAN非公開の場合)では時刻がずれる可能性があります。 / In single mode (when LAN is not open to the public), the time may be shifted.  
入力(util: in) : なし(nothing)  
出力(util: out): 日時データ({year:int,month:int,day:int,hour:int,minute:int,second:int,weekday:int})  
weekday: 0=月曜/Monday, 1=火曜/Tuesday, ... 6:日曜/Sunday

```nim
function #util:datetime
data get storage util: out
-> {year: 2023, month: 1, day: 23, hour: 23, minute: 39, second: 35, weekday: 1}
```

## 連絡はこちら / Contact

<https://twitter.com/AiAkaishi>

## ライセンス / LICENSE

These codes are released under the MIT License, see LICENSE.
