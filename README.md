# Vehicle Insurance Prediction

# Project Setup and Deployment Guide
---

## 1. Create Project Template

- Execute the `template.py` file to initialize the project template.


## 2. Setup Packaging

- Write your `setup.py` and `pyproject.toml` to import local packages.
- Refer to the "setup.py and pyproject.toml" details in `crashcourse.txt` for guidance.

## 3. Virtual Environment and Dependencies

conda create -n vehicle python=3.10 -y
conda activate vehicle

Add required modules to requirements.txt
pip install -r requirements.txt
pip list # Verify local packages are installed


---

## MongoDB Atlas Setup

1. Sign up on MongoDB Atlas, create a new project with a name and proceed.
2. Create a cluster with M0 free tier and default settings.
3. Set up username and password, create a DB user.
4. Configure network access: add IP address `0.0.0.0/0` for open access.
5. Get the connection string with Python driver (3.6+), replace password in the string and save.
6. Create a folder `notebook`, then create `mongoDB_demo.ipynb` in it.
7. Add the dataset to the `notebook` folder.
8. Push data to MongoDB via Python notebook.
9. Verify data in MongoDB Atlas under Database > browse collections.

---

## Logging, Exception Handling & Notebooks

- Write logger and exception handling files; test on `demo.py`.
- EDA and feature engineering notebooks have been added.

---

## Data Ingestion Component

- Declare variables in `constants/__init__.py`.
- Add MongoDB connection code in `configuration/mongo_db_connections.py`.
- Use `data_access/proj1_data.py` to fetch and transform MongoDB data to a dataframe.
- Define `DataIngestionConfig` in `entity/config_entity.py`.
- Define `DataIngestionArtifact` in `entity/artifact_entity.py`.
- Implement ingestion logic in `components/data_ingestion.py`.
- Update training pipeline and run `demo.py` after setting MongoDB URL.

Set MongoDB URL environment variable:

For Bash
export MONGODB_URL="mongodb+srv://<username>:<password>......"
echo $MONGODB_URL

For Powershell
$env:MONGODB_URL="mongodb+srv://<username>:<password>......"
echo $env:MONGODB_URL


For Windows, set `MONGODB_URL` in environment variables. Add `artifact` dir to `.gitignore`.

---

## Data Validation, Transformation & Model Training

- Complete `utils/main_utils.py` and `config/schema.yaml` with dataset schema.
- Implement Data Validation component.
- Implement Data Transformation component; add `estimator.py` to `entity` folder.
- Implement Model Trainer component; add classes in `estimator.py`.

---

## AWS Setup for Model Evaluation

1. Login to AWS console, set region `us-east-1`.
2. Create IAM user `firstproj` with `AdministratorAccess`.
3. Generate Access Key, download CSV.
4. Set environment variables:

Bash
export AWS_ACCESS_KEY_ID="YOUR_ACCESS_KEY_ID"
export AWS_SECRET_ACCESS_KEY="YOUR_SECRET_ACCESS_KEY"
echo $AWS_ACCESS_KEY_ID
echo $AWS_SECRET_ACCESS_KEY

Powershell
$env:AWS_ACCESS_KEY_ID="YOUR_ACCESS_KEY_ID"
$env:AWS_SECRET_ACCESS_KEY="YOUR_SECRET_ACCESS_KEY"
echo $env:AWS_ACCESS_KEY_ID
echo $env:AWS_SECRET_ACCESS_KEY


5. Add keys and region to `constants/__init__.py`.
6. Add AWS S3 connection code in `src/configuration/aws_connection.py`.
7. Create an S3 bucket "my-model-mlopsproj" in `us-east-1`.
8. Implement S3 functions in `entity/s3_estimator.py`.

Constants example:

MODEL_EVALUATION_CHANGED_THRESHOLD_SCORE: float = 0.02
MODEL_BUCKET_NAME = "vehicle-insurance-prediction-mlops-01"
MODEL_PUSHER_S3_KEY = "model-registry"


---

## Model Evaluation and Pusher

- Implement Model Evaluation and Model Pusher components as per previous steps.

## Prediction Pipeline and Web App

- Set up "Prediction Pipeline" structure.
- Create `app.py`.
- Add `static` and `template` directories for frontend assets.

---

## CI/CD Pipeline

- Setup `Dockerfile` and `.dockerignore`.
- Add GitHub workflows in `.github/workflows/aws.yaml`.
- Create an IAM user `usvisa-user` on AWS for CI/CD with appropriate access and key.
- Create an ECR repo `vehicleproj` in AWS to store Docker images.
- Launch an EC2 instance (Ubuntu 24.04, t2.medium).
- Connect to EC2 and install Docker:

sudo apt-get update -y
sudo apt-get upgrade -y
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker ubuntu
newgrp docker


- Connect GitHub self-hosted runner on EC2.
- Add GitHub Secrets: `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_DEFAULT_REGION`, `ECR_REPO`.
- CI/CD runs on next commit push.

---

## EC2 Instance Port Setup

- Add inbound rule to security group for port `5080` (Custom TCP, 0.0.0.0/0).
- Access the app at `http://<EC2-public-IP>:5080`.
- Model training accessible at `/training` route.

---
