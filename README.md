# My_VGG16
This is an implementation of VGG16 on Python 3, Keras, and TensorFlow for detecting weed in cornfields using RGB images taken from UAV.
The model uses Color Segmentation for background removal using HSV, Data Augmentation (including Gaussian Noise), Dropout and Batch Normalization.

*Notice the results on the code section are different, as it was trained on 5 epochs only. The project has been done on 50 epochs.


![image](https://user-images.githubusercontent.com/60111412/86477039-52a80d80-bd50-11ea-9b3c-b9e2b4abc2ed.png)  ![image](https://user-images.githubusercontent.com/60111412/86478421-ea0e6000-bd52-11ea-9f93-671988ce5f56.png)



The original image is an RGB image of 6000x4000 resolution.
The objects in the image were extracted XML files annotated on LabelImg.
The data contained 118 images, from which 2 were taken for inference and from the other 116 images ~30000 objects has been extracted.
The objects were dividing for 70% and 30% corn and weeds respectecly, with 3 different type of weeds who have been merged for 1 class.


![image](https://user-images.githubusercontent.com/60111412/86476900-15dc1680-bd50-11ea-838f-52862286a7ab.png)  ![image](https://user-images.githubusercontent.com/60111412/86478404-e4187f00-bd52-11ea-8469-a30e6a2be71b.png)                             


The image pre-process including Color Segmenation for enhancing the green objects in the images, while removing all none-green pixels
The segmentation is done converting the RGB image into HSV channels using open-cv, and setting a threshold for the Hue levels in the images.


![image](https://user-images.githubusercontent.com/60111412/86476905-18d70700-bd50-11ea-86af-b0764ea8edb0.png)    ![image](https://user-images.githubusercontent.com/60111412/86478411-e7136f80-bd52-11ea-8299-ecd656905f3e.png)

The VGG16 is then constructed using "ImageNet" weights, while retraining the last 8 layers. 2 Batch Normalization and Dropout layers is then added after the FC layers before the output layer.


<img src="https://user-images.githubusercontent.com/60111412/86479994-db757800-bd55-11ea-98a2-0cca67035c1c.png" width="300"> <img src="https://user-images.githubusercontent.com/60111412/86480055-fcd66400-bd55-11ea-8777-ce938fd55778.png" width="300"> <img src="https://user-images.githubusercontent.com/60111412/86480149-20011380-bd56-11ea-858a-89f39e30d00f.png" width="300">
<img src="https://user-images.githubusercontent.com/60111412/86480226-4030d280-bd56-11ea-87f7-aeb222bd0be8.png" width="300"> <img src="https://user-images.githubusercontent.com/60111412/86479994-db757800-bd55-11ea-98a2-0cca67035c1c.png" width="300">


The results by all measures on the test set gave ~99%


<img src="https://user-images.githubusercontent.com/60111412/86479647-365a9f80-bd55-11ea-9ad0-bfe44644e1ad.png" width="400"> 


<img src="https://user-images.githubusercontent.com/60111412/86479946-c26cc700-bd55-11ea-8689-b780b1462138.png" width="400">


In the inference, the input image was divided into cells of size 600x400.
each cell was examined for contating green using HSV channels again.
if the cell contained green, it recursively dividing innto smaller cell and so on, until reaching cell fo size 150x100.
if a cell of size 150x100 contatined green, a prediction was made using the trained network.
if the prediction of the cell contating a weed was over 95%, and the prediction of the cell contating a corn was under 5%, the cell was painted in white.
using the recursion and the green levels threshold, the algorithm reduced the runtime from ~40 seconds to ~5-10 seconds, depending on the desnity of the weeds in the image.



![image](https://user-images.githubusercontent.com/60111412/86477039-52a80d80-bd50-11ea-9b3c-b9e2b4abc2ed.png)  ![image](https://user-images.githubusercontent.com/60111412/86478421-ea0e6000-bd52-11ea-9f93-671988ce5f56.png)









