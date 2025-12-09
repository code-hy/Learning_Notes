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
