# MLflow Docker Compose

Deploy an MLflow tracking server with a single command. Uses MinIO S3 as artifact store and MySQL as backend store.

## Quick Start

```bash
git clone https://github.com/MBrede/mlflow-docker-compose.git
cd mlflow-docker-compose
docker compose up -d --build
```

- **MLflow UI**: http://localhost:5000
- **MinIO UI**: http://localhost:9000

## Architecture

| Container | Purpose |
|-----------|---------|
| MLflow server | Tracking server for experiments and models |
| MinIO | S3-compatible artifact storage |
| MySQL | Backend store for experiment metadata |

## Client Setup

Install dependencies:

```bash
pip install mlflow boto3
```

Configure environment:

```bash
export MLFLOW_TRACKING_URI=http://localhost:5000
export MLFLOW_S3_ENDPOINT_URL=http://localhost:9000
```

Configure MinIO credentials:

```bash
cat <<EOF > ~/.aws/credentials
[default]
aws_access_key_id=minio
aws_secret_access_key=minio123
EOF
```

## Example Usage

Train a model:

```bash
mlflow run https://github.com/sachua/mlflow-example.git -P alpha=0.42
```

Serve a model:

```bash
export MODEL_ID=<your-model-id>
mlflow models serve -m runs:/${MODEL_ID}/model -p 1234 --env-manager conda
```

Test inference:

```bash
curl -X POST -H "Content-Type:application/json" \
  --data '{"dataframe_split":{"columns":["fixed acidity", "volatile acidity", "citric acid", "residual sugar", "chlorides", "free sulfur dioxide", "total sulfur dioxide", "density", "pH", "sulphates", "alcohol"],"data":[[6.2, 0.66, 0.48, 1.2, 0.029, 29, 75, 0.98, 3.33, 0.39, 12.8]]}}' \
  http://127.0.0.1:1234/invocations
```

## Credits

Forked from [sachua/mlflow-docker-compose](https://github.com/sachua/mlflow-docker-compose)
