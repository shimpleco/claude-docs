# コーディング規約 (RSpec)

- `context` ブロックは使用せず、`it` の中にケースを書いてください

```rb
# NG
RSpec.describe "GET /", type: :request do
  context "ログインしていないとき" do
    it "ログインページにリダイレクトすること" do
    end
  end
end

# OK
RSpec.describe "GET /", type: :request do
  it "ログインしていないとき、ログインページにリダイレクトすること" do
  end
end
```
