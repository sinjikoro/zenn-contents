---
title: "effective dart 個人的解釈まとめ(2024年版)"
emoji: "🐷"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["dart"]
published: true
---

# OverView

当記事は2024年夏〜秋くらいの内容となります。
翻訳した内容を主観的に捉えておりますので、解釈違いなどあればコメント頂けると幸いです！

## Dartで重要としていること
- Be consistent（一貫性）：Dartとして書式や大/小文字等に一貫性を持たせることが客観的に役立つ。
- Be brief（簡潔さ）：Dartでは意図をよりシンプルかつ簡単に表現できるよう多数の機能を用意している。

## EffectiveDartの内容
- Style Guide：コードにレイアウトと編成に関するルール
- Documentation Guide：主にコメント内容に関するルール
- Usage Guide：言語機能を最大化するルール
- Design Guide：ライブラリ用の一貫した使用可能なAPI設計

## EffectiveDartのガイド
- 😀  DO：必ず実践すべき内容
- 👌 PREFER：理由が無ければ実践すべき内容
- 🙄 CONSIDER：ケースや考えが様々
- ✋ AVOID：正当な理由が無ければやってはならない内容
- 🥶 DON'T：絶対にやってはならない内容

※EffectiveDartを適用するうえでlinterを活用する！

## 用語集
- library member：最上位フィールド。型ではない全て
- class member：クラス内に宣言されたフィールド
- member：ライブラリまたはクラスのメンバー
- variable：トップレベルの変数、パラメータ、またはローカル変数。静的フィールドやインスタンスフィールドは含まない
- type：任意の名前付き型宣言（class、typedef、enum）
- property：最上位の変数。getter/setter、フィールド



# [Style Guide](https://dart.dev/effective-dart/style)

## Identifiers（命名）

### 😀 DO：name types using UpperCamelCase.(camel_case_types)

class、eum、typedef及び型パラメータでは、UpperCamelCaseを使う

### 😀 DO：name extensions using UpperCamelCase.(camel_case_extensions)

extensionでもUpperCamelCaseを使う


### 😀 DO：name packages, directories, and source files using lowercase_with_underscores.(file_names, package_names)

package、directories、ソースコードではsnake_caseを使う

### 😀 DO：name import prefixes using lowercase_with_underscores.(library_prefixes)

import prefix(import文のas以降のところ)にはsnake_caseを使う

### 😀 DO：name other identifiers using lowerCamelCase.(non_constant_identifier_names)

class内のメンバ変数、関数や引数にはlowerCamelCaseを使う

### 👌 PREFER：using lowerCamelCase for constant names.(constant_identifier_names)

定数名にもlowerCamelCaseを使う

> [!NOTE]
> 定数に元はSCREAMING_CAPS（大文字を_で繋げる）を利用していたけど、現在はlowerCamelCaseへ変更となっている

### 😀 DO：capitalize acronyms and abbreviations longer than two letters like words.

判別が良くわからないけど、例文としてはこんな感じ。
linterも無いので、プロジェクト毎にルールを定めていたほうが良さそう。
HTTP→Http
DB→DB
IO→IO
TV→TV
MR→Mr
ID→Id

### 👌 PREFER：using _, __, etc. for unused callback parameters.

未使用のパラメータは_で定義することで、ひと目で未使用であることを判別させる。
また、未使用のパラメータが複数ある場合は__、___という具合に'_'を追加していく。

### 🥶 DON'T：use a leading underscore for identifiers that aren't private.

_から開始する識別子はprivateを表すが、関数内のローカル変数や関数のパラメータ、ローカル関数などは、そもそもprivateという概念が無いので'_'を付ける必要はない

### 🥶 DON'T：use prefix letters.

ハンガリアン記法（例：strStoreName）はいにしえの文化であり、今の時代にメリットはないのでやめろ

### 🥶 DON'T：explicitly name libraries.

libraryディレクティブを扱う際の注意事項。
libraryの命名は自動で行われるので、名前の指定はしないこと。
libraryディレクティブ自体省略可能だけど、library自体にコメントを付けたい場合やアノテーションを付けたい場合のみ、library表記のみを利用すべし

```dart
///example library
@TestOn('vm')
library  //←　library xxxというのはNG
```


## Ordering（順序）

### 😀 DO：place dart: imports before other imports.(directives_ordering)

```dart
import 'dart:async';
import 'dart:html';

import 'package:bar/bar.dart';
import 'package:foo/foo.dart';
```

### 😀 DO：place package: imports before relative imports.(directives_ordering)

```dart
import 'package:bar/bar.dart';
import 'package:foo/foo.dart';

import 'util.dart';
```

### 😀 DO：specify exports in a separate section after all imports.(directives_ordering)

```dart
import 'src/error.dart';
import 'src/foo_bar.dart';

export 'src/error.dart';
```

### 😀 DO：sort sections alphabetically.(directives_ordering)

importセクションごとにアルファベット順で並べる。

## Formatting（フォーマット）

### 😀 DO：format your code using dart format.

コードのフォーマットは `dart format`を使う
VSCodeだと、VSCodeのDart Extentionsに搭載しているSaveOnFormatを使えば良い

### 🙄 CONSIDER：changing your code to make it more formatter-friendly.

`dart format`にも限界はあるので、変数名の短縮、条件式を変数に格納、カンマを適宜付与する等して、読みやすい形を目指そう

### ✋ AVOID：lines longer than 80 characters.(lines_longer_than_80_chars)

80文字を超える行は可読性を損なうので、行を短縮しよう
とはいえ、最近の開発環境はワイドモニタが主流なので、80文字では少ない気もする。
何文字制限を課すかはプロジェクト毎に決める必要あり

> [!NOTE]
> dart-langリポジトリにて過去「80は少なすぎないか？」というissueが作成されている。
> https://github.com/dart-lang/dart_style/issues/833
> また、`dart format`の`--line-length`にて文字数指定が可能となっている。

