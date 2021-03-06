# Malaria_Detection - Machine Learning Project

## 1.	Introduction
Malaria remains one of the most life-threatening diseases caused by parasites which are transmitted by the bite of a mosquito. Diagnosing such disease is time consuming. The topic of our project is hence – How can we best classify infected and uninfected cells of malaria using machine learning?  The approach we have undertaken is by first carefully studying the dataset, data augmentation, building our own CNN model, using transfer learning i.e already done CNNs and lastly unsupervised learning using k-Means on the output of a trained CNN to discover if there are other subgroups/classes within the dataset.

<img src="figures/details.png">

## 2.	Studying the Dataset
The database is composed by 27,558 RGB images of 64 by 64 pixels. The images are balanced between two classes ‘Parasitized’ and ‘Uninfected’ each of them containing 13,779 images. The number of labels is hence 27,558 and the number of features is 12,288. Later on, we split the data into Training and Testing containing 10,334/images and 3,445/images respectively. 

### PCA vs t-SNE 
The dataset is multidimensional, so we perform dimensionality reduction using PCA to analyze it better. We have plotted 5000 points of infected (1) and uninfected (0) cells. As we see the data is not distinguishable. Hence, we use t-SNE and obtain an improvement compared to PCA, some sort of division, however it is impossible to clearly classify the two classes of the dataset. This implies that a cell-image depends on many features which contain crucial information that cannot be reduced directly to two dimensions.

<img src="figures/pca_tsne.png" width ="600">

### Data Augmentation
By closely studying the dataset we observe that the images are overall very similar within their category. In order to avoid overfitting for our CNN model we augment the data by adding noise, rotating it multiple times or blurring it as seen in the plot. To optimize and save memory we use the pre-defined kera’s function ImageDataGenerator. The training set experiences such modification, while the testing set stays the same. 

## 3.	CNN model
After trying different layers, the final CNN contains about 10 layers 3 of which are convolution layers of kernel size (2,2) and 3 maxpooling layers of pool size 3. After splitting the data into the training and testing dataset with a ratio of 0.75:0.25, we test the model with the original dataset and the augmented one. By observing the details (blue borders), we can conclude that this model overfitted in the training set, with an accuracy of 0.95, an insignificant loss function and a biased confusion matrix. Hence the CNN was not able to accurately predict the test data. The reason for such result can be because our model is not good enough i.e needs more layers or better optimization functions, but it can also be because the dataset is very similar, and the CNN is not able to properly learn it. So, when we test it with the augmented data, and we observe a completely opposite result (orange borders). The accuracy on the test data is 0.9533 and on the training data 0.9518. The model is able to predict the correct malaria cells. 

<img src="figures/blue.png">
<img src="figures/orange.png">

## 4.	Transfer Learning
The fundamental idea behind transfer learning is that when huge CNNs are trained over humongous datasets, such as ImageNet, then the CNN learns not only how to detect the features of the given dataset but also learns some “skills” such as recognizing certain shapes. Therefore, by finding the right CNN we can apply our dataset to it and get a better accuracy of prediction. After some research, we choose to train on ResNet50 and VGG16. We used them as feature extractors for linear classifiers using the LinearSVC() function and we observe that the accuracy for VGG16 is much higher than that of ResNet50. Since VGG16 gives a better result we use that to better classify the features of the dataset and re-plot the PCA and t-SNE. Compared to the first PCA/t-SNE plots the separation of the two classes is much clearer, however needs still improvement. The other option we tried was to re-train the VGG16 model by tuning the last layers or adding more layers. We obtain an accuracy of prediction equal to 0.94.

<img src="figures/transfer.png" width = "600">


## 5.	Unsupervised Learning
In conclusion of the previous sections we know that in order to correctly classify the dataset of malaria cell images we need many features. Perhaps the dataset is not classifiable on only 2 classes, perhaps there are more subgroups. When plotting the uninfected cells with the exception of some outliers the data is undistinguishable i.e. there are no subgroups. This makes sense intuitively since shape/characteristics of healthy cells are the same from human to human. On the other hand, when plotting the infected cells at first instance we observe the creation of small clusters. Therefore, we use k-Means on the PCA values and observe that we can cluster the data into three categories. By looking at various images from the three categories we can hypothesize/conclude that they correspond to the severity of disease/infection i.e. the spreading of the “violet dots” in the cells. 

<img src="figures/unsupervised.png">
