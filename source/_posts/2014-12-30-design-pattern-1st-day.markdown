---
layout: post
title: "デザインパターン勉強するぞい - 1日目"
date: 2014-12-30 00:02:44 +0900
comments: true
categories: study
---

はてなブログ最近使ってないし使う予定もないので，イマドキでCoolらしいgithub pages + static site generatorに乗り換えてみた。
冬休み（存在しない説もあるが）を使ってデザインパターンをちょっとだけ勉強することにしたのでとりあえずそれについて書こうかと。

## Singletonパターン
今日はSingletonパターンについて。
一応断っておくとよくあるデザインパターン入門みたいな本で紹介されている順番とかは完全無視である。独断と偏見でパターン選んでるので。

### Singletonとは
Singletonは一枚札とかいう意味らしい。クラスのインスタンスが唯一無二であることを保証したいときに使うとか。
アプリ起動時にOAuth Tokenとかを取得して，アプリ内でずっと保持して使いまわしたいみたいな感じかな。

### サンプル
コンストラクタを`private`にするのが鍵みたい。
``` java TokenStore.java
public class TokenStore {
  private static TokenStore tokenStore = new TokenStore();
  private TokenStore(){}
  public static TokenStore getInstance() {
    return tokenStore;
  }
}
```
このコードの場合，`TokenStore`のインスタンスが生成されるのは`getInstance()`が呼ばれたとき。このタイミングは言語仕様で異なるらしいのでJava以外は調べてみてね。

### バッドプラクティス
singletonのsの字も知らなかった頃に書いたウ●コード
``` java TokenStore.java
public class TokenStore {
  private static TokenStore tokenStore = new TokenStore();
  private static String token;
  public static void setToken(String token) {
    this.token = token;
  }
  public static String getToken() { return token; }
}
```
そもそもインスタンスなんていらなくね？という発想で全部`static`になっているがコンストラクタは`public`なのでつくろうと思えばいくらでもインスタンスが作れてしまう。カッコ悪い。

