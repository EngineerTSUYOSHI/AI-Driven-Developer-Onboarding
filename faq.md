# よくある質問（FAQ）

このFAQは、プログラミング未経験・微経験の方が、GitHub、AI駆動開発、GitHub Actions を学ぶ中で、実際に躓いた点・調べた内容・共有したい内容をまとめたものです。
エラーメッセージは、検索や照合がしやすいように、原文を載せています。

## 1. GitHub と Git の基本

### Q. GitHub をインストールするには、どうすればよいですか？
A. GitHub は、インターネット上のサービス（Webサービス）なので、PCにインストールするものではありません。Windows では、次の2つを入れて使います。
1. Git（必須）: https://git-scm.com/download/win からインストーラーをダウンロードし、基本は初期設定のまま「Next」で進めます。
2. GitHub Desktop（任意）: コマンドが不安な場合に、ボタン操作で使えます。

インストール後に、PowerShell で `git --version` を実行して、バージョンが表示されれば成功です。初回だけ、次の2つを設定します。
- `git config --global user.name "あなたの名前"`
- `git config --global user.email "GitHubに登録したメールアドレス"`

### Q. 「クローン（clone）」とは、何ですか？
A. GitHub 上にあるリポジトリを、自分のPCに、丸ごとコピーすることです。`git clone リポジトリのURL` で行います。クローンすると、GitHub とのつながりが保たれるので、変更を `git push` で送り返せます。ZIP でダウンロードした場合は、このつながりがありません。

### Q. `fatal: not a git repository (or any of the parent directories): .git` と表示されます。
A. Git のリポジトリではないフォルダで、コマンドを実行しています。PowerShell の「PS」のあとに表示されているのが、現在の場所です。`cd C:\Users\あなたの名前\Documents\リポジトリ名` で、リポジトリのフォルダに移動してから、もう一度実行してください。
`PS C:\Windows\system32>` から始まっている場合は、PowerShell を「管理者として実行」で開いています。通常の権限で開き直してください。

### Q. GitHub 上で作ったファイルが、PC のフォルダにありません。
A. GitHub 上の変更は、自動では PC に届きません。リポジトリのフォルダで `git pull` を実行すると、PC に取り込めます。逆に、PC で変えた内容を GitHub に送るのは `git push` です。

### Q. `git commit` で `nothing to commit, working tree clean` と表示されます。
A. 前回のコミット以降に、変更されたファイルがない、という意味で、エラーではありません。GitHub 上で作った、または編集したファイルを、すでに `git pull` で取り込んでいる場合にも、この表示になります。

## 2. 環境構築

### Q. 開発に必要なソフトは、何ですか？
A. 次のとおりです。
- Git
- Python（最新版）
- AI エディタ（Antigravity、Cursor など）、または VS Code

注釈: 本カリキュラムでは、一定の範囲までは無料で利用できる Antigravity IDE を推奨しています。

### Q. PowerShell で `npm` を実行すると、「このシステムではスクリプトの実行が無効になっているため…」と表示されます。
A. Windows の PowerShell の標準の制限が原因です。次を実行し、確認で `Y` を入力すると、使えるようになります。
`Set-ExecutionPolicy -Scope CurrentUser RemoteSigned`
自分のユーザーだけに適用され、管理者権限は不要です。コマンドプロンプトで実行する方法もあります。

### Q. Python のインストール後に、`python` が認識されません。
A. インストール直後は、PowerShell を、いったん閉じて、開き直してください。公式のインストールマネージャーを使う場合は、途中で y/n を聞かれます。内容を読み、Python 本体のインストールや、`python` コマンドを使えるようにする設定は `y` で進めます。

### Q. 手順書のコードブロックは、すべて PowerShell に入力してよいですか？
A. いいえ。手順書のコードブロックには、3種類あります。
1. 入力するコマンド（`git add .`、`cd ...`、`pip install ...` など）
2. ファイルの中に書く内容（`requirements.txt` の中の `google-genai` など）
3. 画面に表示される結果の例（`new file: ...` など）

2や3を PowerShell に入力すると、「`google-genai` は、コマンドレットとして認識されません」のようなエラーになります。何が書かれているかを見分けてから、入力してください。

### Q. VS Code で新しいファイルを作ったら、意図しない場所（`scripts/.github/...`）に作られました。
A. 「新しいファイル」は、選択しているフォルダの中に作られます。GitHub Actions の設定ファイルは、リポジトリの一番上の `.github/workflows/` に置かないと、読み込まれません。ファイル一覧の空いている場所をクリックしてから作る、または、作ったあとに `git status -u` で、場所を確認してください。

## 3. AI エディタ（Antigravity など）

