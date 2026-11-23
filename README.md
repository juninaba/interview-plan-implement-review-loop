# Interview Plan Implement Review Loop

要件の確認から実装とレビューまでを進める Codex スキルです。リポジトリを調べてから判断が必要な点を一問ずつ確認し、計画ファイルを作成します。ユーザーの承認後に、実装とレビューを別々のサブエージェントに割り当て、修正が必要な指摘を解消するまで続けます。

たとえば検索結果の CSV 出力を追加するときは、全件を出力するか、表示中のページだけを出力するかを先に確認し、決まった方針を計画に記録します。レビューではコミット範囲を指定し、修正のたびに差分を確認したうえで、最後に変更全体を確認します。

## インストール

サブエージェントを利用できる Codex と、作業途中のコミットを作成できる Git リポジトリが必要です。自分の Codex 環境にインストールするには、以下を実行します。

```sh
mkdir -p "$HOME/.agents/skills"
git clone https://github.com/juninaba/interview-plan-implement-review-loop.git \
  "$HOME/.agents/skills/interview-plan-implement-review-loop"
```

スキルが一覧に表示されない場合は Codex を再起動してください。[OpenAI Docs のスキルガイド](https://learn.chatgpt.com/docs/build-skills)

## 使い方

対象のリポジトリで Codex を開き、依頼にスキル名を添えます。

```text
$interview-plan-implement-review-loop を使って、検索結果を CSV に出力する機能を実装してください。
```

計画の保存先と検証コマンドは、作業対象のリポジトリのルールに従います。計画の保存先に指定がなければ、`docs/plans/plan_YYYYMMDDHHMMSS.md` に作成します。ワークフローの詳細は [SKILL.md](SKILL.md) を参照してください。
