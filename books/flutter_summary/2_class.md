---
title: "class"
---

## 1. classの種類
dartのclassにはclassをコントロールするための様々な修飾子があります。

### 1-1. classの基本形
dartのclassの基本形は次のとおりです。

```dart
class Person {
  int age = 20;
  String name = 'Taro';
}
```

### 1-2. private
private修飾子は無く、dartファイル外からアクセスを制限したい場合はclass名の先頭に`_`を付与します。

```dart
class _Person {
  int age = 20;
  String name = 'Taro';
}
```

### 1-3. abstract
抽象的なclassを作成したい場合、`abstract`が利用できます。抽象クラスなので、クラスメンバーを実装することは出来ません。
実装は継承先にて行われます。
```dart
abstract class Animal {
  void makeSound();
}

class Cat extends Animal {
  @override
  void makeSound() {
    print('にゃー!');
  }
}

void main() {
  var cat = Cat();
  cat.makeSound(); // にゃー!
}
```

## 2. class利用方法
dartのclassは様々な利用が可能です。主なものとしては、下記の通りです。

### 2-1. 継承
継承は`extends`で継承元を指定します。
メソッドのオーバーライド時はアノテーション`@override`を付与し、親classのロジックは`super.`で呼び出しされます。
```dart
class Animal {
  void makeSound() {
    print('♪♪♪');
  }
}

class Dog extends Animal {
  @override
  void makeSound() {
    super.makeSound();
    print('ワンワン');
  }
}

void main() {
  Dog dog = Dog();
  dog.makeSound(); // ♪♪♪ ワンワン
}
```

### 2-2. インターフェース実装
abstract classで宣言した抽象的クラスを実装したい場合、`implements`によりインターフェース実装することが可能です。
```dart
abstract class Animal {
  void makeSound();
}

class Dog implements Animal {
  @override
  void makeSound() {
    print('ワンワン');
  }
}

void main() {
  Dog dog = Dog();
  dog.makeSound(); // ワンワン
}
```

### 2-3. immutable（不変）
`@immutable`アノテーションをクラスに付与することで、不変classとして扱われます。
immutableでは、当然ながら可変メンバーを扱うことは基本的に出来ません。（アノテーションを付与して可変クラスとしても、WARMが出るだけでビルドは通る…）
immutableクラス内での変数、コンストラクタ修飾子はfinal、constとなります。
```dart
@immutable
class ImmutablePoint {
  final int x;
  final int y;

  const ImmutablePoint(this.x, this.y);
}

void main() {
  const point = ImmutablePoint(1, 2);
  print(point.x); // 1

// コンパイルエラー
//   point.x = 10; 
}
```

### 2-4. mixin
クラス間に継承などの上下の関係はなく、単にクラスのコードを再利用したい場合は`mixin`を用います。
`mixin`は複数クラスの指定も可能です。

```dart
class Walker {
  void walk() {
    print('歩く');
  }
}

class Swimmer {
  void swim() {
    print('泳ぐ');
  }
}

class Duck with Walker, Swimmer {}

void main() {
  Duck duck = Duck();
  duck.walk(); // 歩く
  duck.swim(); // 泳ぐ
}
```

### 2-5. 拡張class
プリミティブなクラスにカスタム要素を加えたい場合、拡張classが有効です。
`on`で拡張元を指定し、カスタム要素を追加していきます。
```dart
extension IntExtension on int {
  int squared() {
    return this * this;
  }
}

void main() {
  int num = 5;
  int result = num.squared();
  print(result); // 25
}
```

## 3. コンストラクタ
classをインスタンス化する際、コンストラクタを用いてクラスの初期設定を決めることが出来ます。

### 3-1. コンストラクタの基本形
コンストラクタはclass名と同じメソッドを利用することで定義できます。
コンストラクタの引数について、`this`を用いることで、インスタンス作成時の引数をそのままメンバ変数にセットすることも出来ます。
```dart
class Person {
  // コンストラクタ
  Person(this.name, this.age,);

  String name;
  int age;
}

void main() {
  Person p = Person('Shinji', 30);
  print(p.name); // Shinji
}
```

コンストラクタの引数は必須や任意の指定が可能です。
任意としたい場合は`{}`で囲って宣言し、呼び出し側では引数指定の上でセットさせます。
`{}`で囲っても、`required`修飾子を付与すれば必須項目となります。（呼び出し側で引数指定させたい場合向け）

