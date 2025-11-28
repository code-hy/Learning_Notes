# Convolutional Neural Network
##  Types of Neural Network

##  How it works

- Consists of Layers  ie  layer1, layer2, layer3, ... layern
- Two types of Netrworks
    ### convolutional layers
      -  consists of filters (small shapes)
      -  slide filter across the images, everytime we apply filter to image, we see how similar the filter is to the image (from 0 to 9, 10 etc)  similarity
      -  if image is somewhat similar to filter, then it will have a higher number
      -  one filter equal to one feature-map  - higher vzalues is higher similarity
      -  so if we have 6 filters, we have 6 feature-maps (one feature map per filter
      -  input is image, while the output is a feature map
      -  <img width="752" height="443" alt="image" src="https://github.com/user-attachments/assets/31f120d7-f75d-43eb-b247-36df2df2fa32" />
      -  Output of the first convolutional layer is a set of feature maps
      -  Feature maps are fed into next convolutional layer which has also its own set of filters
      -  <img width="544" height="336" alt="image" src="https://github.com/user-attachments/assets/1b4d23de-4824-4509-b4ce-67f685bc850d" />
      -  <img width="803" height="433" alt="image" src="https://github.com/user-attachments/assets/9eab0340-2701-440d-9b96-8d9743d20da2" />
      -  each layer has it's own set of feature maps, because of chaining , it has more and more complex filters....
      -  ie. 3 convolutional layer,  first layer is simple shapes, second layer, could have angles, cross, circles,  combination of different filters, get more complex shapes....  first layer, recognise simple patterns, second layer will recognise more complex patterns, third layer can detect sleeves
      -  <img width="946" height="353" alt="image" src="https://github.com/user-attachments/assets/22c273e0-0258-428d-a92b-dcda4e51a57a" />
      -  what filters do is it looks at the region across all the feature maps... it learns automatically.
      -  <img width="1009" height="492" alt="image" src="https://github.com/user-attachments/assets/90a120c7-283e-4d24-973b-f5092a9da659" />
      -  <img width="980" height="720" alt="image" src="https://github.com/user-attachments/assets/685fc6fc-a9cf-4583-8ae9-36bad3e4f2cb" />
      -  take the image and pass it through convolutional layers, and the output or result is a vector representation.  Input could be 299 x 299 x 3,   while the vector would be an array of size 2048.  some part of the vector represent sleeves, some the bottom, some the top, and so forth
      -   <img width="621" height="439" alt="image" src="https://github.com/user-attachments/assets/52676d0c-8237-4b55-a5c8-47576ca306c7" />

      -  




      -  
    ### dense layers
      -  after feeding the output of the convolutional layers, it will be fed into the dense layer, which will finally output the prediction. role of convolutional layers is to extract vector representation
         while the role of dense layers is to make predictions
         <img width="1060" height="550" alt="image" src="https://github.com/user-attachments/assets/a74e0b7e-53f9-4f99-89ae-b28ede0f60b6" />
    #### components of dense layers
      -  original image is turned into vector ie.  2048
      -  build a model (binary classification)
      -  output is is it a t-shirt or not a t-shirt (0 or 1), use something like logistic regression, our function g is a sigmoid of our features and the weights, the weights are trained
         output is a probability that the object is a t-shirt
  <img width="1038" height="635" alt="image" src="https://github.com/user-attachments/assets/a85f0619-064e-4b49-b37b-8d01af1c0d10" />
      -  build a model with that. use a slightly different notation.
      -  take all the X, and multiply by the W or weights.  and then sum them all together, and we take the sum , and turn into probability, sigmoid, and the probability of being a t-shirt.  we can build 3 models, one for shirt, one for t-shirt, one for dress
         <img width="770" height="591" alt="image" src="https://github.com/user-attachments/assets/8f2cdb55-5d9c-4d6f-8732-ad4b4daf70a4" />
         instead of sigmoid , it will be a softmax function with a 3-dimensional output, first being probability of being a shirt, second is probability of being t-shirt, and third is probability of being dress
      <img width="779" height="578" alt="image" src="https://github.com/user-attachments/assets/43bafa6d-998a-4de6-823c-adf7477a2dab" />
         simplified diagram
        <img width="902" height="670" alt="image" src="https://github.com/user-attachments/assets/9315620f-ceba-478a-b3bf-af8953d01687" />
        
         <img width="1027" height="703" alt="image" src="https://github.com/user-attachments/assets/8a7153eb-f057-4960-b53d-a627c87cfd4b" />

       -  dense layer.... input to dense layer, and output of dense layer, the reason is dense, each element of input is connected to each output.... it is pretty dense....  each output ahs w, put all w together it becomes one w....  multiply w by x...
       -  dense layer is a matrix multiplication, that's why it is dense, because it multiplies each x by the weight, different weights for different models... etc.
      <img width="889" height="697" alt="image" src="https://github.com/user-attachments/assets/aba1d49c-dcf9-452c-9de0-55572f93888f" />
       -   we can put multiple dense layer together
       -   <img width="957" height="694" alt="image" src="https://github.com/user-attachments/assets/8b398950-2ec9-4703-80f2-07a210030974" />

<img width="1061" height="521" alt="image" src="https://github.com/user-attachments/assets/a8a89180-68b1-48f6-a857-074178d39de4" />
  #####  Check out the https://cs231n.github.io/    [detailed explanation](https://cs231n.github.io/)
  It talks about the convolutional layers, and explains it.
  ##  General Pooling Layer
    Makes the feature map smaller,  

    <img width="896" height="485" alt="image" src="https://github.com/user-attachments/assets/f95a0dc3-9cb3-44e3-bc81-9256da8c50a2" />


Convolution Network is the Feature Extraction
Dense Layer is the Classification


<img width="1381" height="773" alt="image" src="https://github.com/user-attachments/assets/77b442fa-e071-481b-8ad8-b4dd366a475e" />


  
  ####   Other sites explaining  [simple explanation of cnn](https://www.youtube.com/watch?v=zfiSAzpy9NM)

  


      -  
      

      -  
