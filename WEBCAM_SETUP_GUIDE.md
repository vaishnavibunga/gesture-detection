# LIVE WEBCAM DETECTION - SETUP GUIDE

## Complete Guide to Real-Time Gesture Detection

---

## 🎥 WHAT YOU GET

A complete real-time webcam-based gesture detection system that:
- ✅ Streams live video from your webcam
- ✅ Detects gestures in real-time
- ✅ Shows bounding boxes on detected gestures
- ✅ Displays FPS and inference time
- ✅ Updates detections 30+ times per second
- ✅ Works on any camera/webcam device

---

## 📁 FILES PROVIDED

### 1. **webcam_api.py** - Streaming Backend
- **Location**: `yolov5/webcam_api.py`
- **Purpose**: Real-time video streaming and detection
- **Features**:
  - MJPEG video streaming
  - Real-time YOLOv5 detection
  - Performance monitoring (FPS, inference time)
  - Built-in HTML dashboard for testing
  - RESTful API endpoints

### 2. **LOVABLE_WEBCAM_PROMPT.txt** - Frontend Prompt
- **Location**: `LOVABLE_WEBCAM_PROMPT.txt`
- **Purpose**: Complete prompt for Lovable.ai
- **Includes**: Live webcam UI + image analysis

### 3. **WEBCAM_SETUP_GUIDE.md** - This Guide
- Complete setup instructions
- Troubleshooting tips
- API reference

---

## 🚀 QUICK START

### Step 1: Install Dependencies (1 minute)
```bash
cd "C:\Users\admin\projects\gesture language\yolov5"
pip install flask flask-cors opencv-python torch torchvision
```

### Step 2: Run Webcam Server (30 seconds)
```bash
python webcam_api.py
```

**You should see:**
```
Loading YOLOv5 model...
Model loaded successfully
Initializing webcam...
Webcam initialized successfully
Starting Webcam Streaming Server...
Access dashboard at http://localhost:5001/dashboard
```

### Step 3: Test the Webcam (Optional)
Open browser: `http://localhost:5001/dashboard`
- Click **"Start Webcam"**
- You should see your live feed
- Perform gestures to see detection
- Click **"Stop Webcam"** when done

### Step 4: Generate Frontend with Lovable (10 minutes)
1. Go to **lovable.ai**
2. Create new project
3. Copy entire content of `LOVABLE_WEBCAM_PROMPT.txt`
4. Paste into Lovable prompt
5. Click "Generate"
6. In app settings, set API URL to `http://localhost:5000` for images
7. Set Webcam API URL to `http://localhost:5001` for live stream

**Done! Live gesture detection ready! 🎉**

---

## 📊 SYSTEM ARCHITECTURE

```
┌──────────────────────────────┐
│      BROWSER / LOVABLE       │
│                              │
│  ┌──────────────────────┐   │
│  │  Webcam Tab          │   │
│  │  - Live video stream │   │
│  │  - Overlay boxes     │   │
│  │  - Detection results │   │
│  └──────────────────────┘   │
│                              │
│  ┌──────────────────────┐   │
│  │  Image Tab           │   │
│  │  - Upload images     │   │
│  │  - Detect            │   │
│  └──────────────────────┘   │
└────────┬─────────────────────┘
         │ HTTP API Calls
         │
    ┌────▼──────────────────┐
    │ WEBCAM STREAMING API  │
    │ (Port 5001)           │
    │                       │
    │ /webcam/stream        │
    │ /webcam/start         │
    │ /webcam/stop          │
    │ /webcam/detections    │
    │ /webcam/capture       │
    └────┬──────────────────┘
         │ OpenCV
         │
    ┌────▼──────────────────┐
    │  YOLOv5 MODEL         │
    │  (best.pt)            │
    │                       │
    │  Detects:             │
    │  - Hand gestures      │
    │  - Sign language      │
    │  - Poses              │
    └────┬──────────────────┘
         │
    ┌────▼──────────────────┐
    │  WEBCAM / CAMERA      │
    │                       │
    │  Resolution: 640x480  │
    │  FPS: 30              │
    └───────────────────────┘
```

---

## 🔗 API ENDPOINTS

### Webcam Streaming

