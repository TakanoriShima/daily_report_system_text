# Lesson0 日報管理システム個人開発ワーク

`Techpit教材の環境構築に準じて改編しています。`

## 1. 環境設定の確認

### 1.1 ターミナルのシェルが /bin/bash となっているか？

ターミナルで以下のコマンドを実行する

```bash
echo $SHELL
```

以下のように　/bin/bashが設定されていればOK

```bash
/bin/bash
```

もしされていない場合は、以下のコマンドを実行する

```bash
chsh -s /bin/bash
```

### 1.2 Homebrew がインストールされているか？

ターミナルで以下のコマンドを実行する

```bash
brew -v
```

以下のように表示されればOK

```bash
Homebrew 4.4.xx
```

もし表示されない場合は、ターミナルで以下を実行

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### 1.3 asdf がインストールされているか？

ターミナルで以下のコマンドを実行する

```bash
asdf -v
```

以下のように表示されればOK

```bash
asdf version 0.16.7
```

もし表示されない場合は、ターミナルで以下を実行

```bash
brew install asdf
echo -e "\n. $(brew --prefix asdf)/libexec/asdf.sh" >> ~/.bash_profile
source ~/.bash_profile
```

もしバージョンが低い場合は、念のためターミナルにて以下のコマンドを実行

```bash
brew upgrade asdf
```

## 1.4 asdf を使って Ruby 3.2.0 がインストールされているか？

まずはターミナルで以下のコマンドを実行し、rubyのプラグインがインストールされているか確認

```bash
asdf plugin list | grep ruby
```

以下のように表示されればOK

```bash
ruby
```

されていない場合は、ターミナルで以下を実行

```bash
asdf plugin add ruby https://github.com/asdf-vm/asdf-ruby.git
```

続いて、ターミナルで以下のコマンドを実行する

```bash
asdf list ruby
```

以下のように 3.2.0系がインストールされていればOK

```bash
3.2.0
```

もしされていない場合は、以下のコマンドを実行する

```bash
asdf install ruby 3.2.0
```

## 1.5 asdf を使って Nodejs lts がインストールされているか？

まずはターミナルで以下のコマンドを実行し、nodejsのプラグインがインストールされているか確認

```bash
asdf plugin list | grep nodejs
```

以下のように表示されればOK

```bash
nodejs
```

されていない場合は、ターミナルで以下を実行

```bash
asdf plugin add nodejs https://github.com/asdf-vm/asdf-nodejs.git
```

続いて、ターミナルで以下のコマンドを実行する

```bash
asdf list nodejs
```

以下のように lts がインストールされていればOK

```bash
lts
```

もしされていない場合は、以下のコマンドを実行する

```bash
asdf install nodejs lts
```

## 1.6 asdf を使って Yarn latest がインストールされているか？

まずはターミナルで以下のコマンドを実行し、yarnのプラグインがインストールされているか確認

```bash
asdf plugin list | grep yarn
```

以下のように表示されればOK

```bash
yarn
```

されていない場合は、ターミナルで以下を実行

```bash
brew install gpg
asdf plugin add yarn
```

続いて、ターミナルで以下のコマンドを実行する

```bash
asdf list yarn
```

以下のように lts がインストールされていればOK

```bash
1.22.22
```

もしされていない場合は、以下のコマンドを実行する

```bash
asdf install yarn latest
```

## 1.7 PCに MySQLがインストールされているか？

ターミナルで以下のコマンドを実行する

```bash
mysql --version
```

以下のように MySQL がインストールされていればOK

```bash
mysql  Ver 8.0.xx for xxxx (Homebrew)
```

もしされていない場合は、以下のコマンドを実行する

```bash
brew install mysql@8.0
echo 'export PATH="/opt/homebrew/opt/mysql@8.0/bin:$PATH"' >> ~/.bash_profile
source ~/.bash_profile  
```

## 2. デモアプリ（test_app）の作成

ターミナルにて以下のコマンドを実行する

```bash
cd ~
[ ! -d tech_academy ] && mkdir tech_academy
cd tech_academy
```

この時点で VSCodeでは ~/tech_academyディレクトリを開く

次にVSCodeのターミナルで以下を実行する

```bash
mkdir test_app
cd test_app
```

次に asdfを使って、ターミナルで以下のコマンドを実行する

