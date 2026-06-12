# E-Commerce AI Platform - FastAPI Version

Complete end-to-end machine learning platform for e-commerce analytics using FastAPI, React, and TensorFlow.

**[Quick Start](#quick-start) | [API Docs](#api-documentation) | [Architecture](#architecture) | [Algorithms](#10-ml-algorithms)**

---

## 🚀 Quick Start

### 1. Backend Setup

```bash
cd backend
python -m venv venv

# Windows
venv\Scripts\activate
# Mac/Linux
source venv/bin/activate

pip install -r requirements.txt
python -m app.main
```

Backend runs on: **http://localhost:8000**
API Docs: **http://localhost:8000/docs**

### 2. Frontend Setup

```bash
cd frontend
npm install
npm start
```

Frontend runs on: **http://localhost:3000**

---

## 📊 Architecture

```
Frontend (React + Tailwind CSS)
         ↓ (HTTP/REST)
         ↓
FastAPI Backend (8000)
         ├─ Data Pipeline
         │  ├─ K-Means Clustering
         │  └─ PCA Dimensionality Reduction
         │
         ├─ Classical ML
         │  ├─ Linear Regression (CLV)
         │  ├─ Logistic Regression (Churn)
         │  ├─ SVM (Fraud)
         │  ├─ Random Forest (Conversion)
         │  └─ XGBoost (Advanced)
         │
         └─ Deep Learning
            ├─ CNN (Image Analysis)
            ├─ LSTM (Sentiment)
            └─ MLP (Recommendations)
```

---

## 🧠 10 ML Algorithms

### Phase 1: Unsupervised Learning

#### 1. K-Means Clustering
- **Purpose**: Segment customers into 3 personas
- **Input**: Customer features (age, spending, engagement)
- **Output**: Cluster assignments + segment names
- **Endpoint**: `POST /api/segment`

#### 2. PCA (Principal Component Analysis)
- **Purpose**: Dimensionality reduction (200+ → 2D visualization)
- **Benefit**: Faster computation, prevent overfitting
- **Used in**: Visualizing segmentation results

### Phase 2: Classical Machine Learning

#### 3. Linear Regression
- **Purpose**: Predict Customer Lifetime Value (CLV)
- **Model**: y = w·x + b
- **Output**: Annual spending forecast
- **Endpoint**: `POST /api/predict/clv`

#### 4. Logistic Regression
- **Purpose**: Predict churn probability
- **Model**: Sigmoid function for 0-1 output
- **Output**: Probability customer will leave
- **Endpoint**: `POST /api/predict/churn`

#### 5. Support Vector Machine (SVM)
- **Purpose**: Fraud detection
- **Model**: RBF kernel for non-linear separation
- **Output**: Fraud risk score (0-1)
- **Endpoint**: `POST /api/predict/fraud`

#### 6. Random Forest
- **Purpose**: Predict conversion rate
- **Model**: Ensemble of 100 decision trees
- **Output**: Purchase probability
- **Endpoint**: `POST /api/predict/conversion`

#### 7. XGBoost
- **Purpose**: Advanced gradient boosting predictions
- **Model**: Sequential tree building with error correction
- **Output**: Flexible (regression or classification)
- **Used in**: Hybrid scoring

### Phase 3: Deep Learning

#### 8. CNN (Convolutional Neural Network)
- **Purpose**: Product image analysis
- **Architecture**: MobileNetV2 transfer learning
- **Output**: Image embeddings + product tags
- **Endpoint**: `POST /api/image/upload`

#### 9. LSTM (Long Short-Term Memory)
- **Purpose**: Review sentiment analysis
- **Architecture**: Stacked LSTM (64→32 units)
- **Output**: Sentiment label + confidence
- **Endpoint**: `POST /api/sentiment`

#### 10. MLP (Multi-Layer Perceptron)
- **Purpose**: Hybrid recommendations
- **Input**: CNN features + LSTM sentiment + history
- **Architecture**: 256→128→64 dense layers
- **Output**: Top-K product recommendations
- **Endpoint**: `POST /api/recommend`

---

## 📚 API Documentation

All endpoints fully documented with request/response examples:

**Swagger UI**: http://localhost:8000/docs
**ReDoc**: http://localhost:8000/redoc

### Key Endpoints

**Health Check**
```bash
GET /api/health
```

**Customer Segmentation**
```bash
POST /api/segment
Content-Type: application/json

{
  "customers": [
    {
      "age": 35,
      "total_spending": 2000,
      "browse_frequency": 60,
      "cart_abandonment": 0.2,
      "session_quality": 0.8
    }
  ]
}
```

**Predict CLV**
```bash
POST /api/predict/clv
Content-Type: application/json

{
  "age": 35,
  "total_spending": 2000,
  "browse_frequency": 60,
  "cart_abandonment": 0.2,
  "session_quality": 0.8
}
```

**Analyze Sentiment**
```bash
POST /api/sentiment
Content-Type: application/json

{
  "review_text": "This product is amazing!",
  "product_id": "P123"
}
```

---

## 🛠️ Technology Stack

### Backend
- **Framework**: FastAPI 0.110.0
- **Server**: Uvicorn 0.28.0
- **ML**: Scikit-Learn 1.4.1, XGBoost 2.0.3
- **DL**: TensorFlow 2.16.1, Keras
- **Data**: NumPy 1.26.4, Pandas 2.2.1

### Frontend
- **Framework**: React 18.2.0
- **Styling**: Tailwind CSS 3.3.6
- **Router**: React Router 6.18.0
- **HTTP**: Axios 1.6.0
- **Charts**: Recharts 2.10.0

### Infrastructure
- **API Format**: REST/JSON
- **CORS**: Enabled for localhost:3000
- **Documentation**: OpenAPI/Swagger

---

## 📁 Project Structure

```
ecommerce-ai-platform/
├── backend/
│   ├── app/
│   │   ├── __init__.py
│   │   ├── main.py              # FastAPI app
│   │   ├── routes.py            # API endpoints
│   │   ├── pipelines/
│   │   │   └── data_prep.py     # K-Means, PCA
│   │   └── models/
│   │       ├── classical_ml.py  # RF, SVM, LR, etc.
│   │       └── deep_learning.py # CNN, LSTM, MLP
│   ├── requirements.txt
│   └── setup.py
│
└── frontend/
    ├── src/
    │   ├── pages/
    │   │   ├── Dashboard.jsx
    │   │   ├── SegmentationPage.jsx
    │   │   ├── PredictionPage.jsx
    │   │   ├── RecommendationPage.jsx
    │   │   └── SentimentPage.jsx
    │   ├── services/
    │   │   └── api.js
    │   ├── App.jsx
    │   └── index.js
    ├── package.json
    └── tailwind.config.js
```

---

## 🔧 Configuration

### Change Backend Port
Edit `backend/app/main.py`:
```python
uvicorn.run("app.main:app", host="0.0.0.0", port=9000)  # Change from 8000
```

### Change Frontend API URL
Create `.env` in frontend:
```
REACT_APP_API_URL=http://your-backend:8000/api
```

### K-Means Clusters
Modify clustering in `backend/app/pipelines/data_prep.py`:
```python
def __init__(self, n_clusters=5):  # Change from 3
    self.kmeans = KMeans(n_clusters=n_clusters, random_state=42)
```

---

## 📊 Performance Tips

1. **Batch Processing**: Process 100+ records at once for better throughput
2. **GPU Acceleration**: Use TensorFlow GPU for CNN/LSTM inference
3. **Caching**: Cache model predictions for repeated requests
4. **Database**: Add indexes on frequently queried columns

---

## 🚀 Deployment

### Deploy to Heroku

```bash
heroku create your-app-name
heroku config:set FLASK_ENV=production
git push heroku main
```

### Deploy to AWS

```bash
# Create ECS task definition
# Push to ECR
# Deploy with ECS Fargate
```

### Deploy with Docker

```bash
docker build -t my-app .
docker run -p 8000:8000 my-app
```

---

## 📖 Code Comments

Every algorithm has detailed comments explaining:
- **Purpose**: What the model does
- **Input/Output**: Data shapes and formats
- **Algorithm**: How it works mathematically
- **Use Cases**: Real-world applications
- **Parameters**: Tunable hyperparameters

Example:
```python
def train(self, X_train: np.ndarray, y_train: np.ndarray):
    """
    Train the predictor.
    
    Args:
        X_train: Feature matrix [n_samples, n_features]
        y_train: Target values [n_samples]
    
    Process:
        1. StandardScaler normalizes features
        2. LogisticRegression learns decision boundary
        3. Stores model weights for inference
    """
```

---

## 🎓 Learning Outcomes

After building this project, you'll understand:

✓ **Data Pipeline**: Loading, cleaning, scaling, transforming data
✓ **Unsupervised Learning**: Clustering (K-Means), dimensionality reduction (PCA)
✓ **Supervised Learning**: Classification and regression with classical ML
✓ **Ensemble Methods**: Random Forests, XGBoost for better accuracy
✓ **Deep Learning**: Neural networks (CNN, LSTM, MLP) with Keras/TensorFlow
✓ **Transfer Learning**: Using pre-trained models (MobileNetV2)
✓ **API Development**: Building REST APIs with FastAPI
✓ **Full-Stack Integration**: Connecting React frontend to Python backend
✓ **Production Deployment**: Containerization and cloud deployment

---

## 📝 License

MIT License - Feel free to use for personal and commercial projects

---

## 🤝 Contributing

Contributions welcome! Please fork and submit pull requests.

---

**Start exploring**: Visit http://localhost:3000 after starting both servers! 🚀
