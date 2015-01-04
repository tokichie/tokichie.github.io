---
layout: post
title: "デザインパターン勉強するぞい - 最終日"
date: 2015-01-04 20:42:25 +0900
comments: true
categories: study
---

## Adapterパターン
今日はAdapterパターン。あと今日で冬休みもおしまいなのでとりあえず最終回。また他のパターン勉強する気になったら書くかも。  
Adapterパターンは，既存のクラスを変更することなく異なるインタフェースを持たせることができるというパターン。イメージとしては普通にカードアダプタとかのイメージ（形状の異なる複数種類のカードから，中にあるデータを取り出す）でいいと思う。継承と委譲の2パターンがあるぽいのでどっちも書いてみる。
<!-- more -->

### 継承を利用したAdapterパターン
とりあえず今日もクラス図書いた。なんか昨日のやつ間違ってたのに気づいた（`interface`も全部`class`になってる）けど，めんどいのでいいかな。  
![継承によるAdapterパターン](/images/post/adapter_extend.png)  

もうお正月ネタ考えるのも面倒なので，Adapterを作成する既存クラスは適当に人物を表すものにしてみる。ということで，
``` java Person.java
public class Person {
  private int age;
  public int getAge() {
    return age;
  }
}
```
アイドルとか一部の有名人は，よく自分の年齢を詐称する傾向があるので，本当の`age`が返ってくると都合の悪い場合がある。そこで，詐称した年齢を返す`interface`を作ることにする。
``` java PersonFalseAge.java
public interface PersonFalseAge {
  public int getFalseAge();
}
```
で，年齢を詐称したい人はこの`interface`を実装した`Adapter`を用いればよいということになる。
``` java PersonAdapter.java
public class PersonAdapter extends Person implements PersonFalseAge {
  public int getFalseAge() {
    return this.getAge() - 5; //5歳若く見せたい
  }
}
```
以上が継承を利用したAdapterパターンである。

### 委譲を利用したAdapterパターン
![委譲によるAdapterパターン](/images/post/adapter_delegate.png)

クラス図では`abstract class`を`extends`するようになっているが，これは`interface`を`implements`で代用できるので，`PersonAdapter`クラスだけ書き換えればよい。
``` java PersonAdapter.java
public class PersonAdapter implements PersonFalseAge {
  private Person person = new Person();
  public int getFalseAge() {
    return person.getAge() - 5;
  }
}
```

既存のライブラリのクラスを再利用したい時とかに自前でAdapterクラスを作れば柔軟な使い方ができるみたいな感じだろうか。