```bash
asdf set ruby 3.2.0
asdf set nodejs lts
asdf set yarn 1.22.22
asdf current
```

以下のようにターミナルに表示されていればOK

```bash
Name            Version         Source                                                  Installed
nodejs          lts             /xxx/tech_academy/test_app/.tool-versions true
ruby            3.2.0           /xxx/tech_academy/test_app/.tool-versions true
yarn            1.22.22         /xxx/tech_academy/test_app/.tool-versions true
```

次に、ターミナルで以下を実行する。test_appディレクトリ直下に Gemfileが生成される

```bash
bundle init
```

Gemfile全体を以下のように上書きし、保存する

```Gemfile
# frozen_string_literal: true

source "https://rubygems.org"

git_source(:github) { |repo_name| "https://github.com/#{repo_name}" }

# gem "rails"
gem 'rails', '7.0.3.1'
```

続いて、ターミナルで以下のコマンドを実行

```bash
bundle install --path vendor/bundle
```

ターミナルで以下のコマンドを実行し、viエディタを起動する

```bash
vi vendor/bundle/ruby/3.2.0/gems/activesupport-7.0.3.1/lib/active_support/logger_thread_safe_level.rb
```

logger_thread_safe_level.rbの 7行目に以下を追記して上書き保存

```rb: logger_thread_safe_level.rb
require "logger"
```

