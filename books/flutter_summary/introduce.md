---
title: "概要"
---

## 1. flutterとは
flutterは”クロスプラットフォーム開発”における、メジャーなオープンソースフレームワークです。2018年12月にGoogleによりメジャーリリースが行われ、以降、着実にアプリ開発におけるシェアを伸ばしつつある。

アプリ開発でのその他選択肢としては、ネイティブ開発（swift、java、kotlin）、ReactNative（javascript+React）、lonic（javascript）等が存在。

特徴としては、下記のとおり。
- クロスプラットフォーム開発（iOS、Android、web、windows、macOS、Linux）が可能
- プログラミング言語はDartを利用（静的型付けのオブジェクト指向言語）
- Flutterは主にFlutterFrameworkとFlutterEngineで構成されており、FlutterFrameworkはUIアプリケーション、FlutterEngineはアプリケーションが実行されるランタイムエンジンです。（下図）
- FlutterでのUI構築はWidgetの組み合わせにて行われます。
- Flutterのレンダリングでは、ネイティブのプリミティブなUIアイテムは利用せず、RenderObjectによって画面上のピクセルを直接制御してUI制御を行います。
- デザインは主にMaterialDesign（または、Cuoertino）をベースとします。

![](https://docs.flutter.dev/assets/images/docs/arch-overview/archdiagram.png)

## 2. flutterの基礎

### 2-1. フォルダ構成
flutterをインストールし、`flutter create . -t app`とかすると、flutterのカウンタアプリが勝手に作成されるかと思います。
その際のフォルダ構成は下記のとおりです。

.
├─ .idea : AndroidStudioでの設定項目
├─ android : Androidプロジェクト。SDKがflutterコードをコンパイルし、このAndroidプロジェクトに注入（※基本的に触らない）
├─ build : ビルド時に生成、SDKがコンパイルした結果を出力（※基本的に触らない）
├─ iOS : iOSプロジェクト。SDKがflutterコードをコンパイルし、このiOSプロジェクトに注入（※基本的に触らない）
├─ lib : 作業フォルダ（触る）\
├─ linux : linuxプロジェクト。SDKがflutterコードをコンパイルし、このlinuxプロジェクトに注入（※基本的に触らない）\
├─ macos : macプロジェクト。SDKがflutterコードをコンパイルし、このmacプロジェクトに注入（※基本的に触らない）\
├─ test : testコードを格納（触って）\
├─ web : webプロジェクト。SDKがflutterコードをコンパイルし、このwebプロジェクトに注入（※基本的に触らない）\
├─ .metadata : アプリケーション構築のためのメタデータ（※基本的に触らない）\
├─ .packages : アプリケーションで利用するパッケージを管理（※基本的に触らない）\
├─ {project name}.iml : プロジェクト内の依存関係を管理（※基本的に触らない）\
├─ pubspec.lock : プロジェクト内で使用されるパッケージ情報を管理（※基本的に触らない）\
└─ pubspec.yaml : パッケージの依存関係、プロジェクト内で使用されるアセット情報（フォント、画像）を管理\

実際に開発で触っていくのは、lib/main.dartとなります。
この中の、下記がソース実行時のエントリポイントとなります。
```dart
void main() {
  runApp(const MyApp());
}
```

### 2-2. Widget
FlutterのWidgetは主に４種類あり、基本的に利用するWidgetはStatelessWidget、StatefulWidgetです。
- StatelessWidget : 状態を持たないWidget。データはコンストラクタ経由でinput可能。
- StatefulWidget : 状態を持つWidget。状態に応じてUI再描画が掛かる。
- InheritedWidget : 状態を管理し、適切にBuildをかけてくWidget（状態管理パッケージ Provider、Riverpodが裏で利用してる）
- RenderObjectWidget : UIレンダリングするWidget

