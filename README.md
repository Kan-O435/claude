# claude

Claude Code 用のカスタムスキルを管理するプロジェクト。

## form-sales

企業リストを上から順番に処理し、各企業の公式サイトから問い合わせフォームを探して営業情報を入力・送信するスキル。

- 1社の処理が成功・失敗にかかわらず、必ず次の企業へ進む
- 処理結果は企業ごとに `results.csv` へ即時記録する
- 送信可否の判断（営業禁止・採用専用フォーム・CAPTCHA など）や重複送信防止のルールを内蔵
- モデルによる自動起動はせず（`disable-model-invocation: true`）、`/form-sales` で明示的に呼び出す

詳細な仕様は [`.claude/skills/form-sales/SKILL.md`](.claude/skills/form-sales/SKILL.md) を参照。

## 導入方法

1. リポジトリを取得する

   ```bash
   git clone <このリポジトリのURL>
   cd claude
   ```

2. [Claude Code](https://docs.claude.com/claude-code) をインストールしておく

3. `form-sales` スキルを使う場合は、以下のファイルを用意する（機密情報を含むため `.gitignore` 対象で、リポジトリには含まれない）

   | ファイル | 内容 |
   | --- | --- |
   | `.claude/skills/form-sales/templates/company-list.csv` | 送信対象の企業リスト（`company_name,url,status`） |
   | `.claude/skills/form-sales/templates/results.csv` | 処理結果の記録先 |
   | `.claude/skills/form-sales/templates/message.md` | 営業本文テンプレート |
   | `.claude/skills/form-sales/references/sender-profile.md` | 送信者（自社）情報 |

4. Claude Code 上で `/form-sales` を実行し、処理方針（自動送信するか、確認画面で止めるか）を指示する

## フォルダ構成

```
.
├── CLAUDE.md                          # Claude Code 向けプロジェクト設定（現状は空）
├── README.md                          # このファイル
├── .gitignore                         # form-sales の機密ファイルを除外
└── .claude/
    ├── settings.local.json            # ローカル権限設定
    └── skills/
        └── form-sales/                # フォーム営業スキル
            ├── SKILL.md                # スキル本体（処理ルール・手順の定義）
            ├── prompt-memo.md          # 実行時の呼び出しプロンプト例
            ├── references/
            │   ├── form-rules.md       # フォーム入力・判定ルール
            │   ├── sender-profile.md  # 送信者情報（gitignore対象）
            │   └── status-definitions.md # 処理ステータス定義
            └── templates/
                ├── company-list.csv    # 企業リスト（gitignore対象）
                ├── message.md          # 営業本文（gitignore対象）
                └── results.csv         # 処理結果（gitignore対象）
```
