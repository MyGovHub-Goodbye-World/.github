# MyGovHub

<img src="MyGovHub-Logo-Dark.png"/>


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
MyGovHub's architecture is a multi-layered system designed to process user requests, primarily involving document analysis and voice commands. Here is a technical elaboration of the key services and their functions within the pipeline.





## Special Features

- **Conversational AI**: Citizens interact in natural language, making it feel like talking to a helpful assistant instead of navigating rigid menus.

- **Speech-to-Text Integration**: Users can simply speak their requests — enhancing accessibility for seniors and non-digital natives.

- **OCR + GenAI**: Snap a photo of documents like TNB bills, ICs, or summons, and the system automatically processes them, thus proceed for payment.

- **WhatsApp Frontend**: Leverages an existing, widely-used platform to reduce infrastructure costs while ensuring broad accessibility.

- **Scalable SaaS Model**: Offered as a subscription service to government agencies and GLCs, designed to grow and expand sustainably.

- **Cost-Optimized Backend**: Built on serverless and pay-per-use architecture to minimize operational costs and maximize efficiency.





## Systems and Repositories
| System | D |
| ----------- | ----------- |
| [S3 Upload API Service](https://github.com/MyGovHub-Goodbye-World/s3-api) | A React Native chatbot application built with Expo for the Great Malaysia AI Hackathon. This WhatsApp-style chat interface connects users with MyGovHub's AI-powered government services assistant. |
| [AWS Transcribe API Service](https://github.com/MyGovHub-Goodbye-World/transcribe-api) | A serverless AWS Lambda function that provides multi-language audio/video transcription services using AWS Transcribe. This service supports English, Chinese, Malay, and Indonesian languages and accepts S3 URLs for audio/video files. |
| [S3 Upload API Service](https://github.com/MyGovHub-Goodbye-World/s3-api) | A serverless file upload service built with AWS Lambda, API Gateway, and S3. This service allows you to upload files to S3 and get pre-signed download URLs. |
| [Billplz Payment Service API](https://github.com/MyGovHub-Goodbye-World/billplz-payment-api) | Serverless payment service for MyGovHub that integrates with the Billplz payment gateway and MongoDB for transaction management. |
| [MongoDB Database Setup](https://github.com/MyGovHub-Goodbye-World/mongodb-setup) | A guide to set up and import multiple MongoDB databases for the Great AI Hackathon project. The setup includes 5 separate databases with Malaysian government service data. |
| **MyGovHub Core Orchestrator** | The main orchestration service that manages the flow of data through the AI pipeline, coordinating AWS Lambda, Textract, and Bedrock services. |
| **Identity & Document Processing API** | A serverless API service for processing uploaded identity cards (ICs) and other documents using Amazon Textract and Bedrock to extract and categorize information. |
| **MyGovHub Intent Recognition Service** | A microservice built on Amazon Bedrock that analyzes text from the conversational interface to determine the user's goal or intent, directing the request to the correct internal service. |
| **MyGovHub Front-end (WhatsApp)** | The codebase for the WhatsApp chatbot integration, handling message routing, user authentication, and API calls to the core MyGovHub services. |





---

## Summary
MyGov Hub transforms multiple government services into one unified, AI-driven experience, thus saving time for citizens, cutting costs for government, and fostering digital-first adoption.

