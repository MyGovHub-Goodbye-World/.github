# MyGovHub

<img width="750" height="750" alt="Image" src="https://github.com/user-attachments/assets/5388e50d-9271-4a01-9e96-976cb3d2bf29" />


## Project Overview

**MyGov Hub** is an **AI-powered, unified government services platform** designed to simplify how citizens interact with public services. Instead of juggling multiple apps, citizens access everything through a **single conversational interface,**  text or voice, integrated into familiar platforms like WhatsApp. From renewing licenses to paying summons, or even uploading a photo of a bill, MyGov Hub makes government services **as easy as chatting with a friend**.





## Problem Statement
Malaysia's current digital government landscape is fragmented, with nearly 200 government websites and 1,500 different government apps serving various functions from passport applications to bill payments [(Source: MadeinMalaysia)](https://madeinmalaysia.com.my/malaysia-government-apps-and-websites/). This fragmented system creates several issues:
- **Citizen Confusion**:  
Navigating a multitude of apps and portals is inconvenient and confusing, especially for seniors and those less familiar with technology.

- **Operational Inefficiency**:  
This patchwork of systems leads to duplication of efforts and inefficiencies for both the public and government agencies.

- **High Costs**:  
Maintaining and managing numerous separate portals results in increased operational costs.

MyGov Hub addresses these issues by unifying multiple services into one accessible, cost-efficient, and user-friendly platform.






## Technical Architecture
MyGov Hub's architecture is a multi-layered, serverless system designed for efficient processing of user requests, particularly those involving document analysis and voice commands.

* **Front-end & API Gateway**: The user-facing application captures input via document uploads or voice commands. The **Model Context Protocol (MCP)** acts as a conceptual framework that governs how session state and contextual information are managed and passed between different services. It ensures the conversation flow remains coherent and stateful.
* **Orchestration Pipeline (AWS Lambda)**: **AWS Lambda** functions are the core of the system's event-driven orchestration. Triggered by events like file uploads to an S3 bucket, a main Lambda function coordinates the flow of data between services. For example, it sends a document to Textract, then Textract's output to Bedrock, and finally stores the processed data, all while adhering to the Model Context Protocol.
* **Document Processing (Amazon Textract & Bedrock)**:
    * **Amazon Textract** is a machine learning service that extracts text, handwriting, and data from uploaded documents. It's used for **Identity Verification** (from an IC) and **Document Classification** (from forms or bills).
    * **Amazon Bedrock**, a managed service providing access to leading foundation models, transforms the raw text from Textract into structured, actionable data. It performs **Data Serialization** into JSON, **Document Categorization** (e.g., "tax filing"), and **Intent Recognition** (e.g., "pay summon"). The data is passed to Bedrock with the context provided by the MCP.
* **Data Storage (Amazon S3 & SQLite)**:
    * **Amazon S3** (Simple Storage Service) is the highly scalable object storage for all user-submitted files. It also generates secure, temporary pre-signed URLs for things like transaction receipts.
    * **SQLite** is a lightweight, file-based relational database used for **Local Chat Memory**. This allows the conversational AI to remember previous interactions and provide coherent, contextual responses without constant calls to a cloud database, which is a key function of the MCP.
* **Speech-to-Text (STT)**: A dedicated function converts a user's spoken voice command into a text string, which is then processed through the same pipeline as a text-based request.






## Special Features

- **Conversational AI**: Citizens interact in natural language, making it feel like talking to a helpful assistant instead of navigating rigid menus.

- **Speech-to-Text Integration**: Users can simply speak their requests — enhancing accessibility for seniors and non-digital natives.

- **OCR + GenAI**: Snap a photo of documents like TNB bills, ICs, or summons, and the system automatically processes them, thus proceed for payment.

- **WhatsApp Frontend**: Leverages an existing, widely-used platform to reduce infrastructure costs while ensuring broad accessibility.

- **Scalable SaaS Model**: Offered as a subscription service to government agencies and GLCs, designed to grow and expand sustainably.

- **Cost-Optimized Backend**: Built on serverless and pay-per-use architecture to minimize operational costs and maximize efficiency.





## Systems and Repositories
| NO.| System | Description |
| ----------- | ----------- |----------- |
| 1 | [GMAiH Chatbot - MyGovHub AI Assistant](https://github.com/MyGovHub-Goodbye-World/GMAiH-ChatBot-Frontend) | A React Native chatbot application built with Expo for the Great Malaysia AI Hackathon. This WhatsApp-style chat interface connects users with MyGovHub's AI-powered government services assistant. |
| 2 | [Backend Agent MCP](https://github.com/MyGovHub-Goodbye-World/backend-agent-mcp) | A serverless AWS Lambda function providing AI-powered government service assistance for MyGovHub, handling license renewal and TNB bill payments. |
| 3 | [eKYC Backend API Documentation](https://github.com/MyGovHub-Goodbye-World/ekyc-backend) | A serverless backend API for eKYC document verification using OCR and database matching, powered by AWS Lambda and Python. |
| 4 | [Document Ingestion and Text Extraction Service](https://github.com/MyGovHub-Goodbye-World/document-ingestion-and-text-extraction) | A comprehensive document analysis tool that combines AWS Textract, Bedrock, and intelligent blur detection. Available as both CLI and serverless Lambda API. |
| 5 | [OTP Verification API](https://github.com/MyGovHub-Goodbye-World/otp-verification-api) | A serverless API providing a complete solution for sending and verifying one-time passwords (OTPs) via both SMS and email. It is built to be deployed on AWS and leverages AWS Lambda, API Gateway, SNS for SMS, and SES for email notifications. OTP records are stored and managed in a MongoDB database.|
| 6 | [Face Recognition API](https://github.com/MyGovHub-Goodbye-World/face-rekon-api) | A Serverless API providing a complete AWS-based solution for comparing faces and analyzing selfie image quality using Amazon Rekognition. It is designed to help verify user identity by comparing a selfie photo with an ID card image stored in Amazon S3.|
| 7 | [S3 Upload API Service](https://github.com/MyGovHub-Goodbye-World/s3-api) | A serverless file upload service built with AWS Lambda, API Gateway, and S3. This service allows you to upload files to S3 and get pre-signed download URLs. |
| 8 | [AWS Transcribe API Service](https://github.com/MyGovHub-Goodbye-World/transcribe-api) | A serverless AWS Lambda function that provides multi-language audio/video transcription services using AWS Transcribe. This service supports English, Chinese, Malay, and Indonesian languages and accepts S3 URLs for audio/video files. |
| 9 | [PDF Receipt Generator API](https://github.com/MyGovHub-Goodbye-World/transcribe-api) | A serverless AWS Lambda function that generates PDF receipts for TNB bills, driving licenses, and transactions. The API processes JSON data and returns secure, time-limited download URLs for generated PDF receipts. |
| 10 | [Billplz Payment Service API](https://github.com/MyGovHub-Goodbye-World/billplz-payment-api) | Serverless payment service for MyGovHub that integrates with the Billplz payment gateway and MongoDB for transaction management. |
| 11 | [MongoDB Database Setup](https://github.com/MyGovHub-Goodbye-World/mongodb-setup) | A guide to set up and import multiple MongoDB databases for the Great AI Hackathon project. The setup includes 5 separate databases with Malaysian government service data. |











---

## Summary
MyGov Hub transforms multiple government services into one unified, AI-driven experience, thus saving time for citizens, cutting costs for government, and fostering digital-first adoption.

