<!-- PBL2 2019 10/29 課題 -->

下記の仕様を満たすサーバプロクラムとクライアントプログラムをpython3で実装し、動作確認の上、サーバとクライアントのソースコードをGitHubで提出しなさい。ファイル名は指定されたものとすること。

提出期限: 10/29 23:59

（これを過ぎてももちろん受け付けますが、この締切を守るかどうかを評価の基準としています。）

## 仕様

### サーバ vcalc_server.py

適切なポート番号（例えば50003）を用いて動作し、TCPによる通信でクライアントから`値1a 値1b 演算子 値2a 値2b`という形式で文字列を受け取り、`演算結果a 演算結果b`という形式の文字列を返送する。演算結果aと演算結果bはそれぞれ、（値1a 指定された演算 値2a）、（値1b 指定された演算 値2b）です。つまり、これはネットワーク経由ベクトル計算機というわけです。

使用できる演算子は、`+`、`-`、`*`、`/`の4つとします。

サーバは、一つのクライアントととの通信を終えると、その通信に使用したソケットは閉じますが、引き続き待ち受け用のソケットは動作させて新たなクライアントからの接続を待ち続けるようにして下さい。特別なことではなく、サンプルにも示しているサーバとして極めて普通の実装です。

### クライアント vcalc_client.py

コマンドラインで指定されたホストとポート番号で動作するサーバプログラムにTCPを用いて接続し、コマンドラインで指定された計算式（例: `'10.3 32 * 3.1 4'`)の文字列をサーバに送信し、サーバから計算結果を含んだ応答を受信して、その結果を画面に表示する。

以下に例を示す。数式の部分はシングルクウォーテーションで必ず囲むこと(`'式...'`)。pythonスクリプトから見たときのコマンドライン引数は4個（つまり`len(sys.argv)) == 4`）となります。また、クライアントはこの第4引数（`sys.argv[3]`)をそのままバイト列（bytes）に`encoding()`で変換してサーバに送ること。

```
$ python3 vcalc_clt.py localhost 50003 '10.3 32 * 3.1 4'
31.93 128
$ 
```

### ヒント

- 文字列を解析する〜文字列のクラスには、空白文字（スペースやタブ、改行）で区切られた文字列を分解してリストに格納する関数が用意されています。

```
>>> phrase = 'What a new world'
>>> words = phrase.split()
>>> words
['What', 'a', 'new', 'world']
>>>
```

- ネットワークに文字列データを送る前には、必ず文字列を`encode()`でバイト列(bytes)に変換してから送信して下さい。

- ネットワークから受け取ったデータを文字列として扱う前に、必ず受け取ったバイト列に対して`encode()`を行って下さい。つまり`received_string = received_bytes.encode()`ということをするわけです。

## おまけ

コマンドラインから得た文字列を元に足し算をするだけのサンプルプログラム。通信はしません。
（サンプルプログラムの中で出てくる英語が分からないときは、ちゃんと辞書を引きましょう。例えば[Weblio](http://ejje.weblio.jp)）

```python
# -*- coding: utf-8 -*-
# simple_txt_parser.py

import sys

if len(sys.argv) < 2:
    sys.error("Usage: {0} 'number number'".format(sys.argv[0]))

print('sys.argv[1]: {0}'.format(sys.argv[1]))

tokens = sys.argv[1].split()
for i in range(len(tokens)):
    print('token[{0}]: {1}'.format(i, tokens[i]))

# add two numbers
s = float(tokens[0]) + float(tokens[1])
print('{0} + {1} = {2}'.format(float(tokens[0]), float(tokens[1]), s))
```


