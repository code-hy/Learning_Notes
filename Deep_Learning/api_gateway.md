#  API Gateway

This video shows how to turn an existing Lambda-based ML model into a public HTTP API using Amazon API Gateway, then test it from a script using the new URL.[1]

## Creating the REST API

- In API Gateway, a new REST API is created specifically for the existing Lambda function that serves the model.[1]
- A resource named `/predict` is added to follow the same pattern used earlier with Flask, representing the prediction endpoint.[1]

## Connecting Lambda to POST /predict

- For the `/predict` resource, a POST method is created that integrates directly with the ML Lambda function (e.g., `clothing-classification`) in the same region.[1]
- While creating the method, API Gateway automatically updates Lambda permissions so the gateway can invoke the function.[1]

## Testing and deploying the API

- The built‑in test tool in API Gateway is used first: a JSON body with the image URL is sent as a POST request to verify the Lambda response and see latency for cold vs warm invocations.[1]
- The API is then deployed to a stage (e.g., `test`), which generates a public base URL; the client script is updated to call this URL plus `/predict` instead of invoking Lambda directly.[1]

## Calling from a client and security note

- A local test script sends POST requests to the new API Gateway URL, which forwards to Lambda, gets the prediction, and returns it back through the gateway as an HTTP response.[1]
- The video warns that this setup leaves the endpoint publicly accessible and stresses that in real projects you must restrict access (auth, IAM, etc.) rather than exposing the Lambda to everyone.[1]

[1](https://www.youtube.com/watch?v=wyZ9aqQOXvs&list=PL3MmuxUbc_hIhxl5Ji8t4O6lPAOpHaCLR&index=85)
