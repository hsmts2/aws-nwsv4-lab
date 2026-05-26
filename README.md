# aws-nwsv4-lab

「基礎からのネットワーク&サーバー構築［改訂4版］」の学習内容を、AWS CloudFormation で再現するためのリポジトリです。

## 概要

このリポジトリでは、書籍で構築する AWS ネットワークおよびサーバー構成を CloudFormation テンプレートとして管理します。

## 作成する主なAWSリソース

- VPC
- パブリックサブネット
- プライベートサブネット
- Internet Gateway
- NAT Gateway
- ルートテーブル
- セキュリティグループ
- IAM ロール
- WEBサーバー用 EC2 インスタンス
- DBサーバー用 EC2 インスタンス

## ディレクトリ構成

```text
.
├── README.md
├── templates/
│   └── aws-nwsv4.yaml
├── parameters/
│   └── aws-nwsv4-lab.example.json
├── diagrams/
│   └── aws-nwsv4-architecture.png
└── docs/
    └── notes.md

