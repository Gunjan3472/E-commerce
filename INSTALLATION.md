# Installation & Setup Guide

Complete step-by-step guide for setting up the E-Commerce AI Platform locally.

---

## Prerequisites

### System Requirements
- **OS**: Windows, macOS, or Linux
- **RAM**: 8GB minimum (16GB recommended)
- **Storage**: 5GB free space
- **Internet**: For downloading packages

### Required Software

- **Python 3.9+**: https://www.python.org/downloads/
- **Node.js 18+**: https://nodejs.org/
- **Git**: https://git-scm.com/
- **VS Code** (optional): https://code.visualstudio.com/

---

## Step 1: Clone Repository

```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/ecommerce-ai-platform-fastapi.git
cd ecommerce-ai-platform-fastapi
```

---

## Step 2: Backend Setup

### 2.1 Create Virtual Environment

**Windows:**
```bash
cd backend
python -m venv venv
venv\Scripts\activate
```

**macOS/Linux:**
```bash
cd backend
python3 -m venv venv
source venv/bin/activate
```

### 2.2 Install Dependencies

```bash
pip install -r requirements.txt
```

This installs:
- `fastapi` - Web framework
- `uvicorn` - ASGI server
- `numpy`, `pandas` - Data processing
- `scikit-learn` - ML algorithms
- `tensorflow` - Deep learning
- `xgboost` - Gradient boosting
- And more...

### 2.3 Verify Installation

```bash
python -c "import fastapi; import tensorflow; import sklearn; print('✓ All packages installed')"
```

### 2.4 Start Backend Server

```bash
python -m app.main
```

Expected output:
```
==============================
🚀 E-Commerce AI Platform Starting Up
==============================
✓ Data pipeline initialized
✓ Classical ML models loaded
✓ Deep learning models loaded
✓ API routes registered
==============================
📚 API Documentation: http://localhost:8000/docs
==============================
```

**Backend is ready at**: http://localhost:8000

---

## Step 3: Frontend Setup

### 3.1 Install Node Modules

**In a new terminal**, navigate to frontend:

```bash
cd frontend
npm install
```

This installs:
- `react` - UI framework
- `react-router-dom` - Navigation
- `tailwindcss` - Styling
- `axios` - HTTP client
- `recharts` - Data visualization
- And more...

### 3.2 Verify Installation

```bash
npm list react react-dom react-router-dom
```

### 3.3 Start Development Server

```bash
npm start
```

Expected output:
```
Compiled successfully!

You can now view ecommerce-ai-platform in the browser.

  Local:            http://localhost:3000
  On Your Network:  http://192.168.x.x:3000
```

**Frontend is ready at**: http://localhost:3000

---

## Step 4: Verify Everything Works

### 4.1 Check Backend Health

```bash
# In a terminal or browser
curl http://localhost:8000/api/health
```

Response:
```json
{
  "status": "healthy",
  "models_loaded": {
    "data_pipeline": true,
    "predictive_engine": true,
    "deep_learning": true
  }
}
```

### 4.2 Test API Endpoints

```bash
# Get mock predictions
curl http://localhost:8000/api/demo/mock-predictions

# Or access Swagger UI
# http://localhost:8000/docs
```

### 4.3 Check Frontend

1. Open browser: http://localhost:3000
2. You should see the E-Commerce AI Platform dashboard
3. Click through pages to verify they load
4. Try clicking "Run K-Means Clustering" or other buttons

---

## Step 5: Test ML Algorithms

### 5.1 Use Swagger UI

1. Open: http://localhost:8000/docs
2. Click on any endpoint (e.g., `/api/segment`)
3. Click "Try it out"
4. Click "Execute"
5. View response

### 5.2 Test via Frontend UI

1. Go to http://localhost:3000/segmentation
2. Click "Run K-Means Clustering"
3. View segmentation visualization
4. Go to http://localhost:3000/prediction
5. Click "Run All Predictions"
6. View results

### 5.3 Test via curl

```bash
# Test segmentation
curl -X POST http://localhost:8000/api/segment \
  -H "Content-Type: application/json" \
  -d '{
    "customers": [
      {
        "age": 35,
        "total_spending": 2000,
        "browse_frequency": 60,
        "cart_abandonment": 0.2,
        "session_quality": 0.8
      }
    ]
  }'

# Test CLV prediction
curl -X POST http://localhost:8000/api/predict/clv \
  -H "Content-Type: application/json" \
  -d '{
    "age": 35,
    "total_spending": 2000,
    "browse_frequency": 60,
    "cart_abandonment": 0.2,
    "session_quality": 0.8
  }'

# Test sentiment analysis
curl -X POST http://localhost:8000/api/sentiment \
  -H "Content-Type: application/json" \
  -d '{
    "review_text": "This product is amazing!",
    "product_id": "P123"
  }'
```

