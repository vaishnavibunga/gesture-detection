# Lovable Web App Prompt for Gesture/Sign Language Detection System

## PROJECT OVERVIEW
Build a modern, professional web application for real-time gesture and sign language detection using a trained YOLOv5 machine learning model. The application should provide an intuitive interface for users to upload images, perform object detection, and view detailed results with high accuracy and proper error handling.

---

## CORE REQUIREMENTS

### 1. BACKEND API INTEGRATION
- **API Base URL**: `http://localhost:5000` (configurable in settings)
- **API Endpoints to integrate**:
  - `GET /health` - Health check endpoint
  - `GET /info` - Get API and model information
  - `POST /predict` - Single image inference
  - `POST /batch-predict` - Multiple image inference
  - All endpoints support CORS

### 2. IMAGE UPLOAD & PREVIEW
- Support multiple upload methods:
  - **Drag & Drop**: Large drop zone for dragging images
  - **File Browser**: Click to browse and select image files
  - **URL Input**: Paste image URL to analyze
  - **Webcam Capture**: Real-time camera feed (optional but ideal)
- Supported formats: JPG, JPEG, PNG, BMP, GIF, WebP
- Max file size: 10MB with validation and error messages
- **Live Preview**: Display uploaded image before processing
- Show image dimensions and file size

### 3. DETECTION INTERFACE

#### Single Image Detection:
- After image selection, show "Detect Gestures" button
- Display loading spinner during inference
- Show inference time (in milliseconds)
- Display number of detections found
- Render annotated image with bounding boxes

#### Detection Results Display:
```
Gesture Detection Results
├── Annotated Image with Boxes
├── Detection Count: X gestures found
├── Inference Time: XXX ms
└── Detections Table/Cards:
    ├── Gesture Name
    ├── Confidence Score (%)
    ├── Bounding Box Coordinates
    └── Position Info (center, width, height)
```

#### Bounding Box Visualization:
- Draw colored bounding boxes on the image
- Label each box with:
  - Gesture class name
  - Confidence score (percentage)
  - Different colors for different confidence levels:
    - Green: 80-100% confidence
    - Yellow: 50-80% confidence
    - Red: <50% confidence

### 4. RESULTS DISPLAY

#### Option 1: Card View (Default)
- Display each detection as a card:
  ```
  [Gesture Icon/Badge] | Gesture Name
  Confidence: 95.3%
  Location: (x1, y1) to (x2, y2)
  Size: 120x80 px
  [Export] [Copy]
  ```

#### Option 2: Table View
- Sortable columns: Gesture Name, Confidence, X1, Y1, X2, Y2, Width, Height
- Export as CSV

#### Option 3: Statistics Panel
- Total gestures detected
- Confidence range (min-max)
- Average confidence
- Chart showing confidence distribution

### 5. ADVANCED FEATURES

#### Batch Processing:
- Upload multiple images at once
- Process sequentially or in parallel
- Show progress bar for batch operations
- Download results as ZIP with:
  - All annotated images
  - CSV file with all detections

#### Confidence Threshold Adjustment:
- Slider to adjust detection confidence (0-100%)
- Real-time filtering of results based on threshold
- Visual feedback on how threshold affects results

#### Download & Export Options:
- **Download Annotated Image**: PNG/JPG format
- **Export Results**: JSON, CSV, or XML format
- **Copy Coordinates**: Quick copy bbox coordinates
- **Share Results**: Generate shareable link (optional)

### 6. USER INTERFACE DESIGN

#### Layout:
```
┌─────────────────────────────────────────────────┐
│                  HEADER/NAVBAR                  │
│  Logo | "Gesture Detection" | Settings | About  │
├─────────────────────────────────────────────────┤
│                                                 │
│  ┌─────────────────┐    ┌──────────────────┐   │
│  │  Upload Zone    │    │ Annotated Image  │   │
│  │  (Drag & Drop)  │    │                  │   │
│  │                 │    │                  │   │
│  │ [Browse Files]  │    │                  │   │
│  │ [Use Webcam]    │    │                  │   │
│  └─────────────────┘    └──────────────────┘   │
│                                                 │
│  ┌─────────────────────────────────────────┐   │
│  │   Detection Results / Statistics        │   │
│  │   ────────────────────────────────────  │   │
│  │   Found: 5 gestures | Time: 125ms       │   │
│  │   [Confidence Slider: ████████░░ 75%]  │   │
│  │                                         │   │
│  │   [Detection 1] [Detection 2]           │   │
│  │   [Detection 3] [Detection 4]           │   │
│  │   [Detection 5]                         │   │
│  │                                         │   │
│  │   [↓ Download Results] [↓ Export CSV]   │   │
│  └─────────────────────────────────────────┘   │
│                                                 │
└─────────────────────────────────────────────────┘
```

