---
title: "[Unity]シンプルな弾幕の作り方"
emoji: "🏫"
type: "tech"
topics: ["弾幕","ゲーム制作","Unity"]
published: false
---
Unityを使って簡単な弾幕を作っていきましょう！

#弾を生成する
まずは敵の横に一つ弾を作ってみます。
敵から弾を生成するためのBulletGenerator.csというスクリプトを作成します。
このスクリプトをボスにアタッチしましょう。

```c#:BulletGenerator 
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class BulletGenerator : MonoBehaviour
{
    [SerializeField]
    private GameObject bulletPrefab;

    // Start is called before the first frame update
    void Start()
    {
        GenerateBullet();
    }

    // Update is called once per frame
    void Update()
    {
        
    }

    public void GenerateBullet()
    {
        Instantiate(bulletPrefab, this.gameObject.transform.position, Quaternion.identity);
    }
}
```
GenerateBullet関数では「bulletPrefab」を、BulletGeneratorをアタッチしているオブジェクトの場所に、bulletPrefabのままの回転で生成しています。

次に生成する弾のプレハブを作っていきます。
Unity上で + → 2DObject → Sprites → Spuare で正方形の2Dオブジェクトを作成します。
名前を「Bullet」としInspector上で以下のように設定します。
![スクリーンショット 2021-11-01 13.53.05.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581177/848a5175-4477-c952-b38b-dcfe5dd8043e.png)
色は自分の好きな色に設定してみましょう！
作成したオブジェクトをProjectの欄にドラッグ&ドロップしプレハブ化します。
下記画像のようにBulletのオブジェクトの名前の部分が青くなっていたらプレハブ化成功です。
![スクリーンショット 2021-11-01 16.33.42.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581177/2adaf4e6-02b7-ff78-aba2-7b868746018c.png)
ここまでできたらHierarcy上のBulletのオブジェクトは削除しておきましょう。
最後にボス(この記事内ではEnemy)にアタッチしているEnemyGenerator.csの「BulletPrafab」のところに先ほど作成したBulletのプレハブを設定します。
![スクリーンショット 2021-11-01 16.36.09.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581177/bff66d89-697a-f182-4d4e-eb2b8324b071.png)
この状態でゲームを再生するとボスの位置に弾が生成されます。

#弾を発射しよう
続いてこの生成された弾を動かしていきましょう。
弾を動かすためのBulletMove.csというスクリプトを作成します。
このスクリプトをBullletのプレハブにアタッチします。

```c#:BulletMove
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class BulletMove : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {
        this.gameObject.transform.position += new Vector3(-1, 0, 0);
    }
}
```

この状態でゲームを再生してみましょう。
![FastBullet.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581177/285d190f-d70c-c4d6-91a6-fb2451107606.gif)
弾が敵から左の方向へと発射されました。
このままだと少し早いので速度を調整します。以下のようにBulletMoveを変更します。


```c#:BulletMove
public class BulletMove : MonoBehaviour
{
    private float speed = 0.2f;

　　　　　　　　public Vector3 direction = new Vector3(-1,0,0);

    // Start is called before the first frame update
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {
        this.gameObject.transform.position += direction * speed;
    }
}
```
>this.gameObject.transform.psoition += direction * speed

の部分ではこのスクリプトがアタッチされているオブジェクトの位置を`new Vector3(-1,0,0)`の**向き**に`0.2f`の**速さ**で進めるということを記述しています。
この状態でゲームを再生すると先ほどよりもゆっくりと弾が発射されます。
![SlowBullet.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581177/b2784ff7-cd32-d8b3-81b2-0f8a7774de99.gif)

#弾を増やす
このままだとPlayerは簡単に弾が避けれてしまうため、発射する球の数を増やしてみましょう。
BulletGeneratorの弾を生成する部分の処理を書き換えます。
まずは弾を5個生成してみましょう。

```c#:BulletGenerator
public class BulletGenerator : MonoBehaviour
{
    [SerializeField]
    private GameObject bulletPrefab;

    private int generateBulletNum = 5;

    // Start is called before the first frame update
    void Start()
    {
        GenerateBullet();
    }

    // Update is called once per frame
    void Update()
    {
        
    }

    public void GenerateBullet()
    {
        for(int i = 0; i < generateBulletNum; i++)
        {
            Instantiate(bulletPrefab, this.gameObject.transform.position, Quaternion.identity);
        }
    }
}
```
変数として`bulletGenerateNum`を宣言しており、この数の分だけfor文を回して弾の生成の処理を呼び出しています。
この状態でゲームを再生すると以下のようにHierarcy上に弾が５つ生成されているのがわかると思います。
![GenerateSomeBullet.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581177/5871e044-4fef-3453-b335-65e9d3eacf0e.gif)

#弾をバラバラな方角に発射しよう
ゲーム画面上では生成された弾が全て重なってしまい、何個生成されたのかがわからない状態です。
そこで生成された弾がバラバラな方角に発射されるようにスクリプトを変更しましょう。
弾が発射される方角はBulletMove上の変数directionによって定まっているため、弾が生成されるタイミングでBulletGeneratorからそれぞれの弾に異なるdirectionを設定することでそれぞれの球が異なる方角に発射されるようになります。

###三角関数と方角について
ここで三角関数を用いた方角の表し方について解説します。
三角関数とは簡潔に表現すると、**原点を中心とする半径1の円**上の`点P(x,y)`と、`点A(1,0)`、`原点O(0,0)`の**なす角POAをθ**とした時に、`cosθ = x,sinθ = y`として定義するものです。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581177/64499b90-2faf-cca4-a803-105224056433.png)
つまり`θ=90°`の時角POAが90°となる点Pは点（0,1）のため`cos90° = 0,sin90° = 1`となります。
また点Pの座標(x,y)は、**点Oの位置から見た点Pの位置の方角**(OからPに向かうには右にどれくらい進んで上にどれくらい進むのか)を指しています。
よって**角AOP = θ**となるような点Pがあった時、Oから見たPの方角を指す**Pの座標(x,y)**は**P(cosθ,sinθ)**と表せるということです。

先程生成した5つの弾は全てθ=180°の方角へと発射されていました。そのため今度は5つの弾がそれぞれ、160°,170°,180°,190°,200°の方角へ発射されるようにスクリプトを変更していきましょう。
BulletGenerator.csのGenerateBullet関数を以下のように書き換えます。

```c#:BulletGenerator
 public void GenerateBullet()
    {
        float moveDirection = 160.0f;

        for(int i = 0; i < generateBulletNum; i++)
        {
            GameObject instantiateBulletObj = Instantiate(bulletPrefab, this.gameObject.transform.position, Quaternion.identity);

            Vector3 direction = new Vector3(Mathf.Cos(moveDirection * Mathf.Deg2Rad), Mathf.Sin(moveDirection * Mathf.Deg2Rad), 0);

            instantiateBulletObj.GetComponent<BulletMove>().direction = direction;

            moveDirection += 10.0f;
        }
    }
```
>float moveDirection = 160.0f;

部分では弾の発射される方角を入れる変数moveDirectionを宣言し、初期値として160°を設定しています。
>GameObject instantiateBulletObj = Instantiate(bulletPrefab,this.gameObject.transform.position,Quartenion.identity);

部分ではInstantiate()関数で生成したオブジェクトの情報を取得し新たに宣言した変数instantiateBulletObjの中に入れています。
>Vector3 direction = new Vector3(Mathf.Cos(moveDirection * Mathf.Deg2Rad),Mathf.Sin(moveDirection * Mathf.Deg2Rad),0);

部分では弾に与える方角を、角度からcos,sinを取ることで設定しています。この時moveDirectionにMathf.Deg2Radがかけられています。これはcosやsinを取得するには、160°という角度の表し方(度数法)ではなく弧度法と呼ばれる表し方をした角度を与えてやらなければならないために、値を変換する必要があるからです。
>instantiateBulletObj.GetComponent<BulletMove>().direction = direction;

部分では生成した弾にアタッチされているスクリプトBulletMoveにアクセスして、その中の変数`direction`に今計算したdirectionを設定しています。

>moveDirection += 10.0f;

最後のこの部分では次の弾の方角として、今生成した弾の方角より10°進んだ値を指定しています。このことによって５つの弾それぞれに進む方角として160°,170°,180°,190°,200°の方角が与えられました。
この状態でゲームを再生してみましょう。
![Fire5Bullet.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581177/258e6ff5-13c4-1147-a35b-ed64846e96fb.gif)
上のように5つの弾がバラバラの方角へ発射されました。


#連続して弾を発射する
続いて弾が何発も連続して発射されるように設定しましょう。
これは簡単に実装できます。
BulletGenerator.cs内でGenerateBullet()関数を呼び出している部分を以下のように変更しましょう。

```c#:BulletGenerator
private float interval = 0.8f;

    // Start is called before the first frame update
    void Start()
    {
        InvokeRepeating(nameof(GenerateBullet),0,interval);
    }
```
InvokeRepeating関数では指定した秒数後から、指定した関数を、指定した秒数間隔で呼び出します。
`nameof(GenerateBullet)`部分では関数名であるGenerateBulletをInvokeRepeating関数に渡せるstring型の形に変換しています。string型で直接"GenerateBullet"と書いても良いのですが、それだとスペルが間違っていた際にエラーが出ず、混乱することがあるのでこのような書き方をしています。
今回は変数として`interval`を宣言しており、この秒数間隔でGenerateBullet関数を呼び出すことで一定間隔で何度も弾が発射されるようにしています。
この状態でゲームを再生してみましょう。
![RepeatBullet.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581177/6a38210f-1b10-67f3-ffe7-aa7b1ac1a71c.gif)
上記のように弾がいくつも連続で発射されました。

以上で基本の弾幕の作り方は終わりです。
生成する弾の数、位置、間隔などなどを変更することで様々な弾幕が作ることができます。

#おまけ
Playerの位置に向かって発射される弾を作ってみましょう。
ulletGenerator.csに先ほどと同じような追尾弾を生成する記述を追加します。

```c#:BulletGenerator
[SerializeField]
private GameObject playerObj;

public void GenerateTrackBullet()
{
     GameObject instantitateBulletObj = Instantiate(bulletPrefab, this.gameObject.transform.position, Quaternion.identity);

     Vector3 direction = (playerObj.transform.position - this.gameObject.transform.position).normalized;

     instantitateBulletObj.GetComponent<BulletMove>().direction = direction;
}
```
先程のGenerateBullet()関数と異なるのは
>Vector3 direction = (playerObj.transform.position - this.transform.position).normalized;

の部分です。ここでは`(playerObj.transform.position - this.transform.position)`の部分でこのスクリプトがアタッチされているオブジェクト(敵)から見たPlayerの相対位置を取得してきています。そして`(playerObj.transform.position - this.transform.position).normalized`の部分で取得した相対座標の大きさを1にしています。
この状態でゲームを再生してみましょう。
![TrackBullet.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581177/5b84cc66-887b-d59e-92ee-9c184109e2d0.gif)
上記のように動くPlayerに対して弾が追尾しています。






