# Object-detection
Start date: 8 June, 2021

In this project, I will write a summary that my understanding is object detection topic:

I. Overview

- Object detection is the task of classification and localization of object in an image or video. Object
detectors need a powerful backbone network to extract rich.

- The goals of object detection is to recognise instances of a predefined set of object classes(people, cars, bikes,..) 
and describe the locations of each detected object in the image using a bounding box. 
features.

- There are two kinds of object detector:

1. One stage detector: YOLO, SSD, RetinaNet
![img_2.png](img_2.png)
   
The one-stage detectors: 

+ Propose predicted boxes from input images directly without region proposal step,
thus they are time efficient and can be used for real-time devices.
  
+ Perform classification and regression on dense anchor boxes without generating a sparse RoI set.

+ Using single convolutional network predicts the bounding boxes and the class probabilities for these boxes.

2. Two stage detector: Fast R-CNN
![img_1.png](img_1.png)
   
+ The basic architecture of two stage of includes region proposal network to feed region proposals into classifier and
regressor.

+ Sparse region proposals are generated in the first stage and then are further regressed and classified in the second stage

II. One-stage detector for YOLO

There are three versions for YOLO. In this part, I will summary from YOLO V1 to YOLO V3

- YOLO V1:
    - Idea for object detection:
        + YOLO splits image into SxS 

- YOLO V3

   + YOLOv3 sử dụng darknet53 làm backbone( có 53 lớp chập). mỗi lớp sẽ bao gồm batch normalization layer và hàm kích hoạt Leaky ReLU. Không sử dụng pooling, và một lớp tích chập với stride 2 được sử dụng để downsample cái feature maps. Điều này giúp tránh khỏi mất mất của low-level features thường bị ảnh hưởng bởi pooling.

what is Anchor box?

Anchor boxes are a set of predefined bounding boxes of a certain height and width. These boxes are defined to capture the scale and aspect ratio of specific object classes you want to detect and are typically chosen based on object sizes in your training datasets. During detection, the predefined anchor boxes are tiled across the image. The network predicts the probability and other attributes, such as background, intersection over union (IoU) and offsets for every tiled anchor box. The predictions are used to refine each individual anchor box. You can define several anchor boxes, each for a different object size. Anchor boxes are fixed initial boundary box guesses.

The network does not directly predict bounding boxes, but rather predicts the probabilities and refinements that correspond to the tiled anchor boxes. The network returns a unique set of predictions for every anchor box defined. The final feature map represents object detections for each class. The use of anchor boxes enables a network to detect multiple objects, objects of different scales, and overlapping objects.


Advantage of Using Anchor Boxes

When using anchor boxes, you can evaluate all object predictions at once. Anchor boxes eliminate the need to scan an image with a sliding window that computes a separate prediction at every potential position. Examples of detectors that use a sliding window are those that are based on aggregate channel features (ACF) or histogram of gradients (HOG) features. An object detector that uses anchor boxes can process an entire image at once, making real-time object detection systems possible.

Because a convolutional neural network (CNN) can process an input image in a convolutional manner, a spatial location in the input can be related to a spatial location in the output. This convolutional correspondence means that a CNN can extract image features for an entire image at once. The extracted features can then be associated back to their location in that image. The use of anchor boxes replaces and drastically reduces the cost of the sliding window approach for extracting features from an image. Using anchor boxes, you can design efficient deep learning object detectors to encompass all three stages (detect, feature encode, and classify) of a sliding-window based object detector.

How Do Anchor Boxes Work?

The position of an anchor box is determined by mapping the location of the network output back to the input image. The process is replicated for every network output. The result produces a set of tiled anchor boxes across the entire image. Each anchor box represents a specific prediction of a class. For example, there are two anchor boxes to make two predictions per location in the image below.

![image](https://user-images.githubusercontent.com/22832922/131110139-33bbf37e-5f63-4f26-ab42-bcace707541d.png)

Each anchor box is tiled across the image. The number of network outputs equals the number of tiled anchor boxes. The network produces predictions for all outputs.

Localization Errors and Refinement

The distance, or stride, between the tiled anchor boxes is a function of the amount of downsampling present in the CNN. Downsampling factors between 4 and 16 are common. These downsampling factors produce coarsely tiled anchor boxes, which can lead to localization errors.

![image](https://user-images.githubusercontent.com/22832922/131110331-715e68e0-c2ed-4b30-85d7-bc2eeaab9516.png)

To fix localization errors, deep learning object detectors learn offsets to apply to each tiled anchor box refining the anchor box position and size.

![image](https://user-images.githubusercontent.com/22832922/131111001-af4973b4-9bcc-499f-a9fe-eb66abf45d93.png)

Downsampling can be reduced by removing downsampling layers. To reduce downsampling, lower the ‘Stride’ property of the convolution or max pooling layers, (such as convolution2dLayer (Deep Learning Toolbox) and maxPooling2dLayer (Deep Learning Toolbox).) You can also choose a feature extraction layer earlier in the network. Feature extraction layers from earlier in the network have higher spatial resolution but may extract less semantic information compared to layers further down the network

Generate Object Detections

To generate the final object detections, tiled anchor boxes that belong to the background class are removed, and the remaining ones are filtered by their confidence score. Anchor boxes with the greatest confidence score are selected using nonmaximum suppression (NMS). For more details about NMS, see the selectStrongestBboxMulticlass function.

![image](https://user-images.githubusercontent.com/22832922/131111218-f4deb444-ef2d-4d00-9f26-b8674c5c2268.png)

Anchor Box Size

Multiscale processing enables the network to detect objects of varying size. To achieve multiscale detection, you must specify anchor boxes of varying size, such as 64-by-64, 128-by-128, and 256-by-256. Specify sizes that closely represent the scale and aspect ratio of objects in your training data. For an example of estimating sizes, see Estimate Anchor Boxes From Training Data.

Estimate Anchor Boxes From Training Data

Anchor boxes are important parameters of deep learning object detectors such as Faster R-CNN and YOLO v2. The shape, scale, and number of anchor boxes impact the efficiency and accuracy of the detectors.

Reference:

[1] https://arxiv.org/pdf/2104.11892.pdf

[2] https://arxiv.org/pdf/1907.09408.pdf

[3] https://www.ecva.net/papers/eccv_2020/papers_ECCV/papers/123590528.pdf

[4] https://nghichcodechovui.blogspot.com/2020/03/giai-thich-ly-thuyet-cua-yolo-v3.html

[5] https://aicurious.io/posts/tim-hieu-yolo-cho-phat-hien-vat-tu-v1-den-v5/

[6] https://forum.machinelearningcoban.com/t/object-detection-yolo/503

[7] https://lilianweng.github.io/lil-log/2018/12/27/object-detection-part-4.html

[8] https://www.mathworks.com/help/vision/ug/anchor-boxes-for-object-detection.html

[9] https://github.com/decanbay/YOLOv3-Calculate-Anchor-Boxes
