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

## ファイルパス

- Request specのファイルパスは `spec/requests/<コントローラー名>/<アクション名>_spec.rb` の形式で配置してください

```rb
# 例: UsersControllerのshowアクションのテスト
# ファイルパス: spec/requests/users/show_spec.rb

RSpec.describe "GET /users/:id", type: :request do
  it "ユーザー情報を表示すること" do
    user = FactoryBot.create(:user)

    get user_path(user)

    expect(response).to have_http_status(200)
    expect(response.body).to include(user.name)
  end
end

# 例: Api::V1::EpisodesControllerのindexアクションのテスト
# ファイルパス: spec/requests/api/v1/episodes/index_spec.rb

RSpec.describe "GET /api/v1/episodes", type: :request do
  it "エピソード一覧を返すこと" do
    episode = FactoryBot.create(:episode)

    get api_v1_episodes_path

    expect(response).to have_http_status(200)
    json = JSON.parse(response.body)
    expect(json["episodes"].length).to eq(1)
  end
end
```
