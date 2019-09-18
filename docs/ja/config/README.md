# 設定

::: tip 🔥Starshipの開発は現在も進んでいます。 多くの新しいオプションが今後のリリースで利用可能になります。 :::

Starshipの設定を開始するには、`~/.config/starship.toml` ファイルを作成します。

```shell
$ touch ~/.config/starship.toml
```

Starshipのすべての設定は、この[TOML](https://github.com/toml-lang/toml)ファイルで行われます。

```toml
# Don't print a new line at the start of the prompt
add_newline = false

# Replace the "❯" symbol in the prompt with "➜"
[character]      # The name of the module we are configuring is "character"
symbol = "➜"     # The "symbol" segment is being set to "➜"

# Disable the package module, hiding it from the prompt completely
[package]
disabled = true
```

### 用語

**モジュール**: OSのコンテキスト情報に基づいて情報を提供するプロンプト内のコンポーネントです。 たとえば、現在のディレクトリがNodeJSプロジェクトである場合、「nodejs」モジュールは、現在コンピューターにインストールされているNodeJSのバージョンを表示します。

**セグメント**: モジュールを構成する小さなサブコンポーネントです。 たとえば、「nodejs」モジュールの「symbol」セグメントには、バージョン番号の前に表示される文字が含まれています（デフォルト: ⬢）。

以下はNode モジュールの表現です。 次の例では、「シンボル」と「バージョン」はその中のセグメントです。 すべてのモジュールには、デフォルトの端末色であるprefixとsuffixもあります。

    [prefix]      [symbol]     [version]    [suffix]
     "via "         "⬢"        "v10.4.1"       ""
    

### スタイルの設定

Starshipのほとんどのモジュールでは、表示スタイルを設定できます。 これは、設定を指定する文字列であるエントリ（`style`）で行われます。 スタイル文字列の例とその機能を次に示します。 完全な構文の詳細については、詳細は [高度な設定](/advanced-config/)を参照してください 。

- `"fg:green bg:blue"` は、青色の背景に緑色のテキストを設定します
- `"bg:blue fg:bright-green"` は、青色の背景に明るい緑色のテキストを設定します
- `"bold fg:27"` は、 [ANSIカラー](https://i.stack.imgur.com/KTSQa.png) 27の太字テキストを設定します
- `"underline bg:#bf5700"` は、焦げたオレンジ色の背景に下線付きのテキストを設定します
- `""` はすべてのスタイルを明示的に無効にします

スタイリングがどのように見えるかは、端末エミュレータによって制御されることに注意してください。 たとえば、一部の端末エミュレータはテキストを太字にする代わりに色を明るくします。また、一部のカラーテーマは通常の色と明るい色と同じ値を使用します。また、斜体のテキストを取得するには、端末で斜体をサポートする必要があります。スタイリングがどのように見えるかは、端末エミュレータによって制御されることに注意してください。たとえば、一部の端末エミュレータはテキストを太字にする代わりに色を明るくします。また、一部のカラーテーマは通常の色と明るい色と同じ値を使用します。

## プロンプト

これは、プロンプト全体のオプションのリストです。

### オプション

| 変数             | デフォルト                         | 説明                       |
| -------------- | ----------------------------- | ------------------------ |
| `add_newline`  | `true`                        | プロンプトの開始前に新しい行を追加します。    |
| `prompt_order` | [link](#default-prompt-order) | プロンプトモジュールを出力する順序を設定します。 |


### 設定例

```toml
# ~/.config/starship.toml

# プロンプト表示の改行を無効にする
add_newline = false
# デフォルトのプロンプト表示順を書き換える
prompt_order=["rust","line_break","package","line_break","character"]
```

### デフォルトのプロンプト表示順

`default_prompt_order``オプションは、空または`prompt_order`が指定されていない場合に、プロンプトにモジュールが表示される順序を定義するために使用されます。 デフォルトは次のとおりです。

    default_prompt_order = [
        "username",
        "hostname",
        "directory",
        "git_branch",
        "git_state",
        "git_status",
        "package",
        "nodejs",
        "ruby",
        "rust",
        "python",
        "golang",
        "nix_shell",
        "cmd_duration",
        "line_break",
        "jobs",
        "battery",
        "character",
    ]
    

## バッテリー

`battery`モジュールは、デバイスのバッテリー残量と現在の充電状態を示します。 モジュールは、デバイスのバッテリー残量が10％未満の場合にのみ表示されます。

### オプション

| 変数                   | デフォルト        | 説明                        |
| -------------------- | ------------ | ------------------------- |
| `full_symbol`        | `"•"`        | バッテリーが満タンのときに表示される記号です。   |
| `charging_symbol`    | `"⇡"`        | バッテリーの充電中に表示される記号です。      |
| `discharging_symbol` | `"⇣"`        | バッテリーが放電しているときに表示される記号です。 |
| `style`              | `"bold red"` | モジュールのスタイルです。             |
| `disabled`           | `false`      | `battery`モジュールを無効にします。    |


### 設定例

```toml
# ~/.config/starship.toml

[battery]
full_symbol = "🔋"
charging_symbol = "⚡️"
discharging_symbol = "💀"
```

## 文字

`character`モジュールは、端末でテキストが入力される場所の横に文字（通常は矢印）を表示します。

文字は、最後のコマンドが成功したかどうかを示します。 これは、色の変更（赤/緑）またはその形状の変更(❯/✖) の2つの方法で行うことができます。 後者は`use_symbol_for_status`に`true`設定されている場合にのみ行われます。

### オプション

| 変数                      | デフォルト          | 説明                                           |
| ----------------------- | -------------- | -------------------------------------------- |
| `symbol`                | `"❯"`          | プロンプトでテキストを入力する前に使用される記号です。                  |
| `error_symbol`          | `"✖"`          | 前のコマンドが失敗した場合にテキスト入力の前に使用される記号です。            |
| `use_symbol_for_status` | `false`        | シンボルを変更してエラーステータスを示します。                      |
| `vicmd_symbol`          | `"❮"`          | シェルがvimの通常モードである場合、プロンプトのテキスト入力の前に使用される記号です。 |
| `style_success`         | `"bold green"` | 最後のコマンドが成功した場合に使用されるスタイルです。                  |
| `style_failure`         | `"bold red"`   | 最後のコマンドが失敗した場合に使用されるスタイルです。                  |
| `disabled`              | `false`        | `character`モジュールを無効にします。                     |


### 設定例

```toml
# ~/.config/starship.toml

[character]
symbol = "➜"
error_symbol = "✗"
use_symbol_for_status = true
```

## コマンド実行時間

The `cmd_duration` module shows how long the last command took to execute. The module will be shown only if the command took longer than two seconds, or the `min_time` config value, if it exists.

::: warning Do not hook the DEBUG trap in Bash If you are running Starship in `bash`, do not hook the `DEBUG` trap after running `eval $(starship init $0)`, or this module **will** break. :::

Bash users who need preexec-like functionality can use [rcaloras's bash_preexec framework](https://github.com/rcaloras/bash-preexec). Simply define the arrays `preexec_functions` and `precmd_functions` before running `eval $(starship init $0)`, and then proceed as normal.

### オプション

| 変数         | デフォルト           | 説明                                  |
| ---------- | --------------- | ----------------------------------- |
| `min_time` | `2`             | 時間を表示する最短期間です。                      |
| `style`    | `"bold yellow"` | モジュールのスタイルです。                       |
| `disabled` | `false`         | Disables the `cmd_duration` module. |


### 設定例

```toml
# ~/.config/starship.toml

[cmd_duration]
min_time = 4
```

## ディレクトリ

The `directory` module shows the path to your current directory, truncated to three parent folders. Your directory will also be truncated to the root of the git repo that you're currently in.

When using the fish style pwd option, instead of hiding the path that is truncated, you will see a shortened name of each directory based on the number you enable for the option.

For example, given `~/Dev/Nix/nixpkgs/pkgs` where `nixpkgs` is the repo root, and the option set to `1`. You will now see `~/D/N/nixpkgs/pkgs`, whereas before it would have been `nixpkgs/pkgs`.

### オプション

| 変数                          | デフォルト         | 説明                                     |
| --------------------------- | ------------- | -------------------------------------- |
| `truncation_length`         | `3`           | 現在のディレクトリを切り捨てる親フォルダーの数です。             |
| `truncate_to_repo`          | `true`        | 現在いるgitリポジトリのルートに切り捨てるかどうかです。          |
| `fish_style_pwd_dir_length` | `0`           | fish shellのpwdパスロジックを適用するときに使用する文字数です。 |
| `style`                     | `"bold cyan"` | モジュールのスタイルです。                          |
| `disabled`                  | `false`       | Disables the `directory` module.       |


### 設定例

```toml
# ~/.config/starship.toml

[directory]
truncation_length = 8
```

## Git ブランチ

The `git_branch` module shows the active branch of the repo in your current directory.

### オプション

| 変数                  | デフォルト           | 説明                                                                                    |
| ------------------- | --------------- | ------------------------------------------------------------------------------------- |
| `symbol`            | `" "`          | 現在のディレクトリのリポジトリのブランチ名の前に使用されるシンボルです。                                                  |
| `truncation_length` | `2^63 - 1`      | gitブランチをX書記素に切り捨てます                                                                   |
| `truncation_symbol` | `"…"`           | The symbol used to indicate a branch name was truncated. You can use "" for no symbol |
| `style`             | `"bold purple"` | モジュールのスタイルです。                                                                         |
| `disabled`          | `false`         | Disables the `git_branch` module.                                                     |


### 設定例

```toml
# ~/.config/starship.toml

[git_branch]
symbol = "🌱 "
truncation_length = "4"
truncation_symbol = ""
```

## Git の進行状態

The `git_state` module will show in directories which are part of a git repository, and where there is an operation in progress, such as: *REBASING*, *BISECTING*, etc. If there is progress information (e.g., REBASING 3/10), that information will be shown too.

### オプション

| 変数                 | デフォルト              | 説明                                                                                                               |
| ------------------ | ------------------ | ---------------------------------------------------------------------------------------------------------------- |
| `rebase`           | `"REBASING"`       | The text displayed when a `rebase` is in progress.                                                               |
| `merge`            | `"MERGING"`        | The text displayed when a `merge` is in progress.                                                                |
| `revert`           | `"REVERTING"`      | The text displayed when a `revert` is in progress.                                                               |
| `cherry_pick`      | `"CHERRY-PICKING"` | The text displayed when a `cherry-pick` is in progress.                                                          |
| `bisect`           | `"BISECTING"`      | The text displayed when a `bisect` is in progress.                                                               |
| `am`               | `"AM"`             | The text displayed when an `apply-mailbox` (`git am`) is in progress.                                            |
| `am_or_rebase`     | `"AM/REBASE"`      | The text displayed when an ambiguous `apply-mailbox` or `rebase` is in progress.                                 |
| `progress_divider` | `"/"`              | The symbol or text which will separate the current and total progress amounts. (e.g., `" of "`, for `"3 of 10"`) |
| `style`            | `"bold yellow"`    | モジュールのスタイルです。                                                                                                    |
| `disabled`         | `false`            | Disables the `git_state` module.                                                                                 |


### 設定例

```toml
# ~/.config/starship.toml

[git_state]
progress_divider = " of "
cherry_pick = "🍒 PICKING"
```

## Git の状態

The `git_status` module shows symbols representing the state of the repo in your current directory.

### オプション

| 変数                | デフォルト        | 説明                                |
| ----------------- | ------------ | --------------------------------- |
| `conflicted`      | `"="`        | このブランチにはマージの競合があります。              |
| `ahead`           | `"⇡"`        | このブランチは、追跡されるブランチよりも先にあります。       |
| `behind`          | `"⇣"`        | このブランチは、追跡されているブランチの背後にあります。      |
| `diverged`        | `"⇕"`        | このブランチは、追跡されているブランチから分岐しています。     |
| `untracked`       | `"?"`        | 作業ディレクトリに追跡されていないファイルがあります。       |
| `stashed`         | `"$"`        | ローカルリポジトリ用のスタッシュが存在します。           |
| `modified`        | `"!"`        | 作業ディレクトリにファイルの変更があります。            |
| `staged`          | `"+"`        | 新しいファイルがステージング領域に追加されました。         |
| `renamed`         | `"»"`        | 名前が変更されたファイルがステージング領域に追加されました。    |
| `deleted`         | `"✘"`        | ファイルの削除がステージング領域に追加されました。         |
| `show_sync_count` | `false`      | 追跡されているブランチの先行/後方カウントを表示します。      |
| `style`           | `"bold red"` | モジュールのスタイルです。                     |
| `disabled`        | `false`      | Disables the `git_status` module. |


### 設定例

```toml
# ~/.config/starship.toml

[git_status]
conflicted = "🏳"
ahead = "🏎💨"
behind = "😰"
diverged = "😵"
untracked = "🤷‍"
stashed = "📦"
modified = "📝"
staged = "➕"
renamed = "👅"
deleted = "🗑"
```

## Golang

The `golang` module shows the currently installed version of Golang. 次の条件のいずれかが満たされると、モジュールが表示されます。

- The current directory contains a `go.mod` file
- The current directory contains a `go.sum` file
- The current directory contains a `glide.yaml` file
- The current directory contains a `Gopkg.yml` file
- The current directory contains a `Gopkg.lock` file
- The current directory contains a `Godeps` directory
- The current directory contains a file with the `.go` extension

### オプション

| 変数         | デフォルト         | 説明                            |
| ---------- | ------------- | ----------------------------- |
| `symbol`   | `"🐹 "`        | Golangのバージョンを表示する前に使用される記号です。 |
| `style`    | `"bold cyan"` | モジュールのスタイルです。                 |
| `disabled` | `false`       | Disables the `golang` module. |


### 設定例

```toml
# ~/.config/starship.toml

[golang]
symbol = "🏎💨 "
```

## ホスト名

The `hostname` module shows the system hostname.

### オプション

| 変数         | デフォルト                 | 説明                               |
| ---------- | --------------------- | -------------------------------- |
| `ssh_only` | `true`                | SSHセッションに接続されている場合にのみホスト名を表示します。 |
| `prefix`   | `""`                  | ホスト名の直前に表示するprefixです。            |
| `suffix`   | `""`                  | ホスト名の直後に表示するsuffixです。            |
| `style`    | `"bold dimmed green"` | モジュールのスタイルです。                    |
| `disabled` | `false`               | Disables the `hostname` module.  |


### 設定例

```toml
# ~/.config/starship.toml

[hostname]
ssh_only = false
prefix = "⟪"
suffix = "⟫"
disabled = false
```

## ジョブ

The `jobs` module shows the current number of jobs running. The module will be shown only if there are background jobs running. The module will show the number of jobs running if there is more than 1 job, or more than the `threshold` config value, if it exists.

### オプション

| 変数          | デフォルト         | 説明                          |
| ----------- | ------------- | --------------------------- |
| `symbol`    | `"✦ "`        | ジョブの数を表示する前に使用される記号です。      |
| `threshold` | `1`           | 超過した場合、ジョブの数を表示します。         |
| `style`     | `"bold blue"` | モジュールのスタイルです。               |
| `disabled`  | `false`       | Disables the `jobs` module. |


### 設定例

```toml
# ~/.config/starship.toml

[jobs]
symbol = "+ "
threshold = 4
```

## 改行

The `line_break` module separates the prompt into two lines.

### オプション

| 変数         | デフォルト   | 説明                                                                 |
| ---------- | ------- | ------------------------------------------------------------------ |
| `disabled` | `false` | Disables the `line_break` module, making the prompt a single line. |


### 設定例

```toml
# ~/.config/starship.toml

[line_break]
disabled = true
```

## Nix-shell

The `nix_shell` module shows the nix-shell environment. The module will be shown when inside a nix-shell environment.

### オプション

| 変数           | デフォルト        | 説明                               |
| ------------ | ------------ | -------------------------------- |
| `use_name`   | `false`      | nix-shellの名前を表示します。              |
| `impure_msg` | `impure`     | impureメッセージをカスタマイズします。           |
| `pure_msg`   | `pure`       | pureメッセージをカスタマイズします。             |
| `style`      | `"bold red"` | モジュールのスタイルです。                    |
| `disabled`   | `false`      | Disables the `nix_shell` module. |


### 設定例

```toml
# ~/.config/starship.toml

[nix_shell]
disabled = true
use_name = true
impure_msg = "impure shell"
pure_msg = "pure shell"
```

## NodeJS

The `nodejs` module shows the currently installed version of NodeJS. 次の条件のいずれかが満たされると、モジュールが表示されます。

- The current directory contains a `package.json` file
- The current directory contains a `node_modules` directory
- The current directory contains a file with the `.js` extension

### オプション

| 変数         | デフォルト          | 説明                            |
| ---------- | -------------- | ----------------------------- |
| `symbol`   | `"⬢ "`         | NodeJSのバージョンを表示する前に使用される記号です。 |
| `style`    | `"bold green"` | モジュールのスタイルです。                 |
| `disabled` | `false`        | Disables the `nodejs` module. |


### 設定例

```toml
# ~/.config/starship.toml

[nodejs]
symbol = "🤖 "
```

## パッケージのバージョン

The `package` module is shown when the current directory is the repository for a package, and shows its current version. The module currently supports `npm`, `cargo`, and `poetry` packages.

- **npm** – The `npm` package version is extracted from the `package.json` present in the current directory
- **cargo** – The `cargo` package version is extracted from the `Cargo.toml` present in the current directory
- **poetry** – The `poetry` package version is extracted from the `pyproject.toml` present in the current directory

> ⚠️ The version being shown is that of the package whose source code is in your current directory, not your package manager.

### オプション

| 変数         | デフォルト        | 説明                             |
| ---------- | ------------ | ------------------------------ |
| `symbol`   | `"📦 "`       | パッケージのバージョンを表示する前に使用される記号です。   |
| `style`    | `"bold red"` | モジュールのスタイルです。                  |
| `disabled` | `false`      | Disables the `package` module. |


### 設定例

```toml
# ~/.config/starship.toml

[package]
symbol = "🎁 "
```

## Python

The `python` module shows the currently installed version of Python.

If `pyenv_version_name` is set to `true`, it will display the pyenv version name.

Otherwise, it will display the version number from `python --version` and show the current Python virtual environment if one is activated.

次の条件のいずれかが満たされると、モジュールが表示されます。

- The current directory contains a `.python-version` file
- The current directory contains a `requirements.txt` file
- The current directory contains a `pyproject.toml` file
- The current directory contains a file with the `.py` extension
- The current directory contains a `Pipfile` file

### オプション

| 変数                   | デフォルト           | 説明                                                                          |
| -------------------- | --------------- | --------------------------------------------------------------------------- |
| `symbol`             | `"🐍 "`          | Pythonのバージョンを表示する前に使用される記号です。                                               |
| `pyenv_version_name` | `false`         | pyenvを使用してPythonバージョンを取得します                                                 |
| `pyenv_prefix`       | `"pyenv "`      | Prefix before pyenv version display (default display is `pyenv MY_VERSION`) |
| `style`              | `"bold yellow"` | モジュールのスタイルです。                                                               |
| `disabled`           | `false`         | Disables the `python` module.                                               |


### 設定例

```toml
# ~/.config/starship.toml

[python]
symbol = "👾 "
pyenv_version_name = true
pyenv_prefix = "foo "
```

## Ruby

The `ruby` module shows the currently installed version of Ruby. 次の条件のいずれかが満たされると、モジュールが表示されます。

- The current directory contains a `Gemfile` file
- The current directory contains a `.rb` file

### オプション

| 変数         | デフォルト        | 説明                          |
| ---------- | ------------ | --------------------------- |
| `symbol`   | `"💎 "`       | Rubyのバージョンを表示する前に使用される記号です。 |
| `style`    | `"bold red"` | モジュールのスタイルです。               |
| `disabled` | `false`      | Disables the `ruby` module. |


### 設定例

```toml
# ~/.config/starship.toml

[ruby]
symbol = "🔺 "
```

## Rust

The `rust` module shows the currently installed version of Rust. 次の条件のいずれかが満たされると、モジュールが表示されます。

- The current directory contains a `Cargo.toml` file
- The current directory contains a file with the `.rs` extension

### オプション

| 変数         | デフォルト        | 説明                          |
| ---------- | ------------ | --------------------------- |
| `symbol`   | `"🦀 "`       | Rustのバージョンを表示する前に使用される記号です。 |
| `style`    | `"bold red"` | モジュールのスタイルです。               |
| `disabled` | `false`      | Disables the `rust` module. |


### 設定例

```toml
# ~/.config/starship.toml

[rust]
symbol = "⚙️ "
```

## ユーザ名

The `username` module shows active user's username. 次の条件のいずれかが満たされると、モジュールが表示されます。

- カレントユーザーがroot
- カレントユーザーが、ログインしているユーザーとは異なる
- ユーザーがSSHセッションとして接続されている

### オプション

| 変数           | デフォルト           | 説明                              |
| ------------ | --------------- | ------------------------------- |
| `style_root` | `"bold red"`    | ユーザーがrootのときに使用されるスタイルです。       |
| `style_user` | `"bold yellow"` | 非rootユーザーに使用されるスタイルです。          |
| `disabled`   | `false`         | Disables the `username` module. |


### 設定例

```toml
# ~/.config/starship.toml

[username]
disabled = true
```