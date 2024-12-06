---
title: "Unity Sentisのススメ"
emoji: "🤖"
type: "tech"
topics: ["Unity","Sentis","機械学習","アドベントカレンダー"]
publication_name: "lit_akb"
published: false
---

## はじめに
こちらの記事は[Akihabara Advent Calendar 2024](https://adventar.org/calendars/10338)の7日目の記事です。  
✅ 6日目の記事はこちら：[とっぽの何か](https://note.com/risako070310/n/n64fb1a5d138b)  
✅ 8日目の記事はこちら：[がはくさんの何か]()  

みなさんこんにちは！初めましての方は初めまして、お久しぶりの方はお久しぶりです。Life is Tech! 秋葉原スクールでUnityコースとAIコースのメンターをしています、しまとーです！  

今回は、UnityとAIコースを担当している私ならではの記事として、2023年6月に提供が開始された**Unityランタイム上でAIモデルを動作させるライブラリ**「Unity Sentis」をご紹介します！
https://unity.com/ja/products/sentis

---

## Sentisとは
Sentisは、Unityによって2023年6月に提供が開始され、2024年8月にリリースされた「Sentis 2.x」以降で公式版となりました。現在、軽微なバグ修正を施した「Sentis 2.1.1」が2024年11月に公開されています。  

Sentisの主な特徴は以下の4点です：
1. **プラットフォーム制限がない**  
   Unityが対応しているすべてのプラットフォーム上で動作可能です。
2. **バックエンド不要**  
   Unityでのビルドだけで済み、バックエンドのデプロイ作業が不要です。
3. **完全ローカルで動作**  
   ネットワーク接続が不要で、クラウドにデータが保存される心配がなく、ネットワーク遅延の影響を受けません。
4. **推論速度の最適化**  
   Unityランタイム環境に合わせてモデルが最適化されるため、高速な推論が可能です。

詳しくはこちらの動画で解説されています。
https://www.youtube.com/watch?v=IfdRfy1Cynk

## 導入方法

Sentisの導入はかなり簡単です！

1. Unity 2023.2以降を使用して任意のプロジェクトを作成します。  
2. Package Managerを開きます。  
![Package Managerを開く](/images/advent_sentis/packege_manager.png)  
3. 「Install package by name」を選択します。  
![パッケージ名の入力](/images/advent_sentis/install_byname.png)  
4. **com.unity.sentis**を入力してインストールを完了させます。  
![Sentisがインストールされた様子](/images/advent_sentis/installed_sentis.png)  

## 使用の基本的な流れ

Sentisを利用して推論を行う際の流れは以下の通りです：

1. **ONNX形式のモデルをインポート**  
   TensorFlowやScikit-learnなどで学習したモデルをONNX形式に変換。
2. **ランタイム形式に変換**  
   Unityランタイムで動作するモデル形式へ変換。
3. **入力テンソルを作成**  
   モデルに渡すためのデータを準備。
4. **推論エンジン（Worker）を作成**  
   Workerを使用して推論を実行。
5. **結果をテンソル形式で受け取る**  

ONNX形式については以下のリポジトリでサンプルモデルを入手可能です。
https://github.com/onnx/models

## サンプル紹介

Unity公式が提供するサンプルプロジェクトを確認しましょう。サンプルは以下のリポジトリで公開されています。
https://github.com/Unity-Technologies/sentis-samples

### 物体検出
Googleの「MediaPipe」フレームワークを活用した「顔」「手」「姿勢」の検出モデルを利用しています。  
![Blazeプロジェクトの顔検出の様子](/images/advent_sentis/blaze_face_output.png)

入力データをテンソルに変換する際には、モデルの入力形式を確認することが重要です。たとえば、Blaze-Faceモデルでは以下のような入力形式が求められます：

1x128x128x3（画像数×画像幅×画像高×色チャンネル）  

入力形式はモデルのInspectorウィンドウで確認できます：  
![Blaze-Faceモデルの入力形式](/images/advent_sentis/blaze_input_shape.png)  

テンソルへの変換例は以下の通りです：

```csharp
// 入力画像をレイアウト指定してテンソルに変換
var tTrans = new TextureTransform();
tTrans.SetDimensions(128, 128, 3);
tTrans.SetTensorLayout(TensorLayout.NHWC);
TextureConverter.ToTensor(_renderTexture, _inputTensor, tTrans);
```

## 最後に

今回はSentisとそのサンプルプロジェクトを紹介しました。ONNX形式に変換した学習済みモデルをUnityプロジェクト内で手軽に利用できるのは非常に魅力的ですね！今後のさらなる発展が期待されます！  

明日は秋葉原スクールの長、CMのがはくによる記事をお楽しみに！