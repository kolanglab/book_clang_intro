# typedef：型に別名を付ける

基本型（[](types.md)）・ポインタ（[](pointers.md)）・構造体（[](struct.md)）・共用体（[](union.md)）を学んだところで、`typedef` を改めて取り上げます。`typedef` は既存の型に**別名（alias）**を付ける機能で、言語処理系づくりで特に効いてきます。

## typedef の基本

```c
typedef int64_t Value;   // 「Value」は int64_t の別名になる

Value x = 100;           // int64_t x = 100; と同じ意味
```

なぜ別名が役立つのでしょうか。第一に、**意図が名前で伝わる**からです。`int64_t result` と書くより `Value result`（処理系が扱う値だ）と書くほうが、コードを読む人に役割が伝わります。

第二に、**後から変えやすい**ことです。いま値を `int64_t` で表していても、将来「小数も扱いたい」と思うかもしれません。コード中のあちこちに `int64_t` と直書きしていると全部を直す羽目になりますが、`typedef int64_t Value;` の一行だけにまとめておけば、その一行を `typedef double Value;` に変えるだけで済みます。この「一箇所で型を切り替えられる」性質は、設計を試行錯誤する言語処理系づくりと相性が良いのです[](#cite:appel1998)。

## 構造体・共用体・関数ポインタとの組み合わせ

`typedef` の本当の威力は、構造体（[](struct.md)）や共用体（[](union.md)）、関数ポインタ（[](function-pointers.md)）のような、書くと長くなる型に短い名前を付けられる点にあります。これらの章で `typedef struct` や `typedef union` のパターンが登場しましたが、改めて整理します。

```c
// 構造体に別名を付ける（struct.md で使ったパターン）
typedef struct Token {
    int   kind;
    int   line;
    char *text;
} Token;

Token t;   // struct Token t; と書かずに済む
```

```c
// 関数ポインタ型に分かりやすい名前を付ける
typedef int (*BinaryOp)(int, int);

BinaryOp ops[] = { add, sub, mul };   // int (*ops[])(int, int) と書くより明快
```

> [!TIP]
> `typedef` は新しい型を作るわけではありません。あくまで既存の型の**あだ名**です。`typedef int Meters; typedef int Seconds;` としても、`Meters` と `Seconds` は中身が同じ `int` なので、取り違えてもコンパイラは怒りません。型による厳密な区別がほしい場合は、構造体で包む手があります。

## この章のまとめ

- `typedef` は既存の型に別名を付け、意図を伝え、後からの差し替えを容易にする。
- 構造体・共用体・関数ポインタのような複雑な型に短い名前を付けるのに特に有効。
- 別名を作るだけで新しい型ではないため、コンパイラは型の取り違えを検出しない。

次章では、プログラムの実行中に必要なだけメモリを確保・解放する仕組みを学びます。
