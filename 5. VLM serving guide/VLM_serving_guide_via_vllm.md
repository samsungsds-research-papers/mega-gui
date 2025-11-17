# General vLLM Serving Guide for VLM
#### General vLLM(Virtual Large Language Model) Serving Guide for VLM(Vision Large Language Model)

## 1. Experiment Environment
- **GPU**: V100 (32G) x 8  
- **Docker Version**: 28.0.0  
- **Driver Version**: 550.120  
- **CUDA Version**: 12.4  

---

## 2. Serving Guide for Large Language Models

### Step 1: Download Docker Image
- Use the following command to pull the Docker image:  
  ```bash
  docker pull vllm/vllm-openai:v0.9.1
  ```

### Step 2: Download Model Checkpoint
Download the checkpoint of your desired model from the Hugging Face Model Hub. For example:  
- **UI-TARS-72B-DPO**: [UI-TARS-72B-DPO Checkpoint](https://huggingface.co/ByteDance-Seed/UI-TARS-72B-DPO)  
- **Qwen2.5-VL-72B-Instruct**: [Qwen2.5-VL-72B-Instruct Checkpoint](https://huggingface.co/Qwen/Qwen2.5-VL-72B-Instruct)  
- **GTA1-72B**: [GTA1-72B Checkpoint](https://huggingface.co/HelloKKMe/GTA1-72B)  
- **GTA1-7B**: [GTA1-7B Checkpoint](https://huggingface.co/HelloKKMe/GTA1-7B)  

### Step 3: Run Docker Container
Place the checkpoint in the directory `/DATA/USER_PATH/serving_model/<MODEL_NAME>` and use the following Docker command to serve the model:  

```bash
sudo docker run -d -v /DATA/USER_PATH:/DATA/USER_PATH -it -p 6008:8002 --gpus all --shm-size=250g -e CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7 jihoon/vllm:v0.9.1 /opt/conda/bin/vllm serve /DATA/USER_PATH/serving_model/<MODEL_NAME> --served-model-name <MODEL_NAME> --trust-remote-code --host 0.0.0.0 --port 8002 --enforce-eager --device auto --dtype=half --tensor_parallel_size=8 --limit-mm-per-prompt image=2 --max-model-len 40960 --gpu_memory_utilization 0.99
```

Replace `<MODEL_NAME>` with the name of your model (e.g., `ui-tars-72b`, `qwen2.5-vl-72b`, `gta1-7b`, `gta1-72b`).

### Step 4: Verify Serving
Once the model is successfully served:  
- **VLLM_SERVER_IP**: The IP address of the server where the model is served.  
- **VLLM_SERVER_PORT**: Set to `8002`.  

### Step 5: Inference
Update the `get_coordinate` code with the correct `VLLM_SERVER_IP` and `VLLM_SERVER_PORT` to perform inference.  

---

## 3. Notes
This guide is designed to be generic and can be applied to any large language model available on the Hugging Face Model Hub. Simply replace the checkpoint and model name with your desired model, and follow the same steps to serve it using vLLM.  

For more detailed information on vLLM configurations and options, refer to the [vLLM documentation](https://docs.vllm.ai/en/v0.9.1/).