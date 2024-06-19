# Liver Tumor Dissection

##  1、The transformation from CT Image to Grep-Scale Map
<ol>

<li>Using <font color='RED'><strong>Primitive_Pixel</strong></font> value to measure <font color='RED'><strong> HU_Vulue </strong></font>: Pixel_Data*Rescale_slope+Rescale_intercept</li>

<li>Then adopt 16 Grey_levels to represent Grey_level either: assume that window width:W window length:L y=a*pre_Grey_level + b, a:g/W b: (W/2-L)g/W</li>

<li>
If over the L+W/2 express 255, less the L+W/2 express 0
</li>

</ol>

## 2、Windowing menthod for CT Intensified
<ol>

<li>Window Width: include much larger range of Pixel</li>

<li>window Length: the brightness of the CT image</li>

<li>Through changing Width/Length level to change constraction and brightness</li>
<br>
<img src="Windowing.png" title="Windowing Changing">

</ol>

```python
#给定windowing自定义函数
def windowing(img, window_width, window_center):
    #img： 需要增强的图片
    #window_width:窗宽
    #window_center:中心
    minWindow = float(window_center)-0.5*float(window_width)
    new_img = (img-minWindow)/float(window_width)
    new_img[new_img<0] = 0
    new_img[new_img>1] = 1
    return (new_img*255).astype('uint8') #把数据整理成标准图像格式

```

## 3、Histogram Equalization

### 1、The reason of we using it  
Since using Windowing method to intensifize the CT image, the image would become too bright or too dark. We need to make the picture clear or say smooth. That`s why we using Histogram Equalization.

### 2、How to attain Histogram Equalization
First we need to calculate every Gray-level`s pabability density. Secondly, accumulating them to get each probability distribution.Lastly, we get all the smooth gray_level by using distinct probability distribution times gray_scope. 

