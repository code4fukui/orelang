# orelang

orelangはJSONで記述するシンプルなプログラミング言語です。

## 機能

-   **S式構文**: すべての操作はLispのようにJSON配列で表現されます。
-   **基本構文**: 変数（`set`、`get`）、順次実行（`step`）、コンソール出力（`print`）。
-   **制御フロー**: `if`、`while`、`until`、`break`。
-   **関数**: `func`で関数を定義し、`call`で呼び出し、`return`を使用します。`import`を使用してURLまたはローカルパスからモジュールを読み込むことができます。
-   **スタックベースの引数**: 関数は`push`と`pop`を使用して、グローバルスタック経由で引数を受け取ります。
-   **標準ライブラリ**: 基本的な演算子に加え、`String`、`Math`、`Array`のユーティリティが含まれています。

## 使い方

### コマンドライン (Deno使用時)

`ore.js`ヘルパースクリプトを使用して、コマンドラインから直接`.orelang`ファイルを実行できます。

```sh
# geo3x3.orelangモジュールを使用するデモプログラムを実行
cd demo
deno run -A ../ore.js ./simple_geo3x3.orelang
```

### JavaScriptモジュールとして

`orelang.js`をインポートして、DenoまたはブラウザベースのJavaScriptコード内でプログラムを実行できます。

```javascript
import { orelang } from "https://code4fukui.github.io/orelang/orelang.js";

// リモートの.orelangファイルを呼び出し、引数として10と20を渡す
const result = await orelang.run(["call", "https://code4fukui.github.io/orelang/demo/sum.orelang", 10, 20]);

console.log(result);
```

## 言語リファレンス

すべての操作はJSON配列であり、最初の要素が演算子名となります。

### 基本構文

-   `["step", expression1, expression2, ...]`
    すべての式を順番に評価し、最後の式の結果を返します。

-   `["set", "variableName", expression]`
    `expression`の結果を変数に代入します。

-   `["get", "variableName"]`
    変数の値を取得します。

-   `["print", expression1, expression2, ...]`
    評価された式をコンソールに出力します。

### 演算子

-   `["+", a, b, ...]`, `["-", a, b, ...]`, `["*", a, b, ...]`, `["/", a, b]`, `["%", a, b]`
-   `["=", a, b]`, `[">", a, b]`, `["<", a, b]`, `[">=", a, b]`, `["<=", a, b]`
-   `["&&", a, b, ...]`, `["||", a, b, ...]`
-   `["!", a]`

### 制御フロー

-   `["if", condition, then_expression, else_expression]`
    `condition`が真の場合は`then_expression`を評価し、そうでない場合は`else_expression`を評価します。

-   `["while", condition, expression]`
    `condition`が真である間、`expression`を繰り返し評価します。

-   `["until", condition, expression]`
    `condition`が真になるまで、`expression`を繰り返し評価します。

-   `["break"]` または `["break", value]`
    `while`または`until`ループを抜け出します。オプションで値を返すこともできます。

### 関数とモジュール

関数の引数は直接渡されません。グローバルスタックにプッシュされ、関数内で`pop`を使用して取得されます。

-   `["func", "functionName", expression]`
    関数を定義します。

-   `["call", "functionName", arg1, arg2, ...]`
    `arg1`、`arg2`などをスタックにプッシュしてから関数を実行します。URLを直接呼び出すことも可能です: `["call", "path/to/file.orelang", arg1, ...]`

-   `["return", expression]`
    関数から値を返します。

-   `["import", "functionName", "URL_or_path"]`
    外部の`.orelang`ファイルを読み込み、`functionName`に割り当てます。

### スタック操作

-   `["push", value1, value2, ...]`
    1つ以上の値をグローバルスタックにプッシュします。

-   `["pop"]`
    グローバルスタックの先頭の値をポップして返します。

### 標準ライブラリ

#### String
-   `["String.charAt", string, index]`
-   `["String.indexOf", string, substring]`
-   `["String.length", string]`
-   `["String.parseInt", string]`
-   `["String.parseFloat", string]`

#### Math
-   `["Math.floor", number]`

#### Array
-   `["Array", item1, item2, ...]`
-   `["Array.length", array]`
-   `["Array.getAt", array, index]`
-   `["Array.setAt", array, index, value]`

## reference

-   [orelang](https://qiita.com/shuetsu@github/items/ac21e597265d6bb906dc)
