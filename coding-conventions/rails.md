# コーディング規約 (Rails)

## 依存関係

- 主要なクラスの依存関係から外れないように書いてください
  - Railsなど外部のライブラリが提供するクラスなどはどのクラスからでも呼び出して大丈夫です

| クラス     | 説明                               | 依存先                                                        |
| ---------- | ---------------------------------- | ------------------------------------------------------------- |
| Component  | 再利用可能なUI要素                 | `Component`, `Form`, `Model`                                  |
| Controller | HTTPリクエスト処理と応答の調整     | `Form`, `Model`, `Record`,<br>`Repository`, `Service`, `View` |
| Form       | フォームオブジェクト               | `Record`, `Validator`                                         |
| Job        | ジョブの定義                       | `Service`                                                     |
| Mailer     | メール送信                         | `Model`, `Record`, `Repository`, `View`                       |
| Model      | データ構造とドメインロジックを表現 | `Model`                                                       |
| Policy     | 認可ルール                         | `Record`                                                      |
| Record     | DBのテーブルから取得・保存する     | `Record`                                                      |
| Repository | ModelとRecordの変換                | `Model`, `Record`                                             |
| Service    | ビジネスロジックのカプセル化       | `Job`, `Mailer`, `Record`                                     |
| Validator  | カスタムバリデーション             | `Record`                                                      |
| View       | 表示処理                           | `Component`, `Form`, `Model`                                  |

## 各クラスの実装方針

- 以下のページに記載している方針で実装します
  - https://wikino.app/s/shimbaco/pages/657
  - ↑のページの内容を読み取ってください

## マイグレーション

- idの生成には `generate_ulid()` 関数を使用してください

```rb
create_table :examples, id: false do |t|
  t.uuid :id, default: "generate_ulid()", null: false, primary_key: true
  # ...
end
```

## I18n (国際化)

### 翻訳ファイルの配置

- `config/locales/` 配下の翻訳ファイルは用途別に分類して配置してください
- 基本的には以下のファイル構成に従ってください:

| ファイル名             | 用途               | 例                                             |
| ---------------------- | ------------------ | ---------------------------------------------- |
| `forms.(ja,en).yml`    | フォーム関連       | バリデーションエラーメッセージ、フォーム属性名 |
| `messages.(ja,en).yml` | メッセージ・説明文 | エラーメッセージ、プレースホルダー、説明文     |
| `meta.(ja,en).yml`     | メタデータ         | ページタイトル、description                    |
| `nouns.(ja,en).yml`    | 名詞・ラベル       | ボタンテキスト、フィールドラベル               |

### 翻訳キーの命名規則

- 機能的に関連するものは同じファイル内にまとめてください
  - 例: 検索機能のプレースホルダーは `messages.search.search_keyword_placeholder` として `messages.ja.yml` に配置する
  - 例: 検索ボタンのテキストは `nouns.search` として `nouns.ja.yml` に配置する

### 多言語対応

- 新しい翻訳を追加する際は、日本語ファイル (`.ja.yml`) と英語ファイル (`.en.yml`) の両方を更新してください
- アプリケーションは日本語と英語の両方をサポートしています (`config.i18n.available_locales = %i[en ja]`)

## Sorbet

- `T.must` は使わず、`config/initializers/not_nil.rb` で定義している `#not_nil!` を使用してください
