# リポジトリの構造

基本的にRailsプロジェクトのディレクトリ構成となっていますが、一部独自のディレクトリがあります。

- `app/assets`: 画像やCSSのファイル
- `app/components`: ViewComponentを使用したコンポーネント
- `app/controllers`: Railsのコントローラー
- `app/forms`: フォームオブジェクト
- `app/javascript`: Hotwireで実装されたフロントエンド
- `app/jobs`: Active Job
- `app/mailers`: Action Mailer
- `app/models`: POROや構造体など
- `app/policies`: 認可ルールが書かれたクラス
- `app/records`: `ActiveRecord::Base` を継承したデータベースのテーブルと1:1の関係となるクラス
- `app/repositories`: RecordをModelに変換するクラス
- `app/services`: サービスクラス
- `app/validators`: カスタムバリデーション
- `app/views`: ViewComponentを使用したビュー
