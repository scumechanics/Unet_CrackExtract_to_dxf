
[日本語版 README はこちら]("https://github.com/JarvisSan22/Unet_CrackExtract_to_dxf/blob/master/README-ja.md")
<br/>
Author: Daniel James Jarvis 
<br/>
Email: jarvissan21@gmail.com

# Unet crack extract to dxf file.

UNET model based for cell segmentation trained on the [deepcrack data set](https://github.com/yhlleo/DeepCrack). Model detect crack region, extracts contours then converts those contours in dxf files for CAD use. 

- [X] Code notebooks created 
- [X] 日本語版のREADME
- [ ] Longer model train 
- [ ] Code to importable module
- [ ] Non DeepCrack training, able to deal with larger images, diffrenataion from tiles and cracks around odd objects 

 
<img src="/pics/Process.png" width="600">


## Code
* [U_NET_CRACKS_20210702_DeepCrackDataset.ipynb](https://colab.research.google.com/github/JarvisSan22/Unet_CrackExtract_to_dxf/blob/master/U_NET_CRACKS_20210702_DeepCrackDataset.ipynb)  
Model training and testing
* [Crack_to_dxf.ipynb](https://colab.research.google.com/github/JarvisSan22/Unet_CrackExtract_to_dxf/blob/master/Crack_to_dxf.ipynb) 
Extract crake and convert to dxf
* [Create_Train_data.ipynb](https://colab.research.google.com/github/JarvisSan22/Unet_CrackExtract_to_dxf/blob/master/Create_Train_data.ipynb) 
Create dataset 

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

### Models accuracy and loss
Test run performet 20210702 bath size 15, validation_split 0.1  with a EarlyStopping call back for val_loss at patience=5

<img src="/pics/Accuracy_20210703.png"  width="600" >
<img src="/pics/Loss_20210703.png"   width=600 >

#### Prediction test 
<img src="/pics/ThreeModelTest_eng_20210702.png" width="600">

## Dataset 

Standard data set: [deepcrack data set](https://github.com/yhlleo/DeepCrack)
Create_Train_data.ipynb allows the creation of new traning and test data from crack images and ground truths. Ground truth -> black background, white crack regions/ 

<img src="/pics/train_data.png" width="600">
## Usage 

###Crack_to_dxf.ipynb 


Walk through 
* Check model save location 
* Load image
* If image is X times larger than training data, image is split into smaller sections (X = 4 standard). From each section cracks will be extracted then all the crack contours will be combined at the end
* Extracted crack contours converted to dxf file. 