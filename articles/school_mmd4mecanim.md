---
title: "[Unity]MMDモデルを自由に動かしてみよう！"
emoji: "🏫"
type: "tech"
topics: ["MMD","ゲーム制作","MMD4Mecanim"]
published: false
---
Unity上でMMDモデルを自由に動かしてみましょう！！
この記事を最後まで進めていくと、以下のような映像が作成できます。
https://youtu.be/s9vAW1D3zIo

#MMD4Mecanimの導入
以下のページからMMD4Mecanimをダウンロードします。
http://stereoarts.jp/#:~:text=MMD4Mecanim_Beta_20201105.zip
ダウンロードしたフォルダの中から「MMD4Macanim.unitypackage」というファイルを選択し、任意のUnityプロジェクトにインポートします。
<img width="300" alt="スクリーンショット 2022-01-14 14.51.25.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581177/b4f0bf58-16bb-fe4f-4544-f44c5ad0815b.png">
上記のような画像が出たらImportを選択してUnity上にインポートします。
ここまでできたらMMD4Mecanimの導入完了です。
#MMDモデルをUnity上で動かしてみましょう
動かしたいMMDモデルを用意しましょう。また、モデルに動きをつける場合はアニメーションも用意しましょう。
〇〇〇.pmxのファイル(MMDモデル)とある場合は□□□.vmdのファイル(アニメーションファイル)をUnity上にドラッグ&ドロップでインポートします。
MMD4Mecanimが正常に導入できていれば、下記画像のように〇〇〇.MMD4Mecanimファイルが生成されます。
<img width="400" alt="MMD4Mecanim.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581177/ebaf51a0-6fe2-965d-0ddb-a4871db5f795.png">
３つの利用規約にチェックをして、同意するを選択します。
すると下記画像のような画面が出てくるので設定をしていきましょう。
<img width="300" alt="スクリーンショット 2022-01-14 15.18.26.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581177/81b6d355-8ed2-54a6-5961-8821cec4e8bd.png">
アニメーションをモデルに設定する際はこの時にVMDの項目にインポートした□□□.vmdファイルを設定しましょう。
設定を終えたらProcessボタンを押しましょう。しばらく変換に時間がかかりますが、変換が終わると.fbxファイルが生成されます。

#Animatorの設定
インポートしたアニメーションを再生するためにAnimatorを設定しましょう。生成された.fbxファイルをシーンのHierarchy上にドラッグ&ドロップで配置しましょう。配置したモデルにAdd ComponentからAnimatorを追加します。
<img width="300" alt="スクリーンショット 2022-01-14 15.59.40.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581177/c2f3091b-2087-e602-4fc7-4b24649ca20d.png">
Projectウィンドウにある+ボタンからAnimationControllerを新規に作成して任意の名前をつけます。先程追加したAnimatorコンポーネントのControllerの欄に今作成したAnimationControllerを設定します。設定したAnimationControllerを開くと下記画像のような画面が出てくるため、.fbxファイルと同時に生成されたアニメーションクリップファイルをドラッグ&ドロップでAnimationController上に配置します。
![スクリーンショット 2022-01-14 16.15.45.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581177/7fe6505f-b079-caa1-492d-7f6f806e053c.png)
この状態でシーンを再生すると無事設定したアニメーションでモデルが動き始めます！

#+αの要素を追加していきましょう
Unityの標準機能であるTimeLineの機能を使ってみましょう。
TimeLineは映像編集ソフトのように時間経過に合わせて、ゲームオブジェクトの表示を切り替えたり、オーディオの再生を切り替えたり、カメラの移動を簡単に行える機能です。
<img width="700" alt="スクリーンショット 2022-01-14 16.32.03.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581177/74859ff7-2b04-0aa1-e0e6-57a19b1ab5a5.png">
Projectウィンドウにある+ボタンからTimeLineを新規に作成して任意の名前をつけます。
<img width="300" alt="スクリーンショット 2022-01-14 16.25.20.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581177/23cc62b9-c597-b79c-a4ca-02451f6ab639.png">
からのゲームオブジェクトを作成してAdd ComponentからPlayableDirectorというコンポーネントを追加して先程作成したTimeLineをPlayableの項目に設定します。
<img width="300" alt="スクリーンショット 2022-01-14 16.29.45.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581177/8f799cab-f15a-a8ae-cf3d-f5feb3963a22.png">
TimeLineではさまざまなことができますが、ここでは例として音を再生する機能と、オブジェクトの表示を切り替える機能の説明をしていきます。

##音を再生する機能
再生したい音楽をUnity上にインポートします。
TimeLineウィンドウの左上の+ボタンから下記画像のようにAudioTrackを選択して追加します。
![スクリーンショット 2022-01-14 16.36.49.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581177/b2aceaf9-b4c4-81cc-6dad-06c341f52853.png)
追加したAudioTrackの右にある３つの丸ぽちから「Add From Audio Clip」を選択し再生したい音楽を選択します。
![スクリーンショット 2022-01-14 16.40.46.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581177/646f4fc2-d9c8-d771-01c6-a8899f93f4a0.png)
この状態でTimeLine左上部分の三角の再生ボタンを押すと音楽が再生されると思います。
![スクリーンショット 2022-01-14 16.48.16.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581177/90340ec6-922b-f50f-d348-29a968cdc132.png)
トラックを左右に移動させると再生されるタイミングを変更でき、トラックの端をつかんで移動させると音の長さを変更することができます。
他にもさまざまなことができるので下記URLなどを参考にしてみてください。
https://learn.unity.com/tutorial/working-with-audio-tracks-in-timeline-2019-3?language=ja#5f714d81edbc2a1aadbf378d

##オブジェクトの表示切り替え
Unityのシーン上にあるオブジェクトならばTimeLineで簡単に任意のタイミングで表示を切り替えることができます。例として今回は文字の表示・非表示を切り替えてみましょう。
まず表示したいテキストを用意します。
![スクリーンショット 2022-01-14 16.59.09.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581177/8cdc3e77-5467-da47-07ea-c083a3e8517e.png)
用意したテキストをTimeLine上にドラッグ&ドロップします。するとControl Trackというものが生成されます。TimeLine上の白い棒がControlTrack上に被っている間はテキストが表示され、そうでない間はテキストが非表示になります。先程のオーディオと同じようにトラックの端を掴んで移動させることで表示時間の長さを変えることもできます。

