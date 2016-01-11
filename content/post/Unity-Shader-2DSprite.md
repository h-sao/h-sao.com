+++
date = "2016-01-11T18:00:00+09:00"
draft = false
title = "[Unity] 2D Spriteにシェーダーをかける"
tags = ["Unity", "shader"]
+++

[今年の初めに](/blog/2016/01/04/hello2016/)、「Game a Week」という開発手法がすごい！っと書きました   
とりあえず、週に一度は成果物を公開する、の部分を真似してみようかなと  
（やってみて気が付きましたが、実は1週間って、結構長いです）

今をトキメクGame Engine: **Unity** について、去年から触る機会があり、ポチポチとやっております  

そしてこれは既知の情報ですが、先週は2Dスプライトにシンプルなグラデーションのシェーダーを適用してみました

やってみると判るのですが、Unity ではスプライトにシンプルシェーダーだけを適用しようと思っても出来なくて、  
先に結論を書いておくと、スプライトとして扱う場合は必ず何かしらのテクスチャアセットが必要でした  
そのメモと感想文になります

2016年1月11日現在、Unityのバージョンは 5.3.1 です


# Unity のシェーダー言語：ShaderLab

Unity のシェーダーは 「ShaderLab」 という Unity オリジナルのシェーダー言語で記載することになります  
といっても HLSL のラッパーのような言語なので、Unity で使うときのお作法であり、Unity と シェーダーの仲介役の言語、と思って良いみたい

## 最小限の ShaderLab

最小限の ShaderLab の枠組みはこんな感じ  
（これより削ると、エラーが出た）

```
// BG_shader.shader
// 最小限の ShaderLab
Shader "Custom/BG_shader" {
    SubShader
    {
        Pass {}
    }
}
```

実際に、このカスタムシェーダーをマテリアルに適用するとこんな感じ

![](/pic/Unity-Shader-2DSprite_00.png)

何もしないマテリアルを作ることが出来ました

## シンプルなグラデーション

今回、ゲーム背景を単純なカラーグラデーションにしようと思ったので、そういうシンプルシェーダーを書いていきます

```
// BG_shader.shader
// 黄色くグラデーションする
Shader "Custom/BG_shader" {
    SubShader
    {
        Pass{
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag

            // VS2015のグラフィックデバックON
            #pragma enable_d3d11_debug_symbols

            struct VertexInput {
                float4 pos:  POSITION;    // 3D空間座標
                float2 uv:   TEXCOORD0;   // テクスチャ座標
            };

            struct VertexOutput {
                float4 v:    SV_POSITION; // 2D座標
                float2 uv:   TEXCOORD0;   // テクスチャ座標
            };

            // 頂点 shader
            VertexOutput vert(VertexInput input)
            {
                VertexOutput output;
                output.v = mul(UNITY_MATRIX_MVP, input.pos);
                output.uv = input.uv;

                return output;
            }

            // ピクセル shader
            fixed4 frag( VertexOutput output) : SV_Target
            {
                float2 tex = output.uv;
                // 黄色→白色のグラデーション
                return fixed4( 1.0, 1.0, 1.0 - tex.y, 1.0);
            }

            ENDCG
        }
    }
}
```

ちょっとマテリアルではわかりにくいですけど、一応、線形にグラデーションされています

![](/pic/Unity-Shader-2DSprite_01.png)

# 2D Sprite に適用する方法

シンプルなシェーダーとマテリアルが出来たので、実際に、Sprite に登録します

## Sprite と Material だけでは足りない

ただし、ちょっとここでクセがあって、このマテリアルを Sprite にアタッチしても、何も起こりません  
それどころか、ワーニングメッセージが…

![](/pic/Unity-Shader-2DSprite_02.png)

> Material does not have a _MainTex texture property. It is required for SpriteRenderer.

あら…  
Sprite Renderer に登録するマテリアルには、テクスチャが必要ということみたいです  
シェーダーに戻って、言われているとおり、 _MainTex にテクスチャを登録します


```
// BG_shader.shader の Properties を追加
    Properties
    {
        _MainTex( "2D Texture", 2D ) = "white" {}
    }
```

テクスチャを登録できるし、デフォルトでは white テクスチャを使いますよ。という意味になります  
ちなみに、_MainTex() の内蔵テクスチャには

- white
- black
- gray
- bump

の4種類が用意されています  
* ShaderLab: Properties - Unity Documentation  
 http://docs.unity3d.com/Manual/SL-Properties.html

![](/pic/Unity-Shader-2DSprite_03.png)

マテリアルにテクスチャを持つ設定にしました

## Sprite には、ベースとしてリアルな Texture が必要

Sprite のワーニングも消えたのですが、やはりシーンに Sprite object が表示されません

![](/pic/Unity-Shader-2DSprite_04.png)

どうやら、Sprite はあくまで、テクスチャ画像を表示させるための機能に特化しており、マテリアルだけでは動作しない様です  

仕方がないので、Sprite 用のテクスチャを用意します  
サイズ感がよくわからなかったのですが、 white.jpg という 8*8 のテクスチャを Assets の下に入れました

Sprite の Inspector にて、Sprite Rendere > Sprite にて white テクスチャを選択します

> あぁ…これ、デフォルトで白いテクスチャくらい、システムで用意してほしいなかと思いましたが、まぁしょーがないです  
![](/pic/Unity-Shader-2DSprite_05.png)

でたー  

![](/pic/Unity-Shader-2DSprite_06.png)

# まとめ

シンプルな 3D model では

- **Mesh Renderer**  
→ **Material** (たとえば Standard Shader)  
　→ **Texture**  

という構造なので、

2D Sprite では

- **Sprite Renderer**  
→ **Material** or **Texture**

なのかなーと思っていたのですが、実際には

- **Sprite Renderer**  
→ **Material**  
　→ **Texture** (Shader の _MainTex() )  
→ **Sprite**  
　→ **Texture** (リアル画像)

という構造が必要でした

これを受けて、  

「え、シンプルなシェーダーのみを適用したいなら、  
Sprite でなくて 3D Plane Model にしたらいいんでない？」

という疑問が出てきましたが、  
実際のゲーム制作においては、シチュエーション依存ですかね…  
今回のわたしの場合は、Sprite を採用しました

この記事の Unity プロジェクト（ソース、アセット）を Github に置いておきます  
https://github.com/h-sao/UnitySampleCode/tree/master/SpriteGradationalShader  

何かの参考になれば幸いです
