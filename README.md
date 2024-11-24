# docker-study2

## Python をベースにした Flask 開発環境

このサンプルは Python(Flask) を起動させるためのサンプルです。  
データベースを使わないシンプルなAPIやFlaskを使ったWebページを作るためのサンプルです。

### Dockerfile

<pre>
# Pythonの公式イメージをDockerHubからダウンロード
FROM python:3.13-slim-bullseye

# インストールされているソフトウェアを更新し、追加でソフトウェアをインストール
RUN apt update && apt install -y less man-db sudo
</pre>

### docker-compose.yml

<pre>
version: '3.8'

services:
  app: # コンテナごとにdocker-compose内名前を付ける
    build: . # Dockerfileのパスを指定
    container_name: flask-app # コンテナの名前を付ける
    # ttyとstdin_openはコンテナを起動させておくための設定
    tty: true
    stdin_open: true
    # WSLとコンテナのフォルダの共有設定
    volumes:
      - ../:/workspace:cached
</pre>

### devcontainer.js
<pre>
  "name": "Python project",
  "dockerComposeFile": "./docker-compose.yml",
  "service": "app",
  "workspaceFolder": "/workspace",
  "customizations": {
    "vscode": {
      "extensions": ["ms-python.python", "ms-python.debugpy"],
      "settings": {
        "python.autoComplete.extraPaths": [
          "/usr/local/bin/python3"
        ]
      }
    }
  }
</pre>

VSCodeでPythonを動かす場合にインテリセンスを有効にするためにPythonのパスを設定する必要があります。  
settingsの下にpython.autoComplete.extraPathsを設定し、Pythonのパスを指定してください。
```
settings: {
    python.autoComplete.extraPaths: [
      "python3のパス"
    ]
}
```

Pythonのインストールフォルダを調べる方法
```
which python3
> /usr/local/bin/python3
```

### install Flask

表示 → コマンドパレット → 開発コンテナー:コンテナーで再度開く  
ターミナルから以下のコマンドでFlaskをインストール
```
pip install flask
```

pythonのパッケージをインストールしたらrequirements.txtに記載しておきましょう。  
```
pip freeze > requirements.txt
```

### 既にプロジェクトのファイルがインストールされている場合

git clone でソースコードをダウンロードしたらアプリケーションのフォルダを開いてで必要なパッケージをインストールする

```
pip install -r requirements.txt
```
※requirements.txtのあるフォルダで実行すること

### app.pyの作成
<pre>
#"Hello World!"と表示されるWebアプリケーション
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello():
    return 'Hello World!'

if __name__ == '__main__':
    app.run()
</pre>

app.pyを作成したら以下のコマンドでFlaskアプリケーションを起動して  
http://localost:5000  
にアクセスしてみましょう。
```
python3 app.py
```
