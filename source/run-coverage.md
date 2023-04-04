# GitHub Actionsによるカバレッジの実行

ここでは次の内容をGitHub Actionsを利用して実行します

1. カバレッジのレポートを生成（HTML、XML）
2. XMLファイルをもとに、カバレッジのバッジを生成
3. レポート（HTML）とバッジを `coverage` ブランチにコミット・プッシュ

## YAMLファイルの作成

`.github/workflows/coverage.yml` ファイルを作成し、下記の内容を記述します

```{code-block} yaml
:linenos:

name: coverage
on:
  push:
    branches: [ main ]
env:
  BRANCH_NAME: coverage
jobs:
  generate-coverage:
    runs-on: ubuntu-latest
    steps:
      - name: checkout
        uses: actions/checkout@v3
      - name: pip install
        uses: actions/setup-python@v3
        with:
          python-version: '3.10'
      - run: |
          python -m pip install --upgrade pip
          python -m pip install pytest coverage pytest-cov "genbadge[coverage]"
      - name: genarate coverage and badge
        run: |
          pytest --cov=connpass_client tests
          coverage html
          mv htmlcov/* .
          coverage xml
          genbadge coverage -i - < coverage.xml
      - name: commit and push
        run: |
          git config --local user.email "actions@github.com"
          git config --local user.name "GitHub Actions"
          git fetch origin $BRANCH_NAME && git push --delete origin $BRANCH_NAME
          git checkout --orphan $BRANCH_NAME
          git rm -rf .
          git add .
          git commit -m "generate coverage"
          git push origin $BRANCH_NAME
```

```{code-block} yaml
:lineno-start: 2
on:
  push:
    branches: [ main ]
```

`main` ブランチにプッシュされた場合に、ワークフローにトリガーされます

```{seealso}
[ワークフローのトリガー](https://docs.github.com/ja/actions/using-workflows/triggering-a-workflow)
```

```{code-block} yaml
:lineno-start: 6
env:
  BRANCH_NAME: coverage
```

環境変数 `BRANCH_NAME` に `coverage` を設定しています

```{seealso}
[環境変数の設定](https://docs.github.com/ja/actions/using-workflows/workflow-commands-for-github-actions#setting-an-environment-variable)
```

```{code-block} yaml
:lineno-start: 12
        uses: actions/checkout@v3
```

このステップで [actions/checkout@v3](https://github.com/actions/checkout) を実行します

リポジトリをランナーにチェックアウトするアクションであり、コードに対してスクリプトまたはほかのアクション (ビルド、ツールやテスト、ツールなど) を実行できます

```{seealso}
[GitHub Actions を理解する](https://docs.github.com/ja/actions/learn-github-actions/understanding-github-actions)
```

```{code-block} yaml
:lineno-start: 14
        uses: actions/setup-python@v3
        with:
          python-version: '3.10'
```

このステップで [actions/setup-python@v3](https://github.com/actions/setup-python) を実行します

このアクションは、GitHub Actionsのユーザーに対して、次の機能を提供します

- PythonまたはPyPyのバージョンをインストールし、（デフォルトで）PATHに追加
- オプションでpip、pipenv、poetryの依存関係をキャッシュ
- エラー出力のためのプロブレムマッチャーを登録

```{code-block} yaml
:lineno-start: 23
          coverage html
          mv htmlcov/* .
          coverage xml
```

[Coverage.py](https://coverage.readthedocs.io/en/7.2.2/index.html) を利用してカバレッジのレポートを生成しています

`coverage html` コマンドでカバレッジのレポートをHTML形式に出力します

```{seealso}
[HTML reporting: coverage html](https://coverage.readthedocs.io/en/7.2.2/cmd.html#html-reporting-coverage-html)
```

`coverage xml` コマンドでカバレッジデータを [Cobertura](https://cobertura.github.io/cobertura/) と互換性のあるXML形式に出力します

```{seealso}
[XML reporting: coverage xml](https://coverage.readthedocs.io/en/7.2.2/cmd.html#xml-reporting-coverage-xml)
```

```{code-block} yaml
:lineno-start: 26
          genbadge coverage -i - < coverage.xml
```

[genbadge](https://smarie.github.io/python-genbadge/) を利用してバッジを生成します

```{code-block} yaml
:lineno-start: 31
          git fetch origin $BRANCH_NAME && git push --delete origin $BRANCH_NAME
```

`$BRANCH_NAME` ブランチを確認し、ブランチが存在している場合は削除します

```{code-block} yaml
:lineno-start: 32
          git checkout --orphan $BRANCH_NAME
```

空の `$BRANCH_NAME` ブランチを生成します

```{seealso}
[git-checkout - Switch branches or restore working tree files](https://git-scm.com/docs/git-checkout)
```

## ワークフローの実行

コミット、プッシュします

```bash
git add .
git commit -m "Add Github Actions coverage"
git push orgin main
```

ブラウザからforkしたリポジトリにアクセスし、 `Actions` からワークフローの結果を確認します

![](https://i.imgur.com/aU2qIpa.png)

リポジトリの `Code` に移動し、 `coverage` ブランチがあることを確認します

![](https://i.imgur.com/pVYwv3a.png)

`coverage` ブランチに `coverage-badge.svg` が作成されていることを確認します

![](https://i.imgur.com/4NY0hiP.png)