#### 1. **Start Webcam**
```
POST /webcam/start

Response:
{
  "success": true,
  "message": "Webcam started",
  "stream_url": "/webcam/stream"
}
```

#### 2. **Get Video Stream**
```
GET /webcam/stream

Returns: MJPEG video stream
- Use in <img> tag: <img src="http://localhost:5001/webcam/stream">
- Or in <video> tag (some browsers)
- Resolution: 640x480
- Frame rate: 30 FPS
```

#### 3. **Get Current Detections**
```
GET /webcam/detections

Response:
{
  "success": true,
  "detections": [
    {
      "class_id": 0,
      "class_name": "Peace Sign",
      "confidence": 0.95,
      "bbox": {
        "x1": 100, "y1": 50,
        "x2": 200, "y2": 150
      }
    }
  ],
  "detection_count": 1,
  "inference_time": 45.2,
  "fps": 29.8,
  "timestamp": "2024-06-10T10:30:00.123456"
}
```

#### 4. **Capture Current Frame**
```
POST /webcam/capture

Returns: JPEG image of current frame
```

#### 5. **Stop Webcam**
```
POST /webcam/stop

Response:
{
  "success": true,
  "message": "Webcam stopped"
}
```

#### 6. **Get/Update Configuration**
```
GET /webcam/config
POST /webcam/config

POST Body:
{
  "confidence_threshold": 0.25,
  "iou_threshold": 0.45
}
```

### Health Check
```
GET /health

Response:
{
  "status": "healthy",
  "model_loaded": true,
  "webcam_active": true
}
```

---

## 🎮 USING THE DASHBOARD

### Built-In Test Dashboard

**URL**: `http://localhost:5001/dashboard`

#### Features:
- Live webcam preview
- Real-time detection stats
- FPS counter
- Inference time
- Detection list
- Control buttons

#### Controls:
| Button | Function |
|--------|----------|
| Start Webcam | Activate camera and begin detection |
| Stop Webcam | Deactivate camera |
| Capture Frame | Save current frame as JPG |

#### Display:
- **FPS**: Frames per second (30 is good)
- **Inference**: Time to run detection per frame (< 100ms is good)
- **Detections**: Number of gestures currently detected
- **Detected Gestures**: List with confidence scores

---

## ⚙️ CONFIGURATION

### Performance Tuning

#### Change Confidence Threshold
Lower = more detections (but more false positives)
Higher = fewer detections (but higher accuracy)

```bash
# Via API
curl -X POST http://localhost:5001/webcam/config \
  -H "Content-Type: application/json" \
  -d '{"confidence_threshold": 0.5}'
```

Or edit in `webcam_api.py`:
```python
CONFIDENCE_THRESHOLD = 0.25  # Change this value (0.0 to 1.0)
```

#### Adjust Resolution
Edit in `webcam_api.py`:
```python
STREAM_WIDTH = 640      # Increase for quality, decrease for speed
STREAM_HEIGHT = 480
FPS = 30                # Frames per second
```

#### Change Detection Model
Default: YOLOv5s (small, fast)
```python
# In load_model() function, change:
model = torch.hub.load('ultralytics/yolov5', 'custom', path='best.pt')
# to:
model = torch.hub.load('ultralytics/yolov5', 'custom', path='best_large.pt')
```

---

## 🐛 TROUBLESHOOTING

### Issue: Webcam Won't Start

**Solution 1: Check Camera Access**
```bash
# Verify camera exists
wmic logicaldisk get name
# Or check Device Manager in Windows
```

**Solution 2: Release Camera**
```bash
# If camera is locked by another app:
# 1. Close other apps using camera (Zoom, Teams, etc.)
# 2. Restart webcam_api.py
```

**Solution 3: Try Different Camera**
```python
# Edit webcam_api.py, line in start_webcam():
webcam = cv2.VideoCapture(0)  # Try: 0, 1, 2 for different cameras
```

### Issue: Low FPS / Slow Detection

**Solution 1: Reduce Resolution**
```python
STREAM_WIDTH = 480      # was 640
STREAM_HEIGHT = 360     # was 480
```

