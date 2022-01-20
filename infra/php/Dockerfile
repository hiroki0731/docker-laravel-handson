# ベースイメージ指定。nginx構成ならfpm版。alpineが一番軽量だが、メンテナンス等大変とのことでbuster指定。
FROM php:8.0-fpm-buster
# おまじない。デフォルトのシェルをオーバーライドして見やすくしている。"-oeux", "pipefail"これを追加しとくと便利って話みたい
SHELL ["/bin/bash", "-oeux", "pipefail", "-c"]

# 環境変数を設定。ただの変数定義じゃなくてもうこれで設定がされている。
ENV COMPOSER_ALLOW_SUPERUSER=1 \
  COMPOSER_HOME=/composer

# 新しいファイルまたはディレクトリをコンテナのパスにコピー
# --from オプションを付けると別のイメージのファイルを指定できる。(マルチステージビルド)
# composer のインストールがとてもシンプルになっています。
COPY --from=composer:2.0 /usr/bin/composer /usr/bin/composer

# 現在のイメージの上に任意のコマンドを実行した結果をコミットするのがRUN
# Laravelに必要なライブラリとPHP拡張ライブラリのインストール
RUN apt-get update && \
  apt-get -y install git unzip libzip-dev libicu-dev libonig-dev && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/* && \
  docker-php-ext-install intl pdo_mysql zip bcmath

# 左のホスト側ファイルを右のコンテナの場所へコピーしてる。
COPY /infra/php/php.ini /usr/local/etc/php/php.ini
COPY /backend /work

# コンテナの作業ディレクトリを指定
WORKDIR /work