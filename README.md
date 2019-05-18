# LIttleMan-PinkPanther

Project Overview
Pink Panther(https://en.wikipedia.org/wiki/The_Pink_Panther) and The Little Man (https://pinkpanther.fandom.com/wiki/The_Little_Man) are two famous cartoon characters.
We want to write A program that analyse the video at : https://www.youtube.com/watch?v=b_hSyQlRo50 and gives following output
1. Series of Image files (JPG or BMP or TIF or whatever image file format you want)  that highlights the occurrence of either Pink Panther or Little man or both depending upon the presence of character(s) at the given frame
2. The Image file name must have timestamp of that image in the test video. For example: if at 2:40, we see Pink Panther, then we must have an image file called 2_40.jpg with black bounding box for Pink Panther. Another example is at 3:10 we see both Pink Panther and Little Man, then the output file name must be 3_10.jpg.
3. We should be able to execute the solution against any other Pink Panther video to give similar output.

# Plaform Used
1. I have created this project in jupyter notebook in Google Colab
2. Just open the notebook in google colab and execute it
https://drive.google.com/open?id=1uchTLGWxA8nxy5-MIHieevB7PROozS4M

# Approach taken to solve the problem
1. Extract the 1 images/second from a given video using opencv
2. Manually Create the mapping csv file to map object in the images, here pink panther, little man, both and other
3. Plot the count plot to check for data imbalance
4. From a given csv file store all the images name into X and classes name into y
5. Read all the images from a directory using plt.imread
6. check the size of the image and we found its a RGB image with size (360, 480, 3)
7. Since there are Four classes,
•	Other
•	Pink Panther
•	Little Man
•	Both
    We will encode them for our model to use.
8. We will be using a VGG16 pretrained model which takes an input image of shape (224 X 224 X 3). We have used the resize() function of skimage.transform to do this.
9.  All the images have been reshaped to 224 X 224 X 3. But before passing any input to the model, we must preprocess it as per the model’s requirement. Otherwise, the model will not perform well enough. Use the preprocess_input() function of keras.applications.vgg16 to perform this step.
10. We also need a validation set to check the performance of the model on unseen images. We will make use of the train_test_split() function of the sklearn.model_selection module to randomly divide images into training and validation set.
11. Now we will build the model and will now load the VGG16 pretrained model and store it as base_model.
12. We will make predictions using this model for X_train and X_test, get the features, and then use those features to retrain the model.
13. The shape of X_train and X_test is (268, 7, 7, 512), (116, 7, 7, 512) respectively. In order to pass it to our neural network, we have to reshape it to 1-D.
14. Finally, we will build our model. This step can be divided into 3 sub-steps:
•	Building the model
•	Compiling the model
•	Training the model
15. We have a hidden layer with 1,024 neurons and an output layer with 4 neurons (since we have 4 classes to predict).
16. In the final step, we will fit the model and simultaneously also check its performance on the unseen images, i.e., validation images
17. we will call predict_image_classes(model), here we will pass our trained model. This function will loop through all the extracted images and predict their classes and save them into them
