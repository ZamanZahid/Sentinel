# Sentinel Project

## Overview and Purpose

The Sentinel project is an AI-powered safety and security system designed to predict, prevent, and intervene in dangerous situations. It integrates various technologies to analyze video footage, detect threats, and provide real-time information and response capabilities to security personnel. The system aims to enhance safety in various environments by leveraging advanced AI for proactive threat assessment.

## Key Features and Functionality

-   **AI-Powered Video Analysis**: Utilizes YOLOv8 for object detection (persons, other objects) and DeepFace for facial analysis (emotion, age, gender) in video streams.
-   **Threat Assessment**: Employs a sophisticated Llama API-based analysis pipeline to evaluate fight risk, authenticity of distress, camera evasion, and suspicious behaviors from video frames.
-   **Multi-component Architecture**: Comprises several interconnected services for API, YOLO processing, Llama API integration, a comprehensive web frontend, and a mobile application.
-   **Video Ingestion**: Supports uploading video files and processing YouTube video URLs for analysis.
-   **Real-time Monitoring (Web Frontend)**: A web-based Ops Center dashboard for monitoring alerts, camera feeds, incidents, and analytics.
-   **Responder Mobile Application**: A simplified mobile interface for responders with authentication, geolocation tracking, and emergency display capabilities.
-   **RESTful APIs**: Provides APIs for video analysis results and camera management.
-   **Static File Serving**: Serves raw and annotated video files for review.

## Project Structure Explanation

The project is organized into several key directories, each representing a distinct service or component:

