# はじめてのGitHub Actions

[GitHub Actions のクイックスタート](https://docs.github.com/ja/actions/quickstart) の手順に従って、GitHub Actionsを実行してみます

`.github/workflows` ディレクトリを作成します

```bash
mkdir -p .github/workflows
```

`.github/workflows/github-actions-demo.yml` ファイルを作成し、下記の内容を記述します


```{code-block} yaml
:linenos:
name: GitHub Actions Demo
run-name: ${{ github.actor }} is testing out GitHub Actions 🚀
on: [push]
jobs:
  Explore-GitHub-Actions:
    runs-on: ubuntu-latest
    steps:
      - run: echo "🎉 The job was automatically triggered by a ${{ github.event_name }} event."
      - run: echo "🐧 This job is now running on a ${{ runner.os }} server hosted by GitHub!"
      - run: echo "🔎 The name of your branch is ${{ github.ref }} and your repository is ${{ github.repository }}."
      - name: Check out repository code
        uses: actions/checkout@v3
      - run: echo "💡 The ${{ github.repository }} repository has been cloned to the runner."
      - run: echo "🖥️ The workflow is now ready to test your code on the runner."
      - name: List files in the repository
        run: |
          ls ${{ github.workspace }}
      - run: echo "🍏 This job's status is ${{ job.status }}."
```

```{seealso}
[ワークフローファイルを理解する](https://docs.github.com/ja/actions/learn-github-actions/understanding-github-actions#%E3%83%AF%E3%83%BC%E3%82%AF%E3%83%95%E3%83%AD%E3%83%BC%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E3%82%92%E7%90%86%E8%A7%A3%E3%81%99%E3%82%8B)
```

コミット、プッシュします

```bash
git add .
git commit -m "Add Github Actions example"
git push orgin main
```

ブラウザからforkしたリポジトリにアクセスし、 `Actions` からワークフローの結果を確認します

![](https://i.imgur.com/Njm3Zvn.png)

ワークルフローをクリック

![](https://i.imgur.com/vtO7vDX.png)

ジョブ（ `Explore-GitHub-Actions` ）をクリック

![](https://i.imgur.com/8zS2Cvr.png)