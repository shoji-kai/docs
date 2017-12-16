# tmux

## チートシート

### ウィンドウの操作

Ctrl-b c                          // ウィンドウを作成
Ctrl-b p                          // 前のウィンドウに移動
Ctrl-b n                          // 次のウィンドウに移動
Ctrl-b ,                          // ウィンドウの名前を変更
Ctrl-b 0-9                        // 0-9のウィンドウに移動
Ctrl-b l                          // 直前のウィンドウに移動
Ctrl-b w                          // ウィンドウの一覧を表示し、移動先のウィンドウを選択
Ctrl-b &                          // 現在のウィンドウを削除

### ペインの操作

Ctrl-b "                          // 画面を上下に分割
Ctrl-b %                          // 画面を左右に分割
Ctrl-b x                          // 現在のペインを削除
Ctrl-b q                          // ペインの番号を表示
Ctrl-b o                          // 次のペインに移動
Ctrl-b ;                          // 直前のペインに移動
Ctrl-b ↑↓←→                   // 上下左右のペインに移動
Ctrl-b {}                         // ペインを入れ替え
Ctrl-b Ctrl-↑↓←→              // ペインのサイズを変更

### セッション

Ctrl-b $                          // セッションの名前を変更
Ctrl-b s                          // セッションの一覧をウィンドウの一覧と共にツリー表示
Ctrl-b (                          // 前のセッションに移動
Ctrl-b )                          // 次のセッションに移動
Ctrl-b L                          // 直前にいたセッションに移動
Ctrl-b d                          // デタッチ

### コピーモード

Ctrl-b [                          // コピーモードに入る
Ctrl-b ]                          // 貼り付け

### コマンド

tmux list-commands                // コマンドの一覧表示
tmux list-keys                    // ショートカットの一覧表示
tmux source-file ~/.tmux.conf     // 設定ファイルの読み込み
tmux kill-server                  // tmuxサーバのプロセス停止

### プレフィックスの変更 (e.g. プレフィックスをCtrl-bからCtrl-aに変更する場合)

tmux set -g prefix C-a            // プレフィックスキーをCtlr-aに変更
tmux unbind C-b                   // Ctrl-bのキーバインドを解除
tmux binc C-a send-prefix         // Ctrl-aを2回連続して押した場合、ターミナルにCtrl-aを送る

### ショートカットキーの変更 (e.g. last-windowコマンドのショートカットをlからaに変更する)

$ tmux list-keys -T prefix | grep last-window     // last-windowコマンドにどのようなショートカットキーが設定されているか調べる
bind-key    -T prefix l       last-window
$ tmux unbind l                                   // lのショートカットキーを解除する
$ tmux bind a last-window                         // 新しいショートカットキーをaに設定する

### ショートカットキーの種類

1. prefix         // プレフィックスキーを押した後に押すことで実行できるショートカットキーの設定
2. root           // プレフィックスキーなしですぐに実行できるショートカットキーの設定
3. copy-mode      // コピーモード時に実行できるショートカットキーの設定。mode-keysオプションがemacsの場合の設定
4. copy-mode-vi   // copy-modeと同じ。こちらはmode-keysオプションがviの場合の設定

### source-fileコマンドのショートカットキーにRを割り当て、読み込んだらメッセージを表示する

bind R source-file ~/.tmux.conf \; display-message 'Reloaded config!'

### tmuxのコマンドで使える変数

1. #{window_name}   #W    ウィンドウの名前
2. #{window_index}  #I    ウィンドウの番号
3. #{pane_index}    #P    ペインの番号
4. #{session_name}  #S    セッションの名前
5. #{host}          #H    ホスト名
