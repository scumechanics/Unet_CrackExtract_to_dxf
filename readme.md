
[日本語版 README はこちら]("README-ja.md")
Author: Daniel James Jarviws 
Email: jarvissan21@gmail.com

# Unet crack extract to dxf file.

UNET model based for cell segmentation trained on the [deepcrack data set](https://github.com/yhlleo/DeepCrack). Model detect crack region, extracts contours then converts those contours in dxf files for CAD use. 

- [X] Code notebooks created 
- [ ] 日本語版のREADME
- [ ] Longer model train 
- [ ] Code to importable module
- [ ] Non DeepCrack training, able to deal with larger images, diffrenataion from tiles and cracks around odd objects 

 
<img src="/pics/Process.png" width="600">


## Code
* U_NET_CRACKS_20210702_DeepCrackDataset.ipynb  [Modeling tranning and testing]
* Crack_to_dxf.ipynb [Crack extraction from saved model]
* Create_Train_data.ipynb [Dataset creation]

## Setup

Load in GoogleColab and only set required is installing ezdxf for crontours extraction to cad file 
```batch
!pip3 install ezdxf
```

No Colab use, install the requiments.txt 
```batch
pip3 install -r requirments.txt 
```



## Model 

Model source Dr. Sreenivas Bhattiprolu  [U-net](https://github.com/bnsreenu/python_for_microscopists/blob/master/074-Defining%20U-net%20in%20Python%20using%20Keras.py), altenred to for black and white images and tested at diffrent convolution (Standard, small(x0.5), large(x2))
<details>
<summary>Click to show model summary 
```bash

Model: "model"
__________________________________________________________________________________________________
Layer (type)                    Output Shape         Param #     Connected to                     
==================================================================================================
input_1 (InputLayer)            [(None, 128, 128, 1) 0                                            
__________________________________________________________________________________________________
lambda (Lambda)                 (None, 128, 128, 1)  0           input_1[0][0]                    
__________________________________________________________________________________________________
conv2d (Conv2D)                 (None, 128, 128, 16) 160         lambda[0][0]                     
__________________________________________________________________________________________________
dropout (Dropout)               (None, 128, 128, 16) 0           conv2d[0][0]                     
__________________________________________________________________________________________________
conv2d_1 (Conv2D)               (None, 128, 128, 16) 2320        dropout[0][0]                    
__________________________________________________________________________________________________
max_pooling2d (MaxPooling2D)    (None, 64, 64, 16)   0           conv2d_1[0][0]                   
__________________________________________________________________________________________________
conv2d_2 (Conv2D)               (None, 64, 64, 32)   4640        max_pooling2d[0][0]              
__________________________________________________________________________________________________
dropout_1 (Dropout)             (None, 64, 64, 32)   0           conv2d_2[0][0]                   
__________________________________________________________________________________________________
conv2d_3 (Conv2D)               (None, 64, 64, 32)   9248        dropout_1[0][0]                  
__________________________________________________________________________________________________
max_pooling2d_1 (MaxPooling2D)  (None, 32, 32, 32)   0           conv2d_3[0][0]                   
__________________________________________________________________________________________________
conv2d_4 (Conv2D)               (None, 32, 32, 64)   18496       max_pooling2d_1[0][0]            
__________________________________________________________________________________________________
dropout_2 (Dropout)             (None, 32, 32, 64)   0           conv2d_4[0][0]                   
__________________________________________________________________________________________________
conv2d_5 (Conv2D)               (None, 32, 32, 64)   36928       dropout_2[0][0]                  
__________________________________________________________________________________________________
max_pooling2d_2 (MaxPooling2D)  (None, 16, 16, 64)   0           conv2d_5[0][0]                   
__________________________________________________________________________________________________
conv2d_6 (Conv2D)               (None, 16, 16, 128)  73856       max_pooling2d_2[0][0]            
__________________________________________________________________________________________________
dropout_3 (Dropout)             (None, 16, 16, 128)  0           conv2d_6[0][0]                   
__________________________________________________________________________________________________
conv2d_7 (Conv2D)               (None, 16, 16, 128)  147584      dropout_3[0][0]                  
__________________________________________________________________________________________________
max_pooling2d_3 (MaxPooling2D)  (None, 8, 8, 128)    0           conv2d_7[0][0]                   
__________________________________________________________________________________________________
conv2d_8 (Conv2D)               (None, 8, 8, 256)    295168      max_pooling2d_3[0][0]            
__________________________________________________________________________________________________
dropout_4 (Dropout)             (None, 8, 8, 256)    0           conv2d_8[0][0]                   
__________________________________________________________________________________________________
conv2d_9 (Conv2D)               (None, 8, 8, 256)    590080      dropout_4[0][0]                  
__________________________________________________________________________________________________
conv2d_transpose (Conv2DTranspo (None, 16, 16, 128)  131200      conv2d_9[0][0]                   
__________________________________________________________________________________________________
concatenate (Concatenate)       (None, 16, 16, 256)  0           conv2d_transpose[0][0]           
                                                                 conv2d_7[0][0]                   
__________________________________________________________________________________________________
conv2d_10 (Conv2D)              (None, 16, 16, 128)  295040      concatenate[0][0]                
__________________________________________________________________________________________________
dropout_5 (Dropout)             (None, 16, 16, 128)  0           conv2d_10[0][0]                  
__________________________________________________________________________________________________
conv2d_11 (Conv2D)              (None, 16, 16, 128)  147584      dropout_5[0][0]                  
__________________________________________________________________________________________________
conv2d_transpose_1 (Conv2DTrans (None, 32, 32, 64)   32832       conv2d_11[0][0]                  
__________________________________________________________________________________________________
concatenate_1 (Concatenate)     (None, 32, 32, 128)  0           conv2d_transpose_1[0][0]         
                                                                 conv2d_5[0][0]                   
__________________________________________________________________________________________________
conv2d_12 (Conv2D)              (None, 32, 32, 64)   73792       concatenate_1[0][0]              
__________________________________________________________________________________________________
dropout_6 (Dropout)             (None, 32, 32, 64)   0           conv2d_12[0][0]                  
__________________________________________________________________________________________________
conv2d_13 (Conv2D)              (None, 32, 32, 64)   36928       dropout_6[0][0]                  
__________________________________________________________________________________________________
conv2d_transpose_2 (Conv2DTrans (None, 64, 64, 32)   8224        conv2d_13[0][0]                  
__________________________________________________________________________________________________
concatenate_2 (Concatenate)     (None, 64, 64, 64)   0           conv2d_transpose_2[0][0]         
                                                                 conv2d_3[0][0]                   
__________________________________________________________________________________________________
conv2d_14 (Conv2D)              (None, 64, 64, 32)   18464       concatenate_2[0][0]              
__________________________________________________________________________________________________
dropout_7 (Dropout)             (None, 64, 64, 32)   0           conv2d_14[0][0]                  
__________________________________________________________________________________________________
conv2d_15 (Conv2D)              (None, 64, 64, 32)   9248        dropout_7[0][0]                  
__________________________________________________________________________________________________
conv2d_transpose_3 (Conv2DTrans (None, 128, 128, 16) 2064        conv2d_15[0][0]                  
__________________________________________________________________________________________________
concatenate_3 (Concatenate)     (None, 128, 128, 32) 0           conv2d_transpose_3[0][0]         
                                                                 conv2d_1[0][0]                   
__________________________________________________________________________________________________
conv2d_16 (Conv2D)              (None, 128, 128, 16) 4624        concatenate_3[0][0]              
__________________________________________________________________________________________________
dropout_8 (Dropout)             (None, 128, 128, 16) 0           conv2d_16[0][0]                  
__________________________________________________________________________________________________
conv2d_17 (Conv2D)              (None, 128, 128, 16) 2320        dropout_8[0][0]                  
__________________________________________________________________________________________________
conv2d_18 (Conv2D)              (None, 128, 128, 1)  17          conv2d_17[0][0]                  
==================================================================================================
Total params: 1,940,817
Trainable params: 1,940,817
Non-trainable params: 0

```
</summary>
</details>



### Models accuracy and loss
Test run performet 20210702 bath size 15, validation_split 0.1  with a EarlyStopping call back for val_loss at patience=5

<img src="/pics/Accuracy_20210703.png"  width=100 >
<img src="/pics/Loss_20210703.png"   width=100 >

#### Prediction test 
<img src="/pics/ThreeModelTest_eng_20210702.png" width="600">

## Dataset 

Standard data set: [deepcrack data set](https://github.com/yhlleo/DeepCrack)
Create_Train_data.ipynb allows the creation of new traning and test data from crack images and ground truths. Ground truth -> black background, white crack regions/ 

<img src="/pics/train_data.png" width="600">
## Usage 

###Crack_to_dxf.ipynb 
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/weiji14/deepbedmap/]


Walk through 
* Check model save location 
* Load image
* If image is X times larger than training data, image is split into smaller sections (X = 4 standard), then recontructed at the end 
* Extract Data 