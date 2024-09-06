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

## 構成

- グラフDBにAmazon Neptuneを使用しています
- データ登録はPython＋Gremlinを使用しています
- プログラムはJupyter Notebookで実行することを想定しています
- データの参照と可視化はTom Sawyer Graph Database Browserを使用しています

## Tom Sawyerの起動

Tom Sawyerは、Neptuneのエンドポイントに対して、Gremlinクエリを実行するためのツールです。
Tom Sawyer Graph Database BrowserはEC2でマーケットプレイスから起動することができます。
下記を参考にしてください。

https://aws.amazon.com/marketplace/pp/prodview-dhynqyslzrqr2

起動後の初回アクセス方法はUsageに記載があります。
>Ensure the security group rules for inbound traffic enable HTTP port 80. For convenience during launch process, you can create a new Security Group based on seller recommended settings. For more information:
>https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html 

>Once the instance is running, access via a web browser: http://{instance_url}/databasebrowser The instance_url is >determined by your instance host name or IP. On first login, your credentials are:
>User: admin Password: {instanceID}

## NeptuneのIAMロール認証

現時点ではOFFにしています。

## IaCのNeptuneの操作を許可するポリシーについての補足

下記のリンクを参考にしてください。
`Resource`の指定が独特です。
単純にNeptuneクラスターのARNを指定しても動作しません。
クラスターリソースIDを引用する必要があります。

https://docs.aws.amazon.com/ja_jp/neptune/latest/userguide/iam-data-resources.html

## Notebookで実行するPython3のプログラム

こちらの、「Python を使用して汎用 SageMaker ノートブックを Neptune に接続する」を参考にしています
https://docs.aws.amazon.com/ja_jp/neptune/latest/userguide/graph-notebooks.html

## Amazon Neptuneへのローカル端末からの接続

2024年9月時点ではAmazon NeptuneはVPC内に閉じた環境で動作するため、ローカル端末から直接接続することはできません。
Amazon RDSにあるパブリックアクセスの設定はAmazon Neptuneにはまだ無いようです。
したがって、ローカル端末からNeptuneに接続するためには、踏み台サーバーを経由する必要があります。

IaCで踏み台サーバーを作成しているので、下記コマンドをローカル端末で実行することで、Neptuneに接続できるようになります。

```shell
ssh -i /path/to/your-key.pem -L 8182:{Neptuneのエンドポイント}:8182 ec2-user@{踏み台サーバーのパブリックIP}
```

このコマンドを実行した状態で、ローカル端末から`localhost:8182`に接続することで、Neptuneに接続できるようになります。