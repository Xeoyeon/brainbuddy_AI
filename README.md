# BrainBuddy: Real-time Engagement Monitoring System
> **High-performance AI model (CNN-LSTM) validated for production-scale stability and cost-efficiency.**

BrainBuddy is an AI-driven service that measures user engagement levels from video data in real-time. By integrating a **CNN-LSTM architecture** with **MediaPipe-based preprocessing**, the system delivers high-accuracy focus analysis. To ensure practical viability, the system was validated through rigorous **k6 stress testing**, maintaining stability for 200 concurrent users.

<br>

## 🛠 Tech Stack
### **AI & Computer Vision**
- **Core:** ![Python](https://img.shields.io/badge/Python-3.10+-blue?logo=python&logoColor=white) ![PyTorch](https://img.shields.io/badge/PyTorch-%23EE4C2C.svg?logo=pytorch&logoColor=white)
- **Modeling:** MobileNetV3-Large (CNN Backbone), LSTM (Temporal Analysis)
- **Vision:** ![OpenCV](https://img.shields.io/badge/OpenCV-%23white.svg?logo=opencv&logoColor=black) ![Mediapipe](https://img.shields.io/badge/Mediapipe-4285F4?logo=google&logoColor=white) (Face Detection & Cropping)

### **Infrastructure & Optimization**
- **Testing:** ![k6](https://img.shields.io/badge/k6-7D64FF?logo=k6&logoColor=white) (System Load & Stress Testing)
- **Deployment:** ![AWS](https://img.shields.io/badge/AWS-232F3E?logo=amazon-aws&logoColor=white) (t2.micro, t3.medium, r7.ixlarge)
- **Web Server:** ![Nginx](https://img.shields.io/badge/Nginx-009639?logo=nginx&logoColor=white) (Load Balancing & Caching)

<br>

## 📂 Project Structure
```bash
├── EDA                   # Embedding visualization (UMAP, t-SNE)
├── datasets              # Custom video dataset loaders
├── models                # CNN-Encoder, LSTM Engagement Model, Face-cropping logic
├── preprocessing         # Frame extraction and automated labeling (MediaPipe)
└── real_time.py          # Real-time inference script for local execution
```

<br>

## 🏗 Model Architecture
### **1. AI Inference Pipeline**
* **Input:** 30-frame video sequence
* **Backbone:** **MobileNetV3-Large**
* **Temporal Modeling:** **LSTM** 
* **Output:** Binary (Engaged / Disengaged)


<br>

## 🛠 Data Pipeline & Preprocessing
I implemented a robust, end-to-end data pipeline to transform unprocessed, raw video footage into a structured, training-ready format.

- **Robust Face Detection**: Integrated MediaPipe with a fallback mechanism (utilizing previous valid frames) to ensure stable real-time inference during occlusion.

- **Temporal Sampling**: Standardized 30 frames per 10s window to maintain consistent temporal density for LSTM modeling, regardless of source FPS.

- **Data Optimization**: Automated labeling and utilized .pkl exports to maximize I/O efficiency during high-speed training and inference.




## 📊 Model Performance
To ensure a objective comparison between these models, we curated **a custom-collected testset** that reflects real-world variability. The following table summarizes the performance of the final selected model on both the AIHub benchmark and the custom-collected dataset:

| Test Set | Accuracy | Recall | F1-Score |
| :--- | :--- | :--- | :--- |
| **AIHub Test Set** | 80.88% | 86.90% | **0.8149** |
| **Custom Real-world Set** | **81.03%** | **89.11%** | 0.7993 |

<br>

### 🌐 Production-Ready Validation (Load Test)
To ensure the AI model's real-time viability, I collaborated with the Backend team to validate the system stability and cost-efficiency through k6 load testing.

>* **Scalability:** Supported **200 concurrent users** with a **100% success rate** (0.0% failure).
>* **Latency:** Achieved an average response duration of **32ms** and guaranteed sub-second latency under peak load.
>* **Cost Efficiency:** Estimated an optimized monthly operational cost of **$765** for the verified capacity.

<br>

### Cloud Infrastructure Architecture
* **Web Server:** Nginx-based server (AWS **t2.micro**)
* **App Servers (WAS):** 4 instances of AWS **t3.medium**
* **Database:** A single AWS **r7.ixlarge** instance

<br>

### **Load Testing with k6**
We performed a 30-minute stress test to identify the system's breaking point and ensure consistent AI inference delivery. 

1.  **Closed Model (ramping-vus):** Tested concurrency-based load up to 100 VUs. 
2. **Open Model (ramping-arrival-rate):** Tested arrival rate-based load up to 100 iters/s (~380 req/s). 






## 💻 Quick Start
1. Download the pre-trained weight: `best_model.pt`.
2. Configure the `CKPT_PATH` in `real_time.py`.
3. Run the real-time inference:
   ```bash
   python real_time.py
   ```
   *Note: Inference window (T_WINDOW) and stride (STRIDE_SEC) can be adjusted to balance latency and accuracy.*



