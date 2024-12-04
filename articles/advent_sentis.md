---
title: "Unity Sentisのススメ"
emoji: "🤖"
type: "tech"
topics: ["Unity","Sentis","機械学習","アドベントカレンダー"]
publication_name: "lit_akb"
published: false
---

## はじめに
こちらの記事は[Akihabara Advent Calendar 2024](https://adventar.org/calendars/10338)の12日目の記事です。
✅ 11日目の記事はこちら：[]()
✅ 13日目の記事はこちら：[]()

みなさんこんにちは、初めましての人は初めまして、お久しぶりの人はお久しぶりです。LifeisTech!秋葉原スクールでUnityコースとAIコースのメンターをしています、しまとーです！

今回はUnity/AIコース混合を担当するしまとーならではの記事！ということで、2023年6月より提供開始された、**Unityランタイム上でAIモデルを動作させることができるライブラリ**、「Unity Sentis」の紹介記事を書いていければと思います！
https://unity.com/ja/products/sentis

## Sentisとは
Sentisは2023年6月よりUnityより提供され、2024年8月公開のsentis 2.xをもって公式版となりました。現在は軽微なバグ修正を行なったsentis 2.1.1が2024年11月にリリースされています。
Sentisの最大の特徴として、以下の4つが挙げられています。
1. プラットフォームの制限を受けない
    Unityで対応しているすべてのプラットフォーム上で動作するようにビルド、組み込みを行うことが可能
2. Unityでビルドをするだけで、バックエンドのデプロイなどの手間を必要としない
3. 完全ローカルの範囲内で推論が可能
    ネットワークの制限を受けることなく、さらにはクラウド上にデータが残ることもない。ネットワークの遅延の影響を受けることもない
4. Unityランタイムの環境に合わせてモデルを最適化するため、推論にかかる速度がかなり効率化できる

詳しくは以下の動画に記されています。
https://www.youtube.com/watch?v=IfdRfy1Cynk

## 導入方法

Sentisの導入方法は非常にシンプルです！まずは任意のプロジェクト(Unity 2023.2 (or later))を作成します。

Unity Packege Managerを開き、
![PackegeManagerを開く](/images/advent_sentis/packege_manager.png)


「install packege by name」を選択します
![パッケージ名の入力](/images/advent_sentis/install_byname.png)

最後に**com.unity.sentis**を入力してインストール完了です！
![Sentisがインストールされた様子](/images/advent_sentis/installed_sentis.png)

## 使用の際の基本の流れ

Sentisを使用して、推論を行う際の基本の流れは共通して以下の通りです。

1. ONNX形式のモデルをインポート
2. ランタイムで動作するモデル形式に変換
3. モデルに渡すための入力テンソルを作成
4. 推論エンジン(Worker)の作成
5. 推論結果をテンソル形式で受け取る

ONNX形式のモデルはTensorflowやsckikit-learnなどの様々な異なるフレームワークを通して学習したモデルを、共通の規格に落とし込み最適化したものです。
Sentisは、機械学習モデルの共通規格であるONNX形式のモデルをインポートし、Unityランタイム上で推論を実行できるため、学習済みの様々な規格のモデルを、使用することができるようになります。

ONNX形式のデータは以下のリポジトリ内に複数配布されています。
https://github.com/onnx/models


## サンプル

今回はUnity Sentis上で動作させることができる幾つかのサンプルを通して実際の使用例を見てみましょう。

ちなみにUnity公式からも今回取り上げる以上の複数のサンプルプロジェクトが提供されているため、簡単なアレンジが行いたい方はぜひ参考にしてみてください！
https://github.com/Unity-Technologies/sentis-samples

### コード

:::details Web Cameraの描画クラス
```csharp:WebCameraViewer.cs
using UnityEngine;

public class CameraViewer : MonoBehaviour
{
    [SerializeField] private RenderTexture outputTexture; // 出力用のRenderTextureをInspectorから設定
    private WebCamTexture _webCamTexture; // Webカメラの映像を取得するためのWebCamTexture
    private Material _blitMaterial; // 映像を描画するためのマテリアル
    
    void Start()
    {
        InitViewer();
    }
    
    void Update()
    {
        if (_webCamTexture is null || !_webCamTexture.isPlaying || outputTexture is null) return;
        Render();
    }
    
    void OnDestroy()
    {
        if (_webCamTexture is null) return;
        _webCamTexture.Stop();
    }
    
    /// <summary>
    /// 描画するための準備
    /// </summary>
    private void InitViewer()
    {
        // デバイスのカメラを取得
        if (WebCamTexture.devices.Length > 0)
        {
            _webCamTexture = new WebCamTexture(WebCamTexture.devices[0].name);
            _webCamTexture.Play();

            // 映像を描画するためのマテリアルを作成
            _blitMaterial = new Material(Shader.Find("Unlit/Texture"))
            {
                mainTexture = _webCamTexture
            };
        }
        else
        {
            Debug.LogError("Web Camera Not Found");
        }
    }

    /// <summary>
    /// 描画処理
    /// </summary>
    private void Render()
    {
        // RenderTextureに描画
        RenderTexture.active = outputTexture;
        GL.Clear(true, true, Color.clear);
        Graphics.Blit(null, _blitMaterial);
        // RenderTextureの描画を終了
        RenderTexture.active = null;
    }
    
    /// <summary>
    /// 外部からのRenderTexture取得用メソッド
    /// </summary>
    /// <returns></returns>
    public RenderTexture GetRenderTexture()
    {
        return outputTexture;
    }
}
```
:::


## 最後に


明日は秋葉原スクールの長、CMのがはくによる記事です！お楽しみに！

