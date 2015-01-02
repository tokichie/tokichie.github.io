---
layout: post
title: "デザインパターン勉強するぞい - 4日目"
date: 2015-01-02 21:33:31 +0900
comments: true
categories: study
---

## TemplateMethodパターン
昨日は元日だったのでお休み。で今日はTemplateMethodパターン。FactoryMethodの簡易版？みたいな位置づけなのでこっちを先にやるべきだった説はある。  
TemplateMethodパターンは，おおまかな処理の流れを`abstract`なクラスで定義しておいて，具体的な中身はそれを継承した具体クラスに任せるというもの。

### 例
さて，お正月ネタももういいだろ…という気分だけど何かと便利なので今日は年賀状の文面作成機を作ることにする。
年賀状といえば，その年の干支を背景に新年を祝う一言とちょっとした文章を入れればとりあえず出来上がりである。今回は普通の年賀状とオタク年賀状の2種類を作ってみる。
<!-- more -->

ということでテンプレートはこんな感じ。
``` java NewYearCard.java
public abstract class NewYearCard {
  public String setMainMessage();
  public String setSubMessage();
  public Image setBackground();
  public void createNewYearCard() {
    Postcard card = new Postcard();
    card.printImage(setBackGround());
    card.print(setMainMessage());
    card.print(setSubMessage());
  }
}
```
個別の年賀状クラスで`setXXX`メソッドをオーバーライドしておけば，そのクラスの`createNewYearCard`メソッドを呼び出すだけでカスタマイズされた年賀状ができるというワケ。  
正直この程度の例では普通に引数として渡せばいいじゃん・・・と思うけどそれは気にしたら負けね。

``` java NormalNewYearCard.java
public class NormalNewYearCard extends NewYearCard {
  @Override
  public String setMainMessage() {
    return "謹賀新年";
  }
  @Override
  public String setSubMessage() {
    return "本年もよろしくお願い申し上げます\n平成二十七年 元旦";
  }
  @Override
  public Image setBackground() {
    return ImageIO.read(new File("normal_sheep.png"));
  }
}
```
``` java OtakuNewYearCard.java
public class OtakuNewYearCard extends NewYearCard {
  @Override
  public String setMainMessage() {
    return "新春";
  }
  @Override
  public String setSubMessage() {
    return "二〇一五";
  }
  @Override
  public Image setBackground() {
    return ImageIO.read(new File("otaku_sheep.png"));
  }
}
```

最後にそれぞれの年賀状クラスのインスタンスを作って`createNewYearCard()`を呼び出すと年賀状の出来上がり。
``` java
NewYearCard normalCard = new NormalNewYearCard();
NewYearCard otakuCard = new OtakuNewYearCard();
normalCard.createNewYearCard();
otakuCard.createNewYearCard();
```

※イメージ  
![通常年賀状](/images/post/jp15t_et_0001.png)  
![オタク年賀状](/images/post/jp15t_px_0003.png)
