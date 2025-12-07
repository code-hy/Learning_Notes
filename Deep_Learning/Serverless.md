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

<img width="1239" height="752" alt="image" src="https://github.com/user-attachments/assets/5fd7f428-7ed9-474f-b0c4-1934b6aca193" />

  - Create a Test Event

    <img width="1279" height="651" alt="image" src="https://github.com/user-attachments/assets/d4859043-1e7a-46bc-a047-9c64b4754b04" />
  - Deploy the Code and run the test

    <img width="1284" height="663" alt="image" src="https://github.com/user-attachments/assets/42f931bd-e3ae-4654-9986-4635deb9fd89" />

  -  For example, if we pass a parameter, url

    <img width="1275" height="668" alt="image" src="https://github.com/user-attachments/assets/9bd83cc4-4f42-4839-9630-e10fa233cced" />

<img width="1185" height="712" alt="image" src="https://github.com/user-attachments/assets/f8286b06-577b-44fa-8603-cf0c46f9d269" />

<img width="1219" height="657" alt="image" src="https://github.com/user-attachments/assets/1b8b863b-4d19-4312-858e-d5f46982b6db" />


    <img width="1219" height="657" alt="image" src="https://github.com/user-attachments/assets/bd963223-b80d-41fe-a3c0-8a6cc1b0fc4e" />

It will typically return results....results = predict(url)
<img width="1055" height="668" alt="image" src="https://github.com/user-attachments/assets/99aed6b3-c7fe-41fc-a43a-18dec6cbe044" />

Notes:
  - lambda you pay only for the requests
  - no need to set-up infrastructure
  - no need about machines, pay only for the requests
  - very convenient

  <img width="839" height="686" alt="image" src="https://github.com/user-attachments/assets/6723aaa9-2503-4307-9bc3-60105ee39dcf" />

Alex demonstrated the join.datatalks invite

<img width="1090" height="626" alt="image" src="https://github.com/user-attachments/assets/6f77f341-6dc0-47bb-a822-5eb543810036" />

user use link  join.datatalks.club , and it will be redirected to the invite link...use lambda function
  <img width="1107" height="662" alt="image" src="https://github.com/user-attachments/assets/92dd5570-4844-4929-9089-d1a9fc871a0c" />

  simple lambda function, in javascript
  <img width="987" height="682" alt="image" src="https://github.com/user-attachments/assets/5039a373-4155-42a7-9229-1e7796a22148" />

 actual invite URL is in config.

 <img width="1069" height="568" alt="image" src="https://github.com/user-attachments/assets/a874a35b-f5c2-482e-94cc-f12360cd9915" />

- there is a free-tier , 
   
use tensor flow lite....


