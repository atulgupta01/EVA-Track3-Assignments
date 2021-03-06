# Important terms and points to consider in design of CNN Architecture and training

## **1. How many layers**
           
>In a CNN, primary aim is that at the end of the network we should have seen the entire image (case where object covers entire image) or the region size we are targetting in the image. If we had infinite computing power we could have taken kernels of the size of image and be done with it in 1 layer but such s not the case. Hence we accomplish the task of covering the image in number of steps i.e. layer by layer. Hence number of layers will be mainly governed by hardware capacity we have.
           
## **2. 3x3 Convolutions**

>3x3 is smallest odd size kernel with axis of symmetry

>3x3 can be used to achieve receptive field equivalent to higher kernel size eg. 5x5(apply 3x3 twice),7x7(apply 3x3 trice) etc with lesser number of parameters.

>GPUs from nVidia etc have optimised hardware for 3x3 kernel.

>Hence in our CNNs we generally use 3x3 kernels

## **3. Receptive Field**

>Receptive field is the area of the input that the feature is seeing. We have two types of RFs Local and global RFs
>Local RF means the area of the immediate input channel the feature is seeing while global RF means area of the network's input image the current feature is seeing.

## **4. Kernels and how do we decide the number of kernels?**

> Kernels are basically feature extractors. When kernels are convolved with input channel, they extract features (eg. edges, circles....) into output channel. 
> Number of kernel depends on  (1). Variety of features(expressivity) that may be required for given dataset eg. MNIST requires lesser kernels compared to say CIFAR. (2). More number of kernels may be require to achieve better inter and class variation. (3). Number also depends on the hardware capacity.


## **5. MaxPooling**

>MaxPooling reduces the size of the channel by carrying forward only max values in given filter region eg. 2x2 MaxPooling will carry 25% values forward

>We prefer 2x2 maxPooling because if we go for larger size the loss will be too high and we might lose important information

## **6. Concept of Transition Layers**

> The kind of architecture we follow in large CNNs is we go on increasing number of convolution kernels in steps. This block is called Convolution block. Once we reach a layer where we start seeing feature formation we apply transition layer to reduce the size of the channels as well as number of channels. The size is reduced by using MaxPoolng while number of chnnels is reduced by 1x1 convolution layer.


## **7. Position of Transition Layer**

> The transition layer is applied at the layer where the network starts forming features eg. edges and gradients, textures, parts of object. This in turn depends on the size of the image eg. with MNIST data features start forming at RF of 5x5 or 7x7 i.e. after 2 or 3 layers. for image of size 400x400 they may start forming at RF of 11x11 i.e. after 5 layers.

## **8. Position of MaxPooling**

> It is placed in transition block. The position of MaxPooling in transition block is a topic of research right now. Some researchs use it before 1x1 conv layer some after it.  But at present there are no conclusive evidence in support of either case.


## **9. The distance of MaxPooling from Prediction**

> MaxPooling is generally not used 2-3 layers before prediction layer.

## **10. 1x1 Convolutions**

>1x1 convolutions helps us to bring down the number of channels. It does this by combining large number channels into important fewer chnnels by weighted average of sort. What it effectively does is combine contextually related channels to form common channel. Eg. suppose inputs are human hand, human leg, human eyes, dog leg, dog eyes, cat leg, cat eye etc. all jumbeled up, 1x1 will try to combine human features together, similarly for dog and cat.

>Although its called so its not a convolution because it is just multiplications with entire images followed by sum of it. Hence it is much faster compared to convolution operation. 

## **11. When do we stop convolutions and go ahead with a larger kernel or some other alternative (which we have not yet covered)**

> As we apply various convolution layers, maxPooling and 1x1 conv, we go on reducing the channel size. When we reach size of around 7x7 or less, there are no discernible features visible. Hence applying 3x3 type of convolution may not be good option as it will leave out border rows and columns. In this case we go ahead with larger kernel of the size of channel itself.

## **12. Image Normalization**

> Image normalizing means scaling the image pixel value to range of 0.0 to 1.0. In case of 8 bit grayscale image or 24 bit RGB images pixel values are in range 0 to 255.To normalize this image each of the pixel values are divided by 255. Image normalization helps network avoid large computations as now it has to deal with value from 0 to 1.0  instead of 0 to 255.

## **13. SoftMax**

> Softmax is used in the final layer of CNN classifier. It takes output of prediction layers and prvides classification outputs where classes are well spaced out. Although softmax gives good looking classification output it may be misguiding as it accentuates the gap between class outputs in prediction layer. Hence we must take it with a pinch of salt.

## **14. Learning Rate**
> Learning rate is a hyper parameter of the CNN that decides the rate at which Backpropogation will apply correction to the  model parameters. Too low a rate will result in very slow model traiing and too high rate will cause drastic updates and transient behaviour.

## **15. Number of Epochs and when to increase them**

> Epoch means number of times the network will perform training on entire training set. Number of epochs depends on how the model accuracy or loss is progressing. If accuracy is increasing (loss is reducing) we may increase the epochs else if accuracy has plateaued we may discontinue

## **16. Batch Size, and effects of batch size**
> Batch size specifies number of images selected randomly from the training set for one iteration of backpropogation. 

> Larger the batch size lesser the time per epoch but the size of RAM required will also be higher.

> Batch size should atleast larger than number of classes so that for each BackProp iteration there more or less equal reperesentation from the classes.

## **17. When to add validation checks**

> Its better to have validation checks after each epochs because after every epoch the model is changing so it might so happen that model achieved during intermediate epochs may be better than the one at the end of all the specified epoch.

## **18. How do we know our network is not going well, comparatively, very early**

> Usually in initial few epochs itself by observing the training and validation accuracy we can say whether network is going well or not.

## **19. Batch Normalization**

> In a network there may be some features in range 0-1 and others in 0-1000 in this case when back propogation is operating it may not be that effective as correction based on small range features may be too low for large range features similarly corrections based on large range features may be too large for small range features. Batch normalization normalizes features. Thus learning will be improved. Additionally batch normalization introduces some noise hence helps with overfitting as well.

> Batch normalization can be used before or after ReLU.


## **20. The distance of Batch Normalization from Prediction**

> Batch normalization must be one step beyond prediction layer.

## **21. DropOut**

> DropOut randomly switches off specified number of neurons in a layer. DropOut is operational only during the training phase and not validation phase.


## **22. When do we introduce DropOut, or when do we know we have some overfitting**

> When we observe thata the model is overfitting we introduce DropOut. It may be applied after any layer or after each layer but it should not be applied before prediction layer. 

> We can say the model overfitting when we observe that model's training accuracy is higher compared to validation accuracy. i.e. there is a gap between both accuracies. 

## **23. LR schedule and concept behind it**

> When we are training the model the constant learning rate may not always be good. As when the model is far from the minima larger LR may help model reach closer to minima faster but after that if the LR is still high it may start oscillating around minima and never reach there similarly if initially LR is very small it may take very long to reach the minima. LR schedule helps us vary LR on the go. It varies LR after every epoch following some scheme. Thus resulting in much better model training.


## **24. Adam vs SGD**

> Both are optimizers used in NN model training .
