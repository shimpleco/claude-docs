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

## Controller

- コントローラー1つにつき1つのアクションを定義してください

```rb
# typed: true
# frozen_string_literal: true

module Pages
  class ShowController < ApplicationController
    sig { returns(T.untyped) }
    def call
    end
  end
end
```

## Form

- 共通で使うバリデーションは `form_concerns` 配下にモジュールとして定義してください
- 各フィールドは `ActiveModel::Attributes` を使用して定義してください

### 命名規則

- クラス名は `(リソース名)Form::(名詞)` とします
  - 例: `SpaceForm::Creation`

### コード例

```rb
# typed: strict
# frozen_string_literal: true

module SpaceForm
  class Creation < ApplicationForm
    attribute :name, :string
  end
end
```

## Model

- `Model` では `T::Struct` や `T::Enum` を継承したクラスや、PORO (Plain Old Ruby Object) を定義します
- `Model` は必ず `app/models` 配下に配置してください
- `Service` 内で定義される `T::Struct` などは `Model` ではなく、`Service` の内部実装として扱います

```rb
# typed: strict
# frozen_string_literal: true

class User < T::Struct
  extend T::Sig

  include T::Struct::ActsAsComparable

  const :database_id, T::Mewst::DatabaseId
  # ...
end
```

## Service

### 命名規則

- クラス名は `(リソース名)Service::(動詞)` とします

  - 例: `PageService::Update`

- メソッド名は `#call` とします

```rb
# typed: strict
# frozen_string_literal: true

module PageService
  class Update
    sig { void }
    def call
      # ...
    end
  end
end
```

## View

- コントローラーのアクションごとに定義します
- メソッドはアクセサメソッドも含めて全てプライベートに定義してください

```rb
# typed: strict
# frozen_string_literal: true

module Pages
  class ShowView < ApplicationView
    sig { params(page: Page).void }
    def initialize(page:)
      @page = page
    end

    sig { returns(Page) }
    attr_reader :page
    private :page

    sig { returns(String) }
    private def title
      t("meta.title.pages.show")
    end
  end
end
```

## マイグレーション

- idの生成には `generate_ulid()` 関数を使用してください

```rb
create_table :examples, id: false do |t|
  t.uuid :id, default: "generate_ulid()", null: false, primary_key: true
  # ...
end
```