#### Color Scheme:
- **Primary**: Modern Blue (#0066CC or similar)
- **Success**: Green for high confidence (#28A745)
- **Warning**: Yellow/Orange for medium confidence (#FFC107)
- **Danger**: Red for low confidence (#DC3545)
- **Background**: Light gray or white (#F8F9FA or #FFFFFF)
- **Text**: Dark gray (#333333) on light backgrounds

#### Typography:
- **Headers**: Bold, clear, readable (20-28px)
- **Body**: 14-16px for comfortable reading
- **Labels**: 12-14px for metadata
- **Font Family**: Modern sans-serif (Segoe UI, Roboto, Inter, or similar)

### 7. RESPONSIVE DESIGN
- **Mobile (320px-768px)**: 
  - Stacked layout
  - Single column for results
  - Full-width image preview
  - Bottom sheet/modal for controls
  
- **Tablet (768px-1024px)**: 
  - Two column layout
  - Image on left, results on right
  
- **Desktop (1024px+)**: 
  - Full three-column layout
  - Sidebar with controls

### 8. ERROR HANDLING & VALIDATION

#### User-Facing Error Messages:
- "No image file selected" → Guide user to upload
- "File too large (>10MB)" → Suggest compression
- "Unsupported format" → Show allowed formats
- "Failed to connect to API" → Check backend status
- "API returned error" → Show detailed error with retry option

#### Loading States:
- Disable upload during processing
- Show progress percentage for uploads
- Spinner during inference with "Analyzing image..." message
- "Processing X/Y images" for batch operations

#### Success Feedback:
- "✓ Detection complete in 125ms"
- Smooth animation when results appear
- Toast notifications for exports and downloads

### 9. SETTINGS & CONFIGURATION

#### User Settings Panel:
```
⚙️ Settings
├── API Configuration
│   └── API Base URL: [input field]
│       Default: http://localhost:5000
├── Detection Settings
│   ├── Confidence Threshold: [slider 0-100%]
│   ├── Return Annotated Image: [toggle]
│   └── Auto-refresh Results: [toggle]
├── Display Settings
│   ├── View Mode: [Cards|Table|Stats]
│   ├── Color Theme: [Light|Dark]
│   └── Show Coordinates: [toggle]
└── [Save] [Reset to Defaults]
```

### 10. WEBCAM FEATURE (NICE-TO-HAVE)
- **Live Detection Mode**:
  - Real-time inference from webcam feed
  - Display FPS counter
  - Auto-save detected frames
  - Pause/Resume buttons
  - Capture snapshot button
  - Resolution selector (480p, 720p, 1080p)

### 11. HISTORY & MANAGEMENT
- Store recent detections (last 20 in browser storage)
- **History Panel**:
  - List of recent images analyzed
  - Thumbnail view with detection count
  - Quick re-analyze option
  - Delete from history
  - Export full history

### 12. INFORMATION & HELP
- **About Section**:
  - Model info: "YOLOv5s object detection model"
  - Trained on: "Sign Language Dataset v3i"
  - Accuracy metrics displayed
  
- **Quick Help**:
  - Tooltips on key elements
  - "How to use" guide
  - FAQ section
  - API documentation link
  - Keyboard shortcuts cheat sheet

### 13. STATE MANAGEMENT
- Maintain detection results across page navigation
- Save user preferences (theme, confidence threshold)
- Remember last used API URL
- Clear cache/history option

---

## TECHNICAL SPECIFICATIONS

### Frontend Stack:
- **Framework**: React or Vue.js (modern, responsive)
- **State Management**: Context API or Vuex
- **HTTP Client**: Axios or Fetch API
- **UI Components**: Material-UI, TailwindCSS, or similar
- **Image Processing**: Canvas API for drawing bboxes
- **Storage**: LocalStorage for preferences and history

### Key Libraries:
- `axios` - API requests
- `canvas` or `fabric.js` - Image annotation
- `chart.js` - Confidence distribution charts
- `date-fns` - Date/time formatting
- `react-dropzone` - Drag & drop uploads

### Performance Requirements:
- Initial load: <2 seconds
- Image preview: <500ms
- API call: Display spinner immediately
- Annotated image rendering: <1 second
- Responsive to all interactions (no lag)

### Browser Support:
- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+

---

## WORKFLOW EXAMPLE

1. **User lands on app** → See upload zone and recent history
2. **Upload image** → Drag, click, or paste URL
3. **Preview displays** → User sees image dimensions and file size
4. **Click "Detect Gestures"** → Loading spinner appears
5. **Results arrive** → Annotated image shows with bounding boxes
6. **View results** → Toggle between card/table/stats view
7. **Adjust confidence** → Slider filters results in real-time
8. **Download** → Export image or CSV with one click
9. **Try another** → Clear results and upload new image

---

## ADDITIONAL REQUIREMENTS

### Accessibility:
- WCAG 2.1 AA compliance
- Alt text for all images
- Keyboard navigation support
- High contrast mode option
- Screen reader friendly

### Security:
- Client-side image validation before upload
- No storing of sensitive user data
- HTTPS ready (no hardcoded HTTP URLs)
- CORS properly configured

### Analytics (Optional):
- Track number of detections per session
- Average confidence scores
- Most common gestures detected
- User engagement metrics

---

## DELIVERABLES

Your web app should provide:
✅ Professional, clean UI with intuitive controls
✅ Multiple image upload methods (drag-drop, file browser, URL, webcam)
✅ Clear, annotated results with bounding boxes
✅ Flexible result visualization (cards, tables, stats)
✅ Confidence threshold adjustment
✅ Export and download options
✅ Full mobile responsiveness
✅ Error handling and user feedback
✅ Settings/configuration panel
✅ History and recent detections
✅ Fast performance and smooth interactions
✅ Accessibility compliance

---

## IMPORTANT NOTES

- **Backend API runs on**: `http://localhost:5000`
- **API response format**: JSON with structured detection data
- **Image format in API**: Multipart form-data or base64
- **Inference time**: Typically 100-300ms per image
- **No authentication needed** for this MVP version
- **Model is YOLOv5s**: Fast inference, good accuracy trade-off
- **Confidence scores**: 0-1 range (will be displayed as 0-100%)

---

## FINAL NOTES FOR LOVABLE PROMPT

"Create a production-ready web application for gesture/sign language detection that integrates with a YOLOv5 Flask backend API. Focus on user experience, clear visualization of detection results with bounding boxes, and easy export options. The app should handle multiple upload methods, provide real-time confidence adjustment, and work seamlessly on mobile and desktop devices. Make it visually polished, responsive, and include proper error handling with helpful user guidance throughout."
