## neuronjplus docker imageの使い方

- https://github.com/koji-endo/docker/tree/master/neuronjplus からDockerfileとdocker-compose.ymlファイルをダウンロードしてくる

- フォルダ構造
```
.
├── Dockerfile
└── docker-compose.yml
```

- docker コンテナを立ち上げる

```
docker-compose up -d
```

- dockerのコンテナidを調べる この場合5453***だとわかる。

```
docker ps 

# CONTAINER ID        IMAGE                         COMMAND                  CREATED             STATUS              PORTS                               NAMES
# 54539225fd97        koji96/neuronjplus:0.0.1      "sleep infinity"         23 minutes ago      Up 23 minutes       0.0.0.0:9097->8888/tcp              neuronjplus_jplus_1
```

- dockerコンテナに入る。コンテナIDは最初の4文字くらいを指定してやれば良い。(確かID以外に名前指定とかもできるはず。)

```zsh
docker exec -it 5453 bash 
# ここからdocker containerの中での作業 

# pyenvの設定反映のために必要(必要ないように作れるはずだが試していない。)
source ~/.bash_profile
python --version
# 2.7.16にバージョンがなっていればOK

# 角田さんのプログラムを実行する。
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

  - docker-compose.ymlを見るとわかるが、外の9097番ポートと8888番ポートをつないであるのでブラウザでlocalhost:9097にアクセスする。

  - テストの結果はこんな感じのコードで見られる

```python
import pickle
import pprint
import matplotlib.pyplot as plt
%matplotlib inline
with open("./result/2020-04-22T14_45_30.501406/0_1.pickle") as f:
    zeroone=pickle.load(f)
print(len(zeroone["results"]["t"] ))
print(len(zeroone["results"]["r_v_list"][0][1]))
plt.plot(zeroone["results"]["t"],zeroone["results"]["r_v_list"][0][1])
```
