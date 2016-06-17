+++
date = "2014-04-19T21:46:45+09:00"
draft = false
title = "[PowerShell] 32bitと64bitの使い分け"
tags = ["PowerShell", "Oracle"]
+++

http://blog.livedoor.jp/haruka_sao/archives/52067968.html

PowerShellにはちゃんと 32bit版と 64bit版が備わっています

Windows2008 R2  
Windows2012 R2  
共に同じ場所にあります(~o~)

64bit版  
**C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe**

32bit版  
**C:\Windows\SysWOW64\WindowsPowerShell\v1.0\powershell.exe**

デフォルトの64bitマシンであれば、コンソールで

```
> powershell
```

とすると、64bitモードが立ち上がります

これはどういうことかというと、64bitのアプリから使うライブラリなどは、64bitでないとダメってことです


何にはまったかというと、PowerShell から System.Data.OracleClient に接続するとき

たとえDBサーバが 64bit Oracle だったとしても  
Oracle Client はデフォルトでは 32bit になっているようです

powershell から oracle に接続しようとしたときこんな感じになります

```
# アセンブリのロード
[void][System.Reflection.Assembly]::LoadWithPartialName("System.Data.OracleClient")

# 接続
$conn_str = "Data Source=saoDB;User ID=sao;Password=password;Integrated Security=false;"
$ora_conn = New-Object System.Data.OracleClient.OracleConnection($conn_str)

# インスタンス
$ora_cmd = New-Object System.Data.OracleClient.OracleCommand
$ora_cmd.Connection = $ora_conn

# データベースに接続
$ora_conn.Open()
```

（参考）powerShellからOracleを使う（接続・切断）  
[http://harikofu.blog.fc2.com/blog-entry-114.html](http://harikofu.blog.fc2.com/blog-entry-114.html)

このDB接続直後に、64bit のPowerShell だと  
こんなエラーが出ます

```
"0" 個の引数を指定して "Open" を呼び出し中に例外が発生しました: "Oracle クライアント ライブラリを読み込もうとしましたが、BadImageFormatException が発行されました。この問題は、32 ビットの Oracle クライアント コンポーネントが
インストールされている環境で 64 ビット モードを実行すると発生します。"
発生場所 行:1 文字:22
+ $ora_conn.Open <<<< ()
    + CategoryInfo          : NotSpecified: (:) []、MethodInvocationException
    + FullyQualifiedErrorId : DotNetMethodException
```

問題解決方法は2つ

- Oracle Client を 64bit版をインストールする（たとえDBサーバ上であっても…
- 呼び出す側のPowerShellを 32bit版にする

既存の稼働環境に影響を与えない様にするという意味では  
不毛ですが後者を選択すると波風立てずにコトを済ませられます
