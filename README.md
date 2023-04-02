M1 Macの環境でLarge Language Modelを動作させる。
===


# 環境構築

M1 Mac用の機械学習開発環境を整えるために、Miniforgeを活用する。  

MiniforgeはM1 Macで機械学習用のPython環境構築によく使われるパッケージマネージャ．  
Condaみたいな感じで，複数のConda環境(仮想環境)を作成できる．

> MacOSを13.3にアップグレードする必要があります。

## 依存パッケージ
```
$ brew install git-lfs
$ brew install md5sha1sum
```

## miniforge

- miniforgeのインストール
  ```
  $ curl -L -O "https://github.com/conda-forge/miniforge/releases/latest/download/Mambaforge-$(uname)-$(uname -m).sh"
  $ bash Mambaforge-$(uname)-$(uname -m).sh
  ```

- miniforgeのcodaを起動
  ```
  $ source ~/mambaforge/bin/activate
  ```

## conda

- condaの環境構築. mlの名前で環境を作成する。

  ```
  $ conda create -n ml python=3.9
  ```

- mlの環境を有効化する。

  ```
  $ conda activate ml
  ```

## Tensorflow

- Tensorflowのインストール
  ```
  conda install -c apple tensorflow-deps -y
  ```
  - -c: パッケージ取得先のチャネル/リポジトリ?

  ```
  $ python -m pip install tensorflow-macos
  $ python -m pip install tensorflow-metal
  ```


- インストールされたバージョンを確認する。
  ```
  $ python test-tf.py
  2.12.0
  ```

## 機械学習用のパッケージをインストール

- huggingfaceとtransformerのインストール
  ```
  $ conda install -c huggingface transformers -y
  ```


- よく使うパッケージ類を色々インストール
  ```
  $ conda install -c conda-forge scikit-learn -y
  $ conda install -c conda-forge pandas jupyter jupyterlab -y
  $ conda install -c pytorch pytorch torchvision -y
  $ conda install -c conda-forge sentencepiece -y
  $ conda install -c conda-forge ipywidgets -y
  $ pip3 install --pre torch torchvision torchaudio --index-url https://download.pytorch.org/whl/nightly/cpu
  ```

- transformersはv4.27.0にバグがありLLaMaのtokenizerのファイル名が誤っているため動作せず。ソースから最新版を取得した。
  ```
  $ pip install git+https://github.com/huggingface/transformers.git
  ```

- Transformerのバージョン確認
  ```
  $ python test-transformers.py
  4.28.0.dev0
  ```

## Jupyter notebook
- 起動
  ```
  $ jupyter notebook
  ```


# モデルの種類

## LLaMA系統
- 基本: LLaMA-7B:
  - https://huggingface.co/decapoda-research/llama-7b-hf
- 対話用: alpaca-lora-7b
  - https://huggingface.co/tloen/alpaca-lora-7b
- マルチモーダル対応: Open Flamingo-9B
  - https://huggingface.co/openflamingo/OpenFlamingo-9B
- Vicuna 13B
  - チャット対応
    - https://github.com/lm-sys/FastChat


# トラブルシューティング
- M1 MacでGPUを使う時に発生する`error message: MPS does not support cumsum op with int64 input`エラーの解決策.  
  MacOS13.3にアップデートし、次のパッケージをインストールする。  
  ```
  pip3 install --pre torch torchvision torchaudio --index-url https://download.pytorch.org/whl/nightly/cpu
  ```
  - 参考情報  
  `https://github.com/pytorch/pytorch/issues/96610`