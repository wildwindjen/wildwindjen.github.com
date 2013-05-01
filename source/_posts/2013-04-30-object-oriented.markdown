---
layout: post
title: "物件導向"
date: 2013-04-30 14:36
comments: true
categories: 
- Object-Oriented
- Java
---
## 總結
<pre>
「只重其義，不重其招，你忘記所有招式，就練成了！ 」「你記住了沒？」 --  張三豐。
</pre>

這篇是因為我必須要在部門裡面作教育訓練，對於物件導向要怎麼上手，整理出來的心得。重點不在於教物件導向是什麼，而是分享經驗，讓想學物件導向的人，知道如何少繞一點路，減輕一點混亂。

## 必須先知道的事
+ 物件導向，主角是觀念，是思維。專案開發過程都使用物件導向的思維去分析、設計、實作時，這就是導入物件導向。否則，就算使用了 Java、 Dot Net 世界上任何一種支援物件導向能力的語言，都不代表這個專案使用物件導向開發。
+ 需求、需求、需求。你的需求才是決定你採用什麼方式開發專案的唯一因素。
+ 除了繼承、封裝、多型這些物件導向的特性，應該更加注意`需求->各角色職責分工`。

## 跳躍門檻
從開始接觸物件導向到現在，覺得最大的門檻在於如何簡單說明類別與物件，在於多型如何一言以蔽之。一旦這兩個容易讓人混亂的概念容易被釐清，後面的路就好走多了(一路好走)。

所以，要先懂得區分「抽象的概念」與「具體的成品」。

拿安全帽來舉例，「法律規定，騎車要戴安全帽」和「你戴這一頂安全帽出門」，這兩個句子裡安全帽，意義上是不太一樣的。「法律規定，騎車要戴安全帽」的安全帽是一種統稱，泛指安全帽這`種類`的帽子，所以是一種抽象的概念。而「你戴這一頂安全帽出門」的安全帽，則是明確的指出一個實體，是一個具體的成品。

如果你區分得出來我說的這兩種安全帽邏輯上的不同，我們就安心地進入物件導向的世界吧。


## Java 裡的抽象和具體
在 Java 裡，我們使用類別(Class)或者介面(Interface)來定義抽象的概念，而依照類別產生出來的具體成品我們叫物件(Object)，依照介面打造而成的具體成品我們稱為實作(Implementation)。


