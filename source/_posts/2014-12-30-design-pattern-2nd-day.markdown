---
layout: post
title: "デザインパターン勉強するぞい - 2日目"
date: 2014-12-30 22:13:41 +0900
comments: true
categories: study
---

## Builderパターン
今日はBuilderパターンについて調べてみた。Builderパターンはオブジェクトの生成過程を抽象化して，動的なオブジェクト生成を可能にするパターンらしい。
Builderパターンでは「作成過程」を決定するDirectorクラスと，「表現形式」を決定するBuilderクラスの2つを利用する。

### 例
例がないとよくわからないので，もうすぐお正月だしお雑煮を作る過程について考えてみた。

**お雑煮**  
日本のお正月に欠かせない料理であるお雑煮は，地域によって具材や作り方にかなり差異があることが一般に知られている。<sup>\*1</sup>
ちなみにうちでは丸餅，焼かない，すまし汁，具材はするめ，かまぼこ，はまぐり，とまぁこんな感じである。  
パターンに当てはめればDirectorはお雑煮の調理手順を決定し，Builderが餅の形や餅を焼くかどうか，入れる具材，だしの種類を決めるという具合だろうか。

ということで次の2種類のお雑煮を作ることを考えてみる。  
お雑煮の調理手順は面倒なので「だしを作る→餅を作る→餅を焼く」の手順に固定した。

|餅の種類|餅を焼く|だしの種類|
|:------:|:------:|:--------:|
|丸餅    |`false` |すまし    |
|角餅    |`true`  |味噌      |

<!-- more -->
まずお雑煮クラスから
``` java Zoni.java
public class Zoni {
  public enum Mochi { MARU, KAKU }
  public enum Dashi { SUMASHI, MISO }
  private Mochi mochi;
  private boolean isBaked;
  private Dashi dashi;
  public void setMochi(Mochi m, boolean b) {
     this.mochi = m;
     this.isBaked = b;
  }
  public void setDashi(Dashi d) { this.dashi = d; }
}
```

次にインタフェース
``` java ZoniBuilder.java
public interface ZoniBuilder {
  public void makeMochi();
  public void makeDashi();
  public Zoni getZoni();
}
```

インタフェースを実装した具体クラス。角餅版は`setMochi(Zoni.Mochi.KAKU, true)`などと変更して新しく`YourZoniBuilder`みたいに命名しとこう。
``` java MyZoniBuilder.java
public class MyZoniBuilder implements ZoniBuilder{
  private Zoni zoni;
  public MyZoniBuilder() {
    this.zoni = new Zoni();
  }
  public void makeMochi() {
    this.zoni.setMochi(Zoni.Mochi.MARU, false);
  }
  public void makeDashi() {
    this.zoni.setDashi(Zoni.Dashi.SUMASHI);
  }
  public Zoni getZoni() {
    return this.zoni;
  }
}
```
最後にDirector（調理監督者）
``` java Director.java
public class Director {
  private ZoniBuilder builder;
  public Director(ZoniBuilder builder) {
    this.builder = builder;
  }
  public Zoni construct() {
    this.builder.makeDashi();
    this.builder.makeMochi();
    return this.builder.getZoni();
  }
}
```
さて，量が多くなってこんがらがってきたけど役者が揃ったので，やっとお雑煮が作れる。
``` java
Director myDir = new Director(new MyZoniBuilder());
Director yourDir = new Director(new YourZoniBuilder());
Zoni maruMochiZoni = myDir.construct();
Zoni kakuMochiZoni = yourDir.construct();
```
結局色々クラスとかインタフェースで抽象化することで，この4行で異なるお雑煮が作れたということになるのかな。
実装してみた感想としては正直あまりお得感が感じられなかった。この程度のサンプルコードでは当たり前かもしれないけど。

- - -
\*1. [実態調査による雑煮の地域的な特徴](http://ci.nii.ac.jp/naid/110001171552)