**Solution 2: Use GPU (if available)**
```python
# In process_frame(), add:
if torch.cuda.is_available():
    model = model.cuda()
```

**Solution 3: Reduce Model Quality**
Use smaller model (yolov5n instead of yolov5s)

### Issue: No Detections Showing

**Solution 1: Check Model**
```bash
# Verify best.pt exists and is valid
python -c "import torch; m = torch.hub.load('ultralytics/yolov5', 'custom', path='best.pt'); print('Model OK')"
```

**Solution 2: Lower Confidence Threshold**
```bash
curl -X POST http://localhost:5001/webcam/config \
  -H "Content-Type: application/json" \
  -d '{"confidence_threshold": 0.1}'
```

**Solution 3: Test with Dashboard**
Go to `http://localhost:5001/dashboard` - if you see detections there, the model works

### Issue: Stream Not Displaying in Browser

**Solution 1: Check API Running**
```bash
curl http://localhost:5001/health
# Should return JSON with status: "healthy"
```

**Solution 2: Try Different URL**
```
Try: http://127.0.0.1:5001/webcam/stream
Instead of: http://localhost:5001/webcam/stream
```

**Solution 3: Browser Compatibility**
- Chrome/Edge: Best support
- Firefox: Good support
- Safari: May need <video> tag instead of <img>

### Issue: High CPU/Memory Usage

**Solutions:**
1. Reduce resolution
2. Lower FPS setting
3. Use smaller model
4. Check for other background processes
5. Restart the server

### Issue: Port 5001 Already in Use

**Solution 1: Find and Stop Process**
```bash
netstat -ano | findstr :5001
taskkill /PID [PID] /F
```

**Solution 2: Change Port**
Edit `webcam_api.py`, last line:
```python
app.run(host='0.0.0.0', port=8001, debug=False)  # Changed from 5001
```

---

## 📈 PERFORMANCE TARGETS

| Metric | Target | Status |
|--------|--------|--------|
| FPS | 25-30 | Check dashboard |
| Inference Time | 50-150ms | Check dashboard |
| Latency | <200ms | Feel natural |
| Memory | <500MB | Task Manager |
| CPU | <50% | Task Manager |

---

## 🔍 MONITORING & DEBUGGING

### View Server Logs

Terminal output shows:
```
INFO:__main__:Loading YOLOv5 model...
INFO:__main__:Model loaded successfully
INFO:__main__:Initializing webcam...
INFO:__main__:Webcam initialized successfully
```

### Check Detections in Real-Time

```bash
# Keep running in another terminal:
while true; do
  curl http://localhost:5001/webcam/detections | jq '.detections'
  sleep 0.5
done
```

### Test Stream Quality

```bash
# Save 10 seconds of stream to file
ffmpeg -i http://localhost:5001/webcam/stream -t 10 test_stream.mp4
```

---

## 🎓 HOW IT WORKS

### Frame Processing Pipeline

```
1. Capture Frame from Webcam
   └─> OpenCV reads from camera
   
2. Flip Frame (Mirror Effect)
   └─> cv2.flip(frame, 1)
   
3. Run YOLOv5 Inference
   └─> model(frame) returns detections
   
4. Extract Detection Data
   └─> Get bounding boxes, classes, confidence
   
5. Draw Bounding Boxes
   └─> results.render() creates annotated frame
   
6. Encode to JPEG
   └─> cv2.imencode('.jpg', frame)
   
7. Stream as MJPEG
   └─> Send frames with MJPEG boundaries
   
8. Cache Detection Data
   └─> Store for /detections endpoint
   
9. Calculate Metrics
   └─> FPS, inference time
   
10. Return to Client
    └─> Browser displays and updates
```

### Threading Model

```
Main Thread:
- Flask server
- Handle API requests

Webcam Thread (Daemon):
- Continuous frame capture
- YOLOv5 inference on each frame
- Update cached detections
- Calculate FPS metrics
- Write to thread-safe variables

Frontend:
- Displays MJPEG stream
- Fetches detections every 500ms
- Renders bounding boxes
- Shows performance metrics
```

---

## 📱 MOBILE CONSIDERATIONS

### Mobile Browser Support

