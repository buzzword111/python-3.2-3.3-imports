FROM debian:bullseye

RUN apt-get update && apt-get install -y \
  build-essential wget curl ca-certificates \
  libbz2-dev libreadline-dev libsqlite3-dev zlib1g-dev \
  tk-dev uuid-dev libncurses5-dev libffi-dev && \
  rm -rf /var/lib/apt/lists/*

# 古い Python 用に OpenSSL 1.0.2u をビルド
WORKDIR /tmp
RUN wget https://www.openssl.org/source/old/1.0.2/openssl-1.0.2u.tar.gz && \
    tar xzf openssl-1.0.2u.tar.gz && cd openssl-1.0.2u && \
    ./config --prefix=/opt/openssl-1.0.2 no-ssl2 no-ssl3 && \
    make -j"$(nproc)" && make install_sw

# Python 3.2.6 をビルド
WORKDIR /tmp
RUN wget https://www.python.org/ftp/python/3.2.6/Python-3.2.6.tgz && \
    tar xzf Python-3.2.6.tgz && \
    cd Python-3.2.6 && \
    # ★ ここで _ctypes を無効化（configure_ctypes が常に False を返すようパッチ）
    sed -i 's/def configure_ctypes(self, ext):/def configure_ctypes(self, ext):\n        return False\n        #/' setup.py && \
    ./configure --prefix=/opt/python-3.2.6 \
      CPPFLAGS="-I/opt/openssl-1.0.2/include" \
      LDFLAGS="-L/opt/openssl-1.0.2/lib" \
      PKG_CONFIG_PATH="/opt/openssl-1.0.2/lib/pkgconfig" && \
    make -j"$(nproc)" && make install

ENV PATH="/opt/python-3.2.6/bin:${PATH}" \
    LD_LIBRARY_PATH="/opt/openssl-1.0.2/lib"

CMD ["python3.2", "--version"]
