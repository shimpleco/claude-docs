# コーディング規約 (Ruby)

- 文字列はダブルクオートで書いてください
- 最終行に改行を入れてください
  - RuboCopの `Layout/TrailingEmptyLines` に対応してください
- 後置ifは使わないでください

```rb
# NG
puts "Hello" if cond?

# OK
if cond?
  puts "Hello"
end
```

- ハッシュのキーと変数名が同じ場合は、省略記法を使用してください

```rb
# NG
def method(user:)
  User.create(user: user)
end

# OK
def method(user:)
  User.create(user:)
end
```

- 新たにRubyのファイルを作成するときはマジックコメントを書いてください
  - Sorbetで使用する `typed: xxx` はなるべく `strict` を指定します

```rb
# typed: strict
# frozen_string_literal: true

# ...
```

- プライベートメソッドを定義するときは `private def` としてください

```rb
# NG
private

def foo
end

# OK
private def foo
end
```
