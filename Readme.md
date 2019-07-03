# Person Recognition
## Main Goal
The task that the designed neural networks  have to perform is: given two images, state if they are from the same identity or not.
### Same identity

![Robert De Niro](https://github.com/sburrel/AIDL2019_PROJECT_SBD/blob/master/dataset-cfp/Data/Images/407/frontal/02.jpg?raw=true)  ![Robert De Niro](https://github.com/sburrel/AIDL2019_PROJECT_SBD/blob/master/dataset-cfp/Data/Images/407/profile/02.jpg?raw=true)
### Different identity

![Jim Carrey](https://github.com/sburrel/AIDL2019_PROJECT_SBD/blob/master/dataset-cfp/Data/Images/230/frontal/08.jpg?raw=true)![Adam Sandler](https://github.com/sburrel/AIDL2019_PROJECT_SBD/blob/master/dataset-cfp/Data/Images/006/frontal/02.jpg?raw=true)
## Data set
The used data set is Celebrities in Frontal-Profile. The data set contains 10 frontal and 4 profile images of 500 individuals. Similar to LFW, it has 10 splits defined, each containing 350 same and 350 not-same pairs. This data set is to be used for face verification.

![Sample Images from Celebrities in Frontal-Profile (CFP)](https://github.com/sburrel/AIDL2019_PROJECT_SBD/blob/master/Figures/CFP.png?raw=true)

As Sengupta et al. [1] shows, many existing algorithms suffer a decrease over 10% of the accuracy in frontal-profile verification compared to  frontal-frontal. Cross-pose face recognition is still an extremely challenging scene.

## Implemented  Arquitectures
In order to perform the verification task each one of the images passes through a **Siamese Network that that share all the weights** while working in tandem on the two different images. For the Siamese's Networks we use the convolutional layers of a [network pretrained for image classification](https://pytorch.org/docs/stable/torchvision/models.html#classification). With the features extracted from the Siamese's Networks, we explore two different strategies:
1. Use a Decision Network to classify the two images as belonging to the same or different identity.
2. Use a Loss Function that compresses intra-variance (same identity) and enlarges inter-variance (different identities).

![Architecture 1](https://github.com/sburrel/AIDL2019_PROJECT_SBD/blob/master/Figures/Siameses1_v2.png?raw=true)![Architecture 2](https://github.com/sburrel/AIDL2019_PROJECT_SBD/blob/master/Figures/Siameses2_v2.png?raw=true)

### Architecture 1

![Architecture 1](https://github.com/sburrel/AIDL2019_PROJECT_SBD/blob/master/Figures/Architecture1.png?raw=true)
Next, we include , as an example, the code sniped with the implemented model at version 2 (all the versions are described in paragraph: How to reproduce the experiments).

```javascript
import torch
from torch import nn
from torchvision.models import vgg16_bn


class SiameseDecision(nn.Module):
    """
    Siamese network. Siamese_linear = False.
    """
    def __init__(self, pretrained=False):
        super().__init__()

        self.feat = vgg16_bn(pretrained=pretrained).features

        self.fc1 = nn.Sequential(
            nn.Linear(in_features=512*7*7*2, out_features=4096),
            nn.ReLU(True),
            nn.Dropout()
        )
        self.fc2 = nn.Sequential(
            nn.Linear(in_features=4096, out_features=4096),
            nn.ReLU(True),
            nn.Dropout(),
            nn.Linear(in_features=4096, out_features=2) ,
        )

    def forward(self, img1, img2):
        # the input to the vgg16 is a fixed-size 224x224 RGB image
        # we get the vgg16 features
        feat_1 = self.feat(img1)
        feat_2 = self.feat(img2)
        feat_1 = feat_1.view(feat_1.size(0), -1)
        feat_2 = feat_2.view(feat_2.size(0), -1)
        # we concatenate the two tensors of features
        feat = torch.cat((feat_1, feat_2), 1)
        # we run the classifier
        feat_3 = self.fc1(feat)
        tag = self.fc2(feat_3)
        return tag
```


#### Architecture 1, variation
We explore a slight variation of the first architecture, moving one of the fully connected layers to the Siamese's Networks to extract a more compact representation of the features.

![Architecture 1, variation](https://github.com/sburrel/AIDL2019_PROJECT_SBD/blob/master/Figures/Architecture1_var.png?raw=true)
Below, we include, as an example, the code sniped with the implemented model at version 2 (all the versions are described in paragraph: How to reproduce the experiments).

```javascript
class SiameseLinearDecision(nn.Module):
    """
    Siamese network. Siamese_linear = True.
    """
    def __init__(self, pretrained=False):
        super().__init__()

        self.feat = vgg16_bn(pretrained=pretrained).features
        
        self.fc1 = nn.Sequential(
            nn.Linear(in_features=512*7*7, out_features=4096),
            nn.ReLU(True),
            nn.Dropout()
        )
        self.fc2 = nn.Sequential(
            nn.Linear(in_features=4096*2, out_features=4096),
            nn.ReLU(True),
            nn.Dropout(),
            nn.Linear(in_features=4096, out_features=2) ,
        )

    def forward(self, img1, img2):
        feat_1 = self.feat(img1)
        feat_2 = self.feat(img2)
        feat_1 = feat_1.view(feat_1.size(0), -1)
        feat_2 = feat_2.view(feat_2.size(0), -1)
        feat_1 = self.fc1(feat_1)
        feat_2 = self.fc1(feat_2)
       
        feat = torch.cat((feat_1, feat_2), 1)
        tag = self.fc2(feat)
        
        return tag
```

### Architecture 2

![Architecture 2](https://github.com/sburrel/AIDL2019_PROJECT_SBD/blob/master/Figures/Architecture2.png?raw=true)
Next, we include, as an example, the code sniped with the implemented model at version 7 (all the versions are described in paragraph: How to reproduce the experiments).

```javascript
import torch
from torch import nn
from torchvision.models import resnext50_32x4d

def l2norm(x):
  x = x / torch.sqrt(torch.sum(x**2, dim=-1, keepdim=True))
  return x

class SiameseCosine(nn.Module):
    """
    Siamese network
    """
    def __init__(self, pretrained=False):
        super(SiameseCosine, self).__init__()

        resnext50_32x4d_model = resnext50_32x4d(pretrained=pretrained)
        self.feat = resnext50_32x4d_model
        
    def forward(self, img1, img2):
        feat_1 = self.feat(img1)
        feat_1 = l2norm(feat_1)
        
        feat_2 = self.feat(img2)
        feat_2 = l2norm(feat_2)

      
        return feat_1, feat_2
```

#### Architecture 2 during testing or inference

![Architecture 2 during testing or inference](https://github.com/sburrel/AIDL2019_PROJECT_SBD/blob/master/Figures/Architecture2_test.png?raw=true)
To obtain the threshold, we first compute all the cosine similarities for the pairs of images in the validation test and extract the 5 percentile for same identities pairs of images and  the 95 percentile for different identities pairs of images. Second, we calculate the threshold (with only one decimal precision) between the 5 percentile for same identities pairs of images and 95 percentile for different identities pairs of images. To obtain the threshold with the second and the third decimal precision, we iterate between the threshold obtained previously plus/minus half of its precision with increments of one unit of the new precision. For more clarity, the sniped code is included bellow.

```javascript
import numpy as np
# Compute similarities and obtain limit percentiles
sim_pos, sim_neg = val_sim_lim(model, val_loader, epoch, device=device)
pos_5 = np.percentile(sim_pos,5)
neg_95 = np.percentile(sim_neg,95)

```

```javascript
# Select treshold
best_acc = 0
best_tr = 0
sup = numpy.round(neg_95,decimals=3)
inf = numpy.round(pos_5,decimals=3)
for value in numpy.arange(inf, sup, 0.1):
    numpy.round(value,decimals=3)
    val_acc = val_tr(model, val_loader, epoch, device=device, tr=value)
    av_acc = np.mean(val_acc)
    if av_acc > best_acc:
      best_acc = av_acc
      best_tr = value
      print('Best accuracy:', av_acc, 'Treshold:', best_tr)

sup = numpy.round(best_tr+.05,decimals=3)
inf = numpy.round(best_tr-.05,decimals=3)
for value in numpy.arange(inf, sup, 0.01):
    numpy.round(value,decimals=3)
    val_acc = val_tr(model, val_loader, epoch, device=device, tr=value)
    av_acc = np.mean(val_acc)
    if av_acc > best_acc:
      best_acc = av_acc
      best_tr = value
      print('Best accuracy:', av_acc, 'Treshold:', best_tr)

sup = numpy.round(best_tr+.005,decimals=3)
inf = numpy.round(best_tr-.005,decimals=3)
for value in numpy.arange(inf, sup, 0.001):
    numpy.round(value,decimals=3)
    val_acc = val_tr(model, val_loader, epoch, device=device, tr=value)
    av_acc = np.mean(val_acc)
    if av_acc > best_acc:
      best_acc = av_acc
      best_tr = value
      print('Best accuracy:', av_acc, 'Treshold:', best_tr)
      
print('Best accuracy:', av_acc, 'Treshold:', best_tr)
```

## Experiments Performed
### Studied Parameters/Strategies
The following parameters/strategies have been studied in order to see its influence in the network's behavior and increase the accuracy:
1. Architectures described above
2. Learning rate
3. Use of the pre-trained weights of the Convolutional Network
4. Freeze some of the layers of the Convolutional Network
5. Use of different [Convolutional Networks](https://pytorch.org/docs/stable/torchvision/models.html#classification)
6. Data Augmentation
7. Loss Function

The Data augmentation implemented consists on [horizontal flips](https://pytorch.org/docs/stable/_modules/torchvision/transforms/transforms.html#RandomHorizontalFlip) and [random rotations](https://pytorch.org/docs/stable/_modules/torchvision/transforms/transforms.html#RandomRotation) of +20 and -20 degrees.

For the first architecture we have studied the influence of having a single output (vector of dimension 1) at the end of the network and using [Binary Cross Entropy Loss](https://pytorch.org/docs/stable/nn.html#torch.nn.BCELoss) or  having two outputs (vector of dimension 2) and optimizing the [Cross Entropy Loss](https://pytorch.org/docs/stable/nn.html#crossentropyloss).

For the second architecture we have used the [Cosine Embedding Loss](https://pytorch.org/docs/stable/nn.html#cosineembeddingloss). It's a cosine margin based loss that learns to discriminate face features in terms of angular similarity of the feature vectors.
 
### Results Obtained
We start studying the influence of data augmentation and the use or not of the pretrained weights at the convolutional network for Architecture 1 and Binary Cross Entropy Loss (see results bellow).

| Experiment ID | Architecture | Loss | Features from | Learning Rate | Epochs | Freeze | Pretrained |Data Augmentation | Validation Accuracy | Best Epoch |
|:--------:| :--------:| :--------:| :--------:| :--------:| :--------:|:--------:|:--------:|:--------:|:--------:|:--------:|
| 01 | 1 | BCELoss | VGG16's Convolutionals | 1e-4 | 7 | Not | Yes | Not | 0.711 | 4
| 02 | 1 | BCELoss | VGG16's Convolutionals | 1e-4 | 7 | Not applicable |Not | Not | 0.506 | 1   
| 03 | 1 | BCELoss | VGG16's Convolutionals | 1e-4 | 7 | Not | Yes | Yes | 0.709 | 6
| 04 | 1 | BCELoss | VGG16's Convolutionals | 1e-4 | 7 | Not applicable | Not | Yes | 0.501 | 7
| 05 | 1 (variation) | BCELoss | VGG16's Convolutionals + Linear layer | 1e-4 | 7 | Not | Yes | Not | 0.714 | 3
| 06 | 1 (variation) | BCELoss | VGG16's Convolutionals + Linear layer | 1e-4 | 7 | Not applicable |Not | Not | 0.504 | 2   
| [07](https://drive.google.com/open?id=1LJJ9nS2pwngqalRW1mOuUSiC4lvMm7eh) | 1 (variation) | BCELoss | VGG16's Convolutionals + Linear layer | 1e-4 | 7 | Not | Yes | Yes | 0.713 | 4
| 08 | 1 (variation) | BCELoss | VGG16's Convolutionals + Linear layer | 1e-4 | 7 | Not applicable | Not | Yes | 0.501 |  1

Better results are obtained when the training starts with the pretrained weights of the convolutional network.

We continue, exploring the influence of data augmentation ( see results bellow).

| Experiment ID | Architecture | Loss | Features from | Learning Rate | Epochs | Freeze | Pretrained |Data Augmentation | Validation Accuracy | Best Epoch |
|:--------:| :--------:| :--------:| :--------:| :--------:| :--------:|:--------:|:--------:|:--------:|:--------:|:--------:|
| 11 | 1 |  CELoss | VGG16's Convolutionals | 1e-4 | 20 | Not | Yes | Not | 0.793 | 11 
| 13 | 1 |  CELoss | VGG16's Convolutionals | 1e-4 | 14 | Not | Yes | Yes | **0.803** | 11   
| 15 | 1 | CELoss | VGG16's Convolutionals + Linear layer | 1e-4 | 20 | Not | Yes | Not | 0.772 | 14
| 17 | 1 | CELoss | VGG16's Convolutionals + Linear layerr | 1e-4 | 14 | Not | Yes | Yes | 0.710 | 13
| 21 | 2 | CosineEmbeddingLoss | VGG16's Convolutionals | 1e-4 | 14 | Not | Yes | Not | 0.807 | 9
| 21 | 2 | CosineEmbeddingLoss | VGG16's Convolutionals | 5e-4 | 14 | Not | Yes | Not | 0.708 | 11
| 23 | 2 | CosineEmbeddingLoss | VGG16's Convolutionals | 1e-4 | 14 | Not | Yes | Yes | **0.821** | 12    
| 23 | 2 | CosineEmbeddingLoss | VGG16's Convolutionals | 5e-4 | 14 | Not | Yes | Yes | 0.709 | 14 

Data augmentation increases accuracy around 1% when the input features of the decision network are extracted from the convolutional layers of the VGG16 and the Cross Entropy Loss is used.
Data augmentation increases accuracy around 2% when the features are extracted from the convolutional layers of the VGG16 and the Cosine Embedding Loss is used, without freezing any layers.

We try to fine tune only half of the layers of the VGG16 instead of training the whole network for Architecture 2 and the validation accuracy drops around 9% . See table bellow.

| Experiment ID | Architecture | Loss | Features from | Learning Rate | Epochs | Freeze | Pretrained |Data Augmentation | Validation Accuracy | Best Epoch |
|:--------:| :--------:| :--------:| :--------:| :--------:| :--------:|:--------:|:--------:|:--------:|:--------:|:--------:|
| [33](https://drive.google.com/open?id=1KZa1biAASPTvknJrGmOQg8zixsKlLOGO) | 2 | CosineEmbeddingLoss | VGG16's Convolutionals | 1e-4 | 14 | 50% | Yes | Yes | 0.746 | 13
        
We try different Convolutional Networks [2] [3] [4] to extract the features for Architecture 2. See results bellow.

| Experiment ID | Architecture | Loss | Features from | Learning Rate | Epochs | Freeze | Pretrained |Data Augmentation | Validation Accuracy |Best Epoch |
|:--------:| :--------:| :--------:| :--------:| :--------:| :--------:|:--------:|:--------:|:--------:|:--------:|:--------:|
| 23 | 2 | CosineEmbeddingLoss | [VGG16_bn](https://pytorch.org/docs/stable/_modules/torchvision/models/vgg.html#vgg16_bn) | 1e-4 | 14 | Not | Yes | Yes | 0.823 | 9
| [43](https://drive.google.com/open?id=1-3Ahtzf5r7-CxqQIVCDh_DuQKEd45LlI) | 2 | CosineEmbeddingLoss | [ResNet50](https://pytorch.org/docs/stable/_modules/torchvision/models/resnet.html#resnet50)| 1e-4 | 14 | Not | Yes | Yes | 0.822 | 7
| [53](https://drive.google.com/open?id=1-4p-P33YvMwFG6B0_iH1jVI_w3SLhYdf) | 2 | CosineEmbeddingLoss | [ResNet101](https://pytorch.org/docs/stable/_modules/torchvision/models/resnet.html#resnet101) | 1e-4 | 14 | Not | Yes | Yes | 0.823 | 14
| 63 | 2 | CosineEmbeddingLoss | [ResNext50](https://pytorch.org/docs/stable/_modules/torchvision/models/resnet.html#resnext50_32x4d)| 1e-4 | 14 | Not | Yes | Yes | **0.826** | 12

The best results are obtained with ResNext50, although  it is the network with the least number of parameters. ResNext is an architecture inspired in ResNet and Inception. It has a similar architecture to Inception, but adding the Residuals.

The total number of Trainable Parameters of the Convolutional Networks used are summarized in the table bellow.

| Convolutional Network | Number of parameters |
|--------|:--------:|
| VGG16_bn | 117487680
| ResNet50 | 25557032
| ResNet101 | 44549160
| ResNext50 | 25028904

We study the influence of the learning rate for Architecture 1 and Cross Entropy Loss. See results bellow.

| Experiment ID | Architecture | Loss | Features from | Learning Rate |Optimizer| Epochs | Freeze | Pretrained |Data Augmentation | Validation Accuracy | Best Epoch |
|:--------:| :--------:| :--------:| :--------:| :--------:| :--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|
| 13 | 1 | CELoss | VGG16's Convolutionals | 1e-4 | Adam | 20 | Not | Yes | Yes | 0.718 | 20   
| 13 | 1 | CELoss | VGG16's Convolutionals | 1e-4 | SGD | 20 | Not | Yes | Yes | 0.744 | 20 
| 13 | 1 | CELoss | VGG16's Convolutionals | 5e-4 | SGD | 20 | Not |Yes | Yes | 0.825 | 16   
| 13 | 1 | CELoss | VGG16's Convolutionals | 1e-3 | SGD | 20 | Not | Yes | Yes | **0.826** | 20  

When training the network with a learning rate of 5e-4 or 1e-3 we face some problems for convergence with the Adam Optimizer, so we try the SGD Optimizer and it works better in that particular case.

Finally, we train both architectures during  more epochs, with different learning rates and optimizer. See results bellow.

| Experiment ID | Architecture | Loss | Features from | Learning Rate | Optimizer | Epochs | Freeze | Pretrained | Data Augmentation | Validation Accuracy | Best Epoch | Threshold
|:--------:| :--------:| :--------:| :--------:| :--------:| :--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|
| 13 | 1 | CELoss | VGG16's Convolutionals | 1e-4 | Adam | 40 | Not | Yes | Yes | 0.827 | 39   
| 13 | 1 | CELoss | VGG16's Convolutionals | 1e-4 | SGD | 40 | Not | Yes | Yes | 0.805 | 39
| [13](https://drive.google.com/open?id=1YBo9EVCO1SRKRzzkKyqCUGVFgpKePzTS) | 1 | CELoss | VGG16's Convolutionals | 1e-3 | SGD | 40 | Not | Yes | Yes | 0.832 | 33
| 23 | 2 | CosineEmbeddingLoss | VGG16's Convolutionals | 1e-4 | Adam | 40 | Not | Yes | Yes | 0.832 | 17
| [23](https://drive.google.com/open?id=1YwPhbOXRJE0WGSNrHaviHakgxJJ4xRRL) | 2 | CosineEmbeddingLoss | VGG16's Convolutionals | 1e-3 | SGD | 40 | Not | Yes | Yes | 0.835 | 28 | 0.641
| 63 | 2 | CosineEmbeddingLoss | ResNext50 | 1e-4 | Adam | 40 | Not | Yes | Yes | 0.839 | 35
| [63](https://drive.google.com/open?id=1YhukDspQyyuwr2IPuGNrODqxUejys3Dq) | 2 | CosineEmbeddingLoss | ResNext50 | 1e-3 | SGD | 40 | Not | Yes | Yes | **0.845** | 35

**The best results are obtained for architecture 2, extracting the features with ResNext50 pretrained on Imagenet and  minimizing the CosineEmbeddingLoss. Getting a validation accuracy of 0.845 and a test accuracy of 0.859.**

## Conclusions
With the architectures implemented to perform the face verification task and from the experiments executed, we conclude that:
-  Better results are obtained when the training starts with the pretrained weights of the convolutional network.
-  Freezing half of the layers is not beneficial.
- Data augmentation increases accuracy and leads to a better behavior of the validation loss during training. The training plots can be found in each one of the experiment's folders.
- When facing problems during training with one optimizer like Adam, SGD (although being slower) can get better results.
- The best results are obtained when during training we directly optimize a loss relevant to the task, like the Architecture 2 with the CosineEmbeddingLoss.
- ResNext50 is a better feature extractor for the verification task than VGG16, ResNet50 and ResNet101.

## Getting familiar with the repository

### How to reproduce the experiments
All the experiments have been performed with a Colaboratory  Notebook and are stored int the Experiment's folder. There are seven different versions that we summarize bellow:
- Version 1: Architecture 1 with CELoss. Best results obtained for experiment [07](https://drive.google.com/open?id=1LJJ9nS2pwngqalRW1mOuUSiC4lvMm7eh).
-  Version 2: Architecture 1 with BCELoss. Best results obtained for experiment [13](https://drive.google.com/open?id=1YBo9EVCO1SRKRzzkKyqCUGVFgpKePzTS).
-  Version 3: Architecture 2 with CosineEmbeddingLoss and VGG16 as feature extractor. Best results obtained for experiment [23](https://drive.google.com/open?id=1YwPhbOXRJE0WGSNrHaviHakgxJJ4xRRL).
- Version 4: Architecture 2 with CosineEmbeddingLoss and VGG16 as feature extractor, freezing the half bottom layers.  Best results obtained for experiment [33](https://drive.google.com/open?id=1KZa1biAASPTvknJrGmOQg8zixsKlLOGO).
- Version 5: Architecture 2 with CosineEmbeddingLoss and ResNet50 as feature extractor. Best results obtained for experiment  [43](https://drive.google.com/open?id=1-3Ahtzf5r7-CxqQIVCDh_DuQKEd45LlI).
- Version 6: Architecture 2 with CosineEmbeddingLoss and ResNet101 as feature extractor. Best results obtained for experiment  [53](https://drive.google.com/open?id=1-4p-P33YvMwFG6B0_iH1jVI_w3SLhYdf).
- Version 7: Architecture 2 with CosineEmbeddingLoss and ResNext50 as feature extractor. Best results obtained for experiment [63](https://drive.google.com/open?id=1YhukDspQyyuwr2IPuGNrODqxUejys3Dq).

You can find a link to download the weights of the experiment that has the best results for each one of the versions in the list above and also at the results tables, where all the parameters are summarized. Be careful to update all the paths and necessary parameters when needed.

### Demo
A Demo has been realized for version 7. The best results are obtained for this version at experiment 63. The Demo includes only the inference code and allows you to see the results obtained. You can visualize the predicted label the real label and the two images compared.
To execute the demo you have to download the data set and the weights of experiment  [63](https://drive.google.com/open?id=1YhukDspQyyuwr2IPuGNrODqxUejys3Dq).  Be careful to update all the paths if needed. If you want to test different experiments be careful to set up the correct threshold.

### Folders structure
There are for main folders:
- [Demo]() : Contains the demo's collaboratory notebook. 
- [Experiments](https://github.com/sburrel/AIDL2019_PROJECT_SBD/tree/master/Experiments): Contains a folder for each one of the notebook's versions described at the previous paragraph. Inside this folders you can find the corresponding notebook and all the graphs and results of all the experiments organized by experiment ID.
- [Figures](https://github.com/sburrel/AIDL2019_PROJECT_SBD/tree/master/Figures): Contains the images displayed at the present Readme.
- [dataset-cfp](https://github.com/sburrel/AIDL2019_PROJECT_SBD/tree/master/dataset-cfp): Contains the data set. You have to download it in order to reproduce the experiments or execute the Demo.

## TO DO

The final test accuracy obtained is 85.9%, far behind the best accuracy found in the literature for the CFP dataset (98.7%[5] for frontal-frontal images and 94.4%[6] for frontal-profile images).

From the conclusions obtained, the next logical steps would be:
1. Try ResNext101 as feature extractor.
2. Implement ROC analisys and mesure the true accept rate (TAR), which is the fraction of genuine comparisons that correctly exceed the threshold, and the false accept rate (FAR), which is the fraction of impostor comparisons that incorrectly exceed the threshold.
3. Implement the normalization step on features and weights and the ArcFace loss as explained in Deng et al.[7] The authors report a 95.6% accuracy on CFP dataset. 
4. Increase Data Augmentation. The last papers use GAN's to perform "one-to-many augmentation" and "many-to-one-normalization", see reference [8] .

## References
[1]: S. Sengupta, J.C. Cheng, C.D. Castillo, V.M. Patel, R. Chellappa and  D.W. Jacobs.  ["Frontal to Profile Face Verification in the Wild"](http://www.cfpw.io/paper.pdf). IEEE Conference on Applications of Computer Vision, 2016.

[2]:Karen Simonyan and Andrew Zisserman. ["Very Deep Convolutional Networks for large scale Image Recognition"](https://arxiv.org/pdf/1409.1556.pdf). ICLR, 2015.

[3]:He, Kaiming, Xiangyu Zhang, Shaoqing Ren, and Jian Sun. ["Deep residual learning for image recognition."](https://arxiv.org/pdf/1512.03385.pdf). CVPR, 2016.

[4]:Xie, Saining, Ross Girshick, Piotr Dollár, Zhuowen Tu, and Kaiming He.  ["Aggregated residual transformations for deep neural networks."](https://arxiv.org/pdf/1611.05431.pdf) . CVPR, 2017.

[5]: Xi Peng, Xiang Yu, Kihyuk Sohn, Dimitris N. Metaxas and Manmohan Chandraker. ["Reconstruction-Based Disentanglement for Pose-Invariant Face Recognition"](https://arxiv.org/abs/1702.03041). The IEEE International Conference on Computer Vision (ICCV), 2017, pp. 1623-1632

[6]: Xi Yin and Xiaoming Liu. ["Multi-Task Convolutional Neural Network forPose-Invariant Face Recognition"](https://arxiv.org/abs/1702.04710v2). TIP, 2017.

[7]: Jiankang Deng, Jia Guo, Niannan Xue and Stefanos Zafeiriou. ["ArcFace: Additive Angular Margin Loss for Deep Face Recognition"](https://arxiv.org/pdf/1801.07698.pdf).

[8]: Mei Wang and Weihong Deng. ["Deep Face Recognition: A Survey"](https://arxiv.org/pdf/1804.06655.pdf).


<!--stackedit_data:
eyJoaXN0b3J5IjpbNzkxMTAwNzY4LC0xNDgxMTc0MTM3LDQ2Nz
Y1NjM5Niw1NDUwOTk3NzAsMTE5NzE2NTM3LC0xNTk4MjM2Nzg3
LC0xNjg3MDU4Njg0LC0yMTI3NjM2OTAyLDE1MjM2NTgyNDcsLT
k0MDgxMjQxNSwtODYxNzQ0MjQxXX0=
-->