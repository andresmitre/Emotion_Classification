{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Emotion Classification\n",
    "**Module 3: Data acquisition**\n",
    "* Author: [Andrés Mitre](https://github.com/andresmitre), [Center for Research in Mathematics (CIMAT)](http://www.cimat.mx/en) in Zacatecas, México.\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Module   |Title\n",
    "---------|--------------\n",
    "Module 1 |[Introduction](https://github.com/andresmitre/Emotion_Classification/blob/master/introduction.ipynb)\n",
    "Module 2 |[Haar Cascade Algorithm](https://github.com/andresmitre/Emotion_Classification/blob/master/Haar_Feature_based_Cascade_Classifiers.ipynb)\n",
    "Module 3 |[data acquisition](https://github.com/andresmitre/Emotion_Classification/blob/master/data_acquisition.ipynb)\n",
    "Module 4 |[Regression Neural Network](https://github.com/jeffheaton/t81_558_deep_learning/blob/master/assignments/assignment_yourname_class4.ipynb)\n",
    "Module 5 |[Time Series Neural Network](https://github.com/jeffheaton/t81_558_deep_learning/blob/master/assignments/assignment_yourname_class10.ipynb)\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Raw Data\n",
    "\n",
    "The data collection, starts with a FER(Facial Expression Recognition) using [haarcascades](https://github.com/opencv/opencv), with a professional camera/webcam. Images were taken while 23 participants watched a series of films related with emotions. The images were saved into separate emotions folders. GSR lectures were recorded as well since stress and boredom emotions stimulates the activity of sweet glands. the GSR were captured with the [Grove - GSR sensor](https://www.seeedstudio.com/Grove-GSR-sensor-p-1614.html) and saved into a CSV file. \n",
    "\n",
	"**Galvanic Skin Response (GSR)**\n",
	 "\n",
	"The Galvanic Skin Response (GSR) is defined as a change in the electrical properties of the skin. The signal can be used for capturing the autonomic nerve responses as a parameter of the sweat gland function. The measurement is relatively simple, and has a good repeatability. Therefore the GSR measurement can be considered to be a simple and useful tool for examination of the autonomous nervous system function, and especially the peripheral sympathetic system. For detailed information I highly reccomend to check [The complete pocket guide by IMOTIONS](https://imotions.com/guides/)\n",
     "\n",
	"Before we look at specific ways to preprocess data, it is important to consider four basic types of data, as defined by [Stanley Smith Stevens](https://en.wikipedia.org/wiki/Stanley_Smith_Stevens).  These are commonly referred to as the [levels of measure](https://en.wikipedia.org/wiki/Level_of_measurement):\n",
    "\n",
    "* Character Data (strings)\n",
    "    * **Nominal** - Individual discrete items, no order. For example: color, zip code, shape.\n",
    "    * **Ordinal** - Individual discrete items that can be ordered.  For example: grade level, job title, Starbucks(tm) coffee size (tall, vente, grande) \n",
    "* Numeric Data\n",
    "    * **Interval** - Numeric values, no defined start.  For example, temperature.  You would never say \"yesterday was twice as hot as today\".\n",
    "    * **Ratio** - Numeric values, clearly defined start.  For example, speed.  You would say that \"The first car is going twice as fast as the second.\"\n",
    "\n",
    "The following code contains several useful functions to encode the feature vector for various types of data.  Encoding data:\n",
    "\n",
    "* **encode_text_dummy** - Encode text fields, such as the iris species as a single field for each class.  Three classes would become \"0,0,1\" \"0,1,0\" and \"1,0,0\". Encode non-target predictors this way. Good for nominal.\n",
    "* **encode_text_index** - Encode text fields, such as the iris species as a single numeric field as \"0\" \"1\" and \"2\".  Encode the target field for a classification this way.  Good for nominal.\n",
    "* **encode_numeric_zscore** - Encode numeric values as a z-score.  Neural networks deal well with \"centered\" fields, zscore is usually a good starting point for interval/ratio.\n",
    "\n",
    "*Ordinal values can be encoded as dummy or index.  Later we will see a more advanced means of encoding*\n",
    "\n",
    "Dealing with missing data:\n",
    "\n",
    "* **missing_median** - Fill all missing values with the median value.\n",
    "\n",
    "Creating the final feature vector:\n",
    "\n",
    "* **to_xy** - Once all fields are numeric, this function can provide the x and y matrixes that are used to fit the neural network.\n",
    "\n",
    "Other utility functions:\n",
    "\n",
    "* **hms_string** - Print out an elapsed time string.\n",
    "* **chart_regression** - Display a chart to show how well a regression performs.\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Goals\n",
    "\n",
    "*  Basics of face detection using Haar Feature-based Cascade Classifiers.\n",
    "*  Extend the same for eye detection etc.\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Basics\n",
    "\n",
    "Object Detection using Haar feature-based cascade classifiers is an effective object detection method proposed by Paul Viola and Michael Jones in their paper, [Rapid Object Detection using a Boosted Cascade of Simple Features](https://www.cs.cmu.edu/~efros/courses/LBMV07/Papers/viola-cvpr-01.pdf) in 2001. It is a machine learning based approach where a cascade function is trained from a lot of positive and negative images. It is then used to detect objects in other images.\n",
    "\n",
	"Here we will work with face detection. Initially, the algorithm needs a lot of positive images (images of faces) and negative images (images without faces) to train the classifier. Then we need to extract features from it. For this, Haar features shown in the below image are used. They are just like our convolutional kernel. Each feature is a single value obtained by subtracting sum of pixels under the white rectangle from sum of pixels under the black rectangle.\n",
	"\n",
    "![Haar_Feature_based_Cascade_Classifiers](https://raw.githubusercontent.com/andresmitre/Emotion_Classification/master/Images/haar_features.png \"haar features\")\n",
    "\n",
	"Now, all possible sizes and locations of each kernel are used to calculate lots of features. (Just imagine how much computation it needs? Even a 24x24 window results over 160000 features). For each feature calculation, we need to find the sum of the pixels under white and black rectangles. To solve this, they introduced the integral image. However large your image, it reduces the calculations for a given pixel to an operation involving just four pixels. Nice, isn't it? It makes things super-fast.\n",
    "\n",
	"But among all these features we calculated, most of them are irrelevant. For example, consider the image below. The top row shows two good features. The first feature selected seems to focus on the property that the region of the eyes is often darker than the region of the nose and cheeks. The second feature selected relies on the property that the eyes are darker than the bridge of the nose. But the same windows applied to cheeks or any other place is irrelevant. So how do we select the best features out of 160000+ features? It is achieved by Adaboost.\n",
	"\n",
	"![Haar](https://raw.githubusercontent.com/andresmitre/Emotion_Classification/master/Images/haar.png \"haar\")\n",
	"\n",
	"For this, we apply each and every feature on all the training images. For each feature, it finds the best threshold which will classify the faces to positive and negative. Obviously, there will be errors or misclassifications. We select the features with minimum error rate, which means they are the features that most accurately classify the face and non-face images. (The process is not as simple as this. Each image is given an equal weight in the beginning. After each classification, weights of misclassified images are increased. Then the same process is done. New error rates are calculated. Also new weights. The process is continued until the required accuracy or error rate is achieved or the required number of features are found).\n",
	"\n",
	"The final classifier is a weighted sum of these weak classifiers. It is called weak because it alone can't classify the image, but together with others forms a strong classifier. The paper says even 200 features provide detection with 95% accuracy. Their final setup had around 6000 features. (Imagine a reduction from 160000+ features to 6000 features. That is a big gain).\n",
	"\n",
	"So now you take an image. Take each 24x24 window. Apply 6000 features to it. Check if it is face or not. Wow.. Isn't it a little inefficient and time consuming? Yes, it is. The authors have a good solution for that.\n",
	"\n",
	"In an image, most of the image is non-face region. So it is a better idea to have a simple method to check if a window is not a face region. If it is not, discard it in a single shot, and don't process it again. Instead, focus on regions where there can be a face. This way, we spend more time checking possible face regions.\n",
	"\n",
	"For this they introduced the concept of Cascade of Classifiers. Instead of applying all 6000 features on a window, the features are grouped into different stages of classifiers and applied one-by-one. (Normally the first few stages will contain very many fewer features). If a window fails the first stage, discard it. We don't consider the remaining features on it. If it passes, apply the second stage of features and continue the process. The window which passes all stages is a face region. How is that plan!\n",
	"\n",
	"The authors' detector had 6000+ features with 38 stages with 1, 10, 25, 25 and 50 features in the first five stages. (The two features in the above image are actually obtained as the best two features from Adaboost). According to the authors, on average 10 features out of 6000+ are evaluated per sub-window.\n",
	"\n",
	"So this is a simple intuitive explanation of how Viola-Jones face detection works. Read the paper for more details or check out the references in the Additional Resources section.\n",
	"\n",
	"## Haar-cascade Detection in OpenCV \n",
    "\n",
    "OpenCV comes with a trainer as well as detector. If you want to train your own classifier for any object like car, planes etc. you can use OpenCV to create one. Its full details are given here: [Cascade Classifier Training](https://docs.opencv.org/3.4.1/dc/d88/tutorial_traincascade.html).\n",
    "\n",
	"Here we will deal with detection. OpenCV already contains many pre-trained classifiers for face, eyes, smiles, etc. Those XML files are stored in the opencv/data/haarcascades/ folder. Let's create a face and eye detector with OpenCV.\n",
	"\n",
	"First we need to load the required XML classifiers. Then load our input image (or video) in grayscale mode:. \n",
	"\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 1,
   "metadata": {},
   "source": [
    "import numpy as np\n",
    "import cv2 as cv\n",
    "\n",
    "face_cascade = cv.CascadeClassifier('haarcascade_frontalface_default.xml')\n",
    "eye_cascade = cv.CascadeClassifier('haarcascade_eye.xml')\n",
    "\n",
    "img = cv.imread('sachin.jpg')\n",
    "gray = cv.cvtColor(img, cv.COLOR_BGR2GRAY)"
   ]
  },
   {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
	"Now we find the faces in the image. If faces are found, it returns the positions of detected faces as Rect(x,y,w,h). Once we get these locations, we can create a ROI for the face and apply eye detection on this ROI (since eyes are always on the face !!! ).\n",
    "\n"   
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {},
   "source": [
    "faces = face_cascade.detectMultiScale(gray, 1.3, 5)\n",
    "for (x,y,w,h) in faces:\n",
    "    cv.rectangle(img,(x,y),(x+w,y+h),(255,0,0),2)\n",
    "    roi_gray = gray[y:y+h, x:x+w]\n",
    "    roi_color = img[y:y+h, x:x+w]\n",
    "    eyes = eye_cascade.detectMultiScale(roi_gray)\n",
    "    for (ex,ey,ew,eh) in eyes:\n",
    "        cv.rectangle(roi_color,(ex,ey),(ex+ew,ey+eh),(0,255,0),2)\n",
    "cv.imshow('img',img)\n",
    "cv.waitKey(0)\n",
    "cv.destroyAllWindows()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
   "## Additional Resources\n",
   "\n",
   "Video Lecture on [Face Detection and Tracking](https://www.youtube.com/watch?v=WfdYYNamHZ8).\n",
   "\n"  
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
	"In order to use different classifiers for face, eyes, smiles and upper body. OpenCV has several XML files, try some of these [here](https://github.com/opencv/opencv/tree/master/data/haarcascades).\n",
    "\n"   
   ]
  }
 ],
 "metadata": {
  "anaconda-cloud": {},
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.6.3"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 1
}