### 😀 DO：use curly braces for all flow control statements.(curly_braces_in_flow_control_structures)

if文内の処理とかは、基本的に中括弧で囲む
また処理が１行の場合でも、formatの関係で次の行に折り返される場合は中括弧で囲む

```dart
if (overflowChars != other.overflowChars) {
  return overflowChars < other.overflowChars;
}
```

# Documentation

## Comments

### 😀 DO：format comments like sentences.

コメントは文書ライクに書く

### 🥶 DON'T：use block comments for documentation.

文書コメントにブロックコメント（/* */）は使用しない。
// を使って文書コメントを書く

## Doc comments

### 😀 DO：use /// doc comments to document members and types.

メンバーや型の仕様コメントについては、docコメント（///）を利用する。
これにより、dart docにてコメントが検索される。
/** */という記法も可能だが、こちらは特に内容のない複数行コメントで利用する。

### 👌 PREFER：writing doc comments for public APIs.(package_api_docs, public_member_api_docs)

公開するライブラリ、トップレベル変数、型、メンバーは基本的にコメントによって文書化する。

### 🙄 CONSIDER：writing a library-level doc comment.

ライブラリには下記のコメントを書く。
- ライブラリの目的を１行で要約したもの
- ライブラリ全体で利用される用語の解説
- ライブラリの使用方法を解説するコードサンプル
- 最も重要なクラスと関数へのリンク
- 関係するドメインの外部参照へのリンク

### 🙄 CONSIDER：writing doc comments for private APIs.

docコメントは、外部利用者に向けてだけでなく、関係者が理解できるようにprivateにも書く。

### 😀 DO：start doc comments with a single-sentence summary.

コメントはユーザー中心の短文で、簡潔に書く。
簡潔な説明をすれば、必要とあれば開発者はソースの内容を読む。

### 😀 DO：separate the first sentence of a doc comment into its own paragraph.

長文の説明が必要な場合、まず要約を一行でまとめてから、一行段落を空けて補足として続きの説明を書く。

### ✋ AVOID：redundancy with the surrounding context.

コメントには読み手の知らないことを重点的に記載する。
クラスの名前や型など、見ればわかるものをあえてコメントに記載する必要はない。

### 👌 PREFER：starting function or method comments with third-person verbs.

コメントはコードの動作に焦点を当てて記載する。

### 👌 PREFER：starting a non-boolean variable or property comment with a noun phrase.

プロパティのdocコメントでは、そのプロパティが何者であるかを説明する必要がある。
プロパティがもたらす結果を明記し、読み手が理解する必要ない動作内容については明記しない

### 👌 PREFER：starting a boolean variable or property comment with "Whether" followed by a noun or gerund phrase.

ブール値のdocコメントでは、”〜かどうか”を説明する。
それにより、そのbool値の持つ状態を明確に表す。

### 🥶 DON'T：write documentation for both the getter and setter of a property.

getterとsetterの両方にコメントを書く必要はなく、getterとsetterにまとめて一つのコメントを書く。

### 👌 PREFER：starting library or type comments with noun phrases.

ライブラリや型にコメントを書く場合、それが何であるかを名詞を用いて解説する必要がある。

### 🙄 CONSIDER：including code samples in doc comments.

コメントにコードサンプルを含むことで、読み手の理解は簡単になる。

### 😀 DO：use square brackets in doc comments to refer to in-scope identifiers.

コメント内の変数、メソッド、型名などを[]（角括弧）で囲うことで、関連するドキュメントへのリンクを作成できる。

### 😀 DO：use prose to explain parameters, return values, and exceptions.

パラメータや戻り値について、一つ一つ箇条書するのではなく、文書に混ぜる形で記載する。

### 😀 DO：put doc comments before metadata annotations.

コメントは注釈`@`の前に書く。

## Markdown

### ✋ AVOID：using markdown excessively.

マークダウンは多用すればよいのではなく、読み手に伝わることを意識し、必要最低限の利用に留める。

### ✋ AVOID：using HTML for formatting.

フォーマットにHTMLを利用するとリッチな表現は可能だが、ほとんどの場合はMarkDownの標準仕様で十分である。

### 👌 PREFER：backtick fences for code blocks.

コメント内でコードブロックを記載する場合、先頭にスペースを4つ入れる方法もあるが、段落との区別がつかない問題もある為、控えるべき。

## Writing

### 👌 PREFER：brevity.

コメント内容は明確かつ正確に、それであって簡潔に記載すべき。

### ✋ AVOID：abbreviations and acronyms unless they are obvious.

一般的でない略語（`i.e.`、`e.g.`等）の利用は、チーム内での共有認識がない限りは控えるべき。

### 👌 PREFER：using "this" instead of "the" to refer to a member's instance.

メンバーのインスタンスを指す場合は`this`を使うべき。（英語コメント）

# [Usage](https://dart.dev/effective-dart/usage)

## Libraries

### 😀 DO：use strings in part of directives.
part of内でlibraryの名前importを利用することはNG

```dart
part of my_library //NG
```

### 🥶 DON'T：import libraries that are inside the src directory of another package.
library内のソースと他のlibraryソースが相互参照することはNG

### 🥶 DON'T：allow an import path to reach into or out of lib.
lib外からコードをimportする場合、package参照する必要がある

```dart
// in api_test.dart
import 'package:my_project/api.dart' //OK
import '../lib/api.dart' //NG
```

### 👌 PREFER：relative import paths.
lib内であれば、相対パスでimportする。

## Null

### 🥶 DON'T：explicitly initialize variables to null.(avoid_init_to_null)
null許容変数は、初期化しない限りはnullとして扱われる為、明示的にnullでの初期化はしなくても良い

