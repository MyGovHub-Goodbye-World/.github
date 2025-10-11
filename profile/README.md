# MyGovHub

<img src="assets/MyGovHub-Logo-Dark.png" alt="The full Architecture Diagram of this system"/>


## 📘 Project Overview

**MyGov Hub** is an **AI-powered, unified government services platform** designed to simplify how citizens interact with public services. Instead of juggling multiple apps, citizens access everything through a **single conversational interface,**  text or voice, integrated into familiar platforms like WhatsApp. From renewing licenses to paying summons, or even uploading a photo of a bill, MyGov Hub makes government services **as easy as chatting with a friend**.





## 🚨 Problem Statement
Malaysia's current digital government landscape is fragmented, with nearly 200 government websites and 1,500 different government apps serving various functions from passport applications to bill payments [(Source: MadeinMalaysia)](https://madeinmalaysia.com.my/malaysia-government-apps-and-websites/). This fragmented system creates several issues:
- **Citizen Confusion**:  
Navigating a multitude of apps and portals is inconvenient and confusing, especially for seniors and those less familiar with technology.

- **Operational Inefficiency**:  
This patchwork of systems leads to duplication of efforts and inefficiencies for both the public and government agencies.

- **High Costs**:  
Maintaining and managing numerous separate portals results in increased operational costs.

MyGov Hub addresses these issues by unifying multiple services into one accessible, cost-efficient, and user-friendly platform.

## ⚙️ Technical Architecture

MyGovHub’s architecture is a multi-layered, serverless ecosystem designed to process eKYC verification, document analysis, and conversational workflows through a unified government chatbot interface.

It leverages AWS cloud services, MongoDB Atlas, and a React Native frontend, ensuring scalability, fault tolerance, and modularity across all major functional domains.

The architecture is organized into four layers:
1. Layer I - Frontend (User Interaction),
2. Layer II - Backend MCP (Middleware & Application Logic),
3. Layer III - Backend Services (Specialized Microservices), and
4. Layer IV - Database (Data Storage).

### Layer I - Frontend (User Interaction)

The React Native application serves as the main entry point for users. It mimics a WhatsApp-style conversational interface, integrating text, image, and voice interactions.

This layer is responsible for capturing user input and displaying responses. It includes:

1. **Mobile Application (React Native + Expo):**
    - Provides voice and text input for communication with the chatbot.
    - Integrates a local SQLite database to temporarily cache session data and uploaded media.
    - Uses Axios-based REST API calls to communicate securely with the Backend MCP layer via HTTPS endpoints.
    - Handles user registration, eKYC onboarding, and payment confirmations.
    - Implements OTP verification and facial recognition steps during identity registration.

2. **eKYC Onboarding Flow:**
    - Users capture a selfie and upload their identity documents (e.g., IC, passport).
    - The app sends these images to the eKYC Backend API, uploading to AWS S3 via signed URLs for verification.
    - Triggers document validation and text extraction service through an API call.
    - Integrates with the OTP Verification API to confirm user identity.
    - Integrates with the Face Recognition API to verify selfie against ID photo.
    - Upon successful verification, user data is stored in the MongoDB database.

3. **Chatbot Interface:**
    - Users interact with the chatbot to request services, upload documents, and receive responses.
    - Supports multimodal input (text, image, voice, and file upload) and output (text, image, links, and files).
    - Captures user input and sends it to the Backend MCP layer via REST API calls.
    - Displays responses received from the Backend MCP layer.
    - Integrates notification service for reminders, transaction alerts, and OTP messages.

### Layer II - Backend MCP (Middleware & Application Logic)
The Backend MCP (MyGov Central Processor) orchestrates all API calls, workflow logic, and integration between the frontend and backend microservices.

It is built entirely on Amazon Bedrock, AWS Lambda functions, API Gateway, and MongoDB Atlas triggers for event-driven workflows.

- **Core Responsibilities:**
    - Manage session state, route API requests, and invoke appropriate Lambda functions.
    - Handle message processing, LLM integration, and response generation for chat sessions.
    - Bridge communication between frontend (React Native app) and backend microservices.
    - Amazon Bedrock is used for LLM inference and response generation.

- **Key Components:**
    - Amazon Bedrock: Provide comprehensive, secure and flexible access to leading foundation models to build generative AI app and agent. It powers the conversational AI, handling intent recognition, response generation, and context management.
    - AWS Lambda: Execute backend business logic, including API integrations, database operations, and workflow orchestration.
    - MongoDB Atlas Triggers: Execute Lambda functions in response to database events, such as checking status for government services and payment confirmations.

