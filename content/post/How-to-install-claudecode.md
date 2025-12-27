+++
date = "2025-12-27T23:30:00+09:00"
draft = false
title = "[Claude] Windows への Claude Code インストール方法"
tags = ["claudecode"]
+++

WindowsにClaude Codeをインストールしようとして、公式の `irm` の方法（PowerShell版）では上手くインストールが出来なかったので、その解決策です

環境:  
- Windows11
- PowerShell 7

## 現象

公式には、こんな記述があります

[https://code.claude.com/docs/ja/setup](https://code.claude.com/docs/ja/setup)

```
PS > irm https://claude.ai/install.ps1 | iex
```

けどね、これを実行してもインストールされないのです（2025/12/27現在）

インストールログはこれ

```
PS C:\my\dev> irm https://claude.ai/install.ps1 | iex
Setting up Claude Code...
√ Claude Code successfully installed!
  Version: 2.0.67
  Location: C:\Users\Sao\.local\bin\claude.exe
  Next: Run claude --help to get started
‼ Setup notes:
  • installMethod is native, but claude command not found at C:\Users\Sao\.local\bin\claude.exe
  • Native installation exists but C:\Users\Sao\.local\bin is not in your PATH. Add it by opening: System Properties →
   Environment Variables → Edit User PATH → New → Add the path above. Then restart your terminal.
✅ Installation complete!
PS C:\my\dev> 
```

`but claude command not found at C:\Users\Sao\.local\bin\claude.exe` とあって、確かにこの場所に実行ファイルが無いのです  
なのに最後、`✅ Installation complete!` と表示される矛盾よ…

別のWindowsのPC2台で試しましたが、2台とも同じ状況でして、ダメでした  

Everything ツールを使って、PC内をくまなく探しましたが、どこにも `claude.exe` が無いようです  
なんでやねん…

> 参考:  
WindowsPCのローカルファイルを高速検索してくれる神ツール - https://www.voidtools.com/

## 解決策


こちらの方の記事を参考にして、wingetでインストールすることにしました！

- Claude Codeのネイティブ版をWindows 11にインストールする  
[https://qiita.com/charon/items/d51dc7c74339a795e9ee](https://qiita.com/charon/items/d51dc7c74339a795e9ee)


```
PS > winget install Anthropic.ClaudeCode
```

これでインストールすると、PowerShell再起動後に Claude Code がインストールされました！うれしい！！！

参考までにインストールログです

```
PowerShell 7.5.4
PS C:\Users\sao> winget install Anthropic.ClaudeCode
The `msstore` source requires that you view the following agreements before using.
Terms of Transaction: https://aka.ms/microsoft-store-terms-of-transaction
The source requires the current machine's 2-letter geographic region to be sent to the backend service to function properly (ex. "US").

Do you agree to all the source agreements terms?
[Y] Yes  [N] No: y
Found Claude Code [Anthropic.ClaudeCode] Version 2.0.65
This application is licensed to you by its owner.
Microsoft is not responsible for, nor does it grant any licenses to, third-party packages.
Downloading https://storage.googleapis.com/claude-code-dist-86c565f3-f756-42ad-8dfa-d59b1c096819/claude-code-releases/2.0.65/win32-x64/claude.exe
  ██████████████████████████████   205 MB /  205 MB
Successfully verified installer hash
Starting package install...
Path environment variable modified; restart your shell to use the new value.
Command line alias added: "claude"
Successfully installed
PS C:\Users\sao>
```

動作確認は、とりあえずヘルプ表示でできます

```
PS > claude --help
```

あーよかったよかった

ちょっと前は、公式の irm 方式でも行けたのですが  
なんかどっかが変わったのでしょうかね