### 🥶 DON'T：use an explicit default value of null.(avoid_init_to_null)
null許容変数はデフォルト値もnullである為、明示的にnull代入をしなくても良い

### 🥶 DON'T：use true or false in equality operations.
null非許容bool変数を比較する場合、等号（非等号）演算子を用いない
null許容bool変数を比較する場合、??を用いてnullの場合の値を設定する

### ✋ AVOID：late variables if you need to check whether they are initialized.
lateを用いて変数宣言した場合、その変数がnull非許容であればnullチェックで初期化判定が可能。その変数がnull許容であれば初期化フラグを別途持たせて初期化判定を行う。
そもそも、lateを使用しないという検討も必要

### 🙄 CONSIDER：type promotion or null-check patterns for using nullable types.

「型のプロモーションは、ローカル変数、パラメーター、およびプライベートの最終フィールドに対してのみサポートされます。」

null許容の変数をnull非許容へ昇格するために、下記の方法がある
- パターンマッチング`case`を利用した方法
- ローカル変数へ代入する方法
- !を利用する方法

ローカル変数へ代入する場合、変数値の変更等で注意が必要

## Strings

### 😀 DO：use adjacent strings to concatenate string literals.(prefer_adjacent_string_concatenation)

文字列結合は+などを利用する必要はない。文字列動詞を隣接させるだけで良い。

```dart
raiseAlarm('ERROR: Parts of the spaceship are on fire. Other '
    'parts are overrun by martians. Unclear which are which.');
```


### 👌 PREFER：using interpolation to compose strings and values.(prefer_interpolation_to_compose_strings)

変数を補間して一つの文字列を作成する場合、$や${}を利用することでスマートに書くことができる。

```dart
'Hello, $name! You are ${year - birth} years old.';
```

### ✋ AVOID：using curly braces in interpolation when not needed.(unnecessary_brace_in_string_interps)

文字列の単純な変数補間では、$のみで補間可能だけど、直後に英数字が来る場合は'{}'中括弧で囲む必要がある。

## Collections

### 😀 DO：use collection literals when possible.(prefer_collection_literals)

ListとMap、Setの宣言は、それぞれ`<Point>[]`、`<String, Address>{}`、`<int>{}`という形式をを採用する。
ただし、名前付きコンストラクタは例外（List.from(), Map.fromIterable()）
List等のコレクションリテラルはスプレッド演算子(...)が利用できる為、強力となる。

### 🥶 DON'T：use .length to see if a collection is empty.(prefer_is_empty, prefer_is_not_empty)

コレクションの空判定は`.isEmpty`や`.isNotEmpty`を利用しよう



### ✋ AVOID：using Iterable.forEach() with a function literal.(avoid_function_literals_in_foreach_calls)

Iterableを継承した型において、for Eachを利用する場合は for-in を基本的に使う。
ただし、既存の関数呼び出し時や、Mapはこのガイドラインの適用外となる。

```dart
// good
for (final person in people) {
  ...
}

// good
people.forEach(print);

// bad
people.forEach((person) {
  ...
});
```

### 🥶 DON'T：use List.from() unless you intend to change the type of the result.

コレクションのディープコピーを行うには次の２通りが存在。
２行目の場合、コピー元の型を保持されない
```dart
var copy1 = iterable.toList();
var copy2 = List.from(iterable);
```

よって、型を保持したままのコピーは１行目
型変換しつつのコピーは２行目を推奨

```dart
// 型変換の例
var numbers = [1, 2.3, 4]; // List<num>.
numbers.removeAt(1); // Now it only contains integers.
var ints = List<int>.from(numbers);
```

### 😀 DO：use whereType() to filter a collection by type.(refer_iterable_whereType)

型複合のコレクションから特定の型を取得したい場合、whereType()を利用する。

```dart
var objects = [1, 'a', 2, 'b', 3];
var ints = objects.whereType<int>();
```

`objects.where((e) => e is int)` でも特定の型を取得可能ではあるが、この場合Object型のコレクションとなり、追加でcastが必要と冗長なコードとなってしまう。

### 🥶 DON'T：use cast() when a nearby operation will do.

cast()を使用すると強制的な変換を求められるため、極力利用場面は限定させる。

```dart
// good
var stuff = <dynamic>[1, 2];
var ints = List<int>.from(stuff);

// bad
var stuff = <dynamic>[1, 2];
var ints = stuff.toList().cast<int>();
```

### ✋ AVOID：using cast().

- 正しい型で宣言する。（dynamicやObject型は基本的に利用しない）
- アクセス時に適切な型で型チェック、及びcastする。
- コレクションアイテムの型を変換する場合、List.from()で型変換する。

## Functions

### 😀 DO：use a function declaration to bind a function to a name.(prefer_function_declarations_over_variables)

関数内で関数を定義する場合、インスタンスなものであれば無名関数を作成すればよいが、
名前付き関数を用いることで次のメリットを享受することが出来る。

- 可読性の向上 : 関数に明確な名前を付けることで、コードを読む他の人が関数の目的を簡単に理解できます。
- デバッグの容易さ : 関数名はデバッグ情報に表示されるため、問題のトラブルシューティングがしやすくなります。
- 再利用性 : 同じ関数を複数の場所から呼び出すことが容易になります。



### 🥶 DON'T：create a lambda when a tear-off will do.(unnecessary_lambdas)

カッコ無しで関数、メソッド、または名前付きコンストラクターを参照すると、Dartはtear-offを作成する。

```dart
var charCodes = [68, 97, 114, 116];
var buffer = StringBuffer();

// Function:
charCodes.forEach(print);

// Method:
charCodes.forEach(buffer.write);

// Named constructor:
var strings = charCodes.map(String.fromCharCode);

// Unnamed constructor:
var buffers = charCodes.map(StringBuffer.new);
```

## Variables

### 😀 DO：follow a consistent rule for var and final on local variables.