- **Conversation Management:**
    - The MCP maintains a session state for each user, tracking conversation history and context. This ensures coherent, multi-turn conversations.
    - Each session is uniquely identified via `session_id`.
    - Conversation context is stored in MongoDB (short-term) and fetched on each request.
    - AI responses are generated via AWS Bedrock LLM API, processed, and sent back to the app.

### Layer III - Backend Services (Specialized Microservices)

This layer consists of specialized microservices that handle distinct functionalities, including eKYC verification, document processing, speech-to-text conversion, payment processing, and notification services.

1. **eKYC Services:**
    - OTP Verification:
        - Dual-channel (SMS/Email) OTP delivery via AWS SNS and SES.
        - OTP stored in MongoDB with 5-minute expiry.
    - Facial Recognition:
        - Compares selfie from S3 with ID photo using AWS Rekognition.
        - Returns similarity score and verification decision.
    - Document Validation:
        - Uses AWS Textract for OCR text extraction.
        - Validates extracted text against government templates (e.g., license number, NRIC format).

2. **Chabot Services:**
    - Audio Transcription Service:
        - Uses AWS Transcribe to convert user speech to text.
        - Text is passed to the LLM agent for natural language understanding.
    - Document Analysis and Text Extraction:
        - AWS Textract → AWS Lambda parsing → MongoDB Vstore.
        - Extracted text is summarized or verified through the MCP and displayed to user.
    - Receipt and PDF Generator:
        - AWS Lambda compiles templates and stores PDFs in S3.
        - URLs are shared in chat for download or verification.
    - Notification Service:
        - Scheduled AWS EventBridge + Lambda send reminders (due bills, expiry dates).
        - Notifications are stored in MongoDB and pushed to frontend.

3. **Payment Services:**
    - Bill Creation (Billplz Integration):
        - Lambda creates bills through Billplz API with metadata (amount, user ID, service type).
        - Frontend receives the payment URL and opens in WebView.
    - Payment Webhook Handling:
        - Billplz sends callback → AWS Lambda verifies payment → MongoDB updates transaction status.
        - Trigger receipt generation via PDF service.

### Layer IV - Database (Data Storage)

All data persistence is managed by MongoDB Atlas, providing a fully managed NoSQL environment with auto-scaling and TTL-based cleanup.

- Store **governmental user profiles**, conversation history, payment transactions, and other metadata.
- **Data Lifecycle Management:**
    - Atlas Triggers automate periodic cleanup (e.g., delete expired OTPs).
    - TTL Indexes ensure transient data (chat sessions, OTPs) auto-expire after 7 days.
    - Scheduled Aggregations perform data normalization for reporting dashboards.
- **Integration Points:**
    - MongoDB Realm Triggers → AWS Lambda (event-driven updates)
    - Backend MCP fetches and updates documents via Atlas Data API
    - Indexed searches accelerate eKYC lookups and document matching.

## 🗺️ Architecture Diagram
<!-- ![Architecture Diagram](assets/Full_Architectural_Diagram.png) -->
<img src="assets/Full_Architectural_Diagram.png" alt="The full Architecture Diagram of this system"/>




## ✨ Special Features

- **Conversational AI**: Citizens interact in natural language, making it feel like talking to a helpful assistant instead of navigating rigid menus.

- **Speech-to-Text Integration**: Users can simply speak their requests — enhancing accessibility for seniors and non-digital natives.

- **OCR + GenAI**: Snap a photo of documents like TNB bills, ICs, or summons, and the system automatically processes them, thus proceed for payment.

- **WhatsApp Frontend**: Leverages an existing, widely-used platform to reduce infrastructure costs while ensuring broad accessibility.

- **Scalable SaaS Model**: Offered as a subscription service to government agencies and GLCs, designed to grow and expand sustainably.

- **Cost-Optimized Backend**: Built on serverless and pay-per-use architecture to minimize operational costs and maximize efficiency.


