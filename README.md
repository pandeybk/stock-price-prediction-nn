# Converting Keras Models to ONNX for OpenShift OpenVINO Model Server

This repository demonstrates converting a `.keras` model to ONNX format for deployment on OpenShift OpenVINO Model Server. The project uses LSTM to predict stock prices and prepares the model for scalable deployment.

## Steps

1. **Train and Save the Model**: Train an LSTM-based model for stock price prediction and save it in `.keras` format.
2. **Convert to ONNX**: Convert the `.keras` model to TensorFlow SavedModel format and then to ONNX using `tf2onnx`.
3. **Deploy on OpenShift**: Deploy the ONNX model on OpenVINO Model Server in an OpenShift cluster.
4. **Perform Inference**: Query the deployed model using REST API for predictions.

## Notebook

The notebook `notebook.ipynb` contains all the code:
- Data loading with `yfinance`
- Model training and evaluation
- Conversion to ONNX format
- Sample inference requests with or without tokens

## Requirements

- Python 3.7+
- TensorFlow, Keras, tf2onnx, yfinance, requests, matplotlib
- OpenShift cluster with OpenVINO Model Server installed

## Inference Example

```python
import requests
import json

MODEL_SERVER = "https://your-model-server-url/v2/models/stock-prediction/infer"
TOKEN = "your_auth_token"

input_data = {
    "inputs": [
        {
            "name": "inputs",
            "shape": [1, 60, 1],
            "datatype": "FP32",
            "data": [...your data here...]
        }
    ]
}

response = requests.post(
    MODEL_SERVER,
    headers={
        "Content-Type": "application/json",
        "Authorization": f"Bearer {TOKEN}"
    },
    data=json.dumps(input_data)
)

print(response.json())
