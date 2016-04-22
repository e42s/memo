# Linuxのコマンド
## 空のディレクトリ削除
```sh
find . -type d -empty | xargs rmdir
```
これ再帰的じゃない。
