# Complete Flask Application for AWS ECS Deployment

This project is a production-ready Flask web application, containerized with Docker, and designed for seamless deployment on AWS Elastic Container Service (ECS).I used aws elatic beanstalk to deploy it

## Features
- Python Flask backend
- Dockerized for easy container management
- Ready for AWS ECS deployment
- Simple, extensible codebase

## Prerequisites
- Docker installed locally
- AWS account with ECS and ECR permissions
- AWS CLI configured (`aws configure`)

## Getting Started

### 1. Clone the Repository
```bash
git clone https://github.com/your-username/your-repo.git
cd your-repo
```

### 2. Build the Docker Image
```bash
docker build -t flask-app:latest .
```

### 3. Run Locally with Docker
```bash
docker run -p 5000:5000 flask-app:latest
```
Visit [http://localhost:5000](http://localhost:5000) to see your app running.

### 4. Push Image to AWS ECR
1. Create an ECR repository (if not already):
   ```bash
   aws ecr create-repository --repository-name flask-app
   ```
2. Authenticate Docker to your ECR:
   ```bash
   aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.<region>.amazonaws.com
   ```
3. Tag and push your image:
   ```bash
   docker tag flask-app:latest <aws_account_id>.dkr.ecr.<region>.amazonaws.com/flask-app:latest
   docker push <aws_account_id>.dkr.ecr.<region>.amazonaws.com/flask-app:latest
   ```

### 5. Deploy to AWS ECS
- Create a new ECS cluster and task definition using the pushed image.
- Set up a service to run your task and expose port 5000.
- (Optional) Use AWS Fargate for serverless container hosting.

## Project Structure
```
.
├── app.py              # Main Flask application
├── requirements.txt    # Python dependencies
├── Dockerfile          # Docker build instructions
├── ...                 # Other files (static, templates, etc.)
```

## Example Dockerfile
```dockerfile
FROM python:3.10-slim
WORKDIR /app
COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE 5000
CMD ["python", "app.py"]
```

## Example app.py
```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def home():
    return "Hello from Flask on AWS ECS!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

## License
MIT