finalとvarの使い方は、コード内で一貫性をもたせる必要がある。
例えば、変更許容性がない場合は必ずfinalを利用する。function local内では必ずvarを利用する。…等

### ✋ AVOID：storing what you can calculate.

計算コードを書く際、キャッシュやパフォーマンスについて考える必要がある。
例えば次のコードについて、このコードはareaとcircumferenceをclass内に保持することとなり、無駄なメモリ消費が発生する。また、cacheのクリアを考慮しないと、以前の計算結果を間違って出力しかねない。

```dart
class Circle {
  double radius;
  double area;
  double circumference;

  Circle(double radius)
      : radius = radius,
        area = pi * radius * radius,
        circumference = pi * 2.0 * radius;
}
```

上記のコードについては、下記とすることで必要な際にareaやcircumferenceを行う為、無駄なメモリ消費を抑え、またcacheを気にする必要もなくなる。

```dart
class Circle {
  double radius;

  Circle(this.radius);

  double get area => pi * radius * radius;
  double get circumference => pi * 2.0 * radius;
}
```

## Members

### 🥶 DON'T：wrap a field in a getter and setter unnecessarily.(unnecessary_getters_setters)

JavaやC#では、フィールド値へのアクセスにgetter/setterを利用することが一般的であった。
その背景として、getter/setter処理内を後で処理変更を行うことが可能であり、フィールドへ直接アクセスすることとは異なり、変更に伴っての呼び出し元コード修正が必要なかった。

Dartはgetter/setterを利用しようがしまいが、フィールドへ直接アクセスすることに変わりはない為、都度getter/setterを用意する必要はない。

### 👌 PREFER：using a final field to make a read-only property.

読み取り専用プロパティへはget only Propretyではなく、finalを利用する。
ただし、コンストラクタ外部で内部的にフィールド代入する場合はPrivate Field, Public Getterを利用するが、本当に必要かは検討が必要である。

### 🙄 CONSIDER：using => for simple members.(prefer_expression_function_bodies)

シンプルなメンバ関数の定義にはアロー関数（=>）を利用する。

```dart
double get area => (right - left) * (bottom - top);

String capitalize(String name) =>
    '${name[0].toUpperCase()}${name.substring(1)}';
```

ただし、アロー関数やカスケード演算子、三項演算子を多用してコード可読性を損なうことは避けるべきである。

### 🥶 DON'T：use this. except to redirect to a named constructor or to avoid shadowing.(unnecessary_this)

`this`を利用する場面は下記の２通り
1. 同じ名前のローカル変数へアクセスする場合

```dart
class Box {
  Object? value;

  void update(Object? value) {
    this.value = value;
  }
}
```

2. コンストラクタへリダイレクトする場合

```dart
class ShadeOfGray {
  final int brightness;

  ShadeOfGray(int val) : brightness = val;
  ShadeOfGray.black() : this(0);
  ShadeOfGray.alsoBlack() : this.black();
}
```

1の例に関しては、下記の記載でも可能である。

```dart
class Box extends BaseBox {
  Object? value;

  Box(Object? value)
      : value = value,
        super(value);
}
```

### 😀 DO：initialize fields at their declaration when possible.

メンバ変数の初期値について、コンストラクタに依存しない変数であれば、宣言時に初期化しておく。

```dart
class ProfileMark {
  final String name;
  final DateTime start = DateTime.now();

  ProfileMark(this.name);
  ProfileMark.unnamed() : name = '';
}
```

## Constructors

### 😀 DO：use initializing formals when possible.(prefer_initializing_formals)

コンストラクタで渡される引数にてメンバ変数を初期化したい場合、`this.`を利用してスマートに記載する。

### 🥶 DON'T：use late when a constructor initializer list will do.

null非許容型をコンストラクタで初期化する場合、`:`で初期化リストとして宣言することで`late`や`var`の利用をすること無く初期化が出来る。

```dart
class Point {
  double x, y;
  Point.polar(double theta, double radius)
      : x = cos(theta) * radius,
        y = sin(theta) * radius;
}
```

### 😀 DO：use ; instead of {} for empty constructor bodies.(empty_constructor_bodies)

コンストラクタ内での処理が無い場合、空のボディ`{}`を付けずにセミコロンで終了させる。

### 🥶 DON'T：use new.(unnecessary_new)

コンストラクタを呼び出す際に`new`を利用する言語仕様があったが、現在は非推奨の為、利用しないこと。

### 🥶 DON'T：use const redundantly.(nnecessary_const)

下記の場合、`const`は暗黙的に宣言されている。その為、無闇にconstを付与する必要はない

- const コレクションリテラル
- const コンストラクター呼び出し
- メタデータの注釈
- const変数宣言の初期化子
- switch-case分のcaseの`:`より前の部分

（上記に加え、可変変数の初期化子についてもサポート予定）


## Error handling

### ✋ AVOID：catches without on clauses.(avoid_catches_without_on_clauses)

catchではonを利用しないと全てのExceptionをキャッチすることが可能ではあるが、本来個別に検知・対応すべきエラーをスルーしてしまうこともある。
その為、onを使ってExceptionごとのエラーハンドリングを正しく行うべき

```dart
try {
 somethingRisky()
}
on Exception catch(e) {
  doSomething(e);
}
```

＃全てのエラーをキャッチすることを `Pokémon exception handling` と呼ぶらしい

### 🥶 DON'T：discard errors from catches without on clauses.

onを利用しないcatchについては、必ず何かしらのエラーハンドリングを行うべき。
何もせずに破棄してはならない。

### 😀 DO：throw objects that implement Error only for programmatic errors.

`Error`クラスはコード実装のバグを検知した場合にのみ利用されるものであり、例外発生時に利用される `Exception` と区分けして利用すべきである。

### 🥶 DON'T：explicitly catch Error or types that implement it.(avoid_catching_errors)

