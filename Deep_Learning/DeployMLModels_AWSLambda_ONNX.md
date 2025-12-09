# Deploy Machine Learning Models with AWS Lambda and ONNX

This workshop teaches how to package and deploy Scikit-learn, Keras, and PyTorch models as ONNX-based serverless inference endpoints on AWS Lambda using Docker and ECR, and how to expose them via API Gateway for pay-per-execution workloads.[1][2]

## Why ONNX + Lambda

- ONNX is used as a common, optimized model format so models from different frameworks (Scikit-learn, Keras, PyTorch) can be run efficiently with ONNX Runtime inside Lambda under memory and cold-start constraints.[2][1]
- Converting models to ONNX helps reduce load and inference time, which is important for Lambda’s short-lived, on-demand execution model.[1][2]

## Packaging and deployment flow

- Models are trained locally (e.g., a churn prediction model) and exported to ONNX, then bundled with inference code and dependencies in a Docker image.[1]
- The image is pushed to Amazon ECR, and an AWS Lambda function is created from that image, tested locally with Docker first, then wired up to triggers in the cloud.[3][1]

## Lambda integration and triggers

- AWS API Gateway is commonly used as the front-end that receives HTTP requests, triggers the Lambda function, and returns the inference result to the client.[2][1]
- Lambda functions can also be triggered by other AWS services such as EventBridge or data pipelines, making them suitable for both real-time APIs and event-driven ML workflows.[4][1]

## Performance and cost considerations

- Lambda is positioned as a good fit for spiky or low-to-moderate traffic ML workloads due to pay-per-execution pricing and no need to manage servers.[2][1]
- The workshop covers minimizing cold starts (e.g., optimizing image size and dependency set) and compares Lambda to SageMaker in terms of cost and operational complexity for different deployment scenarios.[3][1]

[1](https://www.youtube.com/watch?v=sHQaeVm5hT8)
[2](https://pyimagesearch.com/2025/11/03/introduction-to-serverless-model-deployment-with-aws-lambda-and-onnx/)
[3](https://aws.amazon.com/blogs/machine-learning/build-reusable-serverless-inference-functions-for-your-amazon-sagemaker-models-using-aws-lambda-layers-and-containers/)
[4](https://workshops.aws/categories/Serverless)
[5](https://www.youtube.com/watch?v=sHQaeVm5hT8&list=PL3MmuxUbc_hIhxl5Ji8t4O6lPAOpHaCLR&index=86)
[6](https://quillbot.com/summarize)
[7](https://www.scribbr.com/text-summarizer/)
[8](https://www.mindgrasp.ai/study-aids/key-takeaways-ai)
[9](https://www.fluig.cc/blog/post/10-best-free-ai-document-summarizers)
[10](https://app.cedille.ai/summarization/bullet-points-extraction)
[11](https://www.scholarcy.com/article-summarizer)


Deploying ML models in this workshop follows a repeated pattern: train/export the model, package it into a Docker image, push the image to ECR, and create a Lambda from that image with appropriate memory/timeout settings.[1]

## General step-by-step flow

- Train a model locally (e.g., Scikit-learn churn model, Keras CNN, or PyTorch model) and save it to disk in the framework’s native format (pickle, Keras .h5, PyTorch .pt) or TensorFlow SavedModel.[1]
- Convert framework-specific models to ONNX where possible (Keras/TensorFlow via TF→SavedModel→ONNX, PyTorch via torch.onnx.export) to get a lightweight, framework-agnostic artifact.[1]

## Building a Lambda-ready Docker image

- Start from an AWS Lambda base image in a Dockerfile, install required Python dependencies (e.g., scikit-learn, onnxruntime, keras-image-helper), and copy both the model file and handler code into the image.[1]
- Define the Lambda entrypoint using CMD so Lambda knows which module and function to call (for example, lambda_function.lambda_handler) and ensure heavy objects (model load, ONNX session creation) are initialized at import time, outside the handler, to benefit from warm starts.[1]

## Local testing and debugging

- Run the Lambda container locally with docker run and call it via the AWS Lambda Runtime Interface Emulator or simple HTTP request code to verify that requests and responses work as expected before pushing to the cloud.[1]
- For image models, use helper code to fetch an example image, preprocess it to the required input tensor shape, send it through onnxruntime, and check that class probabilities and labels look reasonable.[1]

## Publishing to AWS and wiring Lambda

- Create an ECR repository, log in with the AWS CLI, tag the local image with the ECR URI, and push the versioned image (e.g., :v1) to ECR.[1]
- In the Lambda console, create a new function “from container image,” point it at the ECR image tag, and then configure memory and timeout high enough for model load and inference (e.g., 512 MB–1 GB RAM and up to 30–60 seconds for heavy models).[1]

## Invoking the deployed model

- Invoke the Lambda directly using boto3 (lambda_client.invoke) with a JSON payload that matches the handler’s expected schema (e.g., {"customer": {...}} or {"url": "<image-url>"}), then parse the JSON-encoded prediction from the response.[1]
- Optionally attach API Gateway or EventBridge to the Lambda to expose an HTTPS endpoint or event-driven trigger, reusing the same containerized ONNX-based model for scalable, pay-per-execution inference.[1]

[1](https://www.youtube.com/watch?v=sHQaeVm5hT8&list=PL3MmuxUbc_hIhxl5Ji8t4O6lPAOpHaCLR&index=86)
