This video wraps up the serverless deployment session by emphasizing that AWS Lambda plus API Gateway allows deploying ML (including deep learning) without managing servers, paying only when requests are actually processed.[1]

## Key takeaways

- Lambda is a good fit for low-volume or personal ML projects because it scales automatically and charges only for invocation time, not idle time.[1]
- Packaging the model and code in Docker lets you fully test the Lambda environment locally before deploying, avoiding surprises in the cloud.[1]
- Once the containerized function is stable, increasing RAM and timeout in Lambda and exposing it through API Gateway is usually enough to get a production-ready HTTP endpoint.[1]

## TensorFlow Lite and model size

- TensorFlow Lite is introduced as an inference‑only runtime for TensorFlow models that dramatically reduces package size compared with full TensorFlow (on the order of hundreds‑to‑one reduction), which is valuable under Lambda’s size and cold‑start constraints.[1]
- The trade‑off is that using TensorFlow Lite typically requires a bit more custom inference code, but in return deployments are lighter and faster to start.[1]

## Beyond AWS and model types

- Similar serverless offerings exist in other major clouds (Google Cloud, Azure), and the same pattern can be applied there, so the concepts are portable.[1]
- The approach is not limited to deep learning: traditional models like XGBoost, linear models, or scikit‑learn models can also be packaged and deployed with Lambda for use in projects and capstone work.[1]

[1](https://www.youtube.com/watch?v=bu3nPiHCNLU&list=PL3MmuxUbc_hIhxl5Ji8t4O6lPAOpHaCLR&index=86)
