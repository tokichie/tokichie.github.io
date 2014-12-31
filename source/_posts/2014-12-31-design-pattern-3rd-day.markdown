---
layout: post
title: "デザインパターン勉強するぞい - 3日目"
date: 2014-12-31 22:30:52 +0900
comments: true
categories: study
---

## FactoryMethodパターン
今日はFactoryMethodパターン。ジャバ使ってるとよくなんとかFactoryというのを見る気がするので調べてみた。
FactoryMethodパターンは，インスタンスの生成をサブクラスに委託することで，柔軟なインスタンス生成を可能にし，クラスの再利用性を高める。
クラス図があったほうがわかりやすいのかと思ったけど作るのがめんどくさいので調べてどうぞ。

### 例
昨日はお雑煮を例にしたので，今日はおせちにする。ということで自家製おせちを作ることを考える。
<!-- more -->

**おせち**  
さて，おせちに入っている料理は一概に「おせち料理」という括りでまとめることができるが，その調理法は焼く，煮る，漬けるなど様々である。
一人で全てを作っていては効率が悪いので，調理法ごとに別々の料理人が作っているものと考える。焼く人が`RoastingCook`，煮る人が`BoilingCook`という具合である。
とはいえ料理人は料理人なので，全ての料理人をまとめる抽象クラス`Cook`を作る。
`Cook`はおせち料理を作るメソッド`cookOsechiDish`を持っている。

``` java Cook.java
public abstract class Cook {
  public OsechiDish cookOsechiDish(Food ingredient) {
    return cook(ingredient);
  }

  // これがFactoryMethod - 料理人によって調理法が変わる
  protected abstract OsechiDish cook(Food ingredient);
}
```

次に「おせち料理」という括りを表すインタフェース`OsechiDish`を作る。
``` java OsechiDish.java
public interface OsechiDish {
  public void eaten();
}
```

これらを用いて焼き物専門の料理人，焼き物のクラスを作る。（他の調理法も同様）
``` java RoastingCook.java
public class RoastingCook extends Cook {
  @Override
  protected OsechiDish cook(Food ingredient) {
    OsechiDish roastedDish = new RoastedOsechiDish(ingredient);
    return roastedDish;
  }
}
```
``` java RoastedOsechiDish.java
public class RoastedOsechiDish implements OsechiDish {
  public RoastedOsechiDish(Food ingredient) {
    // constructor
  }
  public void eaten() {
    dropSoySause(); //醤油をかけてみる
  }
}
```

さて，これで全部実装できたので，おせち料理を作ってみる。
``` java
OsechiDish roastedLobster = new RoastingCook().cookOsechiDish(new Lobster());
OsechiDish nimame = new BoilingCook().cookOsechiDish(new Kuromame());
```
ということで焼いたロブスターと黒豆の煮豆ができた。

実装した感想：なんか例に無理がありすぎる気がしてきた。