## 類別(Class)
物件導向觀念裡的類別，簡單地說，就是分類出來的類型。就像是生物學裡的[生物分類法](https://zh.wikipedia.org/wiki/%E7%94%9F%E7%89%A9%E5%88%86%E9%A1%9E%E6%B3%95 "生物分類法")。屬於同一類型的東西，都有相同的特徵，相同的基礎能力。

如果要說這個類別跟生物學的種類差異點在哪裡的話，生物學的種類是將已存在的東西做分類，而程式的類別則是你先定義出這個類別的特徵與標準，然後再製作出來。這個就像是工廠製造，先刻一個模子出來，然後再去生產成品。{% img center /images/wildwind/object_oriented/class.jpg 600 350 'class and objects' 'class and objects' %}

所以，是的，你得先知道你想要的特徵跟分類定義，才能打造這個模子，你才能寫出這個類別。這代表，你得先圈畫出你想要解決的問題領域，然後從這個領域範圍裡蒐集所有的相關需求，最後從需求裡面作職責分工、分類。

程式的類別同時滿足了現實世界中人為分類跟生物分類的特性。人為分類滿足的就是先設計，再批次製造。生物分類重要的特性是演化，也就是程式中提到的繼承。


## 繼承(Inheritance)
物件導向觀念裡的類別跟真實世界的物種一樣，有遺傳的概念。在物件導向世界裡叫做繼承。不過跟這篇[繼承是父子關係？才怪！ 物件導向初學者應該要知道的事情(四)](http://milikao.pixnet.net/blog/post/543716-%E7%B9%BC%E6%89%BF%E6%98%AF%E7%88%B6%E5%AD%90%E9%97%9C%E4%BF%82%EF%BC%9F%E6%89%8D%E6%80%AA%EF%BC%81-%E7%89%A9%E4%BB%B6%E5%B0%8E%E5%90%91%E5%88%9D%E5%AD%B8%E8%80%85%E6%87%89 "繼承是父子關係？才怪！ 物件導向初學者應該要知道的事情(四)")作者一樣，我覺得遺傳的這個詞比較好。因為繼承是人為的，可以放棄繼承。但遺傳是天生的，後來衍生的種類是沒辦法放棄的。而物件導向的繼承你是不能放棄繼承的，一旦基礎類別(Base Class)有這個特徵，衍生類別(Derived Class )就必須有。這種特性比較像是遺傳。

但是在這裡我們還是隨波逐流，使用繼承一詞。

繼承是類別跟類別間的關係。衍生類別繼承了基礎類別，會有相同的特徵(feature)。接著子類別可以以這個為基礎，繼續擴充、發展自己的獨特特徵。衍生類別雖然不能放棄繼承，但是可以改造這個特徵。程式的世界叫做覆寫( Override)。舉例來說，你遺傳(extends)了你爸爸的單眼皮，你可以請專業醫美改造(override)成雙眼皮，但是你還是有眼皮(特徵)。又或者，人類遺傳了靈長類的特徵，但是又發展出「直立行走」、「語言」等特徵。

在物件導向的世界裡，透過繼承，是實現重用性(reusability)的方式之一。讓已存在的功能或流程得以被重複使用，而不用重複開發。


## 物件(Object)
物件依照類別創造出來的具體東西。ex: 這一頂安全帽。


## 類別與物件
物件有獨特性，而類別有共通性。共同的特徵集中在類別定義上，而物件可以針對各自的狀況加以客製化。所以常常有人拿設計圖與實際的產品來比喻類別和物件間的關係。只要類別變更，創造出來的物件就也會跟著變。


## 請問這些東西怎麼用？
{% img center /images/wildwind/object_oriented/usb.jpg 600 350 'How use these devices' 'How use these devices' %}
為什麼你知道這是 USB 隨身碟？因為你辨識得出來這些東西共同的地方，是 USB 共用的界面 -- 統一的規格。


## 介面(Interface)
用來規範程式必須滿足的功能或者界面。簡單地說，就是用來定義規格的玩意兒。ex: USB 的開發規格書。透過規格的定義，可以不用理會實作的細節，用的人只知道產品有支援某規格，就可以依照該規格定義的方法使用
這個產品。


## 封裝(Encapsulation)
使用的人不用管這個東西如何製造(Process)、實作，只要知道怎麼用它(Input)，用了它會有什麼結果就好(Output)。我們只要知道將 USB 插上電腦就能讓我們存取 USB 裡的資料就好，至於各家的 USB 電路怎麼設計，我們不用知道。這樣隨身碟廠商想要更換內部電路設計(改變實作方式)，不用通知我們。我們之間沒有藕斷絲連，這就是所謂的降低耦合度(decoupling)。

所以看得出來，介面主打的特性之一就是封裝。類別透過 private 屬性搭配 public getter/setter method，也能達到封裝的效果。


## 實作(implementation)
宣告遵守某個規格，並且開發/實作出(implements)該規格規範的功能。ex: 每一個實際的 USB 。都是 USB 規格的實作(成品)。


## 多型(polymorphism)
什麼叫多型？正所謂「多型不易，必自斃。」(哇咧供啥貨)。

這就是我下的定義：`以抽象之名，行具體之實。`一碼勝千言，這邊來看點程式碼，會更清楚。

{% codeblock People.java lang:java %}
    public class People {
        /**
         * <pre>
         * 喊叫
         * </pre>
         * @return
         */
        public String shout(){
            return "吼～～";
        }
    }
{% endcodeblock %}

{% codeblock Male.java lang:java %}
    public class Male extends People {
        /**
         * <pre>
         * 喊叫
         * </pre>
         * @return
         */
        public String shout(){
            return "凍咧！";
        }
    }
{% endcodeblock %}

{% codeblock Female.java lang:java %}
    public class Female extends People {
        /**
         * <pre>
         * 喊叫
         * </pre>
         * @return
         */
        public String shout(){
            return "嗯～";
        }
    }
{% endcodeblock %}

{% codeblock PeoplePolyDemo.java lang:java %}
    public class PeoplePolyDemo {

        List list = new ArrayList();

        public void addPeople(People people){
            this.list.add(people);
        }
        
        public void shoutTogether(){
            Iterator it = list.iterator();
            while(it.hasNext()){
                People people = (People)it.next();
                people.shout();
                System.out.println();
            }
        }
        
        public static void main(String[] args) {
            PeoplePolyDemo demo = new PeoplePolyDemo();

            demo.addPeople(new Male());
            demo.addPeople(new Femail());
            
            demo.shoutTogether();
        }

    }
{% endcodeblock %}
你會看到 shoutTogether 雖然是使用 People 類別執行 shout 動作。可是卻是叫出該性別真正的叫聲。只要你是人，我就可以請你叫，如果你是男人你就會叫出男人的聲音，如果你是女人，就會用女人的叫法。這就是我說的`以抽象之名，行具體之實`。身為使用者，只要管你是不是特定能力的基礎類別，至於物件怎麼執行，就看物件實際的狀況。這邊的抽象不只限於類別，也包含介面。


## Why 物件導向
+ 物件導向模擬了真實世界的特性，因為跟真實世界雷同，角色、職責分工，讓你容易理解、記憶。
+ 透過物種演化、合成，可以實現既有程式碼的重用性(reusability)。
+ 實踐封裝，讓各元件、角色彼此的合作，不再牽一髮而動全身。
+ ...

好吧，這樣太八股了，想要看上面這些優點，請自己去翻書吧。

為什麼要用物件導向？這個問題要問你自己，要看你的需求，你想要物件導向的特性解決你專案常發的甚麼問題？這就是你的需求。經驗告訴我，物件導向的特質會提高創建成本，還有人員教育門檻比較高，但是會降低專案長期維護成本。所以如果專案很小且急，而且不需要維護，我應該不會考慮使用物件導向。

各取所需。