### Q. Antigravity で、フォルダを開く（Open Folder）メニューが見つかりません。
A. 新しい版では、File メニューが「New Conversation」「Create Project」「Command Palette」のみの場合があります。「Create Project」で作られたフォルダが、クローン済みのフォルダとは別の場所になることがあります。うまく開けない場合は、PowerShell でリポジトリのフォルダに移動し、`code .` で、VS Code で開いて進めても構いません。

### Q. Antigravity が「Waiting for user input」のまま、進みません。
A. 次の順に試します。
1. チャット欄に、「Allow」「Run」「Confirm」などの許可ボタンが出ていないか確認する。
2. チャットに「続けてください」と入力して送信する。
3. Cancel で止めて、新しい会話でやり直す。
4. Ctrl + Shift + P →「Developer: Reload Window」を実行する。

## 4. チーム開発のルール（ブランチ・PR・レビュー）

### Q. main ブランチに、直接コミット・push してもよいですか？
A. 非推奨です。通常、main ブランチへの直接のコミットは許可されていません。作業用のブランチを切って作業し、そのブランチ内の修正内容の承認が下りてから、main にマージします。

### Q. プルリクエスト（PR）には、何を書けばよいですか？
A. 基本は、次の3点です。
1. 何を変更したか
2. なぜ変更したか
3. 確認方法

関連する Issue がある場合は、`Closes #番号` と書くと、マージ時に Issue が自動で閉じます。
プロジェクトによって変わります。チケット管理ツールがある場合は、チケット番号、リリース日、テスト結果などのエビデンス資料の URL なども書きます。

### Q. レビューは、誰に依頼しますか？
A. プロジェクトによって変わりますが、主に、リーダーやプロパー社員に依頼します。近年は、AI によるレビューも増えています。

### Q. プルリクエストに、「Review required」「Merging is blocked」と表示され、マージできません。
A. 「書き込み権限のある人の承認が、1件以上必要」というルールがあるためです。自分で出した PR は、自分では承認できません。右側の「Reviewers」でレビュアーを指定して、承認を待ちます。承認後に、マージできます。

### Q. 「Compare & pull request」が表示されません。
A. 上の「Pull requests」タブ →「New pull request」から作成できます。compare を、作業用のブランチにします。

## 5. GitHub Actions と AI（Gemini）

### Q. GitHub Actions のチェックが、赤い × で失敗します。どう調べますか？
A. 「Actions」タブで、失敗した実行を開き、赤い × が付いたステップを開いて、`Error` の前後のログを読みます。
例: `Check required files` のステップが失敗した場合は、CI の設定で確認しているファイル名（`script.js`）と、実際のファイル名（`app.js`）が食い違っていました。設定ファイルを、実際のファイル名に合わせると、直ります。

### Q. Gemini API で、`429`、`You exceeded your current quota` と表示されます。
A. 無料枠の上限に達しています。上限は、1分あたりと、1日あたりの2種類があり、モデルごとに別々に数えられます。メッセージの `retry in ...` の時間を待つ、または、時間をおいて再実行します。上限は、Google AI Studio のレート制限のページで確認できます。

### Q. Gemini の設定で、`unexpected model name format`（400）と表示されます。
A. モデル名に、画面に表示される名前（例: `Gemini 3.5 Flash Lite`）を入れています。設定には、モデルID（例: `gemini-3.5-flash-lite`）を入れます。小文字・ハイフンつなぎ・スペースなしの形式です。公式ドキュメントのモデルのページの「Model code」で、確認できます。

### Q. Gemini CLI で、「Sign in with Google」が失敗します。
A. 個人向けの Google ログインは、使えなくなっている場合があります。認証方法の画面で、「Use Gemini API Key」を選び、Google AI Studio で取得したキーを使います。

### Q. API キーは、どのように扱えばよいですか？
A. コード、チャット、スクリーンショットに、書かない・貼らないでください。GitHub では、リポジトリの「Settings」→「Secrets and variables」→「Actions」に、名前を付けて登録し、ワークフローから `${{ secrets.名前 }}` で参照します。公開リポジトリでは、Actions のログも、誰でも読めます。キーが漏れた場合は、すぐに無効化して、作り直してください。

### Q. GitHub Actions で、Gemini のレビューが、7分でキャンセルされます。
A. 拡張機能のインストール時に、確認（consent）の入力待ちになり、GitHub Actions では、人が答えられないため、時間切れになることがありました。`gemini extensions install ... --consent` のように、確認を省略するオプションで、先にインストールすると、避けられます。

### Q. Actions のログに、`$GITHUB_STEP_SUMMARY upload aborted, supports content up to a size of 1024k` と表示されます。
A. デバッグログ（`GEMINI_DEBUG=true`）を有効にしたまま、ログが 1MB の上限を超えています。調査が終わったら、Variables から `GEMINI_DEBUG` を削除してください。