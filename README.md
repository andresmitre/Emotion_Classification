# UNDER CONSTRUCTION X.X


# Emotion_Classification


[Center for Research in Mathematics (CIMAT)](http://www.cimat.mx/en)

Author: [Andres Mitre](https://www.linkedin.com/in/andres18m)

Email: andresmitre.fic@uas.edu.mx | andres18m@gmail.com

Twitter:  https://twitter.com/andres18m

Spring 2018

**ABOUT COPYING OR USING PARTIAL INFORMATION:**
This script was originally created by [Andres Mitre](https://www.linkedin.com/in/andres18m). READ LICENSE FILE

# Repository description 

"Deep learning is a group of exciting new technologies for neural networks. Through a combination of advanced training techniques and neural network architectural components, it is now possible to create neural networks of much greater complexity. Deep learning allows a neural network to learn hierarchies of information in a way that is like the function of the human brain." [[1]](https://github.com/jeffheaton/t81_558_deep_learning) 

The data collection, starts with a FER(Facial Expression Recognition) using [haarcascades](https://github.com/opencv/opencv), with a professional camera/webcam. Images were taken while 23 participants watched a series of films related with emotions. The images were saved into separate emotions folders. GSR lectures were recorded as well since stress and boredom emotions stimulates the activity of sweet glands. the GSR were captured with the [Grove - GSR sensor](https://www.seeedstudio.com/Grove-GSR-sensor-p-1614.html) and saved into a CSV file.

The classifation for FER was made with a Convolutional Neural Network by [TensorFlow](https://codelabs.developers.google.com/codelabs/tensorflow-for-poets/#0) with a total of 1375 images into 8 different classes, the validation/explotation stage takes 10% of the data set and validates the CNN. On the other hand the identification between emotions was made with use of [Wilcoxon signed-rank test](https://en.wikipedia.org/wiki/Wilcoxon_signed-rank_test) using [IBM SPPS](https://www.ibm.com/products/spss-statistics).

# Abstract

The state of the art of emotion recognition, has shown a significant role for learning and the identification of emotions that affect it. The importance in distinguishing acted and spontaneous emotions, contributes in the avoidance of biases in emotions related to learning, which entails identifying variables that affect it. 
The objective of this work, is the reliable recognition of spontaneous and acted emotions related with learning activities through a classificatory off facial expressions and Galvanic Skin Response (GSR). The induction of spontaneous emotions, were generated by clips from movies that showed a good capacity to induce positive and negative effects, elevated levels of emotional activation and variations in the perception of emotional control. On the other hand, the emotions performed were replicated by the participants imitating a sequence of illustrations referencing the emotions. 
The classificatory was applied to 23 participants of Hispanic ethnic, participants were told a series of instructions. The experiments in this work were based in emotion classification by facial expressions through a Convolutional Neural Network and Galvanic Skin Response. Once, obtained the raw data of the experiment, it processed to the training of classificatory from deep learning and statistical analysis. 
The Convolutional Neural Network, presented a validation accuracy of 96.5%, a confusion matrix was made with the images of the data set at random and external data sets. In the confusion matrix of the external data set, the real emotions are confused with others, mainly in happy and boredom, correctly classifying in the stress. In the results of Galvanic Skin Response, percentage change tests were performed between the emotions in which significant different were found in happy and boredom, except in stress. With the support of the Neuronal Convolutional Network and Galvanic Skin Activity, it is possible to have a correct classifier of emotions related to learning activities.


# Datasets

Contact Author

# Content

Module|Content
---|---
[Module 1](https://github.com/andresmitre/Emotion_Classification/blob/master/introduction.ipynb) | <ul><li>Introduction</ul>
[Module 2](https://github.com/andresmitre/Emotion_Classification/blob/master/Haar_Feature_based_Cascade_Classifiers.ipynb) | <ul><li>Haar Feature-based Cascade Classifiers</ul> 
[Module 3](https://github.com/andresmitre/Emotion_Classification/blob/master/Haar_Cascade.ipynb)<br>Week of 01/29/2018 | <ul><li>TensorFlow and Keras for Neural Networks<li>[Module 2: Assignment](https://github.com/jeffheaton/t81_558_deep_learning/blob/master/assignments/assignment_yourname_class2.ipynb) due: 01/29/2018</ul>
[Module 4](https://github.com/andresmitre/Emotion_Classification/blob/master/Haar_Cascade.ipynb)<br>Week of 02/05/2018 | <ul><li>Training a Neural Network<li>[Module 3 Assignment](https://github.com/jeffheaton/t81_558_deep_learning/blob/master/assignments/assignment_yourname_class3.ipynb) due: 02/05/2018</ul>



VIVA MEXICO !


[1] Heaton Jeff, "T81 558:Applications of Deep Neural Networks", https://github.com/jeffheaton/t81_558_deep_learning
