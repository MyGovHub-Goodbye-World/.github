# Project Description: MyGov Hub

### Project Overview

**MyGov Hub** is an **AI-powered, unified government services platform** designed to simplify how citizens interact with public services. Instead of juggling multiple apps, citizens access everything through a **single conversational interface,**  text or voice, integrated into familiar platforms like WhatsApp. From renewing licenses to paying summons, or even uploading a photo of a bill, MyGov Hub makes government services **as easy as chatting with a friend**.

---

### The Problem It Solves

Malaysia currently has nearly **300 and 1500 agency websites/ portals different government apps**, each serving different functions such as license renewals, passport applications, or bill payments. This fragmented system:

- Creates **confusion** and inconvenience for citizens, especially seniors.
- Leads to **duplication and inefficiency**, both for the public and the government.
- Increases **operational costs** for maintaining and managing multiple portals.

MyGov Hub addresses these issues by unifying multiple services into one accessible, cost-efficient, and user-friendly platform.

---

### Technologies Used

**MyGovHub Technical Architecture**

MyGovHub's architecture is a multi-layered system designed to process user requests, primarily involving document analysis and voice commands. Here is a technical elaboration of the key services and their functions within the pipeline.

1. **Front-end Layer**

The front-end is the user-facing application that initiates the entire process. It can capture user input in two forms: document uploads and voice commands. The MCP (MyGovHub Control Plane) layer acts as a reverse proxy and API gateway, routing these requests to the appropriate backend services.

1. **AWS Lambda**

AWS Lambda functions are the core of the system's event-driven orchestration. They are stateless, serverless compute services that execute code in response to triggers. In this pipeline, Lambdas are triggered by events such as a new file upload to an S3 bucket or an API call from the front end.

Orchestration Pipeline: The Lambda blueprint outlines a sequential workflow. When a trigger event occurs, the main Lambda function is invoked. This function acts as a coordinator, managing the flow of data between different services. For example, it will first send a document to Textract, then take Textract's output and send it to Bedrock, and finally store the processed data in the data layer.

1. **Amazon Textract**

Amazon Textract is a machine learning service that automatically extracts text, handwriting, and data from scanned documents. It's used twice in this pipeline for a two-step recognition process:

Identity Verification: The first Textract call analyzes a user's uploaded IC (Identity Card) to extract key information like the name, IC number, and address. This raw text data is then passed to the next stage.

Document Classification: The second Textract call analyzes a different document (e.g., a form or a letter) to extract the primary text content. The extracted text is then used by the Bedrock service to determine the document's type and the user's intended service.

1. **Amazon Bedrock**

Amazon Bedrock is a fully managed service that provides access to foundation models from leading AI companies. It is crucial for transforming raw, extracted text into structured, actionable data.

Data Serialization: Bedrock takes the raw text from Textract and uses a large language model to parse and serialize it into a structured JSON format. This makes the data easily consumable by other services and the database.

Document Categorization: Using the text content and context, Bedrock classifies the document into predefined categories (e.g., "driving license renewal," "tax filing," "utility bill inquiry"). This classification is critical for directing the user's request to the correct internal service.

Intent Recognition: The service uses Bedrock's NLP (Natural Language Processing) capabilities to identify the user's primary goal or intent from the document's content. This intent is then logged in the database to streamline the user's journey.

1. **Amazon S3 (Simple Storage Service)**

Amazon S3 is used as a highly scalable object storage solution.

Document Storage: When a user uploads a document or media file, it is stored in a designated S3 bucket. This serves as the primary repository for all user-submitted files.

Secure URLs: S3 is configured to generate pre-signed URLs. These are time-limited, temporary URLs that grant a user secure access to a specific object (in this case, the transaction receipt PDF) without making the entire bucket public.

1. **Speech-to-Text (STT) Function**

This function converts a user's spoken voice command into a text string. The resulting text is then handled as a standard prompt, passing through the same Bedrock and data processing pipeline as a text-based request. This integration allows the system to seamlessly handle both written and spoken input.

1. **SQLite**

SQLite is a lightweight, file-based relational database. 

Local Chat Memory: It is used to store chat memory locally on the user's device or in a session-specific context. This allows the conversation to be stateful, enabling the chatbot to remember previous interactions and provide more coherent and contextual responses. This avoids the overhead and latency of making constant calls to a cloud-based database for every turn of the conversation.

---

### Special Features

- **Conversational AI**: Citizens interact in natural language, making it feel like talking to a helpful assistant instead of navigating rigid menus.
- **Speech-to-Text Integration**: Users can simply speak their requests — enhancing accessibility for seniors and non-digital natives.
- **OCR + GenAI**: Snap a photo of documents like TNB bills, ICs, or summons, and the system automatically processes them, thus proceed for payment.
- **WhatsApp Frontend**: Leverages an existing, widely-used platform to reduce infrastructure costs while ensuring broad accessibility.
- **Scalable SaaS Model**: Offered as a subscription service to government agencies and GLCs, designed to grow and expand sustainably.
- **Cost-Optimized Backend**: Built on serverless and pay-per-use architecture to minimize operational costs and maximize efficiency.

---

In summary, **MyGov Hub transforms multiple government services into one unified, AI-driven experience, thus saving time for citizens, cutting costs for government, and fostering digital-first adoption.**