プログラム実行で`Error`を検知すると、StackTraceが生成されるが、catchにより `Error` を検知してしまうと、StackTrace生成が中断されたりしてしまう
`Error`はコード欠陥を示すものである為、catchしてエラーハンドリングするのではなく、StackTraceからコード修正を行うこと

### 😀 DO：use rethrow to rethrow a caught exception.

catchした例外を呼び出し元へ再スローする場合、同じExceptionをthrowするのではなく、`rethrow`を利用する。

```dart
try {
  somethingRisky();
} catch (e) {
  if (!canHandle(e)) rethrow;
  handle(e);
}
```

## Asynchrony

### 👌 PREFER：async/await over using raw futures.

非同期のコードを使う場合、async/awaitを利用する。

### 🥶 DON'T：use async when it has no useful effect.

次のケース以外では、asyncは付けない。
- awaitで非同期処理を書く
- 非同期型のErrorを返す（return Future.error(...)と書くより、簡潔に書ける）
- 非同期型の値を返す（Future.value(...)と書くより、簡潔に書ける）

```dart
Future<void> usesAwait(Future<String> later) async {
  print(await later);
}

Future<void> asyncError() async {
  throw 'Error!';
}

Future<String> asyncValue() async => 'value';
```

### 🙄 CONSIDER：using higher-order methods to transform a stream.

streamは高次メソッド（mapとか）を利用して順次処理を行うように記載する

### ✋ AVOID：using Completer directly.

Comoleterは新しい非同期プリミティブ、およびフューチャーを使用しない非同期コードとのインターフェイスでは利用が推奨されるが、殆どの非同期処理での利用は必要としない。

### 😀 DO：test for Future<T> when disambiguating a FutureOr<T> whose type argument could be Object.

FutureOr\<T\>を扱う際、Future\<T\>であるかの判定処理が必要である。しかし、Future\<T\>型はT型と一致とみなされてしまう為、判定の際は`value is Future<T>`判定から優先すべきである。

```dart
Future<T> logValue<T>(FutureOr<T> value) async {
  if (value is Future<T>) {
    var result = await value;
    print(result);
    return result;
  } else {
    print(value);
    return value;
  }
}
```

# [Design](https://dart.dev/effective-dart/design)

## Names

### 😀 DO：use terms consistently.

コード全体で同じものには同じ名前を統一して付ける。
言語として慣例のある命名規則がある場合（例えばtoListのtoXXX、asMapのasXXX等）、慣例に沿った命名を付与する必要がある。

```dart
// bad
renumberPages()      // Confusingly different from pageCount.
convertToSomething() // Inconsistent with toX() precedent.
wrappedAsSomething() // Inconsistent with asX() precedent.
Cartesian            // Unfamiliar to most users.
```

### ✋ AVOID：abbreviations.

略語が一般的に通用しない場合、略さない
逆に略語が一般的な場合は略す
もし略す場合は大文字を正しく付けること

### 👌 PREFER：putting the most descriptive noun last.

名前の中で、その内容を最もよく説明する言葉は最後に持ってくること

```dart
// good
pageCount
// bad
numPages
```

### 🙄 CONSIDER：making the code read like a sentence.

コードの内容を説明する文書で名前を付ける
但し、文書にこだわりすぎて名前が長くなるケースもあるので注意すること

```dart
// good
if (errors.isEmpty) ...
subscription.cancel();
monsters.where((monster) => monster.hasClaws);

// bad
if (errors.empty) ...
subscription.toggle();
monsters.filter((monster) => monster.hasClaws);
```

### 👌 PREFER：a noun phrase for a non-boolean property or variable.

読者はそのプロパティが「何であるか」に注目する。
名前つけでは、それが何かを端的に名詞で表現する。
プロパティの決定方法を重視する場合は、動名詞を持つメソッドを使用する必要がある。

### 👌 PREFER：a non-imperative verb phrase for a boolean property or variable.

ブール値の名前は制御フローの条件として用いられやすい為、読みやすい名前が必要。
良い名前は数種類の動詞のいづれかで始まる傾向がある。

- "to be" : isEnabled, wasShown, willFire
- 助動詞 : hasElements, canClose, shouldConsume, mustSave
- 能動的動詞 : ignoresInput, wroteFile（名前が述語として読み取れる場合のみ、能動的動詞は許可される）

### 🙄 CONSIDER：omitting the verb for a named boolean parameter.

先ほどのルールの補足。
bool型の名前付きパラメータにおいて、役割が明確な場合は動詞を省略する

```dart
var regExp = RegExp(pattern, caseSensitive: false);
```

### 👌 PREFER：the "positive" name for a boolean property or variable.

bool型のプロパティ名は肯定形の命名を付与する。
ただし、一部プロパティでは否定形が圧倒的なニーズとなっている場合がある為、その際は否定形を優先して採用する。

### 👌 PREFER：an imperative verb phrase for a function or method whose main purpose is a side effect.

副作用を目的とする関数やメソッド名には命令形動詞を使用する。

```dart
list.add('element');
queue.removeFirst();
window.refresh();
```

### 👌 PREFER：a noun phrase or non-imperative verb phrase for a function or method if returning a value is its primary purpose.

副作用はなく、呼び出し元に有用な結果を返す関数やメソッド名には名詞、または非命令形の動詞を使用する。

```dart
// 名詞パターン
var element = list.elementAt(3);
var first = list.firstWhere(test);
var char = string.codeUnitAt(4);
// 動詞パターン
list.take();
string.split();
```

### 🙄 CONSIDER：an imperative verb phrase for a function or method if you want to draw attention to the work it performs.

副作用なく、呼び出し元に有用な結果を返す関数やメソッド名においても、その結果を取得するために必要な重要な作業がある場合、その作業を説明する動詞句名も使用する。

```dart
var table = database.downloadData();
var packageVersions = packageGraph.solveConstraints();
```

