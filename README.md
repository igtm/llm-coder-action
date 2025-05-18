# llm-coder GitHub Action

このリポジトリは、[llm-coder](https://github.com/igtm/llm-coder) ライブラリを使用した GitHub Action Composite を提供します。

## 概要

llm-coder GitHub Action を使用すると、LLM を活用したコード生成を GitHub ワークフローに簡単に統合できます。

以下の event を想定しています

- Issue へのコメント時
- Pull Request へのコメント時

## 使い方

ワークフローファイル (`.github/workflows/llm-coder.yml`) に以下のように追加します:

```yaml
name: LLM Coder

on:
  issue_comment:
    types: [created]
  pull_request_review_comment:
    types: [created]

permissions:
  contents: write
  pull-requests: write
  issues: write

jobs:
  llm-coder:
    if: startsWith(github.event.comment.body, '/llm-coder') && github.event.comment.user.type != 'Bot'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0
      - name: Run llm-coder
        uses: igtm/llm-coder-action@v1
        with:
          llm_coder_command: '/llm-coder'
          llm_model: ${{ vars.LLM_MODEL || 'gpt-4o' }}
          github_token: ${{ steps.github_app_token.outputs.token }}
          extra_pip: "boto3 poetry"
        # env:
        #   OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}  # For OpenAI users
        #   AWS_REGION_NAME: ${{ vars.AWS_REGION_NAME }}   # For Bedrock users
```