## 📱 MyGovHub Interface Overview
<div style="display: flex; justify-content: center; align-items: center; gap: 20px; flex-wrap: wrap;">

  <div style="text-align: center;">
    <img src="assets/interface_entry_page.jpeg" alt="Welcome Page" height="400" style="border:none; outline:none;"/><br/>
    <b>Welcome Page</b>
  </div>

  <div style="text-align: center;">
    <img src="assets/interface_chatbot.jpeg" alt="Chatbot Screen" height="400" style="border:none; outline:none;"/><br/>
    <b>Chatbot Conversation</b>
  </div>

  <div style="text-align: center;">
    <img src="assets/interface_settings.jpeg" alt="Settings Page" height="400" style="border:none; outline:none;"/><br/>
    <b>Settings Page</b>
  </div>

</div>




## 🧩 Systems and Repositories
| NO.| System | Description |
| ----------- | ----------- |----------- |
| 1 | [GMAiH Chatbot - MyGovHub AI Assistant](https://github.com/MyGovHub-Goodbye-World/GMAiH-ChatBot-Frontend) | A React Native chatbot application built with Expo for the Great Malaysia AI Hackathon. This WhatsApp-style chat interface connects users with MyGovHub's AI-powered government services assistant. |
| 2 | [Backend Agent MCP](https://github.com/MyGovHub-Goodbye-World/backend-agent-mcp) | A serverless AWS Lambda function providing AI-powered government service assistance for MyGovHub, handling license renewal and TNB bill payments. |
| 3 | [eKYC Backend API Documentation](https://github.com/MyGovHub-Goodbye-World/ekyc-backend) | A serverless backend API for eKYC document verification using OCR and database matching, powered by AWS Lambda and Python. |
| 4 | [Document Ingestion and Text Extraction Service](https://github.com/MyGovHub-Goodbye-World/document-ingestion-and-text-extraction) | A comprehensive document analysis tool that combines AWS Textract, Bedrock, and intelligent blur detection. Available as both CLI and serverless Lambda API. |
| 5 | [OTP Verification API](https://github.com/MyGovHub-Goodbye-World/otp-verification-api) | A serverless API providing a complete solution for sending and verifying one-time passwords (OTPs) via both SMS and email. It is built to be deployed on AWS and leverages AWS Lambda, API Gateway, SNS for SMS, and SES for email notifications. OTP records are stored and managed in a MongoDB database.|
| 6 | [Face Recognition API](https://github.com/MyGovHub-Goodbye-World/face-rekon-api) | A Serverless API providing a complete AWS-based solution for comparing faces and analyzing selfie image quality using Amazon Rekognition. It is designed to help verify user identity by comparing a selfie photo with an ID card image stored in Amazon S3.|
| 7 | [Notification Service](https://github.com/MyGovHub-Goodbye-World/notification-greatai) | AWS Lambda function which fetches data from MongoDB, notifying MyGovHub users about upcoming & overdue bill payments or documentation renewal reminders. Setup includes 2 seperate AWS Lambda functions.|
| 8 | [S3 Upload API Service](https://github.com/MyGovHub-Goodbye-World/s3-api) | A serverless file upload service built with AWS Lambda, API Gateway, and S3. This service allows you to upload files to S3 and get pre-signed download URLs. |
| 9 | [AWS Transcribe API Service](https://github.com/MyGovHub-Goodbye-World/transcribe-api) | A serverless AWS Lambda function that provides multi-language audio/video transcription services using AWS Transcribe. This service supports English, Chinese, Malay, and Indonesian languages and accepts S3 URLs for audio/video files. |
| 10 | [PDF Receipt Generator API](https://github.com/MyGovHub-Goodbye-World/transcribe-api) | A serverless AWS Lambda function that generates PDF receipts for TNB bills, driving licenses, and transactions. The API processes JSON data and returns secure, time-limited download URLs for generated PDF receipts. |
| 11 | [Billplz Payment Service API](https://github.com/MyGovHub-Goodbye-World/billplz-payment-api) | Serverless payment service for MyGovHub that integrates with the Billplz payment gateway and MongoDB for transaction management. |
| 12 | [MongoDB Database Setup Guide](https://github.com/MyGovHub-Goodbye-World/mygovhub-mongodb) | This guide helps you set up and import multiple MongoDB databases for the Great AI Hackathon project. The setup includes 5 separate databases with Malaysian government service data.











---

## 📊 Summary
MyGov Hub redefines how citizens interact with the government — by turning dozens of disjointed apps into a single AI-powered experience.

It’s simple, scalable, and human-friendly, designed to:
- 💡 Save citizens time
- 💰 Reduce government expenses
- 🌐 Accelerate digital-first adoption