ただし、このガイドラインは前の２つのルールほど厳密に適用する必要は無い

（8/19ここまで）

### ✋ AVOID：starting a method name with get.

メソッド名をgetで開始するのは避ける。
- 単純にvalueを返す場合、名前のみを使う
- 動詞を利用する場合、もっと詳しい内容を表した名称を利用する。（create, download, fetch, calculate, request, aggregate, etc.）

### 👌 PREFER：naming a method to___() if it copies the object's state to a new object.(use_to_and_as_if_applicable)

オブジェクトの新しい状態を返すメソッドは、toXXXという命名とする

```dart
list.toSet();
stackTrace.toString();
dateTime.toLocal();
```

### 👌 PREFER：naming a method as___() if it returns a different representation backed by the original object.(use_to_and_as_if_applicable)

オブジェクトを別の表現で返すメソッドは、asXXXという命名とする。

```dart
var map = table.asMap();
var list = bytes.asFloat32List();
var future = subscription.asFuture();
```

### ✋ AVOID：describing the parameters in the function's or method's name.

関数名に引数名を参照させることは可読性には繋がらない。

```dart
// good
list.add(element);
map.remove(key);
//bad
list.addElement(element)
map.removeKey(key)
```

ただし、曖昧さ回避の観点から有効なケースも存在する

```dart
map.containsKey(key);
map.containsValue(value);
```

### 😀 DO：follow existing mnemonic conventions when naming type parameters.

ジェネリック型のパラメータを命名する際は、ニモニック規約に則って命名する

- ニモニック規約
    - E : Element
    - K : Key
    - V : Value
    - R : Return
    - T,S and U : single type parameter

```dart
class IterableBase<E> {}

class Map<K, V> {}

abstract class ExpressionVisitor<R> {
  R visitBinary(BinaryExpression node);
}

class Future<T> {
  Future<S> then<S>(FutureOr<S> onValue(T value)) => ...
}
```

上記に宛はならない場合は、別のわかりやすい文字を使用してもOK

```dart
class Graph<N, E> {
  final List<N> nodes = [];
  final List<E> edges = [];
}

class Graph<Node, Edge> {
  final List<Node> nodes = [];
  final List<Edge> edges = [];
}
```

## Libraries

### 👌 PREFER：making declarations private.

パブリックに公開されたclassは、他のライブラリからアクセスされるものである為、その宣言をサポートし、アクセスがあった場合に適切に動作することを約束する必要がある。
必要のないclassについてはプライベート宣言すべきである。

### 🙄 CONSIDER：declaring multiple classes in the same library.

Javaなどでは1ファイルに1classといった制限があるが、dartではその制限はなく、1ファイルに複数のclassを宣言しても良い。（friend class）

## Classes and mixins

### ✋ AVOID：defining a one-member abstract class when a simple function will do.(one_member_abstracts)

classの構成が一つのメンバ変数や関数のみの場合はclassの利用は避けるべき

```dart
// bad
abstract class Predicate<E> {
  bool test(E element);
}
```

### ✋ AVOID：defining a class that contains only static members.(avoid_classes_with_only_static_members)

静的メンバのみで構成されるclassは極力控えるべき。
静的メンバでの構成とした目的が、名前空間の代用であれば、dartではlibraryのほうが適している。

ただし、こちらは厳格なルールではなく、定数や列挙型の場合は適しているケースも存在している。

```dart
// good
class Color {
  static const red = '#f00';
  static const green = '#0f0';
  static const blue = '#00f';
  static const black = '#000';
  static const white = '#fff';
}
```

### ✋ AVOID：extending a class that isn't intended to be subclassed.

classがサブクラス化を許容するか否かは慎重に検討すべき。
super classを一部変更したことにより、サブクラス全体に影響が派生してしまう。
サブクラス化を禁止する場合、ドキュメンテーションコメントかclass名にて明示させる必要がある。

### 😀 DO：document if your class supports being extended.

上記ルールに関して、逆にサブクラスを許容するケースにおいても明示する必要がある。
この場合、同様にドキュメンテーションコメントに記載するか、class名の末尾にBaseをつけるなどがある

### ✋ AVOID：implementing a class that isn't intended to be an interface.

interface機能はdartにおいて強力なツールであるが、同時に実装classと密接な関係となる為、一つの修正を行うとしても
多大な影響を及ぼしかねない機能である。
その為、interfaceとしての提供は実装が明確となっているクラスのみとすべきである。

### 😀 DO：document if your class supports being used as an interface.

classがinterfaceをサポートしている場合、ドキュメンテーションコメントへ記載等して明確にすべきである。

### 👌 PREFER：defining a pure mixin or pure class to a mixin class.

dart3.0以前はデフォルト以外のコンストラクタやスーパークラスなどがないclassは他のclassと混在させることが可能であった。
その為、意図せぬ混在などが含まれるケースも有り、開発者に混乱を招く結果となった。
dart3.0以降では、それらの混在においてmixin classを利用して、曖昧さを回避する必要がある。

## Constructors

### 🙄 CONSIDER：making your constructor const if the class supports it.

すべてのフィールドが final で、コンストラクターがフィールドの初期化のみを行うクラスがある場合、そのコンストラクターはconstを利用できます。
これにより、ユーザーは、定数が必要な場所 (他の大きな定数、スイッチ ケース、デフォルトのパラメーター値など) でクラスのインスタンスを作成できます。

ただし、後々コンストラクタをconst以外に変更すると、従来の箇所で呼び出せなくなる可能性もある為、注意が必要です。

## Members

### 👌 PREFER：making fields and top-level variables final.(prefer_final_fields)

フィールドやトップレベル変数は、特別な扱いでない限りはfinalにて定義する。

### 😀 DO：use getters for operations that conceptually access properties.

