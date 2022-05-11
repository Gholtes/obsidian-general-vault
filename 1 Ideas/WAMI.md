Wide area motion imagery (WAMI) is a technique to capture and analyse the motion of objects over many square km through the use of high resolution arial imagery platforms. This is used in military surveillance, where the area under observation may be a military base or hostile town.  

Based on publicly available information, the analysis pipeline consists of the following steps at ~1 FPS.
1. Image capture: Multiple high resolution CCDs capture the scene from an arial platform
2. Stitching: Overlapping areas of capture between CCDs is used to stitch the captured images into a single image
3. Registration: The image is registered onto a base-image, to allowing pixel level comparison over time and geolocation within the image. This may involve a transform to account for the movement of the arial platform.
4. Detection: Objects of interest are detected by background subtraction and/or deep learning based models.
5. Tracking: Detected objects are tracked between frames.
6. Analysis: Tracked object paths are analysed to meet specific needs, such as exclusion zone alarms, anomaly detection 

In more modern iterations, steps 4 and 5 are sometimes combined into a single deep model.

### Sources
Data: [WPAFB 2009](https://www.sdms.afrl.af.mil/index.php?collection=wpafb2009)
Paper: [Detecting and Tracking Small Moving Objects in Wide Area Motion Imagery (WAMI) Using Convolutional Neural Networks (CNNs)](https://arxiv.org/pdf/1911.01727.pdf)

#computervision 