#  Using PyTorch to Short-cut the AI Training - Fine Tuning and Best practices
The video from machine learning zoomcamp -> [pytorch](https://www.youtube.com/watch?v=Ne25VujHRLA)
demonstrates how to implement image classification using PyTorch,  it is an advanced module for an existing machine learning curriculum
that already teaches **Tensorflow** and **Keras**

The lecture demonstrates how to apply **transfer learning** by taking a **pre-trained** **MobileNet V2 model** , adapting its output layer for a new
10-class fashion dataset, and running training using the **Google Colab** GPU.  A key theme involves contrasting the concise training methods of **Keras** 
with the more explicit code required for custom **PyTorch** training and validation loops.  Alex details meticulously the various critical optimization steps,
including parameter tuning, incorporating **model checkpointing** , and utilizing regularisation techniques like **dropout** and **data augmentation**.  Finally, Alex
exports the finalised, specialised deep learning model into an universally accepted **ONNX** format in preparation for serverless deployment.

## Steps in AI Smart Training
1. Understand the training problem
Training a *blank slate* AI model requires huge computing resources (days and weeks of running), and huge amount of dataset (ie. ImageNet over 14 million images)



2. Solution:  Transfer Learning
Train a pre-trained model like Mobilenet V2,  it is already an expert in identifying things in the ImageNet dataset, it knows what dogs, cats, cars looks like.  We need
to teach our new expert in identifying 10 fashion clothing items

3.  AI *Surgery* stitch exising model core layer, remove top layer, and add new layer for classifying 10 fashion objects
  - Load Genius - start with the pre-trained model
  - Freeze Layers - lock existing layers so its core knowledge isn't erased, remove the top layer 
  - Put a Final Layer - classifies 1,000 objects
  - Plug the Final Layer into the existing layer
  - Train the New Part - train *only* this new layer with our fashion images
  
  
4.  Make it Robust - Overfitting
   - overfitting, if the model memorises everything, it will be very good at identifying exact image in the training dataset, but fails, when the new image looks slightly different ie.
     image may be rotated, or flipped, or slightly deformed
   - need to show the image with slightly different orientation
   - instead of getting new set of data, we alter existing images , this is called **Data Augmentation** (programmatically creating new training data by altering existing images (eg rotating,
     zooming, or flipping
   - we get lots of training data for free, but by changing orientation it generates new set of data based on existing training data; we don't touch validation data, so we get true honest score on how data is doing 
    <img width="345" height="347" alt="image" src="https://github.com/user-attachments/assets/f3b2b5ef-cbe9-4196-81d7-fd93f1c3fbf4" />
   - *you want to just build on top of what they did* someone does the heavy lifting,  
   - critical technique
5. Allows anyone to build powerful model 
 
## Mind Map
   ![pytorch mind map](images/NotebookLM Mind Map (2).png)
   
