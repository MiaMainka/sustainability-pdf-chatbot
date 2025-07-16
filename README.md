# AI-based Applications By Team BrAInstorm
# Conversational Sustainability Report Analysis Through Chatbots
A lightweight web application designed for the interactive analysis of sustainability reports.Users can upload PDF files and ask questions in natural language to receive clear answers or structured summaries in JSON format.

The application leverages a Retrieval-Augmented Generation (RAG) approach and integrates a Large Language Model via the GWDG API to generate context-aware responses.

###    Team Members

| Name          | Student Number |
|---------------|----------------|
| Mia Mainka    | 3047072        |
| Marisa Jordan | 4000444        |
| Daniel Kähm   | 4001842        |
| Nadeem Hakimi | 4000409        |

## Project Description

This project was developed as part of the seminar **AI-based Applications** at Leuphana University of Lüneburg. The seminar focused on applying Large Language Models (LLMs) and AI architectures to real-world use cases in a practical, project-oriented setting.

The goal of our project was to design a system that simplifies access to information within lengthy sustainability reports. Instead of manually searching for key content, users interact with the report through questions.

The system focuses on three main capabilities:
- Uploading and extracting text from PDFs
- Retrieving relevant sections using vector similarity
- Generating responses with a hosted large language model

This approach is especially helpful for identifying topics such as CO₂ emissions, risk factors, environmental strategies, and future targets in company reports.


---

##  Installation & Usage Guide

### Requirements

To run this project, ensure the following are installed on your system:

- Python 3.13
- `pip` (Python package installer)
-  PyCharm (Recommended) or Visual Studio Code as IDE

### 1. Install Dependencies
Open a terminal in the project root folder and run:

`pip install -r requirements.txt`

### 2.Configure API Access
Create or edit the config.ini file and insert your GWDG API credentials:

**[API]**

api_key = YOUR_API_KEY

api_endpoint = YOUR_ENDPOINT_URL

### 3. Launch the Application
Start the local development server with:

`python backend5.py`

The app will run on localhost:5000 by default.

### 4. Use the Application
In your browser, navigate to `http://localhost:5000`.  
You can:
- Upload a sustainability report in PDF format
- Ask natural language questions about its content
- View responses in text or structured JSON format
- Download results or clear/reset the interface

##  Demo Video

A short demonstration video is available in the GitHub repository. It showcases:

- Uploading a PDF
- Asking example questions
- Interpreting answers and using additional features

You can view the video file directly in the repository under:

**Filename**: `Ai-Based-Applications.mp4`

---
## System Architecture
Our system is built around a local RAG (Retrieval-Augmented Generation) pipeline. The following components work together to transform a sustainability report PDF into a structured, conversational interface:

1. **Frontend (HTML/CSS)**  
   - Allows the user to upload a PDF and enter questions  
   - Sends form data to the Flask backend via HTTP POST  
   - Displays model responses (text or JSON)

2. **Backend (Flask)**  
   - Manages routing, file uploads, and API calls  
   - Reads the uploaded PDF and extracts raw text  
   - Coordinates chunking, embedding, and answer generation

3. **PDF Processing (PyMuPDF)**  
   - Extracts plain text from the uploaded PDF  
   - Handles multiple pages and removes empty content

4. **Chunking (LangChain)**  
   - Uses `RecursiveCharacterTextSplitter` to break the text into overlapping chunks  
   - Ensures that each chunk maintains meaningful context without exceeding token limits

5. **Embedding (HuggingFace via LangChain)**  
   - Converts each text chunk into a dense vector representation using MiniLM  
   - This enables similarity-based retrieval later on

6. **Vector Store (FAISS)**  
   - Stores the embeddings locally  
   - On each question, the system performs a similarity search to find the top-k relevant chunks

7. **LLM Access (GWDG API)**  
   - Sends the user question and retrieved context to a hosted LLM (Meta-LLaMA 3.1)  
   - Receives the final answer as either a natural language response or a structured JSON output

All components are run **locally** during development. The vector database and file handling are temporary and reset with each new session.


---
## Development Process

We approached development in a structured and iterative way, inspired by agile workflows. At the start of the project, we set up our development infrastructure: Trello for task management and GitHub for version control. Development environments were aligned using VS Code and PyCharm.

In early sessions, we worked together to clarify the task, brainstorm technical solutions, and break the implementation down into manageable tasks. These were tracked and updated regularly on our Trello SCRUM board.

The development then proceeded in logical phases:

###  Setup & Planning
- Created project repositories and cloud workspace
- Defined responsibilities and established communication tools
- Clarified project requirements and possible data flow

