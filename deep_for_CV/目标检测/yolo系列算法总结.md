# yolo 系列算法的总结文档

## yolo-V1

### 优劣

- 检测快，全局处理使背景错误少，泛化性能好；
- 网络粗糙；
- 不如R-CNN的检测效果好，定位召回不够好，全连接处理，固定尺寸；
- 小样本和群体性的检测效果差，定位不准
- 但是是一次性的检测 you only look once

### 知识点

- 输入：448*448的尺寸，最终经过两个全连接网络输出为7\*7\*(类别+4(定位)+1(confidence)+4(定位)+1(confidence)) 正负样本
- 定位的xywh，其中xy的值在0-1之间。
- yolov1没有anchor的概念。
- 损失函数：所有损失采用误差平方和计算，但是边界框损失时w与h采用开根号的误差平方和计算
  - 边界框损失  
  - 置信度损失 此处存在正负样本的问题
  - 类别损失 
  
   $Pr(Class_i|Object)×Pr(Object)×IOU\frac{truth}{pred}=Pr(Class_i)*IOU\frac{truth}{pred}$



