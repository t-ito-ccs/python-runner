#----build環境
#python3の公式イメージDebian10のfullイメージ
FROM python:3.7-buster AS builder

# argsの取得
ARG GIT_USER
ARG GIT_REPOSITORY
ARG GIT_REVISION
ARG GIT_PACKAGE
ARG EXEC_FILE

# ディレクトリの作成
WORKDIR /var/opt/build

# pythonパッケージの追加
ENV PIPENV_VENV_IN_PROJECT=1
RUN pip install --upgrade pip \
&& pip install pipenv \
&& pipenv install git+https://github.com/${GIT_USER}/${GIT_REPOSITORY}.git@${GIT_REVISION}#egg=${GIT_PACKAGE}

# ディレクトリ配下の表示
RUN pwd;find . | sort | sed '1d;s/^\.//;s/\/\([^/]*\)$/|--\1/;s/\/[^/|]*/|  /g'

#----runner環境
FROM gcr.io/distroless/python3-debian10 as runner

# argsの取得
ARG EXEC_FILE

# 作業フォルダに移動
WORKDIR /var/opt/data
WORKDIR /var/opt/dist

# buildしたモジュールをコピー
COPY --from=builder /var/opt/build /var/opt