#### Camera Access on Mobile:
- Android Chrome: ✅ Works great
- iOS Safari: ⚠️ Limited (use HTTPS)
- Android Firefox: ✅ Works
- iOS Chrome: ⚠️ Falls back to Safari

#### For HTTPS (Production):
```python
# Use SSL certificate
app.run(ssl_context='adhoc', host='0.0.0.0', port=5001)
# Or use proper SSL cert:
app.run(ssl_context=('cert.pem', 'key.pem'), ...)
```

#### Performance on Mobile:
- Reduce resolution to 480x360
- Lower FPS to 15-20
- Disable metrics overlay
- Use simpler model (yolov5n)

---

## 🚀 DEPLOYMENT OPTIONS

### Option 1: Local Development
```bash
python webcam_api.py
# Access on same network:
# http://[YOUR_IP]:5001/dashboard
```

### Option 2: Local Network
```python
# In webcam_api.py:
app.run(host='0.0.0.0', port=5001)  # Already does this

# Access from other computer:
# http://[HOST_IP]:5001/webcam/stream
```

### Option 3: Docker
```dockerfile
FROM python:3.8
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt flask flask-cors opencv-python
COPY . .
CMD ["python", "webcam_api.py"]
```

```bash
docker build -t gesture-webcam .
docker run -p 5001:5001 --device /dev/video0 gesture-webcam
```

### Option 4: Cloud Deployment
**For production, use Ngrok:**
```bash
pip install pyngrok
# Then modify webcam_api.py to expose via Ngrok
```

---

## 📊 EXAMPLE RESPONSES

### Stream Endpoint Response
```
GET /webcam/stream

Returns MJPEG binary data:
--frame
Content-Type: image/jpeg
Content-Length: 45632

[JPEG binary data...]
--frame
Content-Type: image/jpeg
Content-Length: 45123

[JPEG binary data...]
```

### Detections Endpoint Response
```json
GET /webcam/detections

{
  "success": true,
  "detections": [
    {
      "class_id": 0,
      "class_name": "Victory",
      "confidence": 0.963,
      "bbox": {
        "x1": 125.4,
        "y1": 102.1,
        "x2": 285.7,
        "y2": 350.8
      }
    },
    {
      "class_id": 1,
      "class_name": "Peace",
      "confidence": 0.847,
      "bbox": {
        "x1": 350.2,
        "y1": 150.3,
        "x2": 450.9,
        "y2": 320.6
      }
    }
  ],
  "detection_count": 2,
  "inference_time": 42.5,
  "fps": 29.8,
  "timestamp": "2024-06-10T10:30:15.456789"
}
```

---

## ✅ SUCCESS CHECKLIST

- [ ] Python 3.8+ installed
- [ ] Dependencies installed: `pip install flask flask-cors opencv-python torch`
- [ ] `webcam_api.py` created
- [ ] `best.pt` model exists
- [ ] Server starts: `python webcam_api.py`
- [ ] Dashboard loads: `http://localhost:5001/dashboard`
- [ ] Webcam can be enabled
- [ ] Detections appear on video
- [ ] FPS shown correctly
- [ ] Inference time displayed
- [ ] API calls work
- [ ] Lovable frontend connects

---

## 🎉 YOU'RE DONE!

Your real-time gesture detection system is ready. The next steps:

1. **Run webcam_api.py** in terminal
2. **Test dashboard** at http://localhost:5001/dashboard
3. **Generate Lovable frontend** with LOVABLE_WEBCAM_PROMPT.txt
4. **Configure API URLs** in the app
5. **Start detecting gestures** in real-time!

---

## 📞 QUICK SUPPORT

| Problem | Solution |
|---------|----------|
| Webcam won't start | Close other camera apps, restart server |
| Slow FPS | Reduce resolution, use smaller model |
| No detections | Lower confidence threshold |
| Port in use | Change port in code |
| Stream not showing | Check API is running, verify URL |
| High CPU | Reduce FPS or resolution |

---

**You now have a complete real-time gesture detection system!** 🎊

For more help, see:
- `FINAL_SUMMARY.md` - System overview
- `BACKEND_SETUP_GUIDE.md` - Additional API info
- `ACTION_CHECKLIST.md` - Step-by-step guide
