# Serverless Deep Learning

## Introduction 

In the previous module, we built and trained a clothes classification deep learning module using Pytorch.  This module focuses
on deploying it.l  The model categorises images of clothing items uploaded by users on a website.  Deployment will be used using
AWS Lamda, a serverless solution to execute code without managing servers, and instead of **TensorFlow**  , we use
*TensorFlow-lite* for this.  But this is finally replaced with *ONNX* serving.

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
   
use tensor flow lite....but it is no longer suitable for AWS Lambda.  The benefits is to use ONNX instead.
  - create a simple AWS lambda function
  - deploy scikit-learn models with docker
  - using ONNX for Keras and Tensorflow models
  - ONNX for Pytorch models

In workshop, use codespaces.  uv+fastapi workshop , show how to use codespaces.  Recommended to use codespaces

### Prerequisites
Create AWS account, have AWS CLI

## Workshop (Deploy ML Models with AWS Lambda and ONNX)
1.  get the train.py from the uv+fastapi workshop, which will train a model and output the trained model
   ```python
     import pickle

import pandas as pd
import sklearn

from sklearn.feature_extraction import DictVectorizer
from sklearn.linear_model import LogisticRegression
from sklearn.pipeline import make_pipeline


print(f'pandas=={pd.__version__}')
print(f'sklearn=={sklearn.__version__}')


def load_data():
    data_url = 'https://raw.githubusercontent.com/alexeygrigorev/mlbookcamp-code/master/chapter-03-churn-prediction/WA_Fn-UseC_-Telco-Customer-Churn.csv'

    df = pd.read_csv(data_url)

    df.columns = df.columns.str.lower().str.replace(' ', '_')

    categorical_columns = list(df.dtypes[df.dtypes == 'object'].index)

    for c in categorical_columns:
        df[c] = df[c].str.lower().str.replace(' ', '_')

    df.totalcharges = pd.to_numeric(df.totalcharges, errors='coerce')
    df.totalcharges = df.totalcharges.fillna(0)

    df.churn = (df.churn == 'yes').astype(int)
    return df


def train_model(df):
    numerical = ['tenure', 'monthlycharges', 'totalcharges']

    categorical = [
        'gender',
        'seniorcitizen',
        'partner',
        'dependents',
        'phoneservice',
        'multiplelines',
        'internetservice',
        'onlinesecurity',
        'onlinebackup',
        'deviceprotection',
        'techsupport',
        'streamingtv',
        'streamingmovies',
        'contract',
        'paperlessbilling',
        'paymentmethod',
    ]


    y_train = df.churn
    train_dict = df[categorical + numerical].to_dict(orient='records')

    pipeline = make_pipeline(
        DictVectorizer(),
        LogisticRegression(solver='liblinear')
    )

    pipeline.fit(train_dict, y_train)

    return pipeline


def save_model(pipeline, output_file):
    with open(output_file, 'wb') as f_out:
        pickle.dump(pipeline, f_out)



df = load_data()
pipeline = train_model(df)
save_model(pipeline, 'model.bin')

print('Model saved to model.bin')
   ```

  ```bash
    cd train
    uv sync  # will actually install all the dependencies
    uv run python train.py
  ```
    
  ```bash
     uv init # initialise the project in the folder it is currently in 
     uv add scikit-learn pandas  # adds packages 
     uv run python train.py  # creates a model file 
  ```

  This finally creates the model.bin file which we will deploy to Lambda...
  - to deploy to Lambda we need to create a file ie. lambda_function.py

<img width="1044" height="559" alt="image" src="https://github.com/user-attachments/assets/2fd5c996-c619-4df9-a17e-de1d48eb8f05" />

  -  sign in to aws, go to lambda
  -  create function, give function name,  runtime ... architecture...create new role, and leave the others as default
  - 
    ```python 
       import pickle
        with open('model.bin', 'rb') as f_in:
        pipeline = pickle.load(f_in)

    def predict_single(customer):
         result = pipeline.predict_proba(customer)[0, 1]
        return float(result)

    def lambda_handler(event, context):
        # print("Parameters:", event)

        customer = event['customer']
        prob = predict_single(customer)

        return {
            "churn_probability": prob,
            "churn": bool(prob >= 0.5)
        }
     ```
  

   Simplest way is to put everything in docker, need sck-learn, numpy all dependencies need to be installed.... simplest way is put lambda function in docker container, and then deploy docker container.... lambda is based on docker image...

### Create Dockerfile

```Dockerfile
   FROM public.ecr.aws/lambda/python:3.13
   RUN pip install uv
   COPY pyproject.toml uv.lock ./
   RUN uv sync
   COPY model.bin .
   COPY lambda_function.py .

   CMD ["LAMBDA_FUNCTION.LAMBDA_HANDLER"]
```
   we are telling it that to invoke the lambda function, go to the lambda handler (entry point to the lambda function)


