---
title: detectron2数据处理
date: 2020-01-18 01:31:45
categories: 计算机视觉
tags:
    - 目标检测
    - 深度学习
mathjax: true
---
Detectron2是FAIR推出的一个目标检测框架，在Detectron的基础上进行了重新设计，基于pytorch。Detectron仅能接受COCO格式，Detectron2中能够接受其它形式的输入。这篇博客简要的介绍了VOC和COCO两种常见的目标检测数据标注格式。

<!--more-->

## Pascal VOC

Pascal Voc标注存储成XML的形式，每个XML对应一张图片。
它包含图片的路径在<path>元素中，每个bbox存储在<object>元素中，举个例子。

```xml
<object>
	<name>fig</name>
	<pose>Unspecified</pose>
	<truncated>0</truncated>
	<difficult>0</difficult>
	<bndbox>
		<xmin>256</xmin>
		<ymin>27</ymin>
		<xmax>381</xmax>
		<ymax>192</ymax>
	</bndbox>
</object>
```

## COCO

COCO的数据格式只是一个单独的JSON文件，对于所有数据集中的标注，或者每个数据集的切片各一个。

bbox表示为左上的坐标和box的宽高的形式，"bbox":[x, y, width, height]。
举个例子，COCO的JSON文件包含1张图片，对应最顶层的"image"元素，3个类别在"categories"元素中，2个标注的bbox在"annotation"元素中。

```json
{
  "type": "instances",
  "images": [
    {
      "file_name": "0.jpg",
      "height": 600,
      "width": 800,
      "id": 0
    }
  ],
  "categories": [
    {
      "supercategory": "none",
      "name": "date",
      "id": 0
    },
    {
      "supercategory": "none",
      "name": "hazelnut",
      "id": 2
    },
    {
      "supercategory": "none",
      "name": "fig",
      "id": 1
    }
  ],
  "annotations": [
    {
      "id": 1,
      "bbox": [
        100,
        116,
        140,
        170
      ],
      "image_id": 0,
      "segmentation": [],
      "ignore": 0,
      "area": 23800,
      "iscrowd": 0,
      "category_id": 0
    },
    {
      "id": 2,
      "bbox": [
        321,
        320,
        142,
        102
      ],
      "image_id": 0,
      "segmentation": [],
      "ignore": 0,
      "area": 14484,
      "iscrowd": 0,
      "category_id": 0
    }
  ]
}
```
因此，对于Detectron而言，需要将数据格式从VOC转换为COCO的格式。