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

Create using uv , uv add packages, then create docker file
  

   Simplest way is to put everything in docker, need sck-learn, numpy all dependencies need to be installed.... simplest way is put lambda function in docker container, and then deploy docker container.... lambda is based on docker image...

### Create Dockerfile

```Dockerfile
   FROM public.ecr.aws/lambda/python:3.13
   RUN pip install uv
   COPY pyproject.toml uv.lock ./
   RUN uv sync
   COPY model.bin .
   COPY lambda_function.py .

   CMD ["lambda_function.lambda_handler"]
```
   we are telling it that to invoke the lambda function, go to the lambda handler (entry point to the lambda function)

### Build Dockerfile

```bash
   docker build -t churn-prediction-lambda .
```

### To run docker file

```bash
   docker run -it --rm -p 8080:8080 churn-prediction-lambda
```

### To test docker file
#### Create test.py
```python
import requests
import json

url = 'http://localhost:8080/2015-03-31/functions/function/invocations'


with open('customer.json', 'r') as f_in:   
    customer = json.load(f_in)

result = requests.post(url, json=customer).json()
print(result)
```

```bash
   python test.py
```

It doesn't work because the pip install within the Dockerfile installs to a virtual environment, but in Lambda, it needs to be done differently. Need to be installed to global environment...  

```dockerfile
FROM public.ecr.aws/lambda/python:3.13
COPY --from=ghcr.io/astral-sh/uv:latest /uv /bin/

COPY pyproject.toml uv.lock ./
RUN uv pip install --system -r <(uv export --format requirements-txt)

COPY lambda_function.py model.bin ./

CMD ["lambda_function.lambda_handler"]
```

need to publish or deploy docker image to ECR amazon elastic container registry, create a publish.sh
```bash
#!/bin/bash

IMAGE_NAME="churn-prediction-lambda"
AWS_REGION="eu-west-1"

AWS_ACCOUNT_ID=$(aws sts get-caller-identity | jq -r ".Account")

# Get latest commit SHA and current datetime
COMMIT_SHA=$(git rev-parse --short HEAD)
DATETIME=$(date +"%Y%m%d-%H%M%S")
IMAGE_TAG="${COMMIT_SHA}-${DATETIME}"

ECR_URI="${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com"
IMAGE_URI="${ECR_URI}/${IMAGE_NAME}:${IMAGE_TAG}"

# run it only once
# aws ecr create-repository \
#   --repository-name ${IMAGE_NAME} \
#   --region ${AWS_REGION}

aws ecr get-login-password \
  --region ${AWS_REGION} \
| docker login \
  --username AWS \
  --password-stdin ${ECR_URI}

docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
docker tag ${IMAGE_NAME}:${IMAGE_TAG} ${IMAGE_URI}
docker push ${IMAGE_URI}

aws lambda update-function-code \
  --function-name churn-prediction-docker \
  --image-uri ${IMAGE_URI} \
  --region ${AWS_REGION}
```

publish with v1 version.... 
- create function
- container image
- churn-prediction-docker
- add the image uri

sagemaker plus lambda is expensive...

if model is 2gb, it will be invoked only once, when function is invoked the first time,  first request it will be loaded, once in memory, it will not require reloading...
if no request, then it will be freed, then next time, it will need to load it again,  if you send many request, the lambda is kept in warm-up state, no need to load model all time, when it goes to sleep stage, then
if you wake it up, it will load the model again. test, it by creating test event, and event json...
<img width="1305" height="660" alt="image" src="https://github.com/user-attachments/assets/0c670c01-bd00-4b7c-a696-32f085775afe" />

For lambda - tensorflow-lite is not good anymore,  tensorflow-lite is okay previously...  
<img width="504" height="289" alt="image" src="https://github.com/user-attachments/assets/1232e18e-4060-4e63-8c94-4afe302cea45" />


convert to onnx, and onnx runtime ...
<img width="502" height="411" alt="image" src="https://github.com/user-attachments/assets/a8527c65-60e0-4218-b3e2-73061411f415" />


<img width="936" height="555" alt="image" src="https://github.com/user-attachments/assets/34475325-c1d1-458d-b08b-4f7a047eaf0c" />

best to convert to onnx within docker...
```python
from tensorflow import keras

model = keras.models.load_model('clothing-model-new.keras')
model.export("clothing-model-new_savedmodel")
```

run the docker image...  run model mapping..
<img width="1217" height="668" alt="image" src="https://github.com/user-attachments/assets/3265dbd4-a6dc-4bbb-866b-ec7944ed54c2" />

<img width="1380" height="612" alt="image" src="https://github.com/user-attachments/assets/45f6efdb-7829-4a0d-81ec-4b92dc1ad6ad" />

keras image helper...
function is simple, it divides the value by 127.5 and then subtract 1.0

package everything into lambda function...

<img width="706" height="353" alt="image" src="https://github.com/user-attachments/assets/dd9fc2aa-fe70-413d-8b05-0fbb72a32d37" />
you can test the docker image locally..

via docker run



#### next steps
1. create ecr repo
2. publish image
3. create lambda function to point to repo
4. configure by giving 500 ram, timeout to 30 s or 1 minute
5. keras convert to tensorflow to to save model, and then to onnx
6. can also use pytorch, and then to onnx
7. ask gemini, convert tensorflow to pytorch
8. mobilenet, require different sizes...
9. export to onnx...
10. pytorch already has the onnx export library

11. totensor divides everything by 255., changes shape, channels, height, width, preprocessing that's different,  normalises,,,  
transpose... etc...  



