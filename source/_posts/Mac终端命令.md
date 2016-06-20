title: Mac终端命令
date: 2015-12-17 14:58:27
tags:
- OS
---
### 在Finer中打开当前目录
`open .`
### 打开关闭Macbook Air (Pro)内置键盘
Macbook的支架不大好带,在外面也想用HHKB的话,把键盘放在Air前面视线感觉很奇怪.还有的时候连桌子都没有,就只能把Air放到腿上用,这时如果还想用HHKB就只能把键盘放在Air内置键盘上面,但是这样就会误触内置键盘.这时就需要打开关闭Macbook Air (Pro)内置键盘的命令了.

#### 关闭键盘:

`sudo kextunload /System/Library/Extensions/AppleUSBTopCase.kext/Contents/PlugIns/AppleUSBTCKeyboard.kext/`
#### 打开键盘:
`sudo kextload /System/Library/Extensions/AppleUSBTopCase.kext/Contents/PlugIns/AppleUSBTCKeyboard.kext/`