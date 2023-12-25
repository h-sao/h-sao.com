+++
date = "2023-12-25T00:30:00+09:00"
draft = false
title = "[C++] std::variantを使ってシミュレータでオートプレイさせる"
tags = ["cpp"]
+++



> この記事は [C++ Advent Calendar 2023](https://qiita.com/advent-calendar/2023/cxx) 24日目の記事です。

ゲームドメインでのお話です  
少し前にC++でAIシミュレータ作ってオートプレイさせてみたので、どんだけの人に刺さる内容かは判らないですが  
自分の振り返り的に紹介します

前半が、シミュレータについての話で、  
後半が、オートプレイの時に `std::variant` が良い感じだった話  
です

記載量が多くなってしまいました  
前半はC++に直接関係のない話なので、読み物として見て頂ければと思います！  

## シミュレータとは

シミュレータと聞いてピンとくる方は少ないと思います  
実際、自分自身もあまり意味が判っていませんでした

きっかけは、ゲームで機械学習を取り入れるには、シミュレータが必要と聞いたことが始まりです  

自分なりにまとめてみると、シミュレータには観点が３つあることに気が付きました  

- Ai学習
- テスト
- リプレイ

### ■ AI学習観点

- 誰かがやったログを再現して、それを学習する
- 自分自身でなんらかのオートプレイを行って、それを学習する


何らかの学習系AIを導入しようと思ったら、必ずシミュレータが必要になります  
そして学習するだけでなく、シミュレータでのオートプレイが出来ると以下のように色々と夢も広がります

### ■ テスト観点 

- オートプレイしてくれるってことは、エージングテスト（長時間のオートテスト）出来るね
- プレイしたログを保存し、それを再生したらデグレテストに使えるね（回帰テスト）

こんな感じで  
シミュレータがあると、テスト関係の人たちも喜ぶわけです

### ■ リプレイ観点

- プレイログを保存しておけば、録画再生みたいに後からでも楽しめるね！
- 同期再生にも有効（クライアント間やサーバーとの整合性が取れる）


つまりシミュレータを作ることが出来たら、

- AI学習
- テスト
- リプレイを用いたゲームのプランニングや設計思想

に有効なネタとなりそうです

## シミュレータを作るのはちょっと工夫が必要

実際にシミュレータを作るには  
周りからの理解も必要であり、性能面でもシビアな要求が求められます

> 例えば、学習系のAIモデルを作るためのシミュレータであれば、億の回数くらいのバトルはしゃっと実行して欲しいところです  
1バトルシミュレーションするのに1分かかります～では話にならないのです  
それだと2日経っても3000バトルすらこなせないことになります

そもそも、「●●を入力したら必ず▲▲の結果になること！」という **決定論的シミュレータ** を作るのは
最初から設計を考えておく必要があり、後付けでは難しいです

また、キー情報を入力値としてリプレイさせるのは昔からよくある手法だと思いますが  
後々のこと（学習系で利用）を考えると、入力値は「キー」ではなく、「状態」が好ましいです

簡単にターン制バトルで言うとこんな感じ


```
＜入力＞
「勇者」が「鉄の盾」「鉄の剣」を装備して
「はぐれメタル」に「剣で攻撃」をした  
（攻撃力１０）

＜それを受けた出力＞
「はぐれメタル」が（内部防御力９で）
「ダメージ－１」を受けた  
（そして逃げるフラグが立った）  
※（カッコ）の中は内部パラメータなので、ゲーム画面では見えない情報
```

こんな感じで「バトルを直接的に変化させる内部状態を入出力の対象にする」のが良いです  
「状態」であれば学習系でも利用できるし、何より人が見ても読めます（キー入力よりは…）

> →実際に目検で状態を読むのは困難ではありますが…  

他にも大量のログを管理する方法なども含めて「シミュレータそのもの」を作るコツはたくさんあるのですが  
今回の話題からはちょっと外れるのでこのへんにしときます

## 作ったシミュレータを活躍させる方法

ヤッター！シミュレータが出来たぞー  
同じログを入力したら、同じ結果になったぞー！

と喜んでいるのもつかの間…

苦労して作ったのですが、もちろんそれだけではとある部品が出来ただけなので、何にも良いことはありません

何かの目的があってシミュレータを作ったのでしょうから、それに向かって突き進むべきなのですが  
最初に成果が出やすくて、チームに役に立ったと思ってもらえるのは  
上に挙げた３つ（Ai学習/テスト/リプレイ）の中でも特に、**テスト観点での活用** が良いように思います

テスト観点であれば、本体プロダクトとは別で話を進めやすく、またシミュレータ作成側のエンジニアのみで作業の完結も出来るので、おすすめです

そこでやっと今回の本題に来ました

**オートプレイ** です！

強化学習にしろ機械学習にしろ、オートプレイは必須！  
それがテストにも転用できる！

となれば、時間のかかる学習系を粛々と進めつつ  
直近はテスト目的でシミュレータを活躍させるのが良いと思います

> →もちろんプランナーが乗り気でリプレイ機能を求めているなら、そっちを作ってください

みんなが学習系に乗り気で、結果をすぐに出したいと思っていても  
残念ながらなかなか早期に学習系の結果は出ないのです…  
R&D要素の大きく、学習させても期待値に満たない可能性もあります

なのでAI学習目的でシミュレータを作り始めたとしても、テスト活躍案は持っておいた方が良いです

しかもテストでの実績が目に見えて出てくると  
リプレイ観点の設計もぐっと提案しやすくなり、録画機能やチート対策などにも話題が広がると思います

> →このリプレイ観点の話題は、GDCセッションでもよく出てます

（本当は学習系で成果がポンと出せるのが一番良いのですがね…）

> ただし、テスト観点のみで走り進めてしまうと、リプレイ出来ない設計になってしまうこともあります  
「テストのためにシミュレータでオートプレイをする」のと  
「学習のためにシミュレータでオートプレイをする」のと  
「リプレイのためにシミュレータでオートプレイをする」のとだと  
結果が似てるようで設計思想が異なるので、良い感じの和集合設計を意識しながら作るのもエンジニアの腕の見せ所ではあります


## オートプレイの実装１（とりあえず動作した版）

今回はＣ＋＋のアドカレ記事で、やっとＣ＋＋の話が出てきます  
長い前置きでしたが、お待たせしました

＜前提＞

- ターン制バトル
- 敵も味方も、色んな技をいっぱい出し続けたい
  - 変な挙動が起きないか？ストップバグなどが無いか？を見る
- 技を出すことが目的なので、自分も相手も無敵モード
- 技を出し尽くしたら、装備を変えて、次のバトルへ遷移する

状況はこんな感じで、実際に実装してみます

＜実装例＞

`main()` で動作するように、1ターンだけオートプレイとして動作するバージョンを例に書いてみました  
Conpiler Explorerに全く同じものをアップしています

- 手続き的に実装した版 - Conpiler Explor  
[https://godbolt.org/z/hxPjz49ab](https://godbolt.org/z/hxPjz49ab)

```cpp
// ---------------------
// main.cpp

#include <functional>
#include <iostream>
#include <vector>
#include <memory>

enum class CharactorId
{
    Player,       // プレイヤー
    FriendAlice,  // 味方
    FriendBob,
    EnemyEve,     // 以下は敵
    /* ... */
};

// 技の種類
enum class OrderType
{
    Attack,    // 攻撃
    Buff,      // バフ
    Summon,    // 召喚
    /* ... */
};

// 実際にそのターンでどんな技を出すのかの情報(★0)
class AutoPlayOrder
{
public:
    AutoPlayOrder(CharactorId id, OrderType type)
    {
        charactorId_ = id;
        CurrentOrderType = type;

        // enumでのタイプ分け(★1)
        switch (CurrentOrderType)
        {
        case OrderType::Attack:
            hitPoint_ = 10;     // default
            ExecuteFunction = [this]() {ExecuteAttack(); };
            break;
        case OrderType::Buff:
            buffPoint_ = 5;     // default
            ExecuteFunction = [this]() {ExecuteBuff(); };
            break;
        case OrderType::Summon:
        default:
            summonFriendId_ = CharactorId::FriendBob;   // default
            ExecuteFunction = [this]() {ExecuteSummon(); };
            break;
        }
    }

    OrderType CurrentOrderType;
    std::function<void()> ExecuteFunction;
    void ExecuteAttack() const { std::cout << "Attack:" << hitPoint_ << "\n"; }
    void ExecuteBuff() const { std::cout << "Buff:" << buffPoint_ << "\n"; }
    void ExecuteSummon() const { std::cout << "Summon:" << (int)summonFriendId_ << "\n"; }

private:
    CharactorId charactorId_;

    // 技によって必要なメンバ変数が異なる(★2)
    int hitPoint_;                 // orderType == Attack なら、攻撃力
    int buffPoint_;                // orderType == Buff なら、バフ量
    CharactorId summonFriendId_;   // orderType == Summon なら、一緒に召喚する味方ID
    /* ... */

};

// オートプレイの取りまとめクラス
class AutoPlay
{
public:
    std::vector<std::shared_ptr<AutoPlayOrder>> OrderList;
};

int main()
{
    std::cout << "===AutoPlay===\n";

    auto autoPlay = std::make_unique<AutoPlay>();

    // 今回は、とあるターンでの技が３つあったとする
    // 何の技が出せるのかは、今出している技を見たりするので結構複雑な処理になる(★3)
    autoPlay->OrderList.emplace_back(std::make_shared<AutoPlayOrder>(CharactorId::Player, OrderType::Attack));
    autoPlay->OrderList.emplace_back(std::make_shared<AutoPlayOrder>(CharactorId::FriendAlice, OrderType::Summon));
    autoPlay->OrderList.emplace_back(std::make_shared<AutoPlayOrder>(CharactorId::FriendBob, OrderType::Buff));

    // execute
    for (auto const& order : autoPlay->OrderList)
    {
        // オートプレイ実行時はポリモーフィックに出来てる(★4)
        order->ExecuteFunction();
    }
}
```

出力結果

```
===AutoPlay===
Attack:10
Summon:2
Buff:5
```

## オートプレイの実装１の考察

これは `OrderList` に出せる技をバンバン登録して、それをしていくコードです  

### ■新規に登録する用の型が必要(★0)

`OrderList` というリストに登録するための管理用の型 `AutoPlayOrder` を新たに作っています  

### ■技にによっては使わないメンバを持っている(★2)

無駄があり、あまり良いコードとは言えないです  
そしてこれは、全部の敵や味方が必要な情報を網羅する必要のある神クラスになってしまってます  

```cpp
// 技によって必要なメンバ変数が異なる(★2)
int hitPoint_;                 // orderType == Attack なら、攻撃力
int buffPoint_;                // orderType == Buff なら、バフ量
CharactorId summonFriendId_;   // orderType == Summon なら、一緒に召喚する味方ID
```

ちょっと何とかしたい…

そんな時には、Orderのベースクラスを作って、継承で攻撃クラスを作れば  
プレイヤーだけが `hitPoint_` のメンバを持てるわけなので  
継承するか  
数が少ないのであれば、我慢して必要情報を全持ちするかで、とりあえずは動作出来ます

どうしよう…これだけのために継承するのは、依存関係や制約が強すぎる気がします  
（今回の場合は継承でリファクタしても悪くないとは思いますが…）

### ■結局enum判定をがっつりやっている(★1)

技を実行している箇所について↓↓↓

```cpp
order->ExecuteFunction();
```

ここあたりは、`std::function` を使ってポリモーフィックで良い感じに見えますが  
結局のところ、「何の技を出せる？」という判断をするときに、今時点で何がリストに上がっているか？を調べたくなるので(★3)  
（サンプルコードには書いてませんけど）実際はリスト内の `OrderType` の `enum` をめちゃめちゃ判定に利用しています  
つまり実行時のみポリモーフィックにしたところで、結局 `enum` で場合分けすることが多いので、あまりうまみが無くてシュン…って感じ

この状態でもオートプレイの動作に問題は無いのですが、リファクタしていこうと思います

## オートプレイの実装２（std::variant版）

継承はあんまり使いたくないという発想のもとで、  
とりあえず、コンストラクタ生成時の引数制約で、必要な情報を入手するようにします  
今回はやってませんが、共通クラスを作ってコンポジションで強制しても良いと思います

また、`enum` 判定を減らしたいので、`std::varriant` を用いて `OrderList` に既に登録されている技を型判定無しで取得する手段を提供してみます


- varriantを用いた版 - Conpiler Explor  
[https://godbolt.org/z/78qx3eTqe](https://godbolt.org/z/78qx3eTqe)


```cpp
// ---------------------
// main.cpp

#include <functional>
#include <iostream>
#include <vector>
#include <memory>
#include <variant>

// ... 既存のenum定義はそのまま記載 ...

// ---------------------------------------------------
// 実際にそのターンでどんな技を出すのかの情報(★0')
// 攻撃
class OrderAttack
{
public:
    OrderAttack(CharactorId id, int hitPoint) :
        CurrentOrderType(OrderType::Attack),
        charactorId_(id),
        hitPoint_(hitPoint) {}

    OrderType CurrentOrderType;
    CharactorId GetCharactor() { return charactorId_; }
    void ExecuteOrder() { std::cout << "Attack:" << hitPoint_ << "\n"; }
    void UpdatePlayerAttack(int newHitPoint){hitPoint_ = newHitPoint;}
private:
    CharactorId charactorId_;
    int hitPoint_;                 // 攻撃力
};
// バフ
class OrderBuff
{
public:
    OrderBuff(CharactorId id, int buff) :
        CurrentOrderType(OrderType::Buff),
        charactorId_(id),
        buffPoint_(buff) {}

    OrderType CurrentOrderType;
    CharactorId GetCharactor() { return charactorId_; }
    void ExecuteOrder() { std::cout << "Buff:" << buffPoint_ << "\n"; }
private:
    CharactorId charactorId_;
    int buffPoint_;                 // バフ量
};
// 召喚
class OrderSummon
{
public:
    OrderSummon(CharactorId id, CharactorId summonFriend) :
        CurrentOrderType(OrderType::Summon),
        charactorId_(id),
        summonFriendId_(summonFriend) {}

    OrderType CurrentOrderType;
    CharactorId GetCharactor() { return charactorId_; }
    void ExecuteOrder() { std::cout << "Summon:" << (int)summonFriendId_ << "\n"; }
private:
    CharactorId charactorId_;
    CharactorId summonFriendId_;            // 一緒に召喚する味方ID
};

// ---------------------------------------------------
// キャラクターIDを取得するための構造体
struct GetCharactorId
{
    void operator()(const std::shared_ptr<OrderAttack>& order) const { order->GetCharactor(); }
    void operator()(const std::shared_ptr<OrderBuff>& order) const { order->GetCharactor(); }
    void operator()(const std::shared_ptr<OrderSummon>& order) const { order->GetCharactor(); }
};

// 技の実行をまとめている構造体
struct ExecuteOrder
{
    void operator()(const std::shared_ptr<OrderAttack>& order) const { order->ExecuteOrder(); }
    void operator()(const std::shared_ptr<OrderBuff>& order) const { order->ExecuteOrder(); }
    void operator()(const std::shared_ptr<OrderSummon>& order) const { order->ExecuteOrder(); }
};

// ---------------------------------------------------
// オートプレイの取りまとめクラス
class AutoPlay
{
public:
    std::vector<std::variant<std::shared_ptr<OrderAttack>, std::shared_ptr<OrderBuff>, std::shared_ptr<OrderSummon>>> OrderList;

    // このターンで実行したい、全部のオートプレイの技を実行する
    void ExecuteAllOrder()
    {
        for (auto const& order : OrderList)
        {
            std::visit(ExecuteOrder{}, order);
        }
    }

    // 特定の何かの値を更新したい
    // 例）プレイヤーの攻撃力をあとから更新
    void UpdatePlayerAttackPoint(int newHitPoint)
    {
        for (auto& order : OrderList)
        {
            // OrderAttackオブジェクトを探す
            auto attackOrder = std::get_if<std::shared_ptr<OrderAttack>>(&order);
            if (attackOrder && (*attackOrder)->GetCharactor() == CharactorId::Player)
            {
                (*attackOrder)->UpdatePlayerAttack(newHitPoint);  // (★3')
                break;
            }
        }
    }
};

int main()
{
    std::cout << "===AutoPlay===\n";

    auto autoPlay = std::make_unique<AutoPlay>();

    // 何の技が出せるのかは、今出している技を見たりして登録していく
    autoPlay->OrderList.emplace_back(std::make_shared<OrderAttack>(CharactorId::Player, 10));
    autoPlay->OrderList.emplace_back(std::make_shared<OrderSummon>(CharactorId::FriendAlice, CharactorId::FriendBob));

    // 例えば、ここの時点で何かバフがかかったとして、既にPlayerに登録されている値をこの時点で更新したい、という時
    autoPlay->UpdatePlayerAttackPoint(20);

    autoPlay->OrderList.emplace_back(std::make_shared<OrderBuff>(CharactorId::FriendBob, 5));

    // 実行したい技が整ったので、実行！
    autoPlay->ExecuteAllOrder();
}
```

```
===AutoPlay===
Attack:20
Summon:2
Buff:5
```

先ほどは `OrderType` のenumを使って、技の処理を振り分けしていたのですが  

```cpp
struct GetCharactorId{}
```

これをうまく使うことによって、場合分けしなくて済むようになりました

さっきは、`std::function` のおかげで実行時のみは、かろうじてポリモーフィックに動作していましたが、`std::variant` のおかげで、他の処理も技の型（ `OrderType` で持っていたもの）に依存せずに記載することができました(★3')

また、`struct GetCharactorId` や `struct ExecuteOrder` を定義する必要があるものの、`class OrderAttack` はインタフェースや継承を持っていないので、なんだか自由を手に入れた気分になります

`std::variant` を使ってみると、こんな書き方が出来るのか～という自分なりの新たな発見でした


## 終わりに

今回、シミュレータを作った際に、色々と勉強になることがあったので、ひっくるめて記載してみました

シミュレータという機能がニッチながらも、ゲーム開発においては重要なポイントになりそうなこと、  
また今回記載した `std::variant` を使って書いた処理は Visitor Pattern というらしいですが  
継承、インタフェース、コンポジションなどなど、やりながらあれ？これは？という箇所がたくさんあったので、そういったコードを試行錯誤できたことなどが良い経験だったなーと思って書きました

自分としては継承を使った記載方法でも、今回は良さそうだなと思ったし、  
最初に記載した、 `AutoPlayOrder` という手続き的な神クラスの記載方法も、技の数が少ないなど状況によってはそこまで悪くないかもなと思いました

シミュレータを突き詰めていくと、技や手札やゲーム環境などのパラメータ追加の容易さと、実行時にいかに高速に動作することが求められてくるので  
そうなっていくとまた設計思想も変わってきそうだなとも思いましたが、今回はこのへんまでで終わろうと思います！

## 参考

`std::variant` に気が付けたのは、CppCon2022の **Klaus Iglberger** さんの  
「Breaking Dependencies - The Visitor Design Pattern in Cpp」  
というセッションを見て、おおおおおーと思ったからです

- Breaking Dependencies - The Visitor Design Pattern in Cpp  
[https://www.youtube.com/watch?v=PEcy1vYHb8A](https://www.youtube.com/watch?v=PEcy1vYHb8A)

C++を言語にしたデザインパターンの書籍も出されています

- 「C++ソフトウェア設計」(オライリー)  
[https://www.oreilly.co.jp/books/9784814400454/](https://www.oreilly.co.jp/books/9784814400454/)

この著書の中で、ベースクラスそんなに気軽に更新できないし、純仮想関数を強いた日には同僚とのバーベキューパーティに呼ばれなくなるかもしれないよ！という注釈が何度も出てきて、個人的にウケました
