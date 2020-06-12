## neuronjplus docker imageの使い方

- https://github.com/koji-endo/docker/tree/master/neuronjplus から`docker-compose.yml`をダウンロードしてくる。

- docker コンテナを立ち上げる

```
docker-compose up -d
```

- dockerのコンテナidを調べる この場合5453***だとわかる。

```
docker ps 

# CONTAINER ID        IMAGE                         COMMAND                  CREATED             STATUS              PORTS                               NAMES
# 54539225fd97        koji96/neuronjplus:0.1.0      "sleep infinity"         23 minutes ago      Up 23 minutes       0.0.0.0:9097->8888/tcp              neuronjplus010
```

- dockerコンテナに入る。コンテナIDは最初の4文字くらいを指定してやれば良い。ID以外に名前指定もできる。

```zsh
docker exec -it 5453 bash 
# 名前指定する場合
# docker exec -it neuronjplus010 bash


# ここからdocker containerの中での作業
# 角田さんのプログラムを実行する。(meta.jsonは設定してある。)
cd ~/pythonNeuronSimulation
python parallelNeuronSimulation.py -s testdata/HHsingle_exsample/run.json
```
- 結果の閲覧
  - 結果を閲覧したり分析したりするようにjupyter notebookを入れてある。

```zsh
# おまじないが必要ここはmacだと変わるかもしれない。
jupyter notebook --port 8888 --ip 0.0.0.0 --allow-root
# The Jupyter Notebook is running at:
# http://(54539225fd97 or 127.0.0.1):8888/?token=f8c20a034790e334221b98cd375121e6c6d5e3d0dad7522d
```

  - docker-compose.ymlを見るとわかるが、外の9097番ポートとdockerの8888番ポートをつないであるのでブラウザで`localhost:9097/?token=f8c20a034790e334221b98cd375121e6c6d5e3d0dad7522d`にアクセスする。トークンは出力を見て適宜変更する。

  - テストの結果はこんな感じのコードで見られる。(jupyter notebookを起動して　このコードをセルに打ち込んで実行する。)

```python
import pickle
import pprint
import glob
import os
import matplotlib.pyplot as plt
%matplotlib inline
list_of_files = glob.glob('result/**/*.pickle')

latest_file = max(list_of_files, key=os.path.getctime)
with open(latest_file) as f:
    zeroone=pickle.load(f)
print(len(zeroone["results"]["t"] ))
print(len(zeroone["results"]["r_v_list"][0][1]))
plt.plot(zeroone["results"]["t"],zeroone["results"]["r_v_list"][0][1])
```

- その他 (1) mount  
  docker-composeのマウント設定で、gatewayフォルダを通じてホストとdockerの間でファイルのやり取りをできるようにしてある。
  
- その他 (2) Dockerfileからビルド  
  `docker-compose_self_build.yml`を使うと良い。githubから`neuronjplus`内の他のファイルを落としてくることは必要である。
