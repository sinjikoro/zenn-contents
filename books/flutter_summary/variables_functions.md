---
title: "変数や関数"
---

## 1. 変数
dartには、その他言語同様に`int`、`String`、`bool`等が存在します。

### 1-1. 変数の基本
基本的な変数宣言は下記のとおりです。
```dart
int productId;
String name, createAt;
bool isChanged;

// 初期値を設定
int count = 0;
```

### 1-2. 不変な変数
不変な変数の宣言は下記のとおりです。
`const`はビルド時に値セットが必要で、`final`はプログラム実行時に値セットが必要となり、いづれも一度セットした値の変更は受け付けません。
```dart
const reqestUrl = 'https://xxx.com/';
final userId = response.id;

// 下記いづれもNG
// reqestUrl = 'https://yyy.com/';
// userId = newResponse.id;
```

### 1-3. 静的変数
静的な変数の宣言はstaticを利用します。
```dart
// 静的変数
static String name = 'pochi';
```

### 1-4. getter/setterとprivate
その他の変数の宣言でgetter/setterも利用可能です。
また、プライベート変数はclass同様に`_`を付与することで宣言できます。
```dart
// getter
int get area => _area;

// setter
set side(int val) {
  _area = val * val;
}

// プライベート変数
int _area = 0;
```

### 1-5. null safty
また、flutterはv2.0以降null safty対応を進めており、null許容とするには`?`を付ける必要があります。また、`late`修飾子によってインスタンス化後に値をセットすることも可能ですが、バグの原因となり得るため、あまり利用しないほうが良さげです。

```dart
class Dog {
  Dog(this.name);
  String name;
  
  // null許容
  String? owner;
  
  // 遅延初期化
  late int birthYear;
}

void main() {
  Dog dog = Dog('pochi');
  dog.birthYear = 2020; // birthYearをセットしないとerrorとなる
  print(dog.name); // pochi
  print(dog.owner); // null
  print(dog.birthYear); //2020
}
```
null許容変数を利用する場合、利用する場面でも変数がnullの可能性有無を`?`と`!`で指定させる必要があります。

```dart
// 変数がnullの可能性あり
String? val = hoge?;
// 変数がnullの可能性なし
String val = hoge!;
```

### 1-6. 列挙型
列挙型も利用が可能です。列挙体利用時は`enum`で宣言し、基本的な利用としては下記のようになります。
```dart
enum Region {
  hokkaido,
  tohoku,
  kanto,
  kinki,
  tokai,
  kansai,
  chugoku,
  shikoku,
  kyushu,
}

main() {
  print(Region.kanto); // Region.kanto
  print(Region.kanto.name); // kanto
  print(Region.kanto.index); // 2
}
```

また、dartの列挙型はclassの如く拡張性を持たすことが可能です。
下記は列挙型にインスタンスを用意し、列挙型アイテムをインスタンス変数化させたものです。加えて、`implements`によるインターフェース実装なども可能です。
```dart
enum Weapon {
  daggerKnife(attack: 50, defense: 10, agility: 30),
  glove(attack: 20, defense: 5, agility: 50),
  woodenBoard(attack: 10, defense: 40, agility: 5);

  final int attack;
  final int defense;
  final int agility;

  const Weapon({
    required this.attack,
    required this.defense,
    required this.agility,
  });
}

main() {
  print(Weapon.daggerKnife.attack); // 50
}
```

## 2. メソッド
dartには、他のプログラミング言語同様に様々な種類のメソッドが用意されてます。
これまでもサンプルコード内に様々なメソッドを記載しておりましたが、改めてdartのメソッドを整理します。

### 2-1. インスタンスメソッド
度々登場していたクラス内のメソッドです。
同一クラス内のインスタンス変数にアクセスが可能です。
```dart
class Square {
  Square({required this.x, required this.y});
  final int x;
  final int y;
  
  // インスタンスメソッド
  int getArea() {
    return x * y;
  }
}
```

### 2-2. パブリックメソッド
メソッドはクラス外にも定義可能です。
```dart
int getSquareArea(int x, int y) {
  return x * y;
}
```

### 2-3. 静的メソッド
静的なメソッドの宣言も`static`を利用します。
```dart
class getArea {
  static double circle(int radius) {
    return radius * radius * 3.14;
  }
  
  static double triangle(int bottom, int height) {
    return bottom * height / 2;
  }
  
  static int square(int bottom, int height) {
    return bottom * height;
  }
}
```

### 2-4. getter/setter
dartにはgetter/setterも用意されてます。それぞれ`get`、`set`で宣言すればOKで、よりカスタマイズ性のある変数を実装可能です。また、実は各インスタンス変数には暗黙的なgetter/setterが用意されております。
```dart
class square {
  double bottom = 0, height = 0;
  
  // getter
  get area => bottom * height;
  // setter
  set circumference(int value) {
    height = (value / 2) - bottom;
  }
}

main() {
  var s = square();
  s.bottom = 10;
  s.circumference = 26;
  print(s.area);  
}
```

