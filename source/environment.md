(content:references:environment)=
# 環境構築


connpass-clientをforkします

[https://github.com/fin-py/connpass-client](https://github.com/fin-py/connpass-client) にアクセスし、 `Fork` をクリックします

![](https://i.imgur.com/B5KnRhS.png)

`Create fork` をクリックします

![](https://i.imgur.com/K8vm1DM.png)

forkしたリポジトリから `Settings` -> `Actions` -> `Workflow permissions` から `Read and write permissions` を選択して `Save` します

![](https://i.imgur.com/Ah24poc.png)


forkしたリポジトリをclone し、カレントディレクトリを移動します

```bash
git clone git@github.com:<your github account>/connpass-client.git
cd connpass-client
```

(content:references:environment-venv)=
## 仮想環境にインストール

```{attention}
Poetryを使う場合は [Poetryを使って仮想環境にインストール](content:references:environment-poetry) に進んでください。
```

仮想環境を作成します。実行環境の合わせて `python` は `python3` などに置き換えてください。

```bash
python -m venv .venv
```

````{note}
Windwsの場合は `py` コマンドから仮想環境を作成します。
```bash
py -m venv .venv
```
````

仮想環境を有効化します。

```bash
source .venv/bin/activate
```

````{note}
WindwsでPowerShellを使う場合、スクリプトの実行許可を与えていない場合は次のコマンドを1回だけ実行します。

```bash
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser -Force
```

仮想環境を有効化します。
```bash
.venv\Scripts\activate
```
````

パッケージをインストールします。

```bash
python -m pip install pip -U
pip install . pytest coverage pytest-cov "genbadge[coverage]"
```

(content:references:environment-poetry)=
## Poetryを使って仮想環境にインストール

```{caution}
[仮想環境にインストール](content:references:environment-venv) の手順を実施している場合は以降の手順は不要です。
```

`virtualenvs.in-project` の設定を確認します。

```bash
poetry config --list
```

`virtualenvs.in-project = true` になっていなければ、仮想環境をカレントディレクトリに生成する設定にします（オプション）。

```bash
poetry config --local virtualenvs.in-project true
```

````{note}
`virtualenvs.in-project` の設定をグローバルにするには `--local` を省略します。
```bash
poetry config virtualenvs.in-project true
```

> 参考: [virtualenvs.in-project](https://python-poetry.org/docs/configuration#virtualenvsin-project)
````

Poetryの環境にインストールします。

```bash
poetry install
```

Poetryの環境を有効化します。

```bash
poetry shell
```