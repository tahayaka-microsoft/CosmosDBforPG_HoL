# 1. コンテナ環境での実施方法
 Cosmos DB for PostgreSQL(Citus)は、Linux + PostgreSQL + Citus extensionによって構成されており、Docker Hubにてコンテナイメージが公開されています。
 
 ここでは、このイメージを利用してハンズオンを実施する方法を説明します。
 
## 1.1 コンテナ環境の前提条件
 上述のコンテナイメージはx86_64(x64)のみが用意されているため、Apple Silicon (M1, M2など）搭載MacのDocker Desktopでは、Rosetta2をインストールしておく必要があります。
```
softwareupdate --install-rosetta --agree-to-license
```
 Podman for macOSでは、もう少し面倒な手順が必要です（2024年2月時点）。
 
 Docker / Podman on Linux(x86_64)、Docker Desktop on Windows、Docker Desktop on Intel Macでは特に難しいところは無いでしょう。

## 1.2 コンテナのpull
 コンテナイメージをDocker Hubからpullします。
```
docker pull citusdata/citus
```

## 1.3 コンテナのrun
 コンテナを実行します。
```
docker run --name citus_standalone -p 5432:5432 -e POSTGRES_PASSWORD='your_password' citusdata/citus
```

## 1.4 psqlでの接続
 citusに接続します。
```
psql -U postgres -p 5432 -h localhost
```
