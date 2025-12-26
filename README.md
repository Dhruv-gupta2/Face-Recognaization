# Face-Recognaization
FaceVault - AI Face Recognition System
A modern, full-stack face recognition system that allows you to register faces with names and recognize them in new images. Built with Python Flask backend and React frontend.

 Features
Face Registration: Upload images and register faces with names permanently
Face Recognition: Identify registered faces in new photos with confidence scores
Persistent Storage: SQLite database stores face encodings forever
Multiple Face Detection: Detect and recognize multiple faces in a single image
Beautiful UI: Modern, cyberpunk-inspired interface with smooth animations
Real-time Stats: Track total registered faces and detection results
Face Management: View all registered faces and delete them if needed
 UI Highlights
Cyberpunk aesthetic with cyan/pink gradients
Animated background with floating gradient orbs
Smooth transitions and hover effects
Real-time face detection boxes with labels
Confidence score visualization
Responsive design for all screen sizes
 Architecture
Backend (Python/Flask)
Framework: Flask with CORS support
Face Recognition: face_recognition library (built on dlib)
Database: SQLite for persistent storage
Image Processing: PIL (Pillow) for image handling
Storage: Pickle for face encoding serialization
Frontend (React)
Framework: React 18 (CDN version for simplicity)
Styling: Custom CSS with modern design
Fonts: Orbitron (display) + IBM Plex Sans (body)
API Communication: Fetch API
Database Schema
faces (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    encoding BLOB NOT NULL,
    image_path TEXT,
    created_at TIMESTAMP
)
 Prerequisites
Python 3.8 or higher
pip (Python package manager)
cmake (required for dlib)
Modern web browser
Webcam or image files for testing
System Dependencies
Ubuntu/Debian:

sudo apt-get update
sudo apt-get install cmake build-essential python3-dev
macOS:

brew install cmake
Windows:

Install CMake from https://cmake.org/download/
Install Visual Studio Build Tools
 Installation
1. Clone or Download the Project
# Navigate to the project directory
cd face-recognition-system
2. Set Up Backend
cd backend

# Create virtual environment (recommended)
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
Note: Installing dlib and face_recognition may take several minutes.

3. Verify Installation
python -c "import face_recognition; print('Face recognition installed successfully!')"
Usage
Starting the Backend Server
cd backend
python app.py
The server will start on http://localhost:5000

Starting the Frontend
Simply open the frontend/index.html file in your web browser:

# Option 1: Double-click the file in your file manager

# Option 2: Use Python's built-in server
cd frontend
python -m http.server 8000
# Then visit http://localhost:8000
 How to Use
Register a New Face
In the "Register Face" section, enter the person's name
Click the upload zone and select an image with a clear face
Click "Register Face" button
The system will detect the face and save it permanently
Tips for best results:

Use well-lit, front-facing photos
Ensure only one face is in the registration image
Higher resolution images work better
Avoid sunglasses or heavy face coverings
Recognize Faces
In the "Recognize Face" section, click the upload zone
Select an image containing one or more faces
Click "Recognize" button
The system will detect all faces and show:
Green boxes around detected faces
Names with confidence scores
Unknown label for unregistered faces
Manage Registered Faces
View all registered faces in the "Registered Faces" section
See registration dates for each face
Delete faces by clicking the "Delete" button
Total count updates automatically
🔌 API Endpoints
POST /api/register
Register a new face with a name.

Request:

{
  "name": "John Doe",
  "image": "data:image/jpeg;base64,..."
}
Response:

{
  "success": true,
  "message": "Face registered successfully for John Doe",
  "id": 1,
  "name": "John Doe"
}
POST /api/recognize
Recognize faces in an image.

Request:

{
  "image": "data:image/jpeg;base64,..."
}
Response:

{
  "success": true,
  "faces": [
    {
      "name": "John Doe",
      "confidence": 95.67,
      "location": {
        "top": 100,
        "right": 300,
        "bottom": 400,
        "left": 150
      },
      "id": 1
    }
  ],
  "count": 1
}
GET /api/faces
Get all registered faces.

Response:

