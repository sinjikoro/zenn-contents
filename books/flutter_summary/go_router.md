---
title: "go_router"
---

## 基本の使い方
次のように、アプリ内のscreen構成に合わせてGoRouterを宣言する

```
final _router = GoRouter(
    routes: [
        GoRoute(
            path: '/',
            builder: (context, state) => const Page1Screen(),
        ),
        GoRoute(
            path: '/page2', // GoRouterで扱うpathは大文字/小文字判定しない
            builder: (context, state) => const Page2Screen(),
        ),
    ],
    // 初期位置
    initialLocation: '/',
);
```

MaterialAppであれば、↓の感じで使える
CupertinoAppでは、`CupertinoApp.router`が利用可能

```

// 
return MaterialApp.router(
    routerConfig: router,
);
```

> [info]
> GoRouteにあるstateでは、主にrouteのpath情報を取得可能
詳しくは[こちら](https://docs.page/csells/go_router/declarative-routing#router-state)


## ページ遷移

次ページへ移動（通常）
↓のどちらでも良い
```
// 通常版
onTap: () => GoRouter.of(context).go('/page2')

// 簡単版
onTap: () => context.go('/page2')
```

次ページへ移動（Stackする）
```
GoRouter.of(context).push('/page2')
context.go('/page2')
```

前ページへ戻る
```
GoRouter.of(context).pop()
context.pop()
```



## tips
現在位置取得（公式に載ってるけど使えない、、、）
```
class RouterLocationView extends StatelessWidget {
  const RouterLocationView({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    final router = GoRouter.of(context);
    return AnimatedBuilder(
      animation: router,
      builder: (context, child) => Text(router.location),
    );
  }
}
```

LinkWidgetに利用したい場合
```
Link(
  uri: Uri.parse('/page2'),
  builder: (context, followLink) => TextButton(
    onPressed: followLink,
    child: const Text('Go to page 2'),
  ),
),
```

