# import_sample

レガシー Python (3.2.6 / 3.3.0) を Docker 上でビルド・実行するサンプルです。

## 前提条件
- Docker がインストール済みであること（Docker Desktop 等）

## ディレクトリ構成
- `python3_2_6_docker/`: Python 3.2.6 用の Dockerfile とサンプル
- `python3_3_0_docker/`: Python 3.3.0 用の Dockerfile とサンプル

## 使い方
以下はリポジトリのルート（この `README.md` がある場所）で実行することを想定しています。

### Python 3.2.6
```bash
cd python3_2_6_docker

# ビルド
docker build -t py326:local .

# 実行（カレントディレクトリを /work としてマウント）
docker run --rm -v "$PWD":/work -w /work py326:local python3.2 sample.py

# pkgutil_sample実行方法
docker run --rm \
  -e PYTHONIOENCODING=UTF-8 \
  -e LANG=C.UTF-8 \
  -e LC_ALL=C.UTF-8 \
  -v "$PWD":/work -w /work \
  py326:local python3.2 pkgutil_sample/pkgutil_sample.py
```

### Python 3.3.0
```bash
cd python3_3_0_docker

# ビルド
docker build -t py33:local .

# 実行（カレントディレクトリを /work としてマウント）
docker run --rm -v "$PWD":/work -w /work py33:local python3.3 sample.py
```

## 補足
- 上記の `-v "$PWD":/work` は macOS / Linux 向けです。Windows の PowerShell では `${PWD}`、CMD では `%cd%` を使用してください。
- タグ名は任意ですが、3.2.6 用は `py326:local`、3.3.0 用は `py33:local` として区別しています。
