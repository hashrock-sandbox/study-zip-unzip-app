# 何がしたいのか

zipファイルの読み書きができるwebアプリを作りたい

# 問題点

- iOSでlocal file system APIが生える見込みがない
  - しかもiPadが結構メイン用途だったりはする…
  - いや、UXは悪いものの、できるぞこれ

# アイデア

- Native File Systemが使えれば使う
- 使えなければFile Load / Saveを行えるようなUIを表示
- なおかつ、indexedDBに最近使ったファイルをためておけばいい？

