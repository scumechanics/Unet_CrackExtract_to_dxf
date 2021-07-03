[英語版 README はこちら]("README.md")
作家: Daniel James Jarviws 
メール: jarvissan21@gmail.com

# Unet 亀裂の抽出からdxfのファイルに変換

UNETの細胞のセグメンテーションを基づいて、[deepcrack](https://github.com/yhlleo/DeepCrack)というデータセットに訓練されました。モデルは亀裂の範囲を検出して、輪郭を抽出し、CADを使用するために、結果をdxfのファイルに変換します。
 
- [X] ニートブックを作成 
- [X] 日本語版のREADME
- [ ] 長期的な訓練 
- [ ] コードを改善する
- [ ] DeepCrack以外のデータセットの訓練、大きな画像、タイルの区別、複雑な物体の周りに亀裂などを扱う機能


<img src="/pics/Process.png" width="600">


## コード
* U_NET_CRACKS_20210702_DeepCrackDataset.ipynb  [モデルの訓練とテスト]
* Crack_to_dxf.ipynb [亀裂を抽出し、dxfへ変換する]
* Create_Train_data.ipynb [データセットを作成する]

## セットアップ
GoogleColabでロードすれば、ezdxfというモジュールだけインストールしなければなりません。簡単ですね。
```batch
!pip3 install ezdxf
```
Colab以外の使用の場合は、requiments.txtをインストールしてください。
```batch
pip3 install -r requirments.txt 
```



## モデル

モデルの参考は Dr. Sreenivas Bhattiprolu  [U-net](https://github.com/bnsreenu/python_for_microscopists/blob/master/074-Defining%20U-net%20in%20Python%20using%20Keras.py)を作成した細胞の抽出するモデルです。 
白黒の画像や、畳み込みのセットの変更するために、更新しました。 (Standard, small(x0.5), large(x2))

### モデルの制度やロース
最初のテストは、2021年７月2日に実行しました。
設定：bath size 15, validation_split 0.1, Callbacks (EarlyStopping "val_loss" patience=5)


![ScreenShot](/pics/Accuracy_20210703.png)
<img src="/pics/Accuracy_20210703.png" width="600">
<img src="/pics/Loss_20210703.png" width="600">

#### Prediction test 
<img src="/pics/ThreeModelTest_jp_20210702.png" width="600">

## Dataset 

標準のデータセット: [deepcrack](https://github.com/yhlleo/DeepCrack)
Create_Train_data.ipynbのコードでは、自分の訓練やテストのデータを作成できます。亀裂の画像や黒白の画像（GroundTruth）が必要です。

<img src="/pics/train_data.png" width="600">
## Usage 

###Crack_to_dxf.ipynb 
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/weiji14/deepbedmap/]


亀裂の抽出する使用し方
* モデルの保存先を確認 
* 画像をロードする
* その画像のサイズは、訓練の画像のサイズの方がx倍に大きい場合は、ロードした画像を分けています。それぞれの分けた画像から亀裂を抽出してかた、もともとの画像に合流します。
* 抽出した亀裂の輪郭から、dxfのファイルに変換する