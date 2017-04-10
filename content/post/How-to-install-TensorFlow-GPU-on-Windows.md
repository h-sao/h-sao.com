+++
date = "2017-04-10T18:00:00+09:00"
draft = false
title = "[TensorFlow] TensorFlow on Windows GPU版インストール方法"
tags = ["Anaconda", "Python", "TensorFlow"]
+++

前回、TensorFlowの環境を作るために、Anacondaをインストールしました

- [Anaconda] Anaconda for Windowsインストール方法  
[http://h-sao.com/blog/2017/03/29/how-to-install-anaconda-for-windows/](http://h-sao.com/blog/2017/03/29/how-to-install-anaconda-for-windows/)

その続きで、TensorFlow GPUバージョンを入れたいと思います

わたしの環境は以下です

- Windows10 x64
- NVIDIA GeForce GTX 960
- Anaconda 4.3.1 For Windows

# はじめに…

TensorFlow公式サイトのインストール方法では、残念ながらうまく動作させることは出来ませんでした…(´・ω・`)

- ~~Installing TensorFlow on Windows~~  
（公式サイトだけど、2017/4/10時点、わたしの環境ではダメだった）  
~~[https://www.tensorflow.org/install/install_windows](https://www.tensorflow.org/install/install_windows)~~

どこを見たら良いかというと、**TensorFlowのGitHubの方** です…！！！  
**ここ見るべし(。-`ω-)ｸﾜﾜｯ**  

- **TensorFlow-Github-Install_Windows** tensorflow/tensorflow/docs_src/install/install_windows.md  
[https://github.com/tensorflow/tensorflow/blob/master/tensorflow/docs_src/install/install_windows.md](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/docs_src/install/install_windows.md)

今回の全インストールログは、わたしのGistにアップしておきました  
→ [Install_log_on_Windows_for_TensorFlow_GPU.log](https://gist.github.com/h-sao/b7ab04b3ba3fe8a9e41ec8af76da7559)

では、やった手順を記載します！

# 1) NVIDIA CUDA Toolkit をインストールする

TensorFlowにはCPU版とGPU版が提供されています

CPUのみでも動くのですけど  
開発環境の構築というのはなんともめんどくさいので  
最初に出来る事をやっとくことにします

まずは、**CUDA** のインストール  
※CUDAとは、GPUを活用するための開発環境です  
　GPUを使うには、NVIDIAのCUDAを入れる必要があります

- **CUDA Installation Guide for Microsoft Windows**  
[http://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/](http://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/)  
2017/4/10時点の NVIDIA CUDA Toolkit のバージョンは 8.0.61 です (Windows10)  
1.3GBありました！

<img src="/pic/How-to-install-TensorFlow-GPU-on-Windows_00.jpg" style="border:solid 5px #e6e6e6"/> 


わたしは **Visual Studio 2017 を入れているのですが、まだサポートされてないよ** って出ました  

> NVIDIA の公式によると、2017/4/10時点では  
> Visual Studio 2015 の VC++14.0 まで対応しているみたいです  
> <img src="/pic/How-to-install-TensorFlow-GPU-on-Windows_02.jpg" style="border:solid 5px #e6e6e6"/> 

NVIDIAのドライバも更新されるため、何度かチラチラと画面が黒くなりますが  
それ以外は特に問題もなく、Toolkitのインストールは完了します


# 2) NVIDIA Developer Membership に登録する

さて、続いて **cuDNN** をインストールする必要があります

**cuDNN** は The NVIDIA CUDA Deep Neural Network library の略  
つまりCUDAのライブラリ群です

- **NVIDIA cuDNN - GPU Accelerated Deep Learning**  
[https://developer.nvidia.com/cudnn](https://developer.nvidia.com/cudnn)

cuDNNをダウンロードするには、**NVIDIA Developer Membership に登録する必要があります**（登録だけなら、お金は要らなかった）  
登録時に簡単な Survey がありますので、良しなに記載しておきます

登録完了すると、速攻で

「Please confirm your email to join the Accelerated Computing Developer Program」

という件名のメールが飛んできました  
中のリンクをクリックしたら、登録完了です

こんな感じで NVIDIA Developer Membership への登録は瞬殺で終わりました   
<img src="/pic/How-to-install-TensorFlow-GPU-on-Windows_01.png" style="border:solid 5px #e6e6e6"/> 

# 3) NVIDIA cuDNN をインストールする（正確にはパスを通す）

登録が済んだら、cuDNN をダウンロード出来ます  
※ここでもまた、Survey があります(^^；


先ほどインストールした CUDA Toolkit は 8.0 なので、それに対応した cuDNN v6.0 を選択！

<img src="/pic/How-to-install-TensorFlow-GPU-on-Windows_03.jpg" style="border:solid 5px #e6e6e6"/> 


落としてきたのはただのZIPファイルなので、展開し、好きなディレクトリに設置します  
わたしの場合は、 **D:\Library\cuda** というディレクトリを作って置きました

> もしくは、cuda と cuDNN のバージョンは必ず一致しないと行けないようなので  
> cudaのインストールパス  
> （C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v8.0\bin）  
> に手コピーするのも、一つの手らしいです  
> cudaディレクトリにコピーした人はパスを設定しなくていいので、以下は飛ばしてください


このパス（わたしの場合、D:\Library\cuda\bin）が Anaconda 環境から見えるように、Windowsのパスを設定しておきます  
パスを通すのくらいは…と思いましたが、ついでにキャプチャは取ったので、それだけ載せておきますね

> コントロールパネルのシステムを開く  
<img src="/pic/How-to-install-TensorFlow-GPU-on-Windows_04.jpg" style="border:solid 5px #e6e6e6"/> 

> システムプロパティの環境設定を開く  
<img src="/pic/How-to-install-TensorFlow-GPU-on-Windows_05.png" style="border:solid 5px #e6e6e6"/> 

> 自分の環境設定のパスを編集する  
<img src="/pic/How-to-install-TensorFlow-GPU-on-Windows_06.jpg" style="border:solid 5px #e6e6e6"/> 

> パスを新規追加  
<img src="/pic/How-to-install-TensorFlow-GPU-on-Windows_07.png" style="border:solid 5px #e6e6e6"/> 

> 一応、パスが通ってることを path コマンドでチェックしとく  
<img src="/pic/How-to-install-TensorFlow-GPU-on-Windows_08.png" style="border:solid 5px #e6e6e6"/> 

ま、こんな感じです


# 4) TensorFlow on Windows (GPU) のインストール

いよいよ TensorFlow のインストールを行います

最初に、Anaconda Promptを開いて、環境をTensorFlow用のやつに変えておきます（[前回の記事](http://h-sao.com/blog/2017/03/29/how-to-install-anaconda-for-windows/)で書いたやつです）

- activateコマンドで環境を切り替えておく

```xml
#Change env

(C:\utils\Anaconda3) D:\dev\anaconda>activate tensorflow

(tensorflow) D:\dev\anaconda>
```

---
(以下、失敗手順も含んでいます)

- pip コマンドを使ってTensorFlow　GPU をインストールする  
（実はこの動作は 2017/4/10現在、間違っています）

```xml
# Installed TensorFlow (1st time, error occurred.)

(tensorflow) D:\dev\anaconda>pip install --upgrade tensorflow-gpu
Collecting tensorflow-gpu
  Downloading tensorflow_gpu-1.0.1-cp35-cp35m-win_amd64.whl (43.1MB)

（中略）

Successfully installed appdirs-1.4.3 numpy-1.12.1 packaging-16.8 protobuf-3.2.0 pyparsing-2.2.0 setuptools-34.3.3 six-1.10.0 tensorflow-gpu-1.0.1

（中略）

FileNotFoundError: [WinError 2] The system cannot find the file specified: 'C:\\utils\\Anaconda3\\envs\\tensorflow\\lib\\site-packages\\setuptools-27.2.0-py3.5.egg'

(tensorflow) D:\dev\anaconda>
```

Tensorflowのインストールは出来ましたが、一番最後、なにやらファイルが無いとエラーが出ています  
よくわからない(￣▽￣)

- インポートの動作確認してみますが、うまくいってないよう…  

**cudnn64_5.dllが無い** って言われる

```xml
# Check import version

(tensorflow) D:\dev\anaconda>python
Python 3.5.3 |Continuum Analytics, Inc.| (default, Feb 22 2017, 21:28:42) [MSC v.1900 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import tensorflow
I c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\stream_executor\dso_loader.cc:135] successfully opened CUDA library cublas64_80.dll locally
I c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\stream_executor\dso_loader.cc:126] Couldn't open CUDA library cudnn64_5.dll
I c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\stream_executor\cuda\cuda_dnn.cc:3517] Unable to load cuDNN DSO
I c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\stream_executor\dso_loader.cc:135] successfully opened CUDA library cufft64_80.dll locally
I c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\stream_executor\dso_loader.cc:135] successfully opened CUDA library nvcuda.dll locally
I c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\stream_executor\dso_loader.cc:135] successfully opened CUDA library curand64_80.dll locally
>>> print( tensorflow.__version__)
1.0.1
>>> exit()

(tensorflow) D:\dev\anaconda>
```



あれ？  
ちゃんと**CUDA8.0 に対応した cuDNN6.0 を入れたのに**…(´・ω・`)

良く見てみると、わたしの落としてきた cuDNN の DLL は **cudnn64_6.dll**  
エラーが出てるDLLは **cudnn64_5.dll**

…もうGetできなくね？？？(;^_^A  
。  
。  

--- 

- 暫定対応

調べてもよく判らなかったので  
持ってるDLLのファイル名を変更して使うことにしました(*ﾉωﾉ)  
なんか今後問題でそうですが、致し方なし…  
誰か、解決策あるなら教えて下さい（T▽T）

<img src="/pic/How-to-install-TensorFlow-GPU-on-Windows_09.png" style="border:solid 5px #e6e6e6"/> 

---

- 再度、インポート動作確認

TensorFlowのバージョンは 1.0.1、インストールしまして、インポートでのエラーは出なくなりましたが…  

```xml
(tensorflow) D:\dev\anaconda>python
Python 3.5.3 |Continuum Analytics, Inc.| (default, Feb 22 2017, 21:28:42) [MSC v.1900 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import tensorflow
I c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\stream_executor\dso_loader.cc:135] successfully opened CUDA library cublas64_80.dll locally
I c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\stream_executor\dso_loader.cc:135] successfully opened CUDA library cudnn64_5.dll locally
I c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\stream_executor\dso_loader.cc:135] successfully opened CUDA library cufft64_80.dll locally
I c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\stream_executor\dso_loader.cc:135] successfully opened CUDA library nvcuda.dll locally
I c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\stream_executor\dso_loader.cc:135] successfully opened CUDA library curand64_80.dll locally
>>> print( tensorflow.__version__)
1.0.1
>>> exit()

(tensorflow) D:\dev\anaconda>
```

実はこの後、Hello TensorFlow! を表示しようとして、別の **OpKernel** エラーが大量発生します  

> E c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\core\framework\op_kernel.cc:943] OpKernel ('op: "BestSplits" device_type: "CPU"') for unknown op: BestSplits

なんだよこれは…(。-`ω-)

```xml
# Remained the same error too. (OpKernel error)

(tensorflow) D:\dev\anaconda>python
Python 3.5.3 |Continuum Analytics, Inc.| (default, Feb 22 2017, 21:28:42) [MSC v.1900 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import tensorflow as tf
I c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\stream_executor\dso_loader.cc:135] successfully opened CUDA library cublas64_80.dll locally
I c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\stream_executor\dso_loader.cc:135] successfully opened CUDA library cudnn64_5.dll locally
I c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\stream_executor\dso_loader.cc:135] successfully opened CUDA library cufft64_80.dll locally
I c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\stream_executor\dso_loader.cc:135] successfully opened CUDA library nvcuda.dll locally
I c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\stream_executor\dso_loader.cc:135] successfully opened CUDA library curand64_80.dll locally
>>> hello = tf.constant('Hello tensorflow!')
>>> sess = tf.Session()
I c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\core\common_runtime\gpu\gpu_device.cc:885] Found device 0 with properties:
name: GeForce GTX 960
major: 5 minor: 2 memoryClockRate (GHz) 1.253
pciBusID 0000:05:00.0
Total memory: 2.00GiB
Free memory: 1.64GiB
I c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\core\common_runtime\gpu\gpu_device.cc:906] DMA: 0
I c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\core\common_runtime\gpu\gpu_device.cc:916] 0:   Y
I c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\core\common_runtime\gpu\gpu_device.cc:975] Creating TensorFlow device (/gpu:0) -> (device: 0, name: GeForce GTX 960, pci bus id: 0000:05:00.0)
>>> print(sess.run(hello))
E c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\core\framework\op_kernel.cc:943] OpKernel ('op: "BestSplits" device_type: "CPU"') for unknown op: BestSplits
E c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\core\framework\op_kernel.cc:943] OpKernel ('op: "CountExtremelyRandomStats" device_type: "CPU"') for unknown op: CountExtremelyRandomStats
E c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\core\framework\op_kernel.cc:943] OpKernel ('op: "FinishedNodes" device_type: "CPU"') for unknown op: FinishedNodes
E c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\core\framework\op_kernel.cc:943] OpKernel ('op: "GrowTree" device_type: "CPU"') for unknown op: GrowTree
E c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\core\framework\op_kernel.cc:943] OpKernel ('op: "ReinterpretStringToFloat" device_type: "CPU"') for unknown op: ReinterpretStringToFloat
E c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\core\framework\op_kernel.cc:943] OpKernel ('op: "SampleInputs" device_type: "CPU"') for unknown op: SampleInputs
E c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\core\framework\op_kernel.cc:943] OpKernel ('op: "ScatterAddNdim" device_type: "CPU"') for unknown op: ScatterAddNdim
E c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\core\framework\op_kernel.cc:943] OpKernel ('op: "TopNInsert" device_type: "CPU"') for unknown op: TopNInsert
E c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\core\framework\op_kernel.cc:943] OpKernel ('op: "TopNRemove" device_type: "CPU"') for unknown op: TopNRemove
E c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\core\framework\op_kernel.cc:943] OpKernel ('op: "TreePredictions" device_type: "CPU"') for unknown op: TreePredictions
E c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\core\framework\op_kernel.cc:943] OpKernel ('op: "UpdateFertileSlots" device_type: "CPU"') for unknown op: UpdateFertileSlots
b'Hello tensorflow!'
>>> exit()
```

ここでようやく、インストールが上手く行ってないことに気が付きます(+o+)

---

よし、気を取り直して  
**公式サイトではなく、GitHubの方に書かれている、インストール方法** を参考にしてみます

- さっき入れた TensorFlow1.0.1 をアンインストール

```
# Uninstalled TensorFlow

(tensorflow) D:\dev\anaconda>pip uninstall tensorflow-gpu
Uninstalling tensorflow-gpu-1.0.1:
  c:\utils\anaconda3\envs\tensorflow\lib\site-packages\external\d3\d3.js
  c:\utils\anaconda3\envs\tensorflow\lib\site-packages\external\d3\d3.min.js
  
（中略）

  c:\utils\anaconda3\envs\tensorflow\scripts\tensorboard.exe
Proceed (y/n)? y
  Successfully uninstalled tensorflow-gpu-1.0.1
```

---


- 再度、**公式のGitHubの方に書かれているWindowsインストール方法を実施**  
[https://github.com/tensorflow/tensorflow/blob/master/tensorflow/docs_src/install/install_windows.md](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/docs_src/install/install_windows.md)

> ※2017/4/10現在の情報です、**正確には公式GitHubサイトを確認してください**  
> \> pip install --ignore-installed --upgrade https://storage.googleapis.com/tensorflow/windows/gpu/tensorflow_gpu-1.1.0rc1-cp35-cp35m-win_amd64.whl


```
# Re-install TensorFlow for Anaconda, it's best way for Anaconda/Windows user!

(tensorflow) D:\dev\anaconda>pip install --ignore-installed --upgrade https://storage.googleapis.com/tensorflow/windows/gpu/tensorflow_gpu-1.1.0rc1-cp35-cp35m-win_amd64.whl
Collecting tensorflow-gpu==1.1.0rc1 from https://storage.googleapis.com/tensorflow/windows/gpu/tensorflow_gpu-1.1.0rc1-cp35-cp35m-win_amd64.whl
  Downloading https://storage.googleapis.com/tensorflow/windows/gpu/tensorflow_gpu-1.1.0rc1-cp35-cp35m-win_amd64.whl (48.6MB)
    100% |################################| 48.6MB 26kB/s
Collecting werkzeug>=0.11.10 (from tensorflow-gpu==1.1.0rc1)
  Downloading Werkzeug-0.12.1-py2.py3-none-any.whl (312kB)
    100% |################################| 317kB 3.0MB/s
Collecting numpy>=1.11.0 (from tensorflow-gpu==1.1.0rc1)
  Using cached numpy-1.12.1-cp35-none-win_amd64.whl
Collecting protobuf>=3.2.0 (from tensorflow-gpu==1.1.0rc1)
  Using cached protobuf-3.2.0-py2.py3-none-any.whl
Collecting six>=1.10.0 (from tensorflow-gpu==1.1.0rc1)
  Using cached six-1.10.0-py2.py3-none-any.whl
Collecting wheel>=0.26 (from tensorflow-gpu==1.1.0rc1)
  Downloading wheel-0.29.0-py2.py3-none-any.whl (66kB)
    100% |################################| 71kB 4.0MB/s
Collecting setuptools (from protobuf>=3.2.0->tensorflow-gpu==1.1.0rc1)
  Using cached setuptools-34.3.3-py2.py3-none-any.whl
Collecting packaging>=16.8 (from setuptools->protobuf>=3.2.0->tensorflow-gpu==1.1.0rc1)
  Using cached packaging-16.8-py2.py3-none-any.whl
Collecting appdirs>=1.4.0 (from setuptools->protobuf>=3.2.0->tensorflow-gpu==1.1.0rc1)
  Using cached appdirs-1.4.3-py2.py3-none-any.whl
Collecting pyparsing (from packaging>=16.8->setuptools->protobuf>=3.2.0->tensorflow-gpu==1.1.0rc1)
  Using cached pyparsing-2.2.0-py2.py3-none-any.whl
Installing collected packages: werkzeug, numpy, six, pyparsing, packaging, appdirs, setuptools, protobuf, wheel, tensorflow-gpu
Successfully installed appdirs-1.4.3 numpy-1.12.1 packaging-16.8 protobuf-3.2.0 pyparsing-2.2.0 setuptools-34.3.3 six-1.10.0 tensorflow-gpu-1.1.0rc1 werkzeug-0.12.1 wheel-0.29.0
```

TensorFlowのバージョンは 1.1.0rc1 になりました  
（2017/4/10現在）

- Hello TensorFlow! の表示で動作確認

```xml
# Hello TensorFlow!

(tensorflow) D:\dev\anaconda>python
Python 3.5.3 |Continuum Analytics, Inc.| (default, Feb 22 2017, 21:28:42) [MSC v.1900 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import tensorflow as tf
>>> hello = tf.constant('Hello tensorflow')
>>> sess = tf.Session()
2017-04-07 17:55:06.792343: W c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\core\platform\cpu_feature_guard.cc:45] The TensorFlow library wasn't compiled to use SSE instructions, but these are available on your machine and could speed up CPU computations.
2017-04-07 17:55:06.794854: W c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\core\platform\cpu_feature_guard.cc:45] The TensorFlow library wasn't compiled to use SSE2 instructions, but these are available on your machine and could speed up CPU computations.
2017-04-07 17:55:06.795650: W c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\core\platform\cpu_feature_guard.cc:45] The TensorFlow library wasn't compiled to use SSE3 instructions, but these are available on your machine and could speed up CPU computations.
2017-04-07 17:55:06.798303: W c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\core\platform\cpu_feature_guard.cc:45] The TensorFlow library wasn't compiled to use SSE4.1 instructions, but these are available on your machine and could speed up CPU computations.
2017-04-07 17:55:06.798924: W c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\core\platform\cpu_feature_guard.cc:45] The TensorFlow library wasn't compiled to use SSE4.2 instructions, but these are available on your machine and could speed up CPU computations.
2017-04-07 17:55:06.799553: W c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\core\platform\cpu_feature_guard.cc:45] The TensorFlow library wasn't compiled to use AVX instructions, but these are available on your machine and could speed up CPU computations.
2017-04-07 17:55:07.171447: I c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\core\common_runtime\gpu\gpu_device.cc:887] Found device 0 with properties:
name: GeForce GTX 960
major: 5 minor: 2 memoryClockRate (GHz) 1.253
pciBusID 0000:05:00.0
Total memory: 2.00GiB
Free memory: 1.64GiB
2017-04-07 17:55:07.171652: I c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\core\common_runtime\gpu\gpu_device.cc:908] DMA: 0
2017-04-07 17:55:07.178823: I c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\core\common_runtime\gpu\gpu_device.cc:918] 0:   Y
2017-04-07 17:55:07.180026: I c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\core\common_runtime\gpu\gpu_device.cc:977] Creating TensorFlow device (/gpu:0) -> (device: 0, name: GeForce GTX 960, pci bus id: 0000:05:00.0)
>>> print(sess.run(hello))
b'Hello tensorflow'
>>> exit()
```

これでOpKernelエラーは出なくなりましたし、GPU利用のログも出ていますね

> name: GeForce GTX 960  
> major: 5 minor: 2 memoryClockRate (GHz) 1.253  
> ...

ほっ…(≧▽≦)

# （おまけ）SSEのワーニングを消すには？

しかし良く見て！？  
エラーは消えたけど、ワーニングは出てるよっ！？！？

どうやらこれについては、本家GitHub issue でも取りざたされています

- **"The TensorFlow library wasn't compiled to use SSE instructions, but these are available on your machine and could speed up CPU computations" in "Hello, TensorFlow!" program** #7778  
[https://github.com/tensorflow/tensorflow/issues/7778](https://github.com/tensorflow/tensorflow/issues/7778)

意味は、ソースからコンパイルし直したら、CPUが早くなるんじゃね？というお知らせの様です

うーん…GPUで動かしてるのにCPUを早くする？？よくわからんです

これは放置でも動作に問題ないらしいのですが、見たくない場合は、ログレベルを変更したら見えなくなると、issueに記載がありました

> import os  
> os.environ['TF_CPP_MIN_LOG_LEVEL']='2'

こんな感じですっきりしました  
GPUの確認ログも消えちゃいますけど…

```
# Changed worning level

(tensorflow) D:\dev\anaconda>python
Python 3.5.3 |Continuum Analytics, Inc.| (default, Feb 22 2017, 21:28:42) [MSC v.1900 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import os
>>> os.environ['TF_CPP_MIN_LOG_LEVEL']='2'
>>> import tensorflow as tf
>>> hello = tf.constant('Hello tensorflow')
>>> sess = tf.Session()
>>> print(sess.run(hello))
b'Hello tensorflow'
>>> exit()
```

とりあえず、TensorFlow on Windows (GPU) のインストールと、最初の動作確認は完了しました

作業の全ログはこちらに置いています…

- **Install_log_on_Windows_for_TensorFlow_GPU.log - Gist/h-sao**  
[https://gist.github.com/h-sao/b7ab04b3ba3fe8a9e41ec8af76da7559](https://gist.github.com/h-sao/b7ab04b3ba3fe8a9e41ec8af76da7559)



おつかれさまでした～  


## その他ポイント1：Anacondaではpipを使う

Anacondaでインストールする場合は、pipを使う（pip3 は無いみたい）
  
```xml
(tensorflow) D:\dev\anaconda>pip3
'pip3' is not recognized as an internal or external command,
operable program or batch file.
```

## その他ポイント2：Pythonのバージョンは3.5

Tensorflowは Python3.5系じゃないとだめ  
3.6に入れようとしたら、怒られました
試しに、CPU版にしても怒られました

```xml
(ten) C:\Users\akiko.kawai>pip install tensorflow-gpu
Collecting tensorflow-gpu
  Could not find a version that satisfies the requirement tensorflow-gpu (from versions: )
No matching distribution found for tensorflow-gpu
```

どうやら、pythonのバージョンは Windowsの場合3.5系でないと対応してないみたいです（2017/4/10現在）


---

（その他参考）

> 同じOpKernelエラー現象が出てる海外のも居た…   
Installation log of Tensorflow 1.0 on Windows using pip  
> [https://gist.github.com/apacha/a595c244f90a27aced56f67f7598d90d](https://gist.github.com/apacha/a595c244f90a27aced56f67f7598d90d)
