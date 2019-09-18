# 高度な設定

Starship は汎用性の高いシェルですが、時には特定の処理を行うために `starship.toml` を編集する以上のことをする必要があります。 このページでは starship で使用される、より高度な設定の一部を詳しく説明していきます。

::: warning ここに載せられた設定は、Starship の将来のリリースで変更される可能性があります。 :::

## Bashのカスタムの事前プロンプトおよび事前実行コマンド

Bashには、他のほとんどのシェルとは違い、正式な preexec / precmd フレームワークを持っていません。 そのため、 `bash`で完全にカスタマイズ可能なフックを提供することは困難です。 ただし、Starship はプロンプトを描画する一連の流れに、限定的に独自の関数を挿入することができます。

- 関数をプロンプトが描画される直前に実行するためには、新しい関数を定義して `starship_precmd_user_func` に割り当ててください。 例として、ロケットをプロンプトの前に表示させたければ、

```bash
function blastoff(){
    echo "🚀"
}
starship_precmd_user_func="blastoff"
```

- コマンドの直前に関数を実行するために、[`DEBUG` トラップの仕組み](https://jichu4n.com/posts/debug-trap-and-prompt_command-in-bash/)を使うことができます。 しかし、Starship を初期化する前に DEBUG シグナルをトラップ*しなければいけません*！ Starship は DEBUGトラップの値を保護できますが、 starship の起動後にトラップが上書きされると、いくつかの機能は壊れてしまうでしょう。

```bash
function blastoff(){
    echo "🚀"
}
trap blastoff DEBUG     # Trap DEBUG *before* running starship
eval $(starship init bash)
```

## ウィンドウのタイトルの変更

いくつかのシェルプロンプトはあなたのためにウィンドウのタイトルを自動的に変更します（例えば、カレントディレクトリを反映するために）。 特に Fish はデフォルトで変更を行います。 Starship はこれをしませんが、この機能を `bash` や `zsh` に追加することは簡単にできます。

まず、ウィンドウのタイトルを変更する関数を定義してください（ bash も zsh も同様に）

```bash
function set_win_title(){
    echo -ne "\033]0; YOUR_WINDOW_TITLE_HERE \007"
}
```

You can use variables to customize this title (`$USER`, `$HOSTNAME`, and `$PWD` are popular choices).

In `bash`, set this function to be the precmd starship function:

```bash
starship_precmd_user_func="set_win_title"
```

In `zsh`, add this to the `precmd_functions` array:

```bash
precmd_functions+=(set_win_title)
```

If you like the result, add these lines to your shell configuration file (`~/.bashrc` or `~/.zsrhc`) to make it permanent.

## スタイルの設定

Style strings are a list of words, separated by whitespace. The words are not case sensitive (i.e. `bold` and `BoLd` are considered the same string). Each word can be one of the following:

- `bold`
- `underline`
- `dimmed`
- `bg:<color>`
- `fg:<color>`
- `<color>`
- `none`

where `<color>` is a color specifier (discussed below). `fg:<color>` and `<color>` currently do the same thing , though this may change in the future. The order of words in the string does not matter.

The `none` token overrides all other tokens in a string, so that e.g. `fg:red none fg:blue` will still create a string with no styling. It may become an error to use `none` in conjunction with other tokens in the future.

色は以下のいずれか1つを指定できます。

- One of the standard terminal colors: `black`, `red`, `green`, `blue`, `yellow`, `purple`, `cyan`, `white`. You can optionally prefix these with `bright-` to get the bright version (e.g. `bright-white`).
- A `#` followed by a six-digit hexadecimal number. This specifies an [RGB color hex code](https://www.w3schools.com/colors/colors_hexadecimal.asp).
- A number between 0-255. This specifies an [8-bit ANSI Color Code](https://i.stack.imgur.com/KTSQa.png).

複数の色が文字色/背景色に指定された際には、最後の指定が優先して選ばれます。