{
  "success": true,
  "faces": [
    {
      "id": 1,
      "name": "John Doe",
      "created_at": "2025-12-23 10:30:00"
    }
  ],
  "count": 1
}
DELETE /api/faces/{id}
Delete a registered face.

Response:

{
  "success": true,
  "message": "Face deleted successfully"
}
GET /api/stats
Get system statistics.

Response:

{
  "success": true,
  "total_faces": 5
}
🗂️ Project Structure
face-recognition-system/
├── backend/
│   ├── app.py              # Flask application
│   └── requirements.txt    # Python dependencies
├── frontend/
│   └── index.html          # React application (single file)
├── database/
│   ├── faces.db           # SQLite database (auto-created)
│   └── images/            # Stored face images (auto-created)
└── README.md              # This file
🔧 Configuration
Backend Settings
Edit backend/app.py:

Change server port
app.run(debug=True, port=5000)

 Adjust face recognition tolerance (lower = stricter)
matches = face_recognition.compare_faces(
    known_face_encodings, 
    face_encoding, 
    tolerance=0.6  # Default: 0.6 (range: 0.0-1.0)
)
Frontend Settings
Edit frontend/index.html:

// Change API URL
const API_URL = 'http://localhost:5000/api';
 How Face Recognition Works
Face Detection: The system uses HOG (Histogram of Oriented Gradients) or CNN to detect faces
Face Encoding: Each face is converted to a 128-dimensional vector (encoding)
Storage: Encodings are serialized with pickle and stored in SQLite
Recognition: New faces are compared with stored encodings using Euclidean distance
Matching: Faces with distance < 0.6 (default) are considered matches
Confidence: Distance is converted to a 0-100% confidence score
 Troubleshooting
Common Issues
1. "No module named 'face_recognition'"

pip install face_recognition --break-system-packages
2. "CMake not found"

Install cmake for your operating system (see Prerequisites)
3. "No face detected in the image"

Ensure the image has a clear, front-facing face
Try a different image with better lighting
Check if the face is too small in the image
4. "CORS error in browser"

Make sure the backend server is running
Check that CORS is enabled in app.py
5. "Database locked"

Close any other processes accessing the database
Restart the backend server
Performance Tips
Use images around 800-1200px wide for best results
Face detection is faster with smaller images
The first recognition may be slower (warming up the model)
Consider using CNN model for better accuracy (slower):
face_locations = face_recognition.face_locations(image_np, model="cnn")
🚀 Deployment
Production Considerations
Use a production WSGI server:
pip install gunicorn
gunicorn -w 4 -b 0.0.0.0:5000 app:app
Serve frontend with nginx or Apache

Use PostgreSQL instead of SQLite for better concurrency

Add authentication and rate limiting

Use environment variables for configuration

Enable HTTPS for secure image transmission

Add image compression before storage

 Performance Metrics
Registration: ~1-2 seconds per face
Recognition: ~1-3 seconds per image (depends on face count)
Storage: ~1KB per face encoding
Database: Handles 10,000+ faces efficiently
 Security Notes
This is a demo application, not production-ready
No authentication or authorization implemented
Images are stored unencrypted
API is open to all origins (CORS: *)
Consider adding:
User authentication
API rate limiting
Image size limits
Input validation
HTTPS encryption
Contributing
Feel free to:

Report bugs
Suggest features
Submit pull requests
Improve documentation
 License
This project is provided as-is for educational purposes.

🎓 Learning Resources
face_recognition documentation
Flask documentation
React documentation
dlib documentation
💡 Future Enhancements
 Live webcam recognition
 Batch face registration
 Face grouping and clustering
 Export/import face database
 Multi-user support with accounts
 Mobile app integration
 Real-time video stream recognition
 Face comparison and similarity scoring
 Age/gender/emotion detection
 Integration with external APIs
📞 Support
If you encounter issues:

Check the troubleshooting section
Verify all dependencies are installed
Check Python and library versions
Review browser console for errors
Check backend logs for error messages
Built with ❤️ using Flask, React, and face_recognition

Happy face recognizing! 🎉
