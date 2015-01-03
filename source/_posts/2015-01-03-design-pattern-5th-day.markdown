---
layout: post
title: "デザインパターン勉強するぞい - 5日目"
date: 2015-01-03 23:24:23 +0900
comments: true
categories: study
---

## Iteratorパターン
イテレータってよく使うけどこれもデザインパターンだったんだーと思ったので調べてみた。

### クラス図
なんか今回は文章で説明するほうがめんどくさそうだったのでクラス図作った。  
![Iteratorパターン](/images/post/iterator.png)

`Aggregate`がイテレートする集合体のインタフェースを表す。

### 例
正直Iteratorは作るより使う方が圧倒的に多いのではないかと思うがとりあえず書いてみた。今日は千歳飴をイテレートしよう。
<!-- more -->

まずイテレータと千歳飴のインタフェースから。
``` java Iterator.java
public interface Iterator {
    public Object next();
    public boolean hasNext();
}
```
``` java ChitoseAme.java
public interface ChitoseAme {
  public Iterator iterator();
}
```
次にイテレータの具体的なクラスと千歳飴インタフェースを実装した金太郎飴クラスを作る。
``` java ChitoseAmeIterator.java
public class ChitoseAmeIterator {
  private ChitoseAme chitoseAme;
  private int index;
  public ChitoseAmeIterator(ChitoseAme ame) {
    this.chitoseAme = ame;
    this.index = 0;
  }
  public Ame next() {
    return chitoseAme.cut(index++);
  }
  public boolean hasNext() {
    if (chitoseAme.length() < index + 1) {
      return false;
    }
    return true;
  }
}
```
``` java KintaroAme.java
public class KintaroAme implements ChitoseAme {
  // 略
  public Iterator iterator() {
    return new ChitoseAmeIterator(this);
  }
}
```

雑だけどたぶんこんな感じ。千歳飴を例にしたのは失敗だった気しかしない。

