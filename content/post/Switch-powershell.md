+++
date = "2025-11-16T01:16:00+09:00"
draft = false
title = "[PowerShell]  PowerShell7のインストールとコマンドラインからの切り替え"
tags = ["PowerShell"]
+++

Windows11では、デフォルトのPowerShellは `version 5` 系です

`version 7` 系を入れるときのインストール方法と、コマンドラインでの切り替え方法とを記載しておきます

前提環境：  
- Windows11

# PowerShell 7 のインストール

- Windowsのインストールガイド（公式）：  
[https://learn.microsoft.com/ja-jp/powershell/scripting/install/installing-powershell-on-windows?view=powershell-7.5](https://learn.microsoft.com/ja-jp/powershell/scripting/install/installing-powershell-on-windows?view=powershell-7.5)

上記に丁寧に記載されてるので私のブログに書かなくても良いのですけど  
ログ付きにしておくので、誰かの参考になればと思います  
わたしは `WinGet` で入れました

## 前提:デフォルトのPowerShellのバージョンを調べておく 

5系でした

```
PS C:\WINDOWS\System32> $PSVersionTable

Name                           Value
----                           -----
PSVersion                      5.1.26100.6899
PSEdition                      Desktop
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}
BuildVersion                   10.0.26100.6899
CLRVersion                     4.0.30319.42000
WSManStackVersion              3.0
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1
```
## 1. `WinGet` でインストールする

```
PS C:\WINDOWS\System32> winget search Microsoft.PowerShell
The `msstore` source requires that you view the following agreements before using.
Terms of Transaction: https://aka.ms/microsoft-store-terms-of-transaction
The source requires the current machine's 2-letter geographic region to be sent to the backend service to function properly (ex. "US").

Do you agree to all the source agreements terms?
[Y] Yes  [N] No: yes
Name               Id                           Version Source
---------------------------------------------------------------
PowerShell         Microsoft.PowerShell         7.5.4.0 winget
PowerShell Preview Microsoft.PowerShell.Preview 7.6.0.5 winget
PS C:\WINDOWS\System32> winget install --id Microsoft.PowerShell --source winget
Found PowerShell [Microsoft.PowerShell] Version 7.5.4.0
This application is licensed to you by its owner.
Microsoft is not responsible for, nor does it grant any licenses to, third-party packages.
Downloading https://github.com/PowerShell/PowerShell/releases/download/v7.5.4/PowerShell-7.5.4-win-x64.msi
  ██████████████████████████████   107 MB /  107 MB
Successfully verified installer hash
Starting package install...
Successfully installed
PS C:\WINDOWS\System32>
```

上記の続きで、同じプロンプトに `$PSVersionTable` tと書いても、まだ `v5` のままです  
`v7` のプロンプトに切り替えなければいけません  
つまり PowerShellは `v5` と `v7` が共存できます

## 2. PowerShell 7を起動

スタートメニューから `powershell` と入れると上部にはWindows11デフォルトのものが表示されます  
よく見ると `PowerShell 7` も選択できるようになっているので、そちらを選択します

<img src="/pic/Switch-powershell_00.png" style="border:solid 5px #e6e6e6"/> 

```
PowerShell 7.5.4
PS C:\Users\sao> $PSVersionTable

Name                           Value
----                           -----
PSVersion                      7.5.4
PSEdition                      Core
GitCommitId                    7.5.4
OS                             Microsoft Windows 10.0.26200
Platform                       Win32NT
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0…}
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1
WSManStackVersion              3.0

PS C:\Users\sao>
```

無事に `PowerShell 7` が利用できるようになりました！

# PowerShell 5/ 7/ cmd のコマンドラインでの切り替え

ずばり、コマンドはこれです: `cmd/powershell/pwsh`

- `cmd` (Command Prompt)
- `powershell`  (PowerShell 5)
- `pwsh`  (PowerShell 7)

## cmd: Command Prompt

```
PS C:\Users\sao> cmd
Microsoft Windows [Version 10.0.26200.6899]
(c) Microsoft Corporation. All rights reserved.

C:\Users\sao>
```

## powershell: PowerShell 5

念のため、`$PSVersionTable` でバージョン確認

```
PS C:\Users\sao> powershell
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

Install the latest PowerShell for new features and improvements! https://aka.ms/PSWindows

PS C:\Users\sao> $PSVersionTable

Name                           Value
----                           -----
PSVersion                      5.1.26100.6899
PSEdition                      Desktop
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}
BuildVersion                   10.0.26100.6899
CLRVersion                     4.0.30319.42000
WSManStackVersion              3.0
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1
```

## powershell: PowerShell 7

```
C:\Users\sao>pwsh
PowerShell 7.5.4
PS C:\Users\sao> $PSVersionTable

Name                           Value
----                           -----
PSVersion                      7.5.4
PSEdition                      Core
GitCommitId                    7.5.4
OS                             Microsoft Windows 10.0.26200
Platform                       Win32NT
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0…}
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1
WSManStackVersion              3.0

PS C:\Users\sao>
```

この `pwsh` ってコマンドをよく忘れてしまい、何度も調べてるので、自分のブログに書きました