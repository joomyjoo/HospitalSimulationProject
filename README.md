# HospitalSimulationProject: Hospital Network Simulation with AI Imaging Analysis

## 1. Project Overview

The goal of this project is to simulate a small hospital network focusing on medical imaging and AI-based analysis. You will build an integrated system with four primary components:

1. **EMR Server:** Manages patient records and imaging analysis prescriptions.
2. **DICOM/PACS Server:** Stores and serves medical images (in DICOM format).
3. **AI Server:** Runs a deep neural network to process images, generate analysis reports, and update the EMR.
4. **Retinal Fundus Imaging Station:** Simulates image capture of the retina (via file upload or simulated capture).

**Data Source Reference:**  
For training or testing the AI model (or for sample data), you may refer to the Kaggle Diabetic Retinopathy Detection dataset:  
[https://www.kaggle.com/c/diabetic-retinopathy-detection/data](https://www.kaggle.com/c/diabetic-retinopathy-detection/data)
Or other datasets as listed in this github repo:
[https://github.com/openmedlab/Awesome-Medical-Dataset?tab=readme-ov-file#retina] ([https://github.com/joomyjoo/HospitalSimulationProject](https://github.com/openmedlab/Awesome-Medical-Dataset?tab=readme-ov-file#retina)


## 2. Project Objectives

- **Design & Build an Integrated System:** Create a hospital-like network that mimics real-world patient flow.
- **Implement End-to-End Workflow:** Cover patient registration, image capture, image storage, AI analysis, and reporting.
- **Handle Heavy Load:** Address the computational demands of AI processing using asynchronous queuing and basic MLOps practices.
- **Demonstrate Inter-Service Communication:** Ensure seamless integration among the EMR, DICOM/PACS, AI, and Imaging Station components.
- **Enhance Practical Skills:** Provide experience with web development, database design, image processing, AI model integration, and distributed computing concepts.

## 3. System Components & Responsibilities

### A. EMR Server
- **Responsibilities:**
  - Create and manage patient records.
  - Allow nurses to enter prescriptions for AI imaging analysis.
  - Store AI-generated reports linked to patient records.
- **Suggested Technologies:**
  - Web framework (e.g., Django/Flask or Node.js).
  - Relational database (e.g., PostgreSQL or SQLite).
  - RESTful API for inter-service communication.

### B. DICOM/PACS Server
- **Responsibilities:**
  - Receive and store retinal fundus images in DICOM format.
  - Provide APIs to query and retrieve stored images.
- **Suggested Technologies:**
  - An open-source DICOM server (e.g., Orthanc) or a custom solution using libraries like pydicom.
  - File storage for managing image data.

### C. Retinal Fundus Imaging Station
- **Responsibilities:**
  - Simulate patient imaging (via a web-based interface that allows image capture/upload).
  - Transfer captured images to the DICOM/PACS Server.
- **Suggested Technologies:**
  - A simple web interface (HTML/JavaScript) with file upload functionality.
  - API integration with the DICOM/PACS server.

### D. AI Server
- **Responsibilities:**
  - Query the EMR Server for imaging prescriptions.
  - Retrieve the corresponding retinal images from the DICOM/PACS Server.
  - Process images using a deep neural network (DNN) to generate an analysis report (output as an image or annotated result).
  - Update the EMR Server with the AI results.
- **Suggested Technologies:**
  - Python with deep learning frameworks (TensorFlow, PyTorch, or Keras).
  - REST API client to interact with both the EMR and DICOM/PACS servers.
  - Image processing libraries (e.g., OpenCV or Pillow).

---

## 4. Workflow Description

1. **Patient Registration & Prescription (EMR Server):**
   - A nurse logs into the EMR.
   - The nurse creates a new patient record and enters a prescription for AI imaging analysis.

2. **Image Capture (Retinal Fundus Imaging Station):**
   - The patient visits the imaging station.
   - An image is captured (or uploaded) and transferred to the DICOM/PACS Server.

3. **AI Analysis Request (AI Server):**
   - The nurse accesses the AI Server’s interface.
   - The AI Server automatically queries the EMR for any pending imaging analysis prescriptions.
   - The nurse selects the relevant prescription and clicks “Run.”

4. **Image Processing & Report Generation (AI Server):**
   - **Job Enqueuing:** Instead of processing the request immediately, the AI Server places the analysis task in a job queue.
   - **Asynchronous Processing:** Worker processes pick up jobs from the queue, retrieve the image from the DICOM/PACS Server, and run the DNN for analysis.
   - **Result Generation:** The AI Server generates an analysis report (e.g., an annotated image) and updates the patient record on the EMR Server.

5. **Feedback:**  
   - The system provides status updates via the AI Server UI, indicating when processing is complete and the EMR has been updated.

---

## 5. Handling Heavy Load on the AI Server with MLOps/Queuing

### Challenge:
Deep neural network inference can be resource-intensive, especially when multiple requests occur simultaneously.

### Proposed Solution:
- **Asynchronous Job Queue:**
  - **Decoupling Requests:** When a nurse triggers an AI analysis, the request is enqueued rather than processed immediately.
  - **Tools:** Use a message broker such as RabbitMQ or Redis Queue with a task manager like Celery.
- **Scalable Worker Pool:**
  - **Parallel Processing:** Deploy multiple worker instances that pull tasks from the queue. Scale horizontally (using Docker containers, Kubernetes, etc.) to handle higher loads.
  - **Monitoring:** Implement basic monitoring (e.g., with Prometheus/Grafana) to track job queue length and processing times, triggering autoscaling as needed.
- **MLOps Best Practices:**
  - **Containerization:** Use Docker to containerize the AI inference environment, ensuring consistency and ease of deployment.
  - **CI/CD Pipeline:** Establish a pipeline to automatically test and deploy updates to the AI code or model.
  - **Logging & Error Handling:** Integrate comprehensive logging to capture processing details and errors for troubleshooting and future optimizations.

---

## 6. Project Milestones & Deliverables

### Phase 1: Planning & Design
- **Deliverable:** A design document including:
  - Overall architecture diagram.
  - Description of each component and inter-service APIs.
  - Chosen technologies with justifications.

### Phase 2: Component Implementation
- **EMR Server:** Develop patient registration and prescription management.
- **DICOM/PACS Server:** Set up image reception, storage, and retrieval endpoints.
- **Retinal Fundus Imaging Station:** Build a web interface to capture/upload images and integrate with the DICOM/PACS Server.
- **AI Server:** Implement job queuing, AI inference, report generation, and update routines.

### Phase 3: Integration & Testing
- **Deliverable:** End-to-end testing of the workflow:
  - Verify patient creation and prescription in the EMR.
  - Confirm image capture and storage on the DICOM/PACS server.
  - Test asynchronous job processing on the AI Server.
  - Validate that the final AI report is correctly updated in the EMR.
- **Documentation:** Report on integration challenges and resolutions.

### Phase 4: Documentation & Final Presentation
- **Deliverables:**
  - A comprehensive written report (up to 10 pages) covering design, implementation, testing, and lessons learned.
  - A slide deck (10–15 slides) summarizing the project.
  - A recorded demonstration or live presentation showcasing the complete workflow.

---

## 7. Evaluation Criteria

You will be evaluated based on:
- **Functionality:** Each component meets its specified requirements.
- **Integration:** Seamless communication and data flow across the system.
- **Scalability & Load Handling:** Effective use of queuing and MLOps practices in the AI Server.
- **Code Quality:** Clean, modular code with appropriate documentation.
- **User Experience:** Intuitive interfaces for nurses (EMR & AI Server) and patients (Imaging Station).
- **Documentation & Presentation:** Clarity in technical documentation and a professional final presentation.

---

## 8. Required Enhancements

- **Security & Authentication:** Implement user authentication for nurse access and secure API communication.
- **Advanced Error Handling:** Enhance logging and error handling for improved reliability.
- **Container Orchestration:** Use Docker Compose for managing multi-container deployments.
- **Detailed AI Reporting:** Expand the AI report to include metrics, confidence scores, or annotated images.

---
Good luck, and please feel free to ask for any clarification during the project!
