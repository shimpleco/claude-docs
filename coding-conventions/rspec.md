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

- `let`, `let!` は使わず、`it` 内に変数を定義してください

```rb
# NG
RSpec.describe "GET /", type: :request do
  let!(:user) { FactoryBot.create(:user) }
  let(:page) { FactoryBot.create(:page) }

  it "xxx" do
    # ...
  end
end

# OK
RSpec.describe "GET /", type: :request do
  it "xxx" do
    user = FactoryBot.create(:user)
    page = FactoryBot.create(:page)
    # ...
  end
end
```

- `described_class` は使用せず、明示的にクラス名を書いてください

```rb
# NG
RSpec.describe User, type: :model do
  it "xxx" do
    user = described_class.new(name: "太郎")
    # ...
  end
end

# OK
RSpec.describe User, type: :model do
  it "xxx" do
    user = User.new(name: "太郎")
    # ...
  end
end
```
