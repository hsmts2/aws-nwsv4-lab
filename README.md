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
```

## CloudFormation テンプレート
CloudFormation テンプレート: [templates/aws-nwsv4.yaml](templates/aws-nwsv4.yaml)


## 構成図
diagrams/aws-nwsv4-architecture.png


## デプロイ例
```shell
aws cloudformation create-stack \
  --stack-name aws-nwsv4-lab \
  --template-body file://templates/aws-nwsv4.yaml \
  --parameters file://parameters/aws-nwsv4-lab.example.json \
  --capabilities CAPABILITY_NAMED_IAM
```


## 削除例
```shell
aws cloudformation delete-stack \
  --stack-name aws-nwsv4-lab
```

## 注意事項
- .pem ファイルはリポジトリにアップロードしないでください。
- AWSアクセスキー、シークレットキー、DBパスワードなどの機密情報は記載しないでください。






## 2026/05/28　Claudeの記載のもの
# aws-nwsv4-lab

![CloudFormation](https://img.shields.io/badge/IaC-CloudFormation-orange?style=flat-square&logo=amazonaws)
![AWS](https://img.shields.io/badge/Cloud-AWS-232F3E?style=flat-square&logo=amazonaws)
![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)

「基礎からのネットワーク&サーバー構築［改訂4版］」の学習内容を AWS CloudFormation で再現するリポジトリです。

---

## Overview

本リポジトリでは、書籍で構築する AWS ネットワーク・サーバー構成を CloudFormation テンプレートとして管理します。
IaC（Infrastructure as Code）として定義することで、構成の再現性・変更管理・レビューを容易にすることを目的としています。

> **学習目的**: VPC 設計・パブリック/プライベートサブネット・NAT Gateway・セキュリティグループ の理解と実装

---

## Architecture

![Architecture](diagrams/aws-nwsv4-architecture.png)

> 構成図は [draw.io](https://app.diagrams.net/) または [Cloudcraft](https://www.cloudcraft.co/) で作成することを推奨します。

---

## AWS Resources

| Resource | 設定値 / 備考 |
|---|---|
| VPC | CIDR: `10.0.0.0/16` / DNS ホスト名: 有効 |
| パブリックサブネット | `10.0.1.0/24` / AZ: ap-northeast-1a |
| プライベートサブネット | `10.0.2.0/24` / AZ: ap-northeast-1a |
| Internet Gateway | VPC にアタッチ |
| NAT Gateway | パブリックサブネット配置 / Elastic IP 割り当て |
| ルートテーブル | パブリック用・プライベート用 |
| セキュリティグループ | Web 用 (80/443/22) / DB 用 (3306) |
| IAM ロール | EC2 用 (SSM Session Manager 対応) |
| EC2 (Web サーバー) | Amazon Linux 2023 / t3.micro |
| EC2 (DB サーバー) | Amazon Linux 2023 / t3.micro |

---

## Directory Structure

```text
.
├── README.md                          # このファイル
├── templates/
│   └── aws-nwsv4.yaml                 # CloudFormation テンプレート
├── parameters/
│   └── aws-nwsv4-lab.example.json     # パラメータ例 (機密情報なし)
├── diagrams/
│   └── aws-nwsv4-architecture.png     # アーキテクチャ図
└── docs/
    └── notes.md                       # 学習メモ・補足手順
```

---

## Prerequisites

以下の環境が必要です。

- AWS CLI v2（設定済み: `aws configure` 完了）
- デプロイ先 AWS アカウントおよびリージョン (`ap-northeast-1` を推奨)
- 以下の IAM 権限:
  - `cloudformation:*`
  - `ec2:*`
  - `iam:CreateRole` / `iam:PassRole`

---

## Usage

### Deploy（スタック作成）

```bash
aws cloudformation create-stack \
  --stack-name aws-nwsv4-lab \
  --template-body file://templates/aws-nwsv4.yaml \
  --parameters file://parameters/aws-nwsv4-lab.example.json \
  --capabilities CAPABILITY_NAMED_IAM \
  --region ap-northeast-1
```

### Update（スタック更新）

```bash
aws cloudformation update-stack \
  --stack-name aws-nwsv4-lab \
  --template-body file://templates/aws-nwsv4.yaml \
  --parameters file://parameters/aws-nwsv4-lab.example.json \
  --capabilities CAPABILITY_NAMED_IAM \
  --region ap-northeast-1
```

### Destroy（スタック削除）

```bash
aws cloudformation delete-stack \
  --stack-name aws-nwsv4-lab \
  --region ap-northeast-1
```

> **注意**: 削除前に EC2 インスタンスの EBS スナップショットが必要な場合は事前に取得してください。

---

## Security

> ⚠️ **以下のファイルは絶対にコミットしないこと**

| 禁止ファイル | 理由 |
|---|---|
| `*.pem` | EC2 接続用秘密鍵 |
| `*accesskey*` / `*secretkey*` | AWS 認証情報 |
| `.env` | 環境変数ファイル |
| `*.tfvars` | Terraform 変数ファイル |
| `terraform.tfstate` | インフラ状態ファイル |

これらは `.gitignore` で除外済みです。万一コミットした場合は **即座にキーをローテーション**してください。

---

## References

- [基礎からのネットワーク&サーバー構築 改訂4版](https://www.amazon.co.jp/dp/XXXXXXXXXX) — 本リポジトリの元書籍
- [AWS CloudFormation ユーザーガイド](https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/)
- [AWS VPC ユーザーガイド](https://docs.aws.amazon.com/ja_jp/vpc/latest/userguide/)
- [AWS CLI コマンドリファレンス](https://docs.aws.amazon.com/cli/latest/reference/)

---

## License

このリポジトリは個人学習・ポートフォリオ目的で公開しています。
コードは [MIT License](LICENSE) のもとで利用可能です。



