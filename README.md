# 2023-IEEE-GRSS-DATA-FUSION-CONTEST
## Calendar
```
· 1 月 3 日：竞赛开幕：发布训练和验证数据

· 1 月 4 日：评估服务器开始接受提交验证数据集

· 2 月 28 日：将每个轨道的 1-2 页方法的简短描述发送至iadf_chairs@grss-ieee.org（使用 IGARSS 论文模板）

· 3 月 7 日：测试数据发布；评估服务器开始接受测试提交

· 3 月 13 日：测评服务器停止接受投稿

· 3 月 15 日：该方法的更新和最终描述

· 3 月 28 日：获奖者公布

· 4 月 23 日：论文内部截止日期，DFC 委员会审查程序

· 5 月 22 日：将在 IGARSS 2023 会议记录中发表的最终论文的提交截止日期
```

## Multi-Task Learning of Joint Building Extraction and Height Estimation

  This track defines the joint task of building extraction and height estimation. Both are two very fundamental and essential tasks for building reconstruction. Same as in Track 1, the input data are multi-modal optical and SAR satellite imagery. Building extraction and height estimation from single-view satellite imagery depend on semantic features extracted from the imagery. Multi-task learning provides a potentially superior solution by reusing features and forming implicit constraints between multiple tasks in comparison to conventional separate implementations. Satellite images are provided with reference data, i.e., building annotations and normalized Digital Surface Models (nDSMs). The participants are required to reconstruct building heights and extract building footprints.Figure shows an example.
  
  ![figure](https://github.com/ISPNU-Signal-Group/2023-IEEE-GRSS-DATA-FUSION-CONTEST/blob/main/images/track2.png)



## Dataset Format
### Download
[DFC23_IEEE-DataPort.zip；track1.zip；track2.zip](https://ieee-dataport.org/competitions/2023-ieee-grss-data-fusion-contest-large-scale-fine-grained-building-classification#files) 

We provide individual RGB and SAR (Synthetic Aperture Radar) remote sensing images. For better use of multi-modal data, we provide a python script to generate 4-channel images concatenated in the channel dimension, in 4-channel (R,G,B,SAR) tif format. You can run the following command to generate the merge direcory:

```  
  python ./make_merge.py $DATASET_ROOT
```

  All the images are in size of **512x512**. The data format follows the MS COCO format, and the annotation is in json format. The topology of the dataset directory is as follows：

```
DFC_Track_2
├── annotations
│   └── buildings_only_train.json
│   └── buildings_only_val.json
│   └── buildings_only_test.json
├── train
│   └── rgb
│   │   ├── P0001.tif
│   │   └── ...
│   │   └── P0009.tif
│   └── sar
│   │   ├── P0001.tif
│   │   └── ...
│   │   └── P0009.tif
│   └── height
│       ├── P0001.tif
│       └── ...
│       └── P0009.tif
├── val
│   └── rgb
│   │   ├── P0011.tif
│   │   └── ...
│   │   └── P0019.tif
│   └── sar
│   │   ├── P0011.tif
│   │   └── ...
│   │   └── P0019.tif
│   └── height
│       ├── P0011.tif
│       └── ...
│       └── P0019.tif
└── test
    └── rgb
    │   ├── P0021.tif
    │   └── ...
    │   └── P0029.tif
    └── sar
    │   ├── P0021.tif
    │   └── ...
    │   └── P0029.tif
    └── height
        ├── P0021.tif
        └── ...
        └── P0029.tif
```
     
## Baseline（赛道二）
  **A baseline that shows how to use the DFC23 data to train models, make submissions, etc., can be found [here](https://github.com/AICyberTeam/DFC2023-baseline).**  
  We choose the classical **mask rcnn with multimodal multitask learning** (height prediction) framework as the contest baseline model. Among the input image modalities are RGB and SAR. We use [MMDetection](https://github.com/open-mmlab/mmdetection) (version 2.25.1) to test the baseline model performance.  
  The [performance ](https://github.com/AICyberTeam/DFC2023-baseline/tree/main/track2) report of multimodal multitask learning (MML) framework on the validation set of track 2 (**instance segmentation and height prediction**) is as follows:
 <table>
	<head>
		<tr>
			<th >Model</th>
			<th >Modality</th>
			<th >mAP</th>
			<th >mAP_50</th>
			<th >Delta_1</th>
			<th >Delta_2</th>
			<th >Delta_3</th>
		</tr>
	</head>
	<body>
		<tr>
			<th >MML</th>
			<th >RGB+SAR</th>
			<th >15.1</th>
			<th > 41.1</th>
			<th > 30.1</th>
			<th > 35.3</th>
			<th > 39.6</th>
		</tr>
	</body>
</table>

<table>
	<head>
		<tr>
			<th >Model</th>
			<th >Modality</th>
			<th >mAP</th>
			<th >mAP_50</th>
			<th >Delta_1</th>
			<th >Delta_2</th>
			<th >Delta_3</th>
		</tr>
	</head>
	<body>
		<tr>
			<th >MML</th>
			<th >RGB+SAR</th>
			<th ></th>
			<th ></th>
			<th ></th>
			<th > </th>
			<th ></th>
		</tr>
	</body>
</table>




### 复现baseline(赛道一)

<table>
	<head>
		<tr>
			<th >Model</th>
			<th >Modality</th>
			<th >mAP</th>
			<th >mAP_50</th>
		</tr>
	</head>
	<body>
		<tr>
			<th >Mask Rcnn</th>
			<th >RGB</th>
			<th ></th>
			<th ></th>
		</tr>
	</body>
	<body>
		<tr>
			<th >Mask Rcnn</th>
			<th >RGB+SAR</th>
			<th ></th>
			<th ></th>
		</tr>
	</body>
</table>

## Submission & Metrics
The submitted results should include **two parts**. The first part is a **building extraction result**, which is the same as the previous track with only one exception that the category is not taken into consideration in this track. The second part is a **pixel-wise height estimation result**, which is a map with the same resolution as the input. The second part should be a tif file with the same name as the corresponding optical image. 

For evaluation, the metric of the building extraction is the same as Track 1 (categories are not taken into consideration) and the metric of height estimation is threshold accuracy δ1=Nt⁄Ntotal, where Ntotal is the total number of pixels and Nt is the number of pixels that meet max(y⁄ŷ, ŷ⁄y)<1.25, where y is the reference height and ŷ is the predicted height. The final metric is the average of the two metrics, i.e., (AP50 + δ1)/2.


## paper
### object detection

- [deep learning object detection](https://github.com/hoya012/deep_learning_object_detection)


- [awesome-object-detection](https://github.com/amusi/awesome-object-detection)

 ![figure](https://github.com/ISPNU-Signal-Group/2023-IEEE-GRSS-DATA-FUSION-CONTEST/blob/main/images/od.png)
 
- [DE⫶TR: End-to-End Object Detection with Transformers](https://github.com/facebookresearch/detr)

### height estimation

- [Object-Height-Estimation](https://github.com/Sanket-Mathur/Object-Height-Estimation)
- [BuildingHeightEstimation](https://github.com/shivshaktiss17/BuildingHeightEstimation)

- Disentangled Latent Transformer for Interpretable Monocular Height Estimation [code](https://paperswithcode.com/paper/disentangled-latent-transformer-for)

用于可解释单目高度估计的解耦潜在变压器  
（高度估计）  
网络：MHE  
网络：Transformer-based MHE networks  
数据集：GTAH、real-world DFC 2019、Washington DC(WDC)。  
1)语义类别：区分树木、道路、建筑物等  
（研究发现不同的类别对高度的预测有影响，且尺度越大，预测的高度值越大。此外，高度预测与阴影无太大相关性）  
2）为防止语义无法识别，增加路径积分梯度的局部归因分析法。  
- Height and Uprightness Invariance for 3D Prediction From a Single View [code](https://paperswithcode.com/paper/height-and-uprightness-invariance-for-3d)


单视图三维预测中的高度和垂直度不变性
(高度和垂直不变性)
CVPR 2020  ·  Manel Baradad, Antonio Torralba ·  Edit social preview
Current state-of-the-art methods that predict 3D from single images ignore 
the fact that the height of objects and their upright orientation is invariant 
to the camera pose and intrinsic parameters. To account for this, we propose a 
system that directly regresses 3D world coordinates for each pixel. First, 
our system predicts the camera position with respect to the ground plane and its intrinsic parameters. 
Followed by that, it predicts the 3D position for each pixel along the rays spanned by the camera. 
The predicted 3D coordinates and normals are invariant to a change in the camera position or its model, 
and we can directly impose a regression loss on these world coordinates. 
Our approach yields competitive results for depth and camera pose estimation 
(while not being explicitly trained to predict any of these) and improves 
across-dataset generalization performance over existing state-of-the-art methods.



- CVNet: Contour Vibration Network for Building Extraction [code](https://paperswithcode.com/paper/cvnet-contour-vibration-network-for-building)

用于建筑物提取的等高线振动网络
（等高线建筑物提取）
网络：CVNet
CVPR 2022  ·  Ziqiang Xu, Chunyan Xu, Zhen Cui, Xiangwei Zheng, Jian Yang ·  Edit social preview
The classic active contour model raises a great promising solution to polygon-based object extraction 
with the progress of deep learning recently. Inspired by the physical vibration theory, 
we propose a contour vibration network (CVNet) for automatic building boundary delineation. 
Different from the previous contour models, the CVNet originally roots in the force and motion principle of contour string.
Through the infinitesimal analysis and Newton's second law, 
we derive the spatial-temporal contour vibration model of object shapes, 
which is mathematically reduced to second-order differential equation. 
To concretize the dynamic model, we transform the vibration model into the space of image features, 
and reparameterize the equation coefficients as the learnable state from feature domain. 
The contour changes are finally evolved in a progressive mode through the computation of contour vibration equation. 
Both the polygon contour evolution and the model optimization are modulated to form a close-looping end-to-end network. 
Comprehensive experiments on three datasets demonstrate the effectiveness and 
superiority of our CVNet over other baselines and state-of-the-art methods for the polygon-based building extraction. 
The code is available at https://github.com/xzq-njust/CVNet.



- Polygonal Building Segmentation by Frame Field Learning [code](https://paperswithcode.com/paper/polygonal-building-segmentation-by-frame)

基于框架场学习的多边形建筑分割
（矢量提取遥感图像的建物物）
30 Apr 2020  ·  Nicolas Girard, Dmitriy Smirnov, Justin Solomon, Yuliya Tarabalka ·  Edit social preview
While state of the art image segmentation models typically output segmentations in raster format, 
applications in geographic information systems often require vector polygons. 
To help bridge the gap between deep network output and the format used in downstream tasks, 
we add a frame field output to a deep segmentation model for extracting buildings from remote sensing images. 
We train a deep neural network that aligns a predicted frame field to ground truth contours. 
This additional objective improves segmentation quality by leveraging multi-task learning and 
provides structural information that later facilitates polygonization; 
we also introduce a polygonization algorithm that utilizes the frame field along with the raster segmentation. 
Our code is available at https://github.com/Lydorn/Polygonization-by-Frame-Field-Learning.



- Polygonal Building Extraction by Frame Field Learning [code](https://paperswithcode.com/paper/polygonal-building-extraction-by-frame-field)

（矢量提取遥感图像的建物物）
CVPR 2021  ·  Nicolas Girard, Dmitriy Smirnov, Justin Solomon, Yuliya Tarabalka ·  Edit social preview
While state of the art image segmentation models typically output segmentations in raster format, 
applications in geographic information systems often require vector polygons. 
To help bridge the gap between deep network output and the format used in downstream tasks, 
we add a frame field output to a deep segmentation model for extracting buildings from remote sensing images. 
We train a deep neural network that aligns a predicted frame field to ground truth contours. 
This additional objective improves segmentation quality by leveraging multi-task learning 
and provides structural information that later facilitates polygonization; 
we also introduce a polygonization algorithm that that utilizes the frame field along with the raster segmentation. 
Our code is available at https://github.com/Lydorn/Polygonization-by-Frame-Field-Learning.

