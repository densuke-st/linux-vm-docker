# これはなに?

これは某学校でLinuxの授業で使うLinux環境をDockerで再現するためのツールです。

* イメージを作れます(pushできるのは私だけですが)
* 起動に使うスクリプトコードが含まれています

配布ライセンスはGPL3といたします。
適宜書き換えて自分の環境を構築できると良いですね。

# 使い方

WindowsでもmacOSでも、Docker(Docker Desktop等)が必要です。
学校で使うので、Docker Desktopでライセンス的には問題ありませんが、企業で使う際にはライセンス料が発生することもあるので注意してください。

続いて、本リポジトリをクローンした後、以下の手順でセットアップをしてください。

## セットアップ

Linux/macOS/Windows共通です。

展開したディレクトリでプロンプトを開いてください(PowerShellでもbash/zshいずれでも大丈夫)。
その後、以下のコマンド2つを入力してください。

`>` はプロンプト文字列です。

```cmd
> docker volume create ubuntu-home
> docker compose create
```

```{note}
何をしているのかですが、

1.  ```
    > docker volume create ubuntu-home
    ```
    Linux環境で利用する個人用データの記憶域(ボリューム)を生成しています。
2.  ```
    > docker compose create
    ```
    授業用のLinux環境を構築しています。
```

これが完了すると、Docker Desktopのダッシュボードの "containers" タブにて、生成された環境が確認できます。

![](images/containers.png)



## 起動方法

起動方法は2つあります、ほとんどの方は前者(GUI)のほうが楽だと思うので、そちらで試してください。

### GUIベースの起動

Docker Desktopのダッシュボードを開き、該当するLinux環境右側にある起動 `▶` ボタンをクリックしてください。
2カ所ありますが、(上側が推奨ですが)どちらでもかまいません。
クリックすると `■` ボタンに切り替わります(停止用ボタン、めったに使いませんが)

![](images/gui-up.png)

![](images/gui-stop.png)

```{warning}
さらに右側にあるゴミ箱ボタンでコンテナの削除もできますが、環境リセット用途意外では使いません。うっかり触ってもキャンセルするようにしてください。
```

### CLI(コマンドライン)ベースでの起動

`docker-compose.yml` ファイルのあるディレクトリに移動して、

```cmd
> docker compose up -d
```

すると起動します(ダッシュボード側でも確認できる)。

## 接続方法

Linuxへの接続は以下の条件での接続となります。

- ユーザー名: `linux`
- パスワード: `penguin`
- 接続ポート: 2022
    - もしかすると22/tcpがホスト側のsshで使っている可能性があるため

### 端末ツールによる接続

* RLogin
* Putty

など、端末アプリケーションを用いることで接続できます、接続時のパラメータについては、上記データを参考に設定してみてください。
授業時に配布するRLoginでは、設定込みになっているため、実行するだけで接続先として選べるようにしてあります。

### `ssh`による接続

`ssh`コマンド(WindowsでもmacOSでも標準で入ってます)を使うのであれば、

```
# Windows/macOS共通
> ssh -p 2022 linux@127.0.0.1
```

で接続できます、初回接続時はホスト鍵の確認が出るので、`yes`を入れてください。

### おまけ: `ssh` コマンドで入りやすくする

`~/.ssh/config` でエントリを作成すると、`ssh` コマンドでの接続が楽になります。

```
Host ls #linuxstudy
        HostName 127.0.0.1
        Port 2022
        User linux
        StrictHostKeyChecking no
        UserKnownHostsFile /dev/null
```

これで接続名 `ls` でパスワードを入れるだけで入れます。

```zsh
% ssh ls
Warning: Permanently added '[localhost]:2022' (ED25519) to the list of known hosts.
linux@localhost's password: #ここでパスワード入力
Welcome to Ubuntu 22.04.2 LTS (GNU/Linux 5.15.49-linuxkit x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage
Last login: Tue Mar 28 20:16:58 2023 from 172.23.0.1
```

公開鍵の登録ができればもう一段落できますが、自分で調べましょう。
仕事で使うときは公開鍵ログインが基本となることでしょう…

# イメージの自動ビルドについて

本リポジトリのmainブランチが更新されると、テスト後にイメージ作成・更新フェイズが動くようになっています。

[![Docker Image CI](https://github.com/densuke-st/linux-vm-docker/actions/workflows/CI.yml/badge.svg?branch=main)](https://github.com/densuke-st/linux-vm-docker/actions/workflows/CI.yml)

# 変更履歴

* v3
    * Ubuntu 24.04ベース
