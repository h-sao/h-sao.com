+++
date = "2017-06-21T18:00:00+09:00"
draft = false
title = "[Unity] 常にカメラの前にSpriteを出す"
tags = ["Unity"]
+++

## Unityでカメラに追従して、前面にSpriteを出す方法

これ、本当は、カメラの子供として指定すればもっと簡単なんですけど…

```
testScript.transform.parent = Camera.main.transform
```

この場合、  
カメラにアニメーションが付いてる時（メインカメラは固定ポジションのままの時）に  
うまく乗せられなかったりして、  
面倒だなーと思い、Update毎に自分で位置を更新することにしました  
（変に親子関係があるのも、後で何か制限されたらやだなーとかとかもあり…）

以下を参考にしたら、簡単にカメラの前に表示するスプライトを簡単に表現できました

- カメラからの距離で求める錐台のサイズ  
[https://docs.unity3d.com/jp/540/Manual/FrustumSizeAtDistance.html](https://docs.unity3d.com/jp/540/Manual/FrustumSizeAtDistance.html)


```
public class TestExecutor : MonoBehaviour
{
    private Texture2D blackTexture;
    private SpriteRenderer testSprite;

    void Start()
    {
        // Create black texture
        blackTexture = new Texture2D(32, 32, TextureFormat.RGB24, false);
        blackTexture.SetPixel(0, 0, Color.white);
        blackTexture.Apply();

        // Create Sprite
        var sprite = Sprite.Create(
          texture: blackTexture,
          rect: new Rect(0, 0, blackTexture.width, blackTexture.height),
          pivot: new Vector2(0.5f, 0.5f)
        );

        // スクリプトからSprite生成
        testSprite = new GameObject("TestSprite").AddComponent<SpriteRenderer>();
        testSprite.sprite = sprite;
    }

    void Update()
    {
        var distance = 10f;

        // カメラの前に置く距離に応じて、ワールド座標でのスクリーンサイズを求める
        // (distance +1f) の意味は、スクリーンサイズよりちょい大きめで画面を覆いたいから
        var worldScreenH = 2.0f * (distance + 1f) * Mathf.Tan(Camera.main.fieldOfView * 0.5f * Mathf.Deg2Rad);
        var worldScreenW = worldScreenH * Camera.main.aspect;

        // Spriteのサイズを調整
        testSprite.transform.localScale = new Vector3(worldScreenW / testSprite.sprite.bounds.size.x, worldScreenH / testSprite.sprite.bounds.size.y);

        // カメラの角度と位置をコピー、前に置くだけならforwardベクトルが便利
        testSprite.transform.position = Camera.main.transform.position + testSprite.transform.forward.normalized * distance;
        testSprite.transform.rotation = Camera.main.transform.rotation;
        
        // フェードとかさせたいなら、ここでαを調整
        testSprite.color = new Color(0, 0, 0, fadingAlpha);
    }
}
```
[Gist - Unity Sprite on front of camera](https://gist.github.com/h-sao/1871a05001c611436dd24b3b11fd9554)


こんな感じで、Spriteがカメラにくっついてきてくれます

 ![](/pic/Unity-Sprite-on-front-of-camera_00.gif)


忘れそうな自分へのメモ
