# aws-nwsv4-lab

「基礎からのネットワーク&サーバー構築［改訂4版］」の学習内容を、AWS CloudFormation で再現するためのリポジトリです。

本リポジトリでは、書籍で構築する基本的な AWS ネットワークおよびサーバー構成を、CloudFormation テンプレートとして管理します。

## 概要

このテンプレートでは、以下の AWS リソースを作成します。

- VPC
- パブリックサブネット
- プライベートサブネット
- Internet Gateway
- NAT Gateway
- ルートテーブル
- セキュリティグループ
- IAM ロール / インスタンスプロファイル
- WEBサーバー用 EC2 インスタンス
- DBサーバー用 EC2 インスタンス

## 構成図

構成図は以下に配置します。

```
diagrams/aws-nwsv4-architecture.png

ディレクトリ構成
.
├── README.md
├── templates/
│   └── aws-nwsv4.yaml
├── parameters/
│   └── aws-nwsv4-lab.json
├── diagrams/
│   └── aws-nwsv4-architecture.png
└── docs/
    └── notes.md
CloudFormation テンプレート

CloudFormation テンプレートは以下に格納しています。

templates/aws-nwsv4.yaml
作成される主な構成
ネットワーク
項目	設定値
VPC CIDR	10.0.0.0/16
パブリックサブネット	10.0.1.0/24
プライベートサブネット	10.0.2.0/24
Internet Gateway	あり
NAT Gateway	あり
EC2 インスタンス
サーバー	配置先	プライベートIP	用途
WEBサーバー	パブリックサブネット	10.0.1.10	Apache / PHP / WordPress
DBサーバー	プライベートサブネット	10.0.2.10	MariaDB
セキュリティグループ
セキュリティグループ	用途
WEB-SG	WEBサーバー用
DB-SG	DBサーバー用
EIC-SG	EC2 Instance Connect Endpoint 検証用
デプロイ方法
1. パラメータファイルを編集する

以下のファイルを作成または編集します。

parameters/aws-nwsv4-lab.json

例：

[
  {
    "ParameterKey": "KeyName",
    "ParameterValue": "my-key"
  },
  {
    "ParameterKey": "MyIpCidr",
    "ParameterValue": "xxx.xxx.xxx.xxx/32"
  }
]

MyIpCidr には、自分のグローバルIPアドレスを /32 で指定します。

2. CloudFormation スタックを作成する
aws cloudformation create-stack \
  --stack-name aws-nwsv4-lab \
  --template-body file://templates/aws-nwsv4.yaml \
  --parameters file://parameters/aws-nwsv4-lab.json \
  --capabilities CAPABILITY_NAMED_IAM
3. スタック作成状況を確認する
aws cloudformation describe-stacks \
  --stack-name aws-nwsv4-lab
4. スタックを削除する

検証後は、不要な課金を避けるためスタックを削除します。

aws cloudformation delete-stack \
  --stack-name aws-nwsv4-lab
注意事項

このテンプレートでは NAT Gateway と Elastic IP を作成します。
NAT Gateway は作成後、利用時間および処理データ量に応じて料金が発生します。

検証が完了したら、CloudFormation スタックを削除してください。

セキュリティ上の注意
秘密鍵ファイル .pem はリポジトリにコミットしないでください。
DB パスワードなどの機密情報は README や YAML に直接記載しないでください。
パブリックリポジトリで公開する場合は、AWS アカウントID、固定IP、アクセスキー、秘密鍵などが含まれていないことを確認してください。