###  Core Development
- **Backend**:
  - Implemented Flask routing and server endpoints
  - Integrated PyMuPDF for PDF parsing
  - Developed embedding logic using LangChain and MiniLM
  - Stored chunks in FAISS vector database
- **Frontend**:
  - Built HTML/CSS layout for PDF upload and question input
  - Implemented result display and UI enhancements
  - Added features like clear/reset buttons and loading indicators

###  LLM Integration
- Connected to Meta-LLaMA 3.1 via GWDG API
- Structured prompt generation using context chunks
- Implemented logic to return either natural language or JSON output

###  Testing & Optimization
- Validated full input-output cycle
- Resolved PDF reload issues and refined chunking process
- Tuned performance for faster interaction

###  Finalization
- Recorded a demo video and designed the final presentation
- Completed all documentation including this README
- Final project structure and code cleanup

Work was reviewed jointly in group meetings and discussions. Even though tasks were assigned individually, major components were integrated and evaluated collaboratively.

---
##  Technologies Used

This application integrates modern web development and natural language processing tools into a modular architecture.  
Each tool or library has a distinct responsibility within the pipeline — from interface rendering to vector-based retrieval and LLM querying.

| Category              | Tool / Library                             | Purpose                                         |
|-----------------------|---------------------------------------------|-------------------------------------------------|
| Web Framework         | Flask (Python)                              | Backend routing and server logic                |
| Frontend              | HTML + CSS                                  | Minimal, responsive user interface              |
| PDF Processing        | PyMuPDF (`fitz`)                            | Extracts text from uploaded PDF reports         |
| Text Chunking         | LangChain `RecursiveCharacterTextSplitter`  | Splits raw text into semantically coherent blocks|
| Embedding             | HuggingFace MiniLM (via LangChain)          | Converts text chunks to vector representations  |
| Vector Storage        | FAISS                                       | Local storage and fast similarity search        |
| LLM Integration       | Meta-LLaMA 3.1 via GWDG API                 | Generates answers and structured JSON           |
| Configuration         | ConfigParser                                | Reads API keys and settings from `config.ini`   |
| File Management       | OS, pathlib                                 | File upload handling and temporary storage      |
| Version Control       | GitHub                                      | Repository hosting and collaboration            |
| Project Management    | Trello                                      | SCRUM-based planning and task tracking          |


---
## Challenges & Solutions

During development, our team encountered several challenges which shaped the final implementation. Addressing these issues helped improve the robustness and clarity of the application.

- **Prompt Reliability**: Early LLM prompts led to inconsistent or overly verbose answers. We refined our prompt structure and tested edge cases to improve accuracy and control.

- **Chunking Granularity**: Finding the right chunk size for embedding was non-trivial. Too small caused loss of context, too large exceeded token limits. We resolved this by configuring LangChain’s RecursiveCharacterTextSplitter with overlapping blocks.

- **PDF Format Variability**: Some PDFs had unreadable layouts or irrelevant content. We implemented preprocessing filters to clean empty pages and standardize text extraction.

- **Token and Output Length Limits**: Long queries or verbose chunks led to API failures. We introduced length guards and adapted the pipeline to pass only the most relevant top-k chunks.

- **GWDG API Stability**: Occasional slowdowns or formatting inconsistencies required fallback handling and prompt validation.

- **User Interface Integration**: Ensuring fast response cycles without reloading the PDF or breaking layout required careful coordination between frontend and backend logic.

Each of these challenges contributed to a more thoughtful, test-driven implementation process.

---
## Contributions
Most components of the application were developed collaboratively, especially in the early phases during group coding and planning sessions. Over time, individual members focused on specific aspects while continuing to contribute to team-wide reviews, testing, and documentation.

The division of responsibilities developed as follows:

- **Backend & LLM Integration** – Nadeem Hakimi, Marisa Jordan  
  Setup of Flask backend, API logic, embedding and retrieval pipeline

- **Frontend & UI Design** – Everyone  
  Layout design, chat-view interaction, and usability enhancements

- **Prompt Design & JSON Output** – Everyone  
  Refinement of prompts for structured and natural language output

- **SCRUM & GitHub Management** – Everyone  
  Task tracking, version control and collaborative review process

- **Presentation** – Mia Mainka, Daniel Kähm  
  Slide content, speaker preparation and visual structure

- **README Documentation** – Marisa Jordan, Mia Mainka  
  Final project description, usage guide and architecture summary

- **Demo & Recording** – Daniel Kähm  
  Recording and narration of demonstration video

This distribution allowed for flexibility, ensured consistent quality, and reflected each member’s strengths.

