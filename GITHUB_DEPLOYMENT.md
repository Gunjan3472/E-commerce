# GitHub Deployment Guide

Complete guide for deploying the E-Commerce AI Platform to GitHub and production.

---

## Phase 1: GitHub Repository Setup

### Step 1: Create GitHub Repository

1. Go to https://github.com/new
2. Create repository: `ecommerce-ai-platform-fastapi`
3. Do NOT initialize with README (we have one)
4. Copy your repository URL

### Step 2: Initialize Git Locally

```bash
cd ecommerce-ai-platform-fastapi

# Initialize git
git init

# Add all files
git add .

# Create initial commit
git commit -m "Initial commit: E-Commerce AI Platform with FastAPI, React, and 10 ML Algorithms"

# Add remote
git remote add origin https://github.com/YOUR_USERNAME/ecommerce-ai-platform-fastapi.git

# Rename branch to main
git branch -M main

# Push to GitHub
git push -u origin main
```

### Step 3: Verify Repository

- Visit: https://github.com/YOUR_USERNAME/ecommerce-ai-platform-fastapi
- Confirm all files are there
- Check README renders correctly

---

## Phase 2: GitHub Actions CI/CD (Optional)

### Create `.github/workflows/deploy.yml`

```yaml
name: Build and Deploy

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  test-backend:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Set up Python
        uses: actions/setup-python@v2
        with:
          python-version: 3.9
      - name: Install dependencies
        run: |
          cd backend
          pip install -r requirements.txt
      - name: Run linting
        run: |
          cd backend
          pip install pylint
          pylint app/
        continue-on-error: true

  test-frontend:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Use Node.js
        uses: actions/setup-node@v2
        with:
          node-version: 18
      - name: Install dependencies
        run: |
          cd frontend
          npm install
      - name: Run tests
        run: |
          cd frontend
          npm test -- --passWithNoTests
```

---

## Phase 3: Deployment Options

### Option A: Heroku Deployment