次の条件を満たす場合、メソッドではなくgetterを選択すべき
- 引数を取らずに値を返す
- 呼び出し側は、結果のみを知りたい
- 副作用がない
- 呼び出し時は状態が変わらない限り、基本的に同じ値を返す
- 結果オブジェクトは元オブジェクトの全てを返すわけではない

```dart
// good
rectangle.area;
collection.isEmpty;
button.canShow;
dataSet.minimumValue;
```

### 😀 DO：use setters for operations that conceptually change properties.(use_setters_to_change_properties)

次の条件を満たす場合、メソッドではなくsetterを選択すべき
- 単一の引数のみを受け、結果は生成しない
- この操作により、オブジェクトの状態が一部変更される
- 2回呼び出されても、2回目の呼び出しは何もしない

```dart
// good
rectangle.width = 3;
button.visible = false;
```

### 🥶 DON'T：define a setter without a corresponding getter.(avoid_setters_without_getters)

getterとsetterは、セットまたはgetterのみを利用する。setterのみの利用は行わない。

### ✋ AVOID：using runtime type tests to fake overloading.

同じメソッド名で引数に複数の型を定義するオーバーロードは基本的にNGである。
ただ、単一メソッドで本体内にisを用いて型チェックを行う、オーバーロードの偽装を行うことは可能だけど、型が限定されているのであれば、限定した引数を用意すべきである。

### ✋ AVOID：public late final fields without initializers.

late finalは以下の用に初期化すべきであり、setterを通じて外部からセットさせることはさせない。
- コンストラクタ内で初期化
- factoryコンストラクタを利用して初期化
- late宣言時に初期化
- lateフィールドをプライベートにして、パブリックのgetterを用意
- そもそもlateを利用しない

### ✋ AVOID：returning nullable Future, Stream, and collection types.

Future、Stream等のcollection結果として、データなしの際にnullを返すのは極力避けるべき
空配列を用意して、呼び出し側はisEmptyとして判断すべき

### ✋ AVOID：returning this from methods just to enable a fluent interface.(avoid_returning_this)

流れるようなインターフェースを有効にするためだけにメソッドから this を返すことは避ける。

```dart
// good
var buffer = StringBuffer()
  ..write('one')
  ..write('two')
  ..write('three');
```

## Types

### 😀 DO：type annotate variables without initializers.(prefer_typing_uninitialized_variables)

型推論で変数宣言する場合、初期化子を付ける

```dart
// good
var foo = 5;

// bad
var bar;
bar = 5;
```

### 😀 DO：type annotate fields and top-level variables if the type isn't obvious.(type_annotate_public_apis)

関数の引数や戻り値などでは、それらの型が明示的な場合を除いて注釈をつけよう

```dart
//bad
install(id, destination) {
  // ...
}

//good
Future<bool> install(PackageId id, String destination) {
  // ...
}

//good (値が明示的な場合)
const screenWidth = 640; // Inferred as int.
```

### 🥶 DON'T：redundantly type annotate initialized local variables.(omit_local_variable_types)

初期化された変数に不要な型注釈をつけると、読み手に不要な情報を読む負担が増えてしまう。

```dart
//bad
List<List<Ingredient>> possibleDesserts(Set<Ingredient> pantry) {
  List<List<Ingredient>> desserts = <List<Ingredient>>[];
  for (final List<Ingredient> recipe in cookbook) {
    if (pantry.containsAll(recipe)) {
      desserts.add(recipe);
    }
  }

  return desserts;
}
```

但し、初期化された型とは異なる型を利用したい場合は型注釈をつけるべきである

```dart
Widget build(BuildContext context) {
  Widget result = Text('You won!');
  if (applyPadding) {
    result = Padding(padding: EdgeInsets.all(8.0), child: result);
  }
  return result;
}
```

### 😀 DO：annotate return types on function declarations.

Dartでは戻り値の型を関数本体から推測する機能は無いので、戻り値の型は注釈を明記する必要がある。
但し、ローカル関数と匿名関数式に関しては、逆に型注釈を明記する必要はない

```dart
//bad
makeGreeting(String who) {
  return 'Hello, $who!';
}
```

### 😀 DO：annotate parameter types on function declarations.

関数のパラメータリストでは、変数初期値から型を推測することはせず、型注釈をつける

```dart
//bad
void sayRepeatedly(message, {count = 2}) {
  for (var i = 0; i < count; i++) {
    print(message);
  }
}
```


### 🥶 DON'T：annotate inferred parameter types on function expressions.(avoid_types_on_closure_parameters)

匿名関数では、ほとんどが何かしらの型のコールバックを取るメソッドをすぐに渡されるため、型推測できる場合がほとんどである。その場合、型注釈は不要となる。

```dart
//good
var names = people.map((person) => person.name);

//bad
var names = people.map((Person person) => person.name);
```

### 🥶 DON'T：type annotate initializing formals.(type_init_formals)

this.やsuper.は元々の型を推論できる為、型注釈は不要である。

```dart
class Point {
  double x, y;
  Point(double this.x, double this.y);
}

class MyWidget extends StatelessWidget {
  MyWidget({Key? super.key});
}
```

### 😀 DO：write type arguments on generic invocations that aren't inferred.

型推論できないジェネリック型には、型注釈を付ける

```dart
//good
var playerScores = <String, int>{};
final events = StreamController<Event>();
```

### 🥶 DON'T：write type arguments on generic invocations that are inferred.

型推論できるジェネリック型には、型注釈は余分に記載しない

```dart
//bad
class Downloader {
  final Completer<String> response = Completer<String>();
}
```

### ✋ AVOID：writing incomplete generic types.

ジェネリック型を使う場合、型推論に失敗するとdyanamic型となってしまうので、型注釈して明示的な型指定をすること

```dart
//bad
List numbers = [1, 2, 3];
var completer = Completer<Map>();
```

