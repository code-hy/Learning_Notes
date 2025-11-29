#  Transfer Learning with Pre-trained Neural Network

## Summary of the entire lesson

This machine learning lesson provides a practical demonstration of transfer learning in Keras, focusing on how to build a specialized image classifier by leveraging a pre-trained network. The core concept involves adopting the generic convolutional layers from a robust base model, like Xception, and attaching new, custom dense layers to adapt the network for a specific classification task. Data is prepared for training using the ImageDataGenerator, which handles image loading, resizing, and inferring the multi-class labels through the directory structure for one-hot encoding. When constructing the new model, the base convolutional layers are frozen to prevent retraining, and a GlobalAveragePooling2D layer converts the extracted feature maps into a vector representation. The final training process uses the Adam optimizer along with CategoricalCrossentropy loss, ultimately demonstrating that while the model achieves quick success (around 80% validation accuracy), it rapidly begins overfitting after only a few epochs.

### Concept of Transfer Learning

Convolutional Layers to convert to Vector Representation and use Dense Layers for making predictions

These are already trained on ImageNet

<img width="1046" height="412" alt="image" src="https://github.com/user-attachments/assets/a6c95f9d-96a6-4643-a759-1f325791d94d" />

From convolutional layers to vector representation, it is already trained on imagenet, so it is quite generic, no need to change it.

<img width="1060" height="688" alt="image" src="https://github.com/user-attachments/assets/ec8c981b-0a68-4605-9c13-73a8b961d01f" />

Dense layers is quite specific to the dataset

<img width="1025" height="614" alt="image" src="https://github.com/user-attachments/assets/e0261e0f-3e3c-435e-9525-cfacdce5fdf7" />

In our T-shirt classification, we don't need the dense layers,  we keep the convolutional layers, and train new dense layers, we are re-using the convolutional layer

<img width="1056" height="726" alt="image" src="https://github.com/user-attachments/assets/28ec02a6-98ba-42a0-9f5f-aaa20c15cd3e" />

#### Training using tensorflow, keras preprocessing ImageData Generator, and using a smaller target size of 150,150 as 299,299 takes 4x longer to train


<img width="1150" height="399" alt="image" src="https://github.com/user-attachments/assets/458cee33-a927-4f0b-9ec7-ae33c6746a4a" />

Batching by 32 images per training

<img width="1074" height="706" alt="image" src="https://github.com/user-attachments/assets/1f2de6fd-e95c-4abb-b231-16f6c2270607" />


