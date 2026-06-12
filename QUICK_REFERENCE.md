# Quick Reference Guide

Fast lookup for common tasks and commands.

---

## 🚀 Quick Start (5 Minutes)

### Terminal 1: Start Backend
```bash
cd backend
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt
python -m app.main
# Backend ready: http://localhost:8000
```

### Terminal 2: Start Frontend
```bash
cd frontend
npm install
npm start
# Frontend ready: http://localhost:3000
```

---

## 📍 URLs Reference

| Service | URL | Purpose |
|---------|-----|---------|
| Frontend | http://localhost:3000 | Main dashboard |
| Backend | http://localhost:8000 | API server |
| Swagger UI | http://localhost:8000/docs | API testing |
| ReDoc | http://localhost:8000/redoc | API documentation |

---

## 🔧 Backend Commands

```bash
# Setup
cd backend
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# Run
python -m app.main

# Install new package
pip install package-name
pip freeze > requirements.txt

# Test specific endpoint
curl -X GET http://localhost:8000/api/health
```

---

## ⚛️ Frontend Commands

```bash
# Setup
cd frontend
npm install

# Run development
npm start

# Build for production
npm run build

# Install new package
npm install package-name
npm install -D package-name  # dev dependency

# Update packages
npm update
npm audit fix
```

---

## 📊 API Endpoints Cheat Sheet

### Health & Info
```
GET  /api/health
GET  /api/models
GET  /api/info
GET  /demo/mock-predictions
```

### Segmentation
```
POST /api/segment
Body: {"customers": [{...}]}
```

### Predictions
```
POST /api/predict/clv
POST /api/predict/churn
POST /api/predict/fraud
POST /api/predict/conversion
Body: {"age": 35, "total_spending": 2000, ...}
```

### Deep Learning
```
POST /api/sentiment
POST /api/sentiment/batch
POST /api/recommend
POST /api/image/upload
```

---

## 🧠 ML Algorithms Quick Reference

| Algorithm | Type | Input | Output | Endpoint |
|-----------|------|-------|--------|----------|
| K-Means | Clustering | Customer features | Cluster ID | /api/segment |
| PCA | Reduction | Features | 2D projection | /api/segment |
| Linear Reg | Regression | Customer features | CLV ($) | /api/predict/clv |
| Logistic Reg | Classification | Customer features | Churn prob | /api/predict/churn |
| SVM | Classification | Customer features | Fraud score | /api/predict/fraud |
| Random Forest | Ensemble | Customer features | Conversion prob | /api/predict/conversion |
| XGBoost | Boosting | Customer features | Scores | Internal |
| CNN | Image | Product image | Embeddings | /api/image/upload |
| LSTM | NLP | Review text | Sentiment | /api/sentiment |
| MLP | Recommendation | Combined features | Top-K products | /api/recommend |

---

## 📁 Key Files

| Path | Purpose |
|------|---------|
| `backend/app/main.py` | FastAPI entry point |
| `backend/app/routes.py` | API endpoints |
| `backend/app/pipelines/data_prep.py` | K-Means, PCA |
| `backend/app/models/classical_ml.py` | RF, SVM, LR, etc. |
| `backend/app/models/deep_learning.py` | CNN, LSTM, MLP |
| `frontend/src/App.jsx` | React main app |
| `frontend/src/services/api.js` | API client |
| `frontend/src/pages/*.jsx` | Feature pages |

---

## 🐛 Troubleshooting

### Port Already in Use
```bash
# Windows
netstat -ano | findstr :8000
taskkill /PID <PID> /F

# macOS/Linux
lsof -i :8000
kill -9 <PID>
```

### Module Not Found (Python)
```bash
pip install -r requirements.txt
pip install package-name
```

### Package Not Found (Node)
```bash
npm install
npm install package-name
```

### CORS Error
```bash
# Check backend is running
curl http://localhost:8000/api/health
# Check frontend .env has correct API URL
```

### Frontend Can't Connect
```bash
# 1. Start backend first
# 2. Check http://localhost:8000/docs loads
# 3. Check browser console for errors
# 4. Try: curl http://localhost:8000/api/health
```

---

## 🔑 Configuration

### Backend Port
```python
# backend/app/main.py
uvicorn.run("app.main:app", host="0.0.0.0", port=9000)
```

### Frontend API URL
```
# frontend/.env
REACT_APP_API_URL=http://localhost:8000/api
```

### K-Means Clusters
```python
# backend/app/pipelines/data_prep.py
def __init__(self, n_clusters=5):  # Change from 3
```

### Backend Debug
```python
# backend/app/main.py
app = FastAPI(docs_url="/docs", debug=True)
```

---

## 🚀 Deployment

### Push to GitHub
```bash
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/user/repo.git
git branch -M main
git push -u origin main
```

### Deploy with Docker
```bash
docker build -t my-app .
docker run -p 8000:8000 my-app
```

### Deploy to Heroku
```bash
heroku login
heroku create app-name
git push heroku main
```

---

## 📚 Documentation Files

- `README.md` - Main overview
- `INSTALLATION.md` - Setup guide
- `GITHUB_DEPLOYMENT.md` - GitHub & deployment
- `QUICK_REFERENCE.md` - This file

---

## 💾 Git Workflow

```bash
# Create new feature
git checkout -b feature/my-feature

# Make changes
git add .
git commit -m "Add feature"

# Push
git push origin feature/my-feature

# Create Pull Request via GitHub

# Merge when done
git checkout main
git pull origin main
git merge feature/my-feature
git push origin main
```

---

## ⚡ Performance Tips

1. Use `.env` for configuration
2. Cache API responses in frontend
3. Use GPU for CNN/LSTM inference
4. Batch process predictions
5. Monitor memory usage
6. Add database indexing

---

## 📞 Common Issues

| Issue | Solution |
|-------|----------|
| Port in use | Kill process using port |
| Module not found | `pip install -r requirements.txt` |
| CORS error | Check backend/frontend URLs |
| Slow startup | TensorFlow takes time to load |
| OOM error | Reduce batch size or increase RAM |

---

## 🎯 Next Steps

1. **Explore**: Visit http://localhost:3000
2. **Test**: Try each page and prediction
3. **Read**: Check inline code comments
4. **Deploy**: Follow GITHUB_DEPLOYMENT.md
5. **Learn**: Understand each algorithm

---

**Happy hacking!** 🚀
