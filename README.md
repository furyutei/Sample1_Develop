開発用ブランチと公開用ブランチとでリポジトリを分けるサンプル
============================================================

- こちらは【開発用】です  
- 開発用には「公開専用ファイル.txt」は含まれません。また、「環境依存ファイル.txt」は内容が異なっています。  

## 目的

これまで会社等で Private リポジトリで作業していたプロジェクト等を、公開用に別途 Public リポジトリを用意し、一部内容を変えて公開するようなケースを想定。  

| 種別  | ブランチ名 | 備考 |
| :--- | :--- | :--- | :--- |
| 開発用 | master | 従来の Private リポジトリをそのまま使用 |
| 公開用 | public | 新規に Public リポジトリ作成 |


## 作業内容

### 公開用 Public リポジトリを作成

公開用に Public リポジトリを作成後、

```sh
$ git remote add release <リポジトリURL>
```

のようにして、リポジトリを新たな名前で追加。  
※ここでは "release" とした。"origin" ではないことに注意！


### .gitattributes 作成

開発用ブランチ（ここでは master）に .gitattributes を追加し、公開しない／公開用とは異なるファイルを export-ignore で指定。  

```sh
$ vi .gitattributes
.gitignore export-ignore
.gitattributes export-ignore
README.md export-ignore
config/* export-ignore
src/開発専用ファイル.txt export-ignore
```


### 公開用ブランチの作成

```sh
$ git checkout --orphan public
```

のように、任意の空きブランチ（ここでは public）を作成し、不要なファイルを削除・公開用の修正などを行い、add → commit → push を実施。  

```sh
# - 必要に応じて不要なファイルを削除
# - 公開用にファイルを修正・追加
$ git add .
$ git commit -m "公開初版"
$ git push -u release public
```

※ここでは、public ブランチを releace リポジトリに push して公開している。  


## ライセンス

[![CC0](http://i.creativecommons.org/p/zero/1.0/88x31.png "CC0")](http://creativecommons.org/publicdomain/zero/1.0/deed.ja)
