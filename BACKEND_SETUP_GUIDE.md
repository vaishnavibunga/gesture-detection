# BACKEND SETUP GUIDE

## Quick Start - Flask Backend API Setup

### Step 1: Install Required Dependencies

```bash
# Navigate to your yolov5 directory
cd "C:\Users\admin\projects\gesture language\yolov5"

# Install Flask and CORS
pip install flask flask-cors torch torchvision
```

### Step 2: Required Dependencies Already Installed
From your requirements.txt, you already have:
- torch
- torchvision
- opencv-python
- numpy
- pillow
- etc.

Only need to add:
```bash
pip install flask flask-cors
```

### Step 3: Run the Backend API

```bash
# From the yolov5 directory
python backend_api.py

# Output should show:
# INFO:__main__:Loading YOLOv5 model...
# INFO:__main__:Model loaded successfully
# INFO:__main__:Starting Flask API server...
# WARNING: This is a development server. Do not use in production.
# Running on http://0.0.0.0:5000
```

### Step 4: Test the API

#### Health Check
```bash
curl http://localhost:5000/health
```

Expected response:
```json
{
  "status": "healthy",
  "timestamp": "2026-06-10T10:30:00.123456",
  "model_loaded": true
}
```

#### Get API Info
```bash
curl http://localhost:5000/info
```

#### Test Prediction
```bash
# Upload an image and test
curl -X POST -F "image=@path/to/image.jpg" http://localhost:5000/predict
```

### Step 5: Configure Frontend to Connect

In your Lovable web app, set the API Base URL to:
```
http://localhost:5000
```

---

## API ENDPOINTS SUMMARY

### 1. Health Check
```
GET /health
Response: { "status": "healthy", "model_loaded": true }
```

### 2. API Information
```
GET /info
Response: Model details, supported formats, endpoints
```

### 3. Single Image Prediction
```
POST /predict
Input: Image file (multipart/form-data) or base64 JSON
Response:
{
  "success": true,
  "detections": [
    {
      "class_id": 0,
      "class_name": "Peace",
      "confidence": 0.95,
      "bbox": {
        "x1": 100, "y1": 50, "x2": 200, "y2": 150,
        "width": 100, "height": 100,
        "center_x": 150, "center_y": 100
      }
    }
  ],
  "detection_count": 1,
  "image": "base64_string_if_requested",
  "message": "Found 1 gesture(s)"
}
```

### 4. Batch Processing
```
POST /batch-predict
Input: { "images": ["base64_image_1", "base64_image_2", ...] }
Response:
{
  "success": true,
  "total_images": 2,
  "results": [
    { "image_index": 0, "success": true, "detections": [...] },
    { "image_index": 1, "success": true, "detections": [...] }
  ]
}
```

---

## TROUBLESHOOTING

### Issue: Model not found error
**Solution**: Ensure `best.pt` is in the yolov5 directory

### Issue: CUDA/GPU errors
**Solution**: Backend API defaults to CPU. If you want GPU:
Edit line in backend_api.py:
```python
# Add device selection
model = torch.hub.load('ultralytics/yolov5', 'custom', path='best.pt', force_reload=False)
model = model.to('cuda')  # Use GPU if available
```

### Issue: Port 5000 already in use
**Solution**: Change port in backend_api.py:
```python
app.run(host='0.0.0.0', port=8000)  # Change to different port
```

### Issue: CORS errors in web app
**Solution**: Already handled in backend_api.py with `CORS(app)`

### Issue: Slow inference time
**Solution**: 
1. Use GPU instead of CPU
2. Reduce image size
3. Use smaller model (yolov5n instead of yolov5s)

---

## PRODUCTION DEPLOYMENT

### For production, modify backend_api.py:

```python
if __name__ == '__main__':
    if load_model():
        logger.info("Starting Flask API server...")
        app.run(
            host='0.0.0.0',
            port=5000,
            debug=False,  # ← Change from False (already correct)
            use_reloader=False
        )
```

### Better: Use Gunicorn for production
```bash
pip install gunicorn

# Run with Gunicorn (4 worker processes)
gunicorn -w 4 -b 0.0.0.0:5000 backend_api:app
```

### Or use Docker
Create `Dockerfile`:
```dockerfile
FROM python:3.8-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt flask flask-cors
COPY . .
CMD ["python", "backend_api.py"]
```

Build and run:
```bash
docker build -t gesture-detection .
docker run -p 5000:5000 gesture-detection
```

---

## KEY PARAMETERS YOU CAN ADJUST

In backend_api.py:

```python
# Adjust these based on your needs:
MAX_FILE_SIZE = 10 * 1024 * 1024  # 10MB max upload
ALLOWED_EXTENSIONS = {'jpg', 'jpeg', 'png', 'bmp', 'gif', 'webp'}
CONFIDENCE_THRESHOLD = 0.25  # Min confidence for detections
IOU_THRESHOLD = 0.45  # Non-max suppression threshold

# Model parameters (in your train.py):
# imgsz = 640  # Input image size
# conf_thres = 0.25  # Confidence threshold
# iou_thres = 0.45  # IOU threshold for NMS
```

---

## WORKFLOW TO RUN EVERYTHING

1. **Terminal 1: Start Backend API**
   ```bash
   cd "C:\Users\admin\projects\gesture language\yolov5"
   python backend_api.py
   ```
   
2. **Terminal 2: Open Lovable Web App** (or local dev server)
   - Create your web app with Lovable
   - Set API URL: `http://localhost:5000`
   - Run your frontend locally or on Lovable platform

3. **Test**: Upload image in web app, see detection results

---

## FILE STRUCTURE AFTER SETUP

```
yolov5/
├── backend_api.py          ← NEW: Production backend
├── app.py                  ← OLD: Simple Flask app (not used)
├── best.pt                 ← Your trained model
├── requirements.txt        ← Your dependencies
├── detect.py
├── train.py
└── ... (other YOLOv5 files)
```

---

## IMPORTANT NOTES

✅ Backend API is production-ready with:
- Proper error handling
- CORS support
- Structured JSON responses
- Image validation
- File size limits
- Batch processing capability

✅ Your Lovable web app will:
- Connect to this API automatically
- Display results beautifully
- Handle all user interactions
- Manage uploads and downloads

✅ Everything is configured to work together perfectly!

**Once both are running, your gesture detection system is complete!**