※ 参考
[viの使い方](https://toshpit.com/howtouse-vi/)

ターミナルで以下を実行し、次のように表示されればOK

```bash
bundle exec rails -v
    Rails 7.0.3.1
```

続いて、ターミナルで以下を実行する

```bash
bundle exec rails new . --database=mysql --css=bootstrap
```

途中で `Y` を入力してエンターキーを押す

次に、ターミナルで以下を実行する

```bash
mkdir tmp/mysql
touch docker-compose.yml
```

プロジェクトディレクトリ直下に作成された docker-compose.yml に以下を追記して上書き保存

```yml: docker-compose.yml
version: '3.8'
services:
  mysql:
    image: mysql:8.0
    volumes:
      - ./tmp/mysql:/var/lib/mysql
    ports:
      - 4306:3306
    environment:
      - MYSQL_ROOT_PASSWORD=root
```

Docker Desktopを起動し、ターミナルで以下を実行する

```bash
docker compose up -d
```

続いて、ターミナルで以下のコマンドを実行し、パスワードを聞かれたら `root` と入力してエンターキーを押す

```bash
mysql -h 127.0.0.1 -u root -p -P 4306
```

以下のように MySQLとの対話モードに入ることができれば OK

[![Image from Gyazo](https://i.gyazo.com/ee35c258b2d438402772af8ecf08a7da.png)](https://gyazo.com/ee35c258b2d438402772af8ecf08a7da)

ターミナルで `exit` と入力し、MySQLとの対話モードを終了する。

次に、config/database.ymlファイルの該当箇所（12行目付近以降）を以下の変更する

```yml: config/database.yml
default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: root
  password: root
  host: 127.0.0.1
  port: 4306
```

次にターミナルで、以下のコマンドを実行し、以下のように表示されればOK

```bash
bundle exec rails db:create
    Created database 'test_app_development'
    Created database 'test_app_test'
```

ターミナルで以下のコマンドを実行し、Railsのサーバを起動する

```bash
bin/dev
```

ブラウザで以下のURLにアクセスする

```
http://127.0.0.1:3000/
```

以下のように表示されればOK

[![Image from Gyazo](https://i.gyazo.com/8287bd765695ae43d471ad18ca7a7ccb.png)](https://gyazo.com/8287bd765695ae43d471ad18ca7a7ccb)

表示されたら、ターミナルで  Ctr + C を入力してサーバを停止する

後処理として、ターミナルで以下のコマンドを実行し、MySQLコンテナとボリュームを削除

```bash
docker compose down --volumes
```

## 3. 日報管理システム（daily_report_system）の作成

### 3.1 初期設定

ターミナルにて以下のコマンドを実行する

```bash
cd ~/tech_academy
```

次にVSCodeのターミナルで以下を実行する

```bash
mkdir daily_report_system
cd daily_report_system
```

次に asdfを使って、ターミナルで以下のコマンドを実行する

```bash
asdf set ruby 3.2.0
asdf set nodejs lts
asdf set yarn 1.22.22
asdf current
```

以下のようにターミナルに表示されていればOK

```bash
Name            Version         Source                                                  Installed
nodejs          lts             /xxx/tech_academy/test_app/.tool-versions true
ruby            3.2.0           /xxx/tech_academy/test_app/.tool-versions true
yarn            1.22.22         /xxx/tech_academy/test_app/.tool-versions true
```

次に、ターミナルで以下を実行する。test_appディレクトリ直下に Gemfileが生成される

```bash
bundle init
```

Gemfile全体を以下のように上書きし、保存する

```Gemfile
# frozen_string_literal: true

source "https://rubygems.org"

git_source(:github) { |repo_name| "https://github.com/#{repo_name}" }

# gem "rails"
gem 'rails', '7.0.3.1'
```

続いて、ターミナルで以下のコマンドを実行

```bash
bundle install --path vendor/bundle
```

ターミナルで以下のコマンドを実行し、viエディタを起動する

```bash
vi vendor/bundle/ruby/3.2.0/gems/activesupport-7.0.3.1/lib/active_support/logger_thread_safe_level.rb
```

logger_thread_safe_level.rbの 7行目に以下を追記して上書き保存

```rb: logger_thread_safe_level.rb
require "logger"
```

ターミナルで以下を実行し、次のように表示されればOK

```bash
bundle exec rails -v
    Rails 7.0.3.1
```

続いて、ターミナルで以下を実行する

```bash
bundle exec rails new . --database=mysql --css=bootstrap
```

途中で `Y` を入力してエンターキーを押す

次に、ターミナルで以下を実行する

```bash
mkdir tmp/mysql
touch docker-compose.yml
```

プロジェクトディレクトリ直下に作成された docker-compose.yml に以下を追記して上書き保存

```yml: docker-compose.yml
version: '3.8'
services:
  mysql:
    image: mysql:8.0
    volumes:
      - ./tmp/mysql:/var/lib/mysql
    ports:
      - 4306:3306
    environment:
      - MYSQL_ROOT_PASSWORD=root
```

Docker Desktopを起動し、ターミナルで以下を実行する

```bash
docker compose up -d
```

続いて、ターミナルで以下のコマンドを実行し、パスワードを聞かれたら `root` と入力してエンターキーを押す

```bash
mysql -h 127.0.0.1 -u root -p -P 4306
```

以下のように MySQLとの対話モードに入ることができれば OK

[![Image from Gyazo](https://i.gyazo.com/ee35c258b2d438402772af8ecf08a7da.png)](https://gyazo.com/ee35c258b2d438402772af8ecf08a7da)

ターミナルで `exit` と入力し、MySQLとの対話モードを終了する。

次に、config/database.ymlファイルの該当箇所（12行目付近以降）を以下の変更する

```yml: config/database.yml
default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: root
  password: root
  host: <%= ENV.fetch("DB_HOST") { "127.0.0.1" } %>
  port: <%= ENV.fetch("DB_PORT") { 4306 } %>
```

次にGemfileの最後に以下を追記する

```Gemfile
gem 'dotenv-rails', groups: [:development, :test]
```

ターミナルで以下のコマンドを実行する

```bash
bundle install
```

また、ターミナルで以下のコマンドを実行する

```bash
touch .env
```

作成された .envファイルに以下を記述して上書き保存

```.env
DB_HOST=127.0.0.1
DB_PORT=4306
```

次にターミナルで、以下のコマンドを実行し、以下のように表示されればOK

```bash
bundle exec rails db:create
    Created database 'daily_report_system_development'
    Created database 'daily_report_system_test'
```

ターミナルで以下のコマンドを実行し、Railsのサーバを起動する

```bash
bin/dev
```

ブラウザで以下のURLにアクセスする

```
http://127.0.0.1:3000/
```

以下のように表示されればOK

[![Image from Gyazo](https://i.gyazo.com/8287bd765695ae43d471ad18ca7a7ccb.png)](https://gyazo.com/8287bd765695ae43d471ad18ca7a7ccb)

表示されたら、ターミナルで  Ctr + C を入力してサーバを停止

次に、.gitignoreファイルに以下を追記

```.gitignore
/vendor
/.env
```

その上で、Gitにコミットする

```bash
git init
git add .
git commit -m "初期設定完了"
```

この段階で、GitHubに daily_report_system というリポジトリを作成し、pushする。`[GitHubのアカウント名]` の箇所は適宜変更の上実行のこと。

```bash
git remote add origin https://github.com/[GitHubのアカウント名]/daily_report_system.git
git branch -M main
git push -u origin main
```

### 3.2 BootstrapとJavaScriptの適用

ターミナルで以下を実行する

```bash
touch app/assets/stylesheets/custom.scss
```

作成された app/assets/stylesheets/custom.scss に以下を追記して上書き保存

```scss: app/assets/stylesheets/custom.scss
/* カスタム背景や余白、フォントサイズの調整など */
body {
    background-color: #f8f9fa;  /* 薄いグレー背景 */
  }
  
h1 {
  margin-bottom: 20px;
  font-weight: bold;
}

.table {
  background-color: #ffffff;
  border-radius: 5px;
  overflow: hidden;
  box-shadow: 0 2px 5px rgba(0,0,0,0.1);
}

/* 必要に応じて、ボタンやフォームのスタイルも追加 */
nav.navbar.navbar-expand-lg.navbar-light.bg-light {
  background-color: #333 !important;
}

a.nav-link,
button.nav-link.btn.btn-link,
span.nav-link,
a.navbar-brand {
  color: #fff;
}

a.btn.btn-primary,
a.btn.btn-success,
a.btn.btn-secondary {
  margin-bottom: 25px;
}

a.btn.btn-primary.btn-sm {
  margin-bottom: 5px;
}

td {
  vertical-align: middle;
}

li {
  list-style: none;
}

ul.flex {
  display: flex;
  justify-content: start;
  padding-left: 0;
  column-gap: 15px;
}

.reports {
  margin-bottom: 20px;
}

/* デフォルトでハンバーガーメニューを隠す */
.sidebar-menu {
  position: fixed;
  top: 0;
  left: -250px; /* 初期位置（画面外） */
  width: 250px;
  height: 100%;
  background: #ffffff;
  box-shadow: 2px 0 5px rgba(0, 0, 0, 0.2);
  transition: transform 0.3s ease-in-out;
  display: flex;
  flex-direction: column;
  padding: 20px;
  z-index: 1050;
}

/* メニューオープン時 */
.sidebar-menu.open {
  transform: translateX(250px);
}

/* オーバーレイ（メニュー開閉時に背景を暗くする） */
.menu-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  opacity: 0;
  visibility: hidden;
  transition: opacity 0.3s ease-in-out;
  z-index: 1049;
}

button#menu-toggle {
  background-color: darkgray;
}

/* オープン時のオーバーレイ */
.menu-overlay.open {
  opacity: 1;
  visibility: visible;
}

.navbar-nav li:nth-child(5){
  margin-top: 15px;
}

/* 768px 以上では通常のヘッダーメニューを表示 */
@media (min-width: 768px) {
  .sidebar-menu,
  .menu-overlay {
    display: none !important;
  }

  .desktop-menu {
    display: flex !important;
  }

  .menu-toggle {
    display: none !important;
  }
}
```
app/assets/stylesheets/application.bootstrap.scss ファイルの末尾に以下を追記して上書き保存

```scss: app/assets/stylesheets/application.bootstrap.scss
@import 'custom';
```

次に、ターミナルで以下のコマンドを実行

```bash
touch app/javascript/menu.js
```

作成された app/javascript/menu.jsに以下を記述して上書き保存

```js:app/javascript/menu.js
document.addEventListener("DOMContentLoaded", function () {
  const menuToggle = document.getElementById("menu-toggle");
  const sidebarMenu = document.getElementById("sidebar-menu");
  const menuOverlay = document.getElementById("menu-overlay");

  // 768px以上の場合はメニューを開閉しない
  function handleResize() {
    if (window.innerWidth >= 768) {
      sidebarMenu.classList.remove("open");
      menuOverlay.classList.remove("open");
    }
  }

  if (menuToggle && sidebarMenu && menuOverlay) {
    menuToggle.addEventListener("click", function () {
      sidebarMenu.classList.toggle("open");
      menuOverlay.classList.toggle("open");
    });

    menuOverlay.addEventListener("click", function () {
      sidebarMenu.classList.remove("open");
      menuOverlay.classList.remove("open");
    });

    // 画面サイズが変更された際に開閉状態をリセット
    window.addEventListener("resize", handleResize);
  }
});
```

app/javascript/application.jsの末尾に以下の1行を追加し、上書き保存

```js:app/javascript/application.js
import "./menu";
```

app/views/layouts/application.html.erb全体を以下に変更し、上書き保存

```app/views/layouts/application.html.erb
<!DOCTYPE html>
<html>
  <head>
    <title><%= content_for(:title) || "Daily Report System" %></title>
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="mobile-web-app-capable" content="yes">
    <%= csrf_meta_tags %>
    <%= csp_meta_tag %>

    <%= yield :head %>

    <%# Enable PWA manifest for installable apps (make sure to enable in config/routes.rb too!) %>
    <%#= tag.link rel: "manifest", href: pwa_manifest_path(format: :json) %>

    <link rel="icon" href="/icon.png" type="image/png">
    <link rel="icon" href="/icon.svg" type="image/svg+xml">
    <link rel="apple-touch-icon" href="/icon.png">

    <%# Includes all stylesheet files in app/assets/stylesheets %>
    <%= stylesheet_link_tag "application", "data-turbo-track": "reload" %>
    <%= javascript_include_tag "application", "data-turbo-track": "reload", type: "module" %>
  </head>
  <body>
  <% flash.each do |key, message| %>
  <div class="alert alert-<%= key == 'notice' ? 'success' : 'danger' %>">
    <%= message %>
  </div>
  <% end %>
  <header>
  <nav class="navbar navbar-expand-lg navbar-light bg-light">
    <div class="container-fluid">
      <%= link_to "日報管理システム", root_path, class: "navbar-brand" %>
        <!-- 768px 以下でのみ表示されるハンバーガーメニュー -->
        <button class="navbar-toggler menu-toggle" type="button" id="menu-toggle">
          <span class="navbar-toggler-icon"></span>
        </button>

        <!-- 768px 以上で表示される通常メニュー -->
        <div class="collapse navbar-collapse desktop-menu">
          <ul class="navbar-nav me-auto mb-2 mb-lg-0">
            <% if user_signed_in? && current_user.admin? %>
              <li class="nav-item"><%= link_to "ユーザー一覧", admin_users_path, class: "nav-link" %></li>
              <li class="nav-item"><%= link_to "日報一覧", admin_reports_path, class: "nav-link" %></li>
            <% elsif user_signed_in? %>
              <li class="nav-item"><%= link_to "日報一覧", reports_path, class: "nav-link" %></li>
            <% end %>
          </ul>

          <!-- 右側のログイン/ログアウトボタン -->
          <ul class="navbar-nav ms-auto">
            <% if user_signed_in? %>
              <li class="nav-item">
                <span class="nav-link text-light fw-bold"><%= current_user.name %> さん</span>
              </li>
              <li class="nav-item">
                <%= button_to "ログアウト", destroy_user_session_path, method: :delete, data: { turbo: false, confirm: "本当にログアウトしますか？" },
                  class: "btn btn-outline-danger rounded-pill px-4 py-2" %>
              </li>
            <% else %>
              <li class="nav-item">
                <%= link_to "ログイン", new_user_session_path, class: "btn btn-outline-success rounded-pill px-4 py-2" %>
              </li>
            <% end %>
          </ul>
        </div>
      </div>
    </nav>

    <!-- 768px 以下で表示されるハンバーガーメニューのサイドバー -->
    <div class="sidebar-menu" id="sidebar-menu">
      <ul class="navbar-nav">
        <li class="nav-item"><%= link_to "ホーム", root_path, class: "nav-link" %></li>
        <% if user_signed_in? && current_user.admin? %>
          <li class="nav-item"><%= link_to "ユーザー一覧", admin_users_path, class: "nav-link" %></li>
          <li class="nav-item"><%= link_to "日報一覧", admin_reports_path, class: "nav-link" %></li>
        <% elsif user_signed_in? %>
          <li class="nav-item"><%= link_to "日報一覧", reports_path, class: "nav-link" %></li>
        <% end %>

        <% if user_signed_in? %>
          <li class="nav-item">
            <span class="nav-link"><%= current_user.name %>さん</span>
          </li>
          <li class="nav-item">
            <%= button_to "ログアウト", destroy_user_session_path, method: :delete, class: "btn btn-outline-danger rounded-pill px-4 py-2 ms-2" %>
          </li>
        <% else %>
          <li class="nav-item">
            <%= link_to "ログイン", new_user_session_path, class: "btn btn-outline-success rounded-pill px-4 py-2 ms-2" %>
          </li>
        <% end %>
      </ul>
    </div>

    <!-- オーバーレイ -->
    <div class="menu-overlay" id="menu-overlay"></div>
  </header>
    <div class="container mt-4">
      <%= yield %>
    </div>
    <script>
      document.addEventListener("DOMContentLoaded", function() {
      // 5秒後にフラッシュメッセージを非表示にする
      setTimeout(function() {
        var flashMessages = document.querySelectorAll(".alert");
        flashMessages.forEach(function(msg) {
          msg.style.transition = "opacity 0.5s ease-out";
          msg.style.opacity = "0";
          // 0.5秒後に完全に非表示にする
          setTimeout(function() {
            msg.style.display = "none";
          }, 500);
        });
        }, 5000); // 5000ミリ秒 = 5秒
      });
    </script>
  </body>
</html>
```

Gitにコミットし、GitHubにpushする

```bash
git add .
git commit -m "BootstrapとJavaScriptの適用"
git push origin main
```

### 3.3 続きの作業
テキストの `Lesson 0 Chapter 31.8 モデルとマイグレーション 認証機能の実装（Devise）` から `Lesson 0 Chapter 31.13 管理者専用のリソースのコントローラーとビューを作成` 通りに実装していく

以下注意点

- 可能な限りコードの解読を試みる
- テキストのある `rails ...` コマンドは、すべて `bundle exec rails ...` に読み替えて実行する
- サーバの起動には ターミナルで `bin/dev` を実行する
- `routes.rb` を編集したタイミングで `bundle exec rails routes` コマンドを実行し、最新ルーティングを確認する
- `bundle exec rails db:migrate` を実行したタイミングで、ターミナルでMySQLにログインし、SQLの `describe文` でテーブルの構造を確認する
- ブラウザ画面上で、何かの登録、更新、削除処理をした場合は、ターミナルでMySQLにログインし、SQLの `select文` でテーブルのレコードを確認する
- deviseを使った認証に関しては、[Ruby / Rails関連 2023.09.01Rails: Deviseを徹底理解する（1）基礎編（翻訳）](https://techracho.bpsinc.jp/hachi8833/2023_09_01/133452?utm_source=chatgpt.com)などを参考にする。特に devise関係は、便利な半面ブラックボックス化していることも多いの要注意です。
- 各Chapterの終わりでは、Gitにコミットし（コミットメッセージは 「Lesson 0 Chapter 31.12まで実装完了」のような感じでいいです）、GitHubへpushする
- 作業が一通り終わったら、ターミナルで以下のコマンドを実行

```bash
bin/dev
```

そして、ブラウザで以下のURLにアクセス

```
http://127.0.0.1:3000/users/sign_in
```

以下の情報でログイン

```
メールアドレス: admin@example.com
パスワード: password
```

ログインはできるが、ログインした瞬間に以下のエラー発生

[![Image from Gyazo](https://i.gyazo.com/bea040b1afadb52da5779f700130e3b5.png)](https://gyazo.com/bea040b1afadb52da5779f700130e3b5)

`Recordモデルがまだ存在しないので当たり前です`

## 4. おまけ
今後の共同開発において、最初に共有のGitHubからコード一式をcloneしローカルで動かします。

その手順のダイジェクトは以下となります。ターミナルで以下を実行します。

```bash
cd ~/tech_academy/
mv daily_report_system daily_report_system_personal
git clone https://github.com/[共有しているGitHubのアカウント名]/daily_report_system.git daily_report_system
cd daily_report_system/
yarn install
rm Gemfile.lock 
bundle install --path vendor/bundle
vi vendor/bundle/ruby/3.2.0/gems/activesupport-7.0.8.7/lib/active_support/logger_thread_safe_level.rb
# 7行目に require "logger" を追記して上書き保存
bundle exec rails -v
  Rails 7.0.8.7
mkdir tmp/mysql
# Githubのリポジトリ所有者から .env, 及び config/master.key ファイルを共有してもらい、cloneしたプロジェクトにコピーして配置する
bin/dev
```