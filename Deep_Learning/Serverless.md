# Serverless Deep Learning

## Introduction 

In the previous module, we built and trained a clothes classification deep learning module using Pytorch.  This module focuses
on deploying it.l  The model categorises images of clothing items uploaded by users on a website.  Deployment will be used using
AWS Lamda, a serverless solution to execute code without maanaging servers, and instead of **TensorFlow**  , we use
*TensorFlow-lite* for this.  

  <img width="1243" height="786" alt="image" src="https://github.com/user-attachments/assets/f31fd6f8-b513-46ba-ac33-1995ff44059d" />

## AWS Lambda

AWS Lambda is a **serverless computing service** allowing one to execute code without managing servers.

Use AWS Lambda for deploying the model 
1.  Go to AWS Console  
2.  Go to Lambda (run code without thinking about servers) *No need to create servers*
3.  Process is
    - Create a Function (choose author from scratch, name your function, select runtime environment ie. python 3.9 and architecture x86_64)
    - Create Function Parameters.  **event** contains input data passed to function , **context** provides details about the invocation, configuration and execution envrionment
    - Update the logic for the function.  For example , in the video
    - ```python
      def lambda_handler(event, context):
        print("Parameters:", event) # Print input parameters
        url = event["url"]  # Extract URL from input
        return {"prediction": "clothes"}  # Sample response
      ``` 
<img width="1280" height="764" alt="image" src="https://github.com/user-attachments/assets/1ee4e2e3-a64e-4eb4-9249-940bf41794c7" />