### 2-5. override
クラス継承で抽象クラスを実装する場合、メソッドのオーバーライドは`@override`を用いて実装します。

```dart
abstract class Animal {
  void makeSound();
}

class Cat extends Animal {
  @override
  void makeSound() {
    print("Meow!");
  }
}

class Dog extends Animal {
  @override
  void makeSound() {
    print("Woof!");
  }
}

void main() {
  Animal cat = Cat();
  Animal dog = Dog();
  cat.makeSound(); // Meow!
  dog.makeSound(); // Woof!
}
```

### 2-6. 拡張メソッド
既存クラスに拡張メソッドを定義させることも可能です。
その場合、`extension <拡張クラス名> on <拡張対象のクラス>`で定義出来ます。また、拡張クラス名は省略可能で、その場合は同一ライブラリ内でのみ利用可能となります。

```dart
extension StringExtension on String {
  String reverse() {
    return split('').reversed.join('');
  }
}

void main() {
  String text = "Hello, world!";
  print(text.reverse()); // !dlrow ,olleH
}
```

### 2-7. callメソッド
callメソッドは通常のメソッドと少し異なり、クラスをインスタンス関数のように呼び出す場合に利用します。
これにより、クラスをインスタンス化せずに関数利用することが可能となり、コードをより簡潔に実装することが可能です。

```dart
class Greeting {
  final String name;
  Greeting(this.name);
  
  // callメソッド
  void call() {
    print("Hello, $name!");
  }
}

void main() {
  Greeting("Shinji")(); // Hello, Shinji!
}
```

## 3. 構文
メソッド内のロジックを実装する上で、様々な便利な構文が存在します。

### 3-1. 演算子
dartでは、その他プログラミング言語同様に主要な演算子が利用可能です。主なものとしては下記のとおりです。

- 代数演算子
    - `+` 加算演算子
    - `-` 減算演算子
    - `*` 乗算演算子
    - `/` 除算演算子
    - `%` 剰余演算子
- 比較演算子
    - `==` 等価演算子
    - `!=` 非等価演算子
    - `<` 小なり演算子
    - `>` 大なり演算子
    - `<=` 小なりイコール演算子
    - `>=` 大なりイコール演算子
- 論理演算子
    - `&&` 論理積演算子（かつ）
    - `||` 論理和演算子（または）
    - `!` 否定演算子

- 代入演算子
    - `=` 代入演算子
    - `+=` 加算代入演算子
    - `-=` 減算代入演算子
    - `*=` 乗算代入演算子
    - `/=` 除算代入演算子
    - `%=` 剰余代入演算子
- その他演算子
    - `??` null合体演算子
    - `?.` null安全アクセス演算子

また、演算子はカスタマイズ可能です。
下記の例では、Vectorクラスの加算演算子を`operator +()`を用いてカスタマイズした例となります。

```dart
class Vector {
  final int x;
  final int y;

  Vector(this.x, this.y);

  Vector operator +(Vector other) {
    return Vector(x + other.x, y + other.y);
  }
}

void main() {
  final v1 = Vector(1, 2);
  final v2 = Vector(3, 4);
  final v3 = v1 + v2;
  print('(${v1.x}, ${v1.y}) + (${v2.x}, ${v2.y}) = (${v3.x}, ${v3.y})');
}
```

### 3-2. 同期/非同期
非同期処理を用いることで、APIレスポンスやファイル読み込み等の待機時間を裏で実行させることが可能です。
具体的には、`Future`を用いて非同期処理を宣言し、`async`と`await`で非同期処理完了を待つことが可能です。

```dart
void main() async {
  print('1');
  await Future.delayed(Duration(seconds: 1), () => print('2'));
  print('3');
}
```

### 3-3. `=>`アロー関数
１行で完結するような簡単な関数は、アロー関数が利用できます。
```dart
void main() {
  final numbers = [1, 2, 3, 4, 5];
  final squareNumbers = numbers.map((n) => n * n);
  print(squareNumbers); // [1, 4, 9, 16, 25]
}
```

### 3-4. `...` スプレット演算子
リストやmap等のコレクションを展開して利用する場合、スプレット演算子が利用できます。(>=dart v2.3)
```dart
void main() {
  final list1 = [1, 2, 3];
  final list2 = [4, 5, 6];
  final mergedList = [...list1, ...list2];
  print(mergedList); // [1, 2, 3, 4, 5, 6]
  
  final map1 = {'a': 1, 'b': 2};
  final map2 = {'c': 3, 'd': 4};
  final mergedMap = {...map1, ...map2};
  print(mergedMap); // {a: 1, b: 2, c: 3, d: 4}
}
```

### 3-5. `..` カスケード演算子
一つのオブジェクトに対して複数のメソッドやプロペティを適用させる場合に有効です。

```dart
class Person {
  String name;
  int age;
  
  Person(this.name, this.age);
  
  void sayHello() {
    print('Hello, my name is $name');
  }
  
  void celebrateBirthday() {
    age++;
  }
}

void main() {
  final person = Person('Alice', 30)
    ..celebrateBirthday()
    ..sayHello();
}
```