---

## Troubleshooting

### Backend Issues

**Error**: `ModuleNotFoundError: No module named 'fastapi'`
```bash
# Solution: Reinstall packages
pip install -r requirements.txt
```

**Error**: `Address already in use :8000`
```bash
# Solution: Find and kill process
# Windows
netstat -ano | findstr :8000
taskkill /PID <PID> /F

# macOS/Linux
lsof -i :8000
kill -9 <PID>
```

**Error**: `Python version too old`
```bash
# Solution: Install Python 3.9+
python --version
# Update from https://www.python.org/
```

**Error**: TensorFlow issues on M1/M2 Mac
```bash
# Solution: Use native TensorFlow
pip install tensorflow-macos
```

### Frontend Issues

**Error**: `npm: command not found`
```bash
# Solution: Install Node.js
# https://nodejs.org/ → Download and install
```

**Error**: Port 3000 already in use
```bash
# Solution: Use different port
PORT=3001 npm start
# Or kill process using port 3000
# Windows: netstat -ano | findstr :3000
```

**Error**: CORS error in browser console
```bash
# Solution: Backend must be running first
# Check http://localhost:8000/docs is accessible
```

### Connection Issues

**Frontend can't reach backend**
```bash
# 1. Check both servers are running
# 2. Check API URL: http://localhost:8000/api
# 3. Check firewall isn't blocking
# 4. Try directly: curl http://localhost:8000/api/health
```

**All endpoints return 404**
```bash
# Check backend is actually running
# Look for "API Documentation: http://localhost:8000/docs" in console
```

---

## File Structure Verification

After setup, verify these files exist:

```
✓ backend/app/main.py
✓ backend/app/routes.py
✓ backend/app/pipelines/data_prep.py
✓ backend/app/models/classical_ml.py
✓ backend/app/models/deep_learning.py
✓ backend/requirements.txt
✓ frontend/src/App.jsx
✓ frontend/src/pages/Dashboard.jsx
✓ frontend/src/services/api.js
✓ frontend/package.json
✓ README.md
✓ GITHUB_DEPLOYMENT.md
```

---

## Development Workflow

### Making Changes

**Backend:**
1. Edit Python files in `backend/app/`
2. Server automatically reloads (if using `--reload`)
3. Test at http://localhost:8000/docs

**Frontend:**
1. Edit files in `frontend/src/`
2. Browser automatically reloads
3. Check browser console for errors

### Running Linting

```bash
# Backend
cd backend
pylint app/

# Frontend
cd frontend
npm run lint
```

### Building for Production

**Frontend:**
```bash
cd frontend
npm run build
# Output in `build/` directory
```

---

## Environment Variables

Create `backend/.env`:
```
UVICORN_HOST=0.0.0.0
UVICORN_PORT=8000
DEBUG=True
```

Create `frontend/.env`:
```
REACT_APP_API_URL=http://localhost:8000/api
```

---

## Performance Optimization

### Speed Up Frontend
```bash
# Use production build
npm run build
npm run serve
```

### Speed Up Backend
```bash
# Use fewer threads if RAM-limited
export OMP_NUM_THREADS=2
python -m app.main
```

### Monitor Resources
```bash
# Windows
tasklist /fi "name eq python*"

# macOS/Linux
top
ps aux | grep python
```

---

## Next Steps

1. ✅ Backend running at http://localhost:8000
2. ✅ Frontend running at http://localhost:3000
3. ✅ All tests passing
4. **Next**: Push to GitHub (see GITHUB_DEPLOYMENT.md)
5. **Then**: Deploy to production (Heroku, AWS, etc.)

---

## Quick Reference

### Startup Commands

Terminal 1 (Backend):
```bash
cd backend
source venv/bin/activate  # macOS/Linux
python -m app.main
```

Terminal 2 (Frontend):
```bash
cd frontend
npm start
```

### Access Points

- **Frontend**: http://localhost:3000
- **Backend API**: http://localhost:8000
- **API Docs**: http://localhost:8000/docs
- **ReDoc**: http://localhost:8000/redoc

### Stop Services

```bash
# Terminal 1 & 2: Ctrl + C
# Or:
pkill -f "uvicorn\|npm"
```

---

**Setup complete!** 🎉

Start exploring the platform at http://localhost:3000
