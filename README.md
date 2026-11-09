# ame-keyring

鍵束(org.freedesktop.secrets)。アプリが API キー・パスワードをしまう Secret Service(gnome-keyring の代わり)。

[ame-shell](https://github.com/TatsuyaM2667/ame-shell)(Wayland のデスクトップシェル)の一部を、単独で使えるように切り出したものです。
ame-shell は画面の要らない仕事を、1 つのことをして標準入出力でやりとりする小さなコマンドに分けています。これはその 1 つです。

## 使い方

```text
ame-keyring: 鍵束(org.freedesktop.secrets、freedesktop の Secret Service)。gnome-keyring の代わり。

  ame-keyring            セッションの D-Bus で org.freedesktop.secrets を持ち、止めるまで動く
                         (ほかのものが持っていれば何もせずに終わる)
  ame-keyring list       しまってあるものの名前と属性(秘密そのものは出さない)

Zed などのアプリが API キー・パスワードをここにしまう(libsecret / oo7 が使う D-Bus の口)。
中身は $XDG_DATA_HOME/ame-shell/keyring/login.keyring に XChaCha20-Poly1305 で暗号化して置く(src/lib/secretstore.zig)。
鍵は同じ場所の master.key(0600、初回に作る)。ファイルを持ち出されたり、ほかの人に読まれたりするのは防ぐが、
このユーザーとして動くプログラムからは読める(ログインのパスワードで解錠する鍵束と違い、いつも開いている)。
やりとりは "plain"(暗号化しない受け渡し。同じユーザーのセッションの D-Bus の中だけ)。鍵束は 1 つ(login、別名 default)。
```

## ビルド

```sh
zig build -Doptimize=ReleaseSafe   # zig-out/bin/ame-keyring(Zig 0.13、libc と libdbus)
zig build test
```

## このリポジトリについて

正本は ame-shell の `src/cmd/ame-keyring.zig` と `src/lib/` です。このリポジトリは `scripts/split-repos.sh` が生成して同期しています。
直したいところがあれば ame-shell の方へお願いします。

ライセンス: GPL-3.0(ame-shell と同じ)