### 😀 DO：annotate with dynamic instead of letting inference fail.

dartでは型推論に失敗するとdynamic型となるが、dynamic型として利用したかったのか推論失敗でdynamic方となったのかを判断が難しくなる。その為、dynamic型が必要な場面では、明示的にdynamic型を利用すること

### 👌 PREFER：signatures in function type annotations.

関数型注釈を利用する場合、戻り値の型を明記すること

```dart
bool isValid(String value, bool Function(String) test) => ...
```

### 🥶 DON'T：specify a return type for a setter.(avoid_return_types_on_setters)

setterは必ずvoidを返すため、型の記載は不要。

### 🥶 DON'T：use the legacy typedef syntax.(prefer_generic_function_type_aliases)

古いtypedef構文を利用しない

```dart
//bad
typedef int Comparison<T>(T a, T b);
//good
typedef Comparison<T> = int Function(T, T);
typedef Comparison<T> = int Function(T a, T b); //パラメータの名前付き
```

レガシーなコードでは、例えば `typedef bool TestNumber(num);` と記載した場合、number型の引数を受け取ってbool型で返す何かしらの関数を連想しますが、その実はnumという名前のdynamic型の引数を受け取ることとなり、混乱の原因となってました。

### 👌 PREFER：inline function types over typedefs.(avoid_private_typedef_functions)

dartでは関数型のtypedefが利用可能であるが、型注釈が利用できれば可能であるインライン関数を優先して検討すべきである。
関数型が特に長い場合や頻繁に使用される場合は、typedef を定義する価値があるかもしれません。しかし、ほとんどの場合、ユーザーは関数型が実際に使用されている場所でその関数型が実際に何であるかを確認したいと考えており、関数型構文によってその明確さが提供されます。

### 👌 PREFER：using function type syntax for parameters.(use_function_type_syntax_for_parameters)

Dartでは関数型のパラメータを定義する際の構文として、次の構文を用意している。

```dart
Iterable<T> where(bool predicate(T element)) => ...
Iterable<T> where(bool Function(T) predicate) => ...
```

### ✋ AVOID：using dynamic unless you want to disable static checking.

Dartにて、全ての型を受け付ける型はObjectとDynamicが存在している。
その使い分けとして、Objectはnull以外の全ての型を受け付け、またObject?とすることで全ての型を受け付ける。
Dynamic型は全ての型を受け付けるだけでなく、全ての操作も受け付ける。Dynamic型へのメンバーアクセスはコンパイル時に許可されるが、実行時に失敗して例外をスローする可能性もある。リスクはあるものの柔軟な動的ディスパッチが必要な場合はDynamicが有効である。

なので、基本的にはObjectを優先すべきである。
Dynamicを利用する例として、ジェネリクス型内で動的を使用する場合、例えばJsonオブジェクトにMap<String, Dynamic>を利用する場合等が挙げられる。

### 😀 DO：use Future<void> as the return type of asynchronous members that do not produce values.

値を返さない非同期関数では、Future<void>を戻り値の型として指定する。

### ✋ AVOID：using FutureOr<T> as a return type.

戻り値にFutureOr<T>を利用してなならない。

```dart
// good
Future<int> triple(FutureOr<int> value) async => (await value) * 3;

// bad
FutureOr<int> triple(FutureOr<int> value) {
  if (value is int) return value * 3;
  return value.then((v) => v * 3);
}
```

## Parameters

### ✋ AVOID：positional boolean parameters.(avoid_positional_boolean_parameters)

bool値を引数とする場合、名前付きコンストラクタや名前付きパラメータを活用することで、その引数が何を意図しているかを明確にする必要がある

```dart
//good
Task.oneShot();
Task.repeating();
ListBox(scroll: true, showScrollbars: true);
Button(ButtonState.enabled);
```

### ✋ AVOID：optional positional parameters if the user may want to omit earlier parameters.

引数の並びとして、受け渡し頻度の高い引数ほど前に持ってくる必要がある。
また引数の穴を防ぐためにも、名前付きパラメータを活用すべきである。


### ✋ AVOID：mandatory parameters that accept a special "no argument" value.

パラメータを渡さないとする場合、意図的にnullを渡すのではなく、オプショナルな引数として省略可能とすべきである。

### 😀 DO：use inclusive start and exclusive end parameters to accept a range.

index要素の範囲を指定する引数に関しては、コアライブラリに倣った引数指定をさせるべきである。
それにより、範囲指定の引数指定に関しての一貫性を持たすことが出来て、利用者の混乱を回避することができる。

```dart
//good
[0, 1, 2, 3].sublist(1, 3) // [1, 2]
'abcd'.substring(1, 3) // 'bc'
```

## Equality

### 😀 DO：override hashCode if you override ==.(hash_and_equals)

==をoverride実装する場合、hashCodeもoverride実装してイコールであることを保証させる。
でないと、要素をhashベースのコレクションに含めた場合、動作が意図せぬ動きをする可能性がある。

```dart
//good
class Better {
  final int value;
  Better(this.value);

  @override
  bool operator ==(Object other) =>
      other is Better &&
      other.runtimeType == runtimeType &&
      other.value == value;

  @override
  int get hashCode => value.hashCode;
}
```

### 😀 DO：make your == operator obey the mathematical rules of equality.

==は次の数学的要素を確保する必要がある

- 反射: a == a は必ずtrueとなる
- 対称: a == b と b == a  は同じ結果を返す
- 推移: a == b と b == c がtrueを返す場合、 a == c もtrueを返す

### ✋ AVOID：defining custom equality for mutable classes.(avoid_equals_and_hash_code_on_mutable_classes)

ミュータブルクラスで==を定義する際、hashCodeも一致させる必要がある

### 🥶 DON'T：make the parameter to == nullable.(avoid_null_checks_in_equality_operators)

==を実装する際、not nullを前提とした実装をする。
