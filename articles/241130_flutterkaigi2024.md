---
title: "FlutterKaigi2024まとめ"
emoji: "🗂"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["flutter", "dart"]
published: false
---

# FlutterKaigi2024 セッションまとめ

## Day 0
https://www.youtube.com/watch?v=gIHP4aU9aEM&t=4246s

#### [LT] Flutterを言い訳にしない！〜アプリの使い心地改善テクニック5選〜

- リストに動画を表示する際、動作がカクつく＆端末が熱くなる
    - TextureWidget(video_player)→PlatformView(native_video_player)を利用する
- 画像をクロップする処理が重い
    - dartでクロップ処理を行っている為、ライブラリでは画像にせずにRGBAバイトをそのまま返すようにライブラリを改造
- iPhoneの時計のところをタップしてもリストが一番上に戻らない
    - 時計タップイベントがScaffoldのPrimaryScrollControllerへ通知されるので、ListView独自で持っているScrollControllerへは通知されない


#### [LT] より良いLint設定を追い求めて

- linterルールの最新は https://github.com/dart-lang/site-www/blob/main/src/_data/linter_rules.json に格納されている。
- 通常の開発では有名なLintPackageを利用するのが手っ取り早い、lintルールをより正確に最新化させたい場合は独自にてlintルールをカスタマイズさせる必要がある。


#### [LT] CustomMultiChildLayoutを使って、あなたの思い描く自由なレイアウトを作ろう！

- CustomMultiChildLayoutとは
    - 複数のWidgetのレイアウトを制御し、任意の位置に配置する
    - ColumnやRow、Stackのようなものを自作可能
    - Widget毎にIDを割り振るので、ID毎に条件指定も可能
    - 親のWidgetサイズを考慮して、子どものWidgetサイズや位置を指定できる
- MultiCustomLayoutDelegate
    - childrenの制御条件を指定
- performLayout
    - MultiCustomLayoutDelegateのメソッドで具体的な条件を指定
    - サイズや位置も指定
- shouldReLayout
    - レイアウトの再計算を行うか指定


## Day 1

### A Dash
https://www.youtube.com/watch?v=QQqW67kDqxs&t=28s

#### Dart & Flutter Team Technical Program Manager @Kevin Chisholm

#### キャンセルします！処理を @Masatoshi Tsushima

#### ImpellerとSkiaについて @mori

#### Figma Dev Modeで変わる！Flutterの開発体験 @よーたん @ゆめみ

#### Flutterによる効率的なAndroid・iOS・Webアプリケーション開発の事例 - スタディサプリ for SCHOOL @若宮浩司

#### アニメーションを最深まで理解してパフォーマンスを向上させる @みね


### B Dash
https://www.youtube.com/watch?v=Guljzr9a3jg&t=9339s

#### ネットスーパーがスクリーンリーダーに対応した話 ~あるいはアクセシビリティ向上によるユーザー獲得~ @futabooo

#### Flutterアプリで可用性を向上させたFeatureFlagの運用戦略とその方法 @batch

#### ステートマシンで実現する高品質なFlutterアプリ開発 @そた

#### 実践的パッケージ戦略 @KyoheiG3

#### キャッシュレス決済アプリでのFlutterの部分的採用から全面採用まで @Ren Edakawa

#### Shorebirdを活用したFlutterアプリの即時アップデート：Code Pushの実践と可能性 @Masahiro Aoki

#### [LT] Master of Isolate @新垣清奈

#### [LT] Flutterテスト戦略の再考 〜品質と効率のバランスを求めて〜 @としき

#### [LT] 僕のstate restorationアカデミア @金ちゃん

#### [LT] Dart Native Assets で広がる開発の幅 @ぎもちん


## Day 2

### A Dash
https://www.youtube.com/watch?v=CE0NTGJLbS8&t=21s

#### Flutterアプリにおけるユーザー体験の可視化と計測基盤構築 @おさたく

#### 体験！マクロ時代のFlutterアプリ開発 @ちゅーやん

#### OS 標準のデザインシステムを超えて - より柔軟な Flutter テーマ管理 @ronnnnn

#### Monetizing Your Indie Flutter App to $1k in Monthly Revenue and Beyond @Jeffrey Bunn

#### Seamless Flutter Native Integration: FFI & Pigeon @Dreamwalker

#### DevTools Extensions で独自の DevTool を開発する @Koki Yoshida

#### SliverAppBarはなぜ変化する？ ~ Sliverを内側から理解する ~ @Aoi Umigishi



### B Dash
https://www.youtube.com/watch?v=zqDJPyHSFV8&t=15s

#### WasmがFlutter on the Webにもたらす変化 @akaboshinit

#### 出前館アプリにおけるFlutterアプリ設計とそれを支えるCICD環境の進化 @廣部貴徳

#### 気をつけたい！Desktop対応で陥りやすい罠とその対策 @後藤 孝輔

#### Flutterと難読化 @chigichan24

#### Firebase Dynamic Links終了に備える：FlutterアプリでのAdjust導入とディープリンク最適化 @techiro

#### マッチングアプリ『Omiai 』のFlutterへのリプレイスの挑戦 @Kosuke Saigusa

#### Effective Form ~ Flutterによる複雑なフォーム開発の実践 ~ @たまねぎ

