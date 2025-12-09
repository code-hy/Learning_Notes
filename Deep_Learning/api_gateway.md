#  API Gateway

This video shows how to turn an existing Lambda-based ML model into a public HTTP API using Amazon API Gateway, then test it from a script using the new URL.[1]

## Creating the REST API

- In API Gateway, a new REST API is created specifically for the existing Lambda function that serves the model.[1]
- A resource named `/predict` is added to follow the same pattern used earlier with Flask, representing the prediction endpoint.[1]
<img width="1286" height="772" alt="image" src="https://github.com/user-attachments/assets/fade1975-b6c9-4ddc-b777-49669370454d" />

1. select REST API
2. new API
3. API name (clothes-classification)
4. Create API
5. Create Resource

## Connecting Lambda to POST /predict

- For the `/predict` resource, a POST method is created that integrates directly with the ML Lambda function (e.g., `clothing-classification`) in the same region.[1]
- While creating the method, API Gateway automatically updates Lambda permissions so the gateway can invoke the function.[1]
<img width="1250" height="660" alt="image" src="https://github.com/user-attachments/assets/882743e1-b98f-46de-8e77-44d81d4bae16" />

1. create method
2. /predict post...
3. integration type, we select lambda function

<img width="1289" height="710" alt="image" src="https://github.com/user-attachments/assets/9f366c21-ce68-47e1-b945-49bd4a632221" />
REQUEST BODY , then click TEST

<img width="1288" height="661" alt="image" src="https://github.com/user-attachments/assets/56230b7b-22a0-47a0-9d6c-75d948e833c8" />


## Testing and deploying the API

- The built‑in test tool in API Gateway is used first: a JSON body with the image URL is sent as a POST request to verify the Lambda response and see latency for cold vs warm invocations.[1]
- The API is then deployed to a stage (e.g., `test`), which generates a public base URL; the client script is updated to call this URL plus `/predict` instead of invoking Lambda directly.[1]

<img width="1290" height="684" alt="image" src="https://github.com/user-attachments/assets/22ebc096-d2c5-4f57-882f-988bedf572da" />

<img width="1244" height="791" alt="image" src="https://github.com/user-attachments/assets/ff6af1fc-834f-4905-8a1a-f41274d151ea" />

## Calling from a client and security note

- A local test script sends POST requests to the new API Gateway URL, which forwards to Lambda, gets the prediction, and returns it back through the gateway as an HTTP response.[1]
- The video warns that this setup leaves the endpoint publicly accessible and stresses that in real projects you must restrict access (auth, IAM, etc.) rather than exposing the Lambda to everyone.[1]

[1](https://www.youtube.com/watch?v=wyZ9aqQOXvs&list=PL3MmuxUbc_hIhxl5Ji8t4O6lPAOpHaCLR&index=85)


Turn lambda function into web service by creating API ...