-   **`api/`**
    -   **Purpose**: FastAPI service acting as a proxy. It accepts video uploads, forwards them to the Node.js `backend` for Llama API analysis, and integrates with the `backend_yolo` service for YOLO detection. It also serves as the main entry point for the frontend to interact with the AI analysis.
    -   **Key Files**: [main.py](file:///Users/zamanzahid/Downloads/Sentinel-final/api/main.py), [routers/analyze.py](file:///Users/zamanzahid/Downloads/Sentinel-final/api/routers/analyze.py), [routers/cameras.py](file:///Users/zamanzahid/Downloads/Sentinel-final/api/routers/cameras.py), [services/gemini_client.py](file:///Users/zamanzahid/Downloads/Sentinel-final/api/services/gemini_client.py) (which proxies to the Node.js Llama API), [requirements.txt](file:///Users/zamanzahid/Downloads/Sentinel-final/api/requirements.txt).
-   **`backend/`**
    -   **Purpose**: Node.js Express server that directly interfaces with the Llama Vision API. It handles video frame extraction (using `ffmpeg`) and sends them to the Llama API for detailed threat assessment based on a comprehensive prompt. This is the core AI analysis component for sophisticated threat detection.
    -   **Key Files**: [server.js](file:///Users/zamanzahid/Downloads/Sentinel-final/backend/server.js), [package.json](file:///Users/zamanzahid/Downloads/Sentinel-final/backend/package.json).
-   **`backend_yolo/`**
    -   **Purpose**: FastAPI service dedicated to YOLOv8 object detection and DeepFace analysis. It processes uploaded videos or YouTube URLs, annotates them with bounding boxes, and provides summary information. This service is primarily for visual detection and basic analysis.
    -   **Key Files**: [main.py](file:///Users/zamanzahid/Downloads/Sentinel-final/backend_yolo/main.py), [detect.py](file:///Users/zamanzahid/Downloads/Sentinel-final/backend_yolo/detect.py), [requirements.txt](file:///Users/zamanzahid/Downloads/Sentinel-final/backend_yolo/requirements.txt).
-   **`frontend/`**
    -   **Purpose**: A comprehensive web application (React, TypeScript) serving as the main user interface for the Sentinel system. It features various dashboards and consoles for operations, prevention, and incident management. It interacts with the `api` service.
    -   **Key Files**: [src/App.tsx](file:///Users/zamanzahid/Downloads/Sentinel-final/frontend/src/App.tsx), [src/main.tsx](file:///Users/zamanzahid/Downloads/Sentinel-final/frontend/src/main.tsx), [package.json](file:///Users/zamanzahid/Downloads/Sentinel-final/frontend/package.json), `components/`, `pages/`.
-   **`mobile_app/`**
    -   **Purpose**: A simple, standalone mobile web application for responders or staff. It provides authentication, geolocation tracking (using Leaflet.js for mapping), and potentially displays emergency information. It integrates with Supabase for backend services.
    -   **Key Files**: [index.html](file:///Users/zamanzahid/Downloads/Sentinel-final/mobile_app/index.html), [app.js](file:///Users/zamanzahid/Downloads/Sentinel-final/mobile_app/app.js), [styles.css](file:///Users/zamanzahid/Downloads/Sentinel-final/mobile_app/styles.css).
-   **`pdf/`**: Contains project documentation and deliverables (e.g., `Sentinel Admin Dashboard UI Deliverables.pdf`).
-   **`videos/`**: Storage for raw and potentially annotated video files.
-   **Root Level**: `cameras.json` (camera configurations), `map.png` (floor map), `yolov8n.pt` (YOLOv8 nano model weights).

## Technologies Used

### Backend/API Services

-   **Python**:
    -   **FastAPI**: For building the `api` and `backend_yolo` RESTful services.
    -   **Uvicorn**: ASGI server for FastAPI applications.
    -   **ultralytics (YOLOv8)**: For efficient object detection.
    -   **OpenCV (`opencv-python-headless`)**: For video processing (frame extraction, annotation).
    -   **DeepFace**: For facial analysis (emotion, age, gender).
    -   **`yt-dlp`**: For downloading videos from platforms like YouTube.
    -   **`httpx`**: Asynchronous HTTP client for internal service communication.
    -   **`aiofiles`**: Asynchronous file operations.
    -   **`python-dotenv`**: Environment variable management.
-   **Node.js**:
    -   **Express**: Web framework for the `backend` service.
    -   **Multer**: Middleware for handling `multipart/form-data` (file uploads).
    -   **`dotenv`**: Environment variable management.
-   **Llama API**: For advanced AI threat assessment (via the Node.js `backend`).
-   **FFmpeg**: External tool used by both Python and Node.js services for video manipulation (frame extraction, re-encoding).

### Frontend (Web)

-   **React**: JavaScript library for building user interfaces.
-   **TypeScript**: Statically typed superset of JavaScript.
-   **Vite**: Fast build tool.
-   **`react-router-dom`**: For client-side routing.
-   **`@tanstack/react-query`**: Data fetching and state management.
-   **Shadcn UI / Radix UI**: UI component library for building accessible and modern interfaces.
-   **Tailwind CSS**: Utility-first CSS framework for styling.
-   **Supabase**: Backend-as-a-service (authentication, potentially database).
-   **`framer-motion`**: For animations.
-   **`recharts`**: For data visualization.
-   **`react-hook-form` / `zod`**: Form management and validation.
-   **`tesseract.js`**: OCR capabilities.

### Mobile Application

-   **HTML, CSS, JavaScript**: Standard web technologies.
-   **Leaflet.js**: Open-source JavaScript library for interactive maps.
-   **Supabase**: Backend-as-as-service (authentication, real-time data).

## Installation and Setup Instructions

To get the Sentinel project up and running, you'll need to set up each component individually.

### Prerequisites

-   **Node.js & npm/bun**: For the `backend` and `frontend` services.
-   **Python & pip**: For the `api` and `backend_yolo` services.
-   **FFmpeg**: A command-line tool for video manipulation. Install it system-wide (e.g., `brew install ffmpeg` on macOS, `sudo apt install ffmpeg` on Ubuntu).
-   **`yt-dlp`**: A command-line program to download videos from YouTube and other sites. Install it (e.g., `pip install yt-dlp`).
-   **Supabase Project**: Set up a Supabase project for authentication and potentially database services for the `frontend` and `mobile_app`. Obtain your Supabase URL and Anon Key.
-   **Llama API Key**: Obtain an API key for the Llama Vision API, which is used by the `backend` service for advanced threat assessment.
-   **Google Gemini API Key**: While the `api` service currently proxies to Llama, future direct Gemini integration might require a key.

### Step-by-Step Setup

1.  **Clone the Repository**:
    ```bash
    git clone <repository-url>
    cd Sentinel-final
    ```

2.  **Backend (`backend/`)**:
    -   Navigate to the `backend` directory:
        ```bash
        cd backend
        ```
    -   Create a `.env` file from `.env.example` and add your Llama API Key:
        ```
        cp .env.example .env
        # Open .env and add:
        # LLAMA_API_KEY="YOUR_LLAMA_API_KEY"
        # PORT=3000 (or your preferred port)
        ```
    -   Install Node.js dependencies:
        ```bash
        npm install
        # or
        bun install
        ```
    -   Start the backend service:
        ```bash
        npm start
        # or
        bun run start
        ```
        This service will typically run on `http://localhost:3000`.

3.  **API Service (`api/`)**:
    -   Navigate to the `api` directory:
        ```bash
        cd ../api
        ```
    -   Create a `.env` file from `.env.example`. This service proxies to the Node.js backend, so ensure `PIPELINE_URL` points to your running Node.js backend.
        ```
        cp .env.example .env
        # Open .env and ensure:
        # PIPELINE_URL="http://localhost:3000/analyze" (adjust port if necessary)
        ```
    -   Install Python dependencies:
        ```bash
        pip install -r requirements.txt
        ```
    -   Start the FastAPI service:
        ```bash
        uvicorn api.main:app --reload --port 8000
        ```
        This service will typically run on `http://localhost:8000`. API documentation will be available at `http://localhost:8000/docs`.

4.  **YOLO Detection Service (`backend_yolo/`)**:
    -   Navigate to the `backend_yolo` directory:
        ```bash
        cd ../backend_yolo
        ```
    -   Install Python dependencies:
        ```bash
        pip install -r requirements.txt
        ```
    -   Start the FastAPI YOLO service:
        ```bash
        uvicorn main:app --reload --port 8001
        ```
        This service will typically run on `http://localhost:8001`.

5.  **Frontend (`frontend/`)**:
    -   Navigate to the `frontend` directory:
        ```bash
        cd ../frontend
        ```
    -   Create a `.env` file. You will need your Supabase credentials here.
        ```
        cp .env.example .env
        # Open .env and add:
        # VITE_SUPABASE_URL="YOUR_SUPABASE_URL"
        # VITE_SUPABASE_ANON_KEY="YOUR_SUPABASE_ANON_KEY"
        ```
    -   Install Node.js dependencies:
        ```bash
        npm install
        # or
        bun install
        ```
    -   Start the development server:
        ```bash
        npm run dev
        # or
        bun run dev
        ```
        The frontend will typically be available at `http://localhost:5173` (or another port if 5173 is in use).

6.  **Mobile App (`mobile_app/`)**:
    -   Navigate to the `mobile_app` directory:
        ```bash
        cd ../mobile_app
        ```
    -   Open `app.js` and replace `SUPABASE_URL` and `SUPABASE_ANON_KEY` with your Supabase project values.
    -   Open `index.html` directly in your web browser, or serve it using a simple static file server (e.g., `python -m http.server`).

## Usage Examples

-   **Web Frontend**: Access the web application through your browser (e.g., `http://localhost:5173`) to monitor camera feeds, view alerts, manage incidents, and interact with the Ops Center dashboard.
-   **Video Upload for Analysis**: Use the `/detect/upload` endpoint of the `backend_yolo` service (e.g., `http://localhost:8001/detect/upload`) to upload video files for AI analysis.
-   **YouTube Video Analysis**: Submit a YouTube URL to the `/detect/youtube` endpoint of the `backend_yolo` service for automated threat detection.
-   **Mobile App**: Open `mobile_app/index.html` in a mobile browser to access the responder console, track location, and view emergency information.

## Any Other Relevant Information

-   **AI Model Details**: The project leverages advanced AI models for both object detection (YOLOv8) and complex situational analysis (Llama Vision API).
-   **Modularity**: The system is designed with modular components, allowing for independent development and deployment of services.
-   **Scalability**: The use of FastAPI and Express, along with containerization potential, suggests an architecture that can be scaled to handle increased loads.
-   **Development Environment**: The `frontend` uses Vite for a fast development experience. The Python services use `uvicorn --reload` for automatic code reloading during development.