#### Prerequisites
- Heroku account (https://heroku.com)
- Heroku CLI installed

#### Backend Deployment

```bash
# Login to Heroku
heroku login

# Create app
heroku create your-app-name-backend

# Set environment variables
heroku config:set DEBUG=False UVICORN_PORT=5000

# Deploy
git push heroku main
```

#### Frontend Deployment (Vercel)

```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
cd frontend
vercel
```

### Option B: AWS Deployment

#### Create EC2 Instance

```bash
# Launch t2.micro instance
# SSH into instance
ssh -i your-key.pem ec2-user@your-instance.com

# Install Python and Node
sudo yum update
sudo yum install python3 nodejs

# Clone repository
git clone https://github.com/YOUR_USERNAME/ecommerce-ai-platform-fastapi.git
cd ecommerce-ai-platform-fastapi

# Setup backend
cd backend
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
# Run with systemd
```

#### Create Systemd Service

```bash
# Create /etc/systemd/system/fastapi.service
[Unit]
Description=E-Commerce AI Platform Backend
After=network.target

[Service]
Type=notify
User=ec2-user
WorkingDirectory=/home/ec2-user/ecommerce-ai-platform-fastapi/backend
ExecStart=/home/ec2-user/ecommerce-ai-platform-fastapi/backend/venv/bin/python -m uvicorn app.main:app --host 0.0.0.0 --port 8000

[Install]
WantedBy=multi-user.target

# Start service
sudo systemctl start fastapi
sudo systemctl enable fastapi
```

### Option C: Docker Compose

Create `docker-compose.yml`:

```yaml
version: '3.8'

services:
  backend:
    build:
      context: ./backend
      dockerfile: Dockerfile
    ports:
      - "8000:8000"
    environment:
      - UVICORN_HOST=0.0.0.0
      - UVICORN_PORT=8000
    volumes:
      - ./backend:/app

  frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile
    ports:
      - "3000:3000"
    environment:
      - REACT_APP_API_URL=http://localhost:8000/api
    depends_on:
      - backend
```

Create `backend/Dockerfile`:

```dockerfile
FROM python:3.9-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .

CMD ["python", "-m", "uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

Create `frontend/Dockerfile`:

```dockerfile
FROM node:18 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

FROM node:18
WORKDIR /app
RUN npm install -g serve
COPY --from=builder /app/build ./build
CMD ["serve", "-s", "build", "-l", "3000"]
```

Run with:
```bash
docker-compose up
```

### Option D: DigitalOcean

1. Create Droplet (Ubuntu 22.04)
2. SSH into droplet
3. Install dependencies and clone repo
4. Setup Nginx as reverse proxy
5. Use Certbot for HTTPS

---

## Phase 4: Production Checklist

- [ ] GitHub repository created and pushed
- [ ] README.md is complete and renders
- [ ] .gitignore excludes unnecessary files
- [ ] Environment variables documented
- [ ] Database backups configured
- [ ] Monitoring and logging set up
- [ ] SSL/HTTPS certificate installed
- [ ] Rate limiting configured
- [ ] Error tracking (Sentry) enabled
- [ ] Performance monitoring active
- [ ] API keys and secrets not committed
- [ ] Frontend optimization (build minification)
- [ ] Backend error handling comprehensive
- [ ] Documentation complete

---

## Phase 5: Continuous Integration

### GitHub Secrets for CI/CD

Add to Settings → Secrets:

```
AWS_ACCESS_KEY_ID=xxx
AWS_SECRET_ACCESS_KEY=xxx
HEROKU_API_KEY=xxx
DOCKER_HUB_USERNAME=xxx
DOCKER_HUB_PASSWORD=xxx
```

Use in workflows:

```yaml
- name: Deploy to AWS
  env:
    AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
    AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
  run: |
    # deployment commands
```

---

## Phase 6: Monitoring & Maintenance

### Setup Error Tracking

```python
# In backend/app/main.py
import sentry_sdk
from sentry_sdk.integrations.fastapi import FastApiIntegration

sentry_sdk.init(
    dsn="https://your-sentry-dsn@sentry.io/project-id",
    integrations=[FastApiIntegration()],
    traces_sample_rate=1.0
)
```

### Setup Logging

```python
import logging

logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
    handlers=[
        logging.FileHandler('app.log'),
        logging.StreamHandler()
    ]
)
```

---

## Phase 7: Security Hardening

### 1. Environment Variables
```bash
# Never commit .env file
echo ".env" >> .gitignore
# Use: export KEY=value in production
```

### 2. CORS Configuration
```python
# Allow specific origins only
app.add_middleware(
    CORSMiddleware,
    allow_origins=["https://yourdomain.com"],
    allow_credentials=True,
    allow_methods=["GET", "POST"],
    allow_headers=["*"],
)
```

### 3. Rate Limiting
```bash
pip install slowapi

from slowapi import Limiter
limiter = Limiter(key_func=get_remote_address)
app.state.limiter = limiter

@app.get("/api/predict")
@limiter.limit("100/minute")
def predict(request: Request):
    pass
```

### 4. Secrets Management
```bash
# Use environment variables
import os
SECRET_KEY = os.getenv("SECRET_KEY")

# Or use AWS Secrets Manager
import boto3
client = boto3.client('secretsmanager')
secret = client.get_secret_value(SecretId='my-secret')
```

---

## Troubleshooting

### Port 8000 Already in Use
```bash
lsof -i :8000
kill -9 <PID>
```

### Frontend Can't Connect to Backend
- Check backend is running
- Check CORS settings
- Check firewall rules
- Verify API URL in `.env`

### Git Push Rejected
```bash
# Force push (use carefully!)
git push -u origin main --force
```

### Database Connection Error
```bash
# Check database is running
# Check connection string in .env
# Verify credentials
```

---

## Quick Commands Reference

```bash
# Clone repository
git clone https://github.com/your-username/ecommerce-ai-platform-fastapi.git

# Pull latest changes
git pull origin main

# Create new branch
git checkout -b feature/my-feature

# Commit changes
git add .
git commit -m "Add new feature"
git push origin feature/my-feature

# Create Pull Request
# Via GitHub web interface

# Merge PR
git checkout main
git pull origin main
git merge feature/my-feature
git push origin main

# Delete branch
git branch -d feature/my-feature
git push origin --delete feature/my-feature
```

---

**Deployment successful!** 🚀

Your E-Commerce AI Platform is now live and accessible to the world.
