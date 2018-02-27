# LAMP環境構築用のDockerFile

## 構成
### WebServer

* CentOS7
* PHP 7.2
* Apache2.4

### DB

* Mysql Stable

## 各種ファイル

- docker-compose.yml
  - コンテナの統合管理。web-dbの関連付けやローカルボリュームのマウントを行う。

- build/app/Dockerfile
  - dockerの構築コマンド管理ファイル。docker-compose.ymlで本ファイルを指定する。

- data/mysql/*
  - mysqlのデータを永続化するためのディレクトリ。ローカルボリュームとマウントすることで永続化を実現。docker-compose.ymlで指定する。

- volumes/app/*
  - WebサーバのDocumentRoot。ローカルボリュームとマウントすることで永続化を実現。docker-compose.ymlで指定する。

## 使い方

1. 80番、3306番ポートを使用したdockerコンテナが起動していないことを確認。起動していたら止める。

2. docker-compose.ymlがあるディレクトリに移動し

```
docker-compose up -d
```

を実行する。
Dockerfileからbuildが始まる。

3. 作成されたコンテナの例

```
CONTAINER ID        IMAGE                          COMMAND                  CREATED             STATUS              PORTS                    NAMES
70319056939a        phpdockerfilemaster_web_php7   "/bin/sh -c /sbin/in…"   31 minutes ago      Up 31 minutes       0.0.0.0:80->80/tcp       web_php7
cd0250950e63        mysql                          "docker-entrypoint.s…"   31 minutes ago      Up 31 minutes       0.0.0.0:3306->3306/tcp   mysqldb

```

4. ローカルボリュームのvolumes/app配下にアプリケーションを配置します。

### docker command tips

```
docker-compose
 options:
  ・config
    - docker-compose.ymlで書かれてる内容が表示される。
  ・down
    - docker-compose.ymlに記載してあるサービスのコンテナを停止し、そのコンテナとネットワークを削除する。--rmi allをつけることでイメージも削除する。
  ・logs
    - ログを出力する。

docker
 options:
  ・ps
    - 起動中コンテナの一覧を表示。-aを付けることで停止中のコンテナ含めて表示される。
  ・start <コンテナ名/ID>
    - 指定したコンテナを起動 
  ・stop <コンテナ名/ID>
    - 指定したコンテナを停止
  ・exec -it <コンテナ名/ID> /bin/bash
    - コンテナにログイン
  ・rm <コンテナ名/ID>
    - 指定した停止中のコンテナを削除
  ・rmi <イメージ名/ID>
    - 指定したイメージを削除
```
