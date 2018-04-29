# UNDER CONSTRUCTION X.X


# Emotion_Classification


[Center for Research in Mathematics (CIMAT)](http://www.cimat.mx/en)

Author: [Andres Mitre](https://www.linkedin.com/in/andres18m)

Email: andresmitre.fic@uas.edu.mx | andres18m@gmail.com

Twitter:  https://twitter.com/andres18m

Spring 2018

ABOUT COPYING OR USING PARTIAL INFORMATION:
This script was originally created by [Andres Mitre](https://www.linkedin.com/in/andres18m) Any explicit usage of this script or its contents is granted according to the license provided and its conditions.

# Repository description 

"Deep learning is a group of exciting new technologies for neural networks. Through a combination of advanced training techniques and neural network architectural components, it is now possible to create neural networks of much greater complexity. Deep learning allows a neural network to learn hierarchies of information in a way that is like the function of the human brain." [[1]](https://github.com/jeffheaton/t81_558_deep_learning) 

The data collection, starts with a FER(Facial Expression Recognition) using [haarcascades](https://github.com/opencv/opencv), with a professional camera/webcam. Images were taken while 23 participants watched a series of films related with emotions. The images were saved into separate emotions folders. GSR lectures were recorded as well since stress and boredom emotions stimulates the activity of sweet glands. the GSR were captured with the [Grove - GSR sensor](https://www.seeedstudio.com/Grove-GSR-sensor-p-1614.html) and saved into a CSV file.

The classifation for FER was made with a Convolutional Neural Network by [TensorFlow](https://codelabs.developers.google.com/codelabs/tensorflow-for-poets/#0) with a total of 1375 images into 8 different classes, the validation/explotation stage takes 10% of the data set and validates the CNN. On the other hand the identification between emotions was made with use of [Wilcoxon signed-rank test](https://en.wikipedia.org/wiki/Wilcoxon_signed-rank_test) using [IBM SPPS](https://www.ibm.com/products/spss-statistics)



Los códigos fueron originalmente por Andrés Mitre Ortiz
Cualquier uso explícito de este código o su contenido se otorga 
de acuerdo con la licencia provista y sus condiciones.

VIVA MÉXICO !


[1] Heaton Jeff, "T81 558:Applications of Deep Neural Networks", https://github.com/jeffheaton/t81_558_deep_learning