```dart
class Hoge {
  Hoge(this.paramA, // {}で囲まなければ必須
       { required this.paramB, //　requiredで必須
         this.paramC = 0, // 任意の場合は初期値が必要
         this.paramD, // 変数がnull許容であれば初期値は不要
       });
  int paramA;
  int paramB;
  int paramC;
  int? paramD;
}

void main() {
  Hoge hoge = Hoge(1, paramB: 2);
  print(hoge.paramA + hoge.paramB + hoge.paramC); // 3
}
```

コンストラクタの基本は上の通りであり、コンストラクタは下記の利用も可能です。

### 3-2. 名前付きコンストラクタ
1つのclassにコンストラクタを複数作成したい場合は名前付きコンストラクタが有効です。コンストラクタに`.`で名前を繋げることで宣言が出来ます。

```dart
class Area {
  // 名前付きコンストラクタ（square）
  Area.square(this.bottom, this.height) {
    area = (bottom * height) as double;
  }
  // 名前付きコンストラクタ（triangle）
  Area.triangle(this.bottom, this.height) {
    area = bottom * height / 2;
  }
  int bottom;
  int height;
  double? area;
}

void main() {
  Area a = Area.square(5, 10);
  Area b = Area.triangle(5, 10);
  print(a.area);
  print(b.area);
}
```

### 3-3. 親クラスコンストラクタ
親クラスのコンストラクタを同時に呼び出したい場合、コンストラクタの後に`:`で親クラスのコンストラクタを指定することも可能です。
また、flutterのv3以降はスーパーイニシャライザパラメータが利用可能となります。これは、引数の値をそのまま親クラスへ橋渡しする機能となります。
```dart
class Animal {
  Animal(this.name);
  
  String name;
}

class Dog extends Animal {
  Dog(
    super.name,  // スーパーイニシャライザパラメータ
    this.breeder,
  ) : super();  // 親クラスのコンストラクタを呼び出し
  
  String breeder;
}

void main() {
  var dog = Dog('koro', 'Shinji');
  print(dog.name);
  print(dog.breeder);
}
```

### 3-4. リダイレクトコンストラクタ
名前付きコンストラクタで複数の類似したコンストラクタを指定した場合、コンストラクタの後に`:`で別のコンストラクタを指定することも可能です。
```dart
class Rectangle {
  int width;
  int height;

  Rectangle(this.width, this.height);

  // リダイレクトコンストラクタ
  Rectangle.square(int size) : this(size, size); 
}

void main() {
  var rectangle = Rectangle.square(5);
  print(rectangle.height);
  print(rectangle.width);
}
```
### 3-5. 定数コンストラクタ
不変のオブジェクトを生成する場合、コンストラクタに`const`を付与することが出来ます。この場合、変数はすべて`final`か`const`で定義する必要があります。
また、classのアノテーションに`@immutable`を付与することも可能です。

```dart
@immutable　// immutable classの宣言
class Circle {
  final double radius;

  // 定数コンストラクタ
  const Circle(this.radius);

  double get area => 3.14 * radius * radius;
}

void main() {
  const circle = Circle(10.0);

  print(circle.area);
}
```

### 3-6. ファクトリコンストラクタ
上記までのコンストラクタの場合、常に新しいインスタンスを返してましたが、場合によっては既存インスタンスを返したくなる時もあります。その場合、ファクトリコンストラクタが有効です。
ファクトリコンストラクタでは、コンストラクタの修飾子に`factory`を付与し、コンストラクタ内で明示的にインスタンスを`return`する必要があります。また、コンストラクタ引数に`this`の指定は出来ません
下記はインスタンスのキャッシュとシングルトンパターンのサンプルです。

```dart
class CachedClass {
  // ファクトリコンストラクタ
  factory CachedClass(String name) {
    if (_cache.containsKey(name)) {
      // nameが_cacheに存在する場合、既存インスタンスを返却
      return _cache[name]!;
    } else {
      // nameが_cacheに存在しない場合、新規インスタンスを返却
      final instance = CachedClass._internal(name);
      _cache[name] = instance;
      return instance;
    }
  }

  CachedClass._internal(this.name);

  static final Map<String, CachedClass> _cache = {};
  final String name;
}
```
```dart
class SingletonClass {
  static final SingletonClass _instance = SingletonClass._internal();

  // ファクトリコンストラクタ（常に同一のインスタンスを返却）
  factory SingletonClass() => _instance;

  SingletonClass._internal();
}
```

(dart3移行のsealedやbase、final、interfaceは後で追記する)