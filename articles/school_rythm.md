---
title: "[Unity]音ゲーのエフェクトの付け方"
emoji: "🏫"
type: "tech"
topics: ["音ゲー","ゲーム制作","Unity"]
published: false
---
音ゲー・リズムゲームに欠かせないエフェクトの付け方について学んでいきましょう！
この記事を最後まで進めると、最終的にノーツをタップした時に以下のようなエフェクトが出るようになります！
![名称未設定 (1).gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581177/e2ff95ef-c425-49a4-a9fa-544771d33c31.gif)
#エフェクトの画像を作ろう
まずは表示させるエフェクトの画像を作成していきましょう。
![スクリーンショット 2021-11-19 13.55.29.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581177/5ee24be2-5899-f7f7-7804-9defc7e65095.png)
私は上のような三つの画像を用意しました。色や線の細さなどを自分好みにカスタマイズしてオリジナルのエフェクト画像を作成してみましょう！
画像が用意できたらUnity上にインポートしていきます。
UnityのAssetsフォルダ配下にImagesフォルダを作成します。
作成したフォルダに用意した画像たちをドラッグ&ドロップし、Unity上にインポートしましょう。
![画面収録-2021-11-19-14.00.42.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581177/3ca81f7c-4e60-7066-43b8-fa24b97e5b05.gif)
インポートした画像たちをUnity内で使用できるように設定を行なっていきます。
画像を選択すると下記のようなInspectorが表示されます。
<img width="300" alt="[スクリーンショット 2021-11-19 14.05.28.png]" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581177/f38f9782-3b58-88e7-dc79-b8773eb4d066.png">
このTextureTypeを**Default**から**Sprite(2D and UI)**に変更しましょう
<img width="300" alt="[スクリーンショット 2021-11-19 14.05.42.png]" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581177/552808d7-cbab-f191-edbf-b42ce0850f49.png">
変更を行ったら必ず右下の**Apply**ボタンから変更を反映することを忘れずに！
<img width="300" alt="[スクリーンショット 2021-11-19 14.05.52.png]" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581177/a7b60982-673f-275b-a64d-4eba6d3a5ae0.png">
この操作を全ての画像に対して行います。
以上でエフェクトに使用する画像の設定は終了です。

#エフェクトの作成
Unity上でエフェクトを作成していきます。
まずはエフェクトの画像を設定するMaterialを作成します。Projectタブから+→Materialで新しくMaterialを作成しましょう。
作成したMaterial内の、**Main Maps**/**Albedo**項目の左にある四角の枠に取り込んだエフェクトの画像を設定します。
<img width="300" alt="[スクリーンショット 2021-11-19 14.23.19.png]" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581177/9e49b4c5-280b-d2e1-6eef-0d466a4043a4.png">
また、**Rendering Mode**をFadeに変更することで透明な部分が反映されるようにします。
これでMaterialの設定は終わりです。全ての画像に対してMaterialを作成し、この操作を行います。

続いて+→3D Object→PlaneよりPlaneを作成します。
作成したPlaneに先ほど設定したMaterialを設定します。
用意した画像の分Planeを作成することができたらノーツの形や位置に合わせて大きさや位置を調整しましょう。
私の場合は以下のようになりました
<img width="200" alt="[スクリーンショット 2021-11-19 14.38.10.png]" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581177/0d571a5f-11d2-c4e5-c09f-2ff5fdd6c2f9.png"><img width="200" alt="[スクリーンショット 2021-11-19 14.38.18.png]" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581177/40753162-5b23-7480-5e07-523ab91491d9.png"><img width="200" alt="[スクリーンショット 2021-11-19 14.38.25.png]" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581177/0e60189b-ee05-33e7-5041-5fbb57195d69.png">
<img width="600" alt="[スクリーンショット 2021-11-19 14.37.58.png]" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581177/edfa4f75-6df3-32cb-a3d4-306ee7d02d84.png">
これらのエフェクトを全て空のゲームオブジェクト**Effect**の子オブジェクトにしましょう。
<img width="600" alt="[スクリーンショット 2021-11-19 16.20.23.png]" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581177/d1e584b5-164f-3dae-dd8a-b34cfc0c11ba.png">
作成したEffectはProjectタブへドラッグ&ドロップをしてプレハブ化しましょう。Hierarcy上でEffectが青く表示されるようになったらプレハブ化できています。
ここまで来たらエフェクトの見た目の部分は完成です！あとはノーツをタップしたときに表示されるようにしましょう。

#エフェクトの表示
エフェクトの表示を行う**Effect.cs**というスクリプトを作成しましょう。
このスクリプトを全てのエフェクトの親オブジェクトとなっている空のゲームオブジェクトにアタッチします。
まずはこのスクリプト内で子オブジェクトを全て取得しましょう。

```c#:Effect.cs
public class Effect : MonoBehaviour
{
    private GameObject circleEffectObj;

    private GameObject spreadEffectObj;

    private GameObject stretchEffectObj;

    private void Awake()
    {
        //各エフェクトをこのオブジェクトの子オブジェクトから取得
        circleEffectObj = this.transform.Find("CircleEffect").gameObject;

        spreadEffectObj = this.transform.Find("SpreadEffect").gameObject;

        stretchEffectObj = this.transform.Find("StretchEffect").gameObject;
    }
}
```
`transform.Find("取得したいオブジェクト名")`という関数はこのオブジェクトの子オブジェクトから指定の名前のオブジェクトのtransformを取得する関数です。
`this.transform.Find("CircleEffect").gameObject`の部分で取得したtransformの情報をGameObjectの型に変換してエフェクトの情報を取得しています。
これらを表示する関数を書いていきましょう。

```c#:Effect.cs
public void DisplayEffect()
    {
        circleEffectObj.SetActive(true);

        spreadEffectObj.SetActive(true);

        stretchEffectObj.SetActive(true);
    }
```
あとはノーツをタップした瞬間にエフェクトを生成し、この関数を呼び出せばエフェクトを表示することができます！
ノーツをタップしたことを判定するスクリプト内に以下の処理を書きます。

```c#:JudgeNotesTap.cs
public GameObject effectPrefab;
private void Tap(int laneNum)
{
    GameObject effectObj = Instantiate(effectPrefab);

    effectObj.transform.position = new Vector3(-2.0f * (laneNum - 2), effectObj.transform.position.y, effectObj.transform.position.z);

    effectObj.GetComponent<Effect>().DisplayEffect();
}
```
`GameObject effectObj = Instantiate(effectPrefab);`の部分ではエフェクトのプレハブを生成しています。
ここの`-2.0f*(laneNum - 2)`の部分は自分でレーンの枠に合うように調整しましょう。
最後の`effectObj.GetComponent<Effect>().DisplayEffect()`の部分では生成したエフェクトのプレハブのEffectというスクリプトにアクセスし、DisplayEffectをいう関数を実行することを指しています。
ここまで書くことができたら実行してみましょう。
![名称未設定.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581177/7bca11f7-bada-5846-6ec9-4eb0ede0afbb.gif)
レーンをタップした瞬間にエフェクトが表示されるようになったでしょうか？
このままではずっとエフェクトが表示されたままになってしまうので一定時間が経過したらエフェクトが消えるような処理を書いていきましょう。



#エフェクトを動かそう





