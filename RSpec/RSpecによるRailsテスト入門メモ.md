### 1.イントロダクション
導入部分のためカット

### 2.RSpecのセットアップ
* .rspec…RSpecインストール時に生成。RSpec設定ファイル
* spec…同上。作成したスペックファイル格納用ディレクトリ
* spec/spec_helper.rb…同上。RSpecの動きをカスタマイズする
* spec/rails_helper.rb…同上。同上。
* .rspecファイルの設定でRSpecの出力形式を変更できる
* gem 'spring-commands-rspec' でSpringを用いてRSpecを高速化できる
* rails generate時に自動生成されるファイルを設定できる  
例えばcontroller_specs: falseでコントローラスペックの生成をスキップできる
* 命名規則に基づいていればスペックの手動追加OK
* 自動生成された不要ファイルの手動削除OK
* RSpecインストールにより、rails generate時にMinitestのファイルはtestディレクトリに自動生成されなくなる  
その代わり、RSpec のファイルが spec ディレクトリに作成される  
testディレクトリは削除しても問題はない
* UI関連（ビュー）のテストは基本的に統合テストで行う

### 3.モデルスペック
* 期待する結果をまとめて記述（describe）する
* example（it で始まる1行）一つにつき、結果を一つだけ期待する  
example が失敗したときに問題を特定しやすくする
* exampleを明示的に書く
* 各 example の説明は動詞で始める。should ではない  
❌「〜が期待した結果と一致すべきだ/すべきでない」  
（something should or should_not match expected output）  
⭕️「私は〜が〜になる/ならないことを期待する」  
（I expect something to or not_to be something else）
* テストする値を expect() メソッドに渡し、それに続けてマッチャを呼び出す
* rails_helperのrequireはテストスイート内のほぼすべてのファイルで必要になる  
ファイル内のテストを実行するためにRailsアプリケーションの読み込みが必要であることを伝えている
* テストを失敗させて期待通りに動いているか確かめる  
・エクスペクテーションを反転させてみる（to を to_not/not_to へ）  
・アプリケーション側のコードを変更してみる
* 正常系のパターンだけでなく、エラーが発生する条件もテストする
* 等値のエクスペクテーションを書くときは == ではなく eq を使う
* RSpec が提供するデフォルトのマッチャを見たい場合は以下READMEを参照  
[https://github.com/rspec/rspec-expectations:title]
* DRYに書くために  
・describeをネストする  
・contextブロックを加えてexampleを切り分ける→同じようなexampleをひとまとめにする  
・describe ではクラスやシステムの機能に関するアウトラインを記述  
・context では特定の状態に関するアウトラインを記述
* beforeブロック  
・ブロックの中に書かれたコードは内側の各テストの実行前に実行される  
・describeやcontextブロックによってスコープが限定される  
・exampleごとに、またはブロック内の各exampleごとに、またはテストスイート全体を実行するごとに実行される  
* before(:each)はdescribeまたはcontextブロック内の各(each)テストの前に実行される  
before(:example) というエイリアスも使える(beforeだけでもOK)  
もしブロック内に4つのテストがあれば、beforeのコードも4回実行される   
* before(:all) はdescribe または context ブロック内の 全(all)テストの前に一回だけ実行される  
before(:context) というエイリアスも使える  
beforeのコードは一回だけ実行され、それから4つのテストが実行される  
* before(:suite) はテストスイート全体の全ファイルを実行する前に実行される
* after  
example の実行後に必要があれば(たとえば外部サービスとの接続を切断する場合など)片付けられる  
beforeと同様、each、all、suiteのオプションがある  
ただしデフォルトでデータベースの後片付けをやってくれるので、使う機会は少ない
