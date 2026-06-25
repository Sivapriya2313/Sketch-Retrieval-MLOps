Advanced Sketch Retrieval Engine (Enterprise MLOps Deployment):
An end-to-end machine learning architecture that allows users to upload hand-drawn sketches and instantly retrieve visually similar real-world product images.
This project goes beyond the AI model by implementing a complete Enterprise MLOps Pipeline using Docker, Azure DevOps Server 2022, and CI/CD automation.

Architecture overview:
This system is built using a decoupled microservice architecture:
Frontend (Streamlit): A lightweight, interactive web dashboard for user uploads and result visualization.
Backend API (FastAPI): A high-performance REST API that loads a PyTorch (ResNet50) neural network into memory to calculate feature vectors and structural similarities.
CI/CD Pipeline (Azure DevOps): A fully automated deployment pipeline powered by a self-hosted Windows agent.

The 4-Phase MLOps Workflow:
On-Premises Infrastructure: Bypassed cloud dependencies by configuring Azure DevOps Server 2022 locally to manage the Git repository and pipeline orchestration.
Self-Hosted Agents: Deployed a local Windows-based agent running as a background service (vstsagent) with Integrated Authentication to act as the bridge between the code repository and the host hardware.
CI/CD Automation: Authored azure-pipelines.yml to listen for commits on the main branch. The agent automatically executes a tear-down (docker compose down) and rebuilds the infrastructure (docker compose up -d --build).
Volume Orchestration: Heavy ML artifacts (Datasets & .pth weights) are dynamically injected into the isolated Linux containers via Absolute Volume Mounts, keeping the Git repository lightweight while ensuring the FastAPI backend has access to production data.

Tech Stack:
AI & Machine Learning: PyTorch, Torchvision, Numpy
Backend: FastAPI, Uvicorn, Python 3.10
Frontend: Streamlit, Requests, Pillow
DevOps & Infrastructure: Docker, Docker Compose, Azure DevOps Server 2022 (On-Premises), YAML Pipelines

How to Run Locally:
Prerequisites
Docker Desktop installed and running (WSL2 integration enabled).
Python 3.10+ (for local testing).
Model weights (ckpt_epoch_80.pth) and the dataset directory (trainB) placed in the designated absolute paths (or updated in the docker-compose.yml).
Deployment
Because this project is containerized, deployment takes a single command:

# Clone the repository
git clone [https://github.com/YourUsername/Sketch-Retrieval-MLOps.git](https://github.com/YourUsername/Sketch-Retrieval-MLOps.git)
cd Sketch-Retrieval-MLOps

# Build and start the microservices in detached mode
docker compose up -d --build


Accessing the App
Once the containers are running, open your web browser and navigate to:
Frontend UI: http://localhost:8501
Backend API Docs (Swagger): http://localhost:8000/docs
📁 Repository Structure
├── app.py                  # FastAPI backend server and PyTorch logic
├── ui.py                   # Streamlit frontend user interface
├── docker-compose.yml      # Container orchestration and volume mapping
├── Dockerfile              # Backend image build instructions
├── requirements.txt        # Python library dependencies
├── azure-pipelines.yml     # Automated CI/CD deployment instructions
└── README.md               # Project documentation


Note on Heavy Files:
To adhere to Git best practices, the model weights (*.pth) and the massive image dataset (trainB/) are deliberately excluded from this repository via .gitignore.
To deploy this project, you must provide your own dataset and pre-trained PyTorch weights and map them correctly via absolute volume paths inside the docker-compose.yml file.
