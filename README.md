# Neptuneで初めてのグラフDBを作成してみる

## ディレクトリ構成

```
.
├── README.md
├── cloudformation/
│   ├── graphdb_1_vpc.yml # VPCを作成するテンプレート
│   └── graphdb_2_neptune.yml # NeptuneとNotebookを作成するテンプレート
└── neptune_conda_python3.yml # Notebookで動かすPython3のプログラム
```

## ポイント

### NeptuneのIAMロール認証

現時点ではOFF

### Neptuneの操作を許可するポリシー

下記のリンクを参考にすること
`Resource`の指定が独特で、単純にNeptuneクラスターのARNを指定しても動作しない
クラスターリソースIDを指定する必要がある

https://docs.aws.amazon.com/ja_jp/neptune/latest/userguide/iam-data-resources.html

### Notebookで実行するPython3のプログラム

こちらの、「Python を使用して汎用 SageMaker ノートブックを Neptune に接続する」を参考にしています
https://docs.aws.amazon.com/ja_jp/neptune/latest/userguide/graph-notebooks.html
