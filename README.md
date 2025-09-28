# Introduction

This is a simple but powerful workflow that creates a Retrieval-Augmented Generation (RAG) system for your FAQ documents. It automatically updates a Pinecone vector database whenever a new file is added to a specific Google Drive folder and then uses that database to power a conversational AI agent.

## How it works: The Automatic Document Ingestion Pipeline
This part of the workflow ensures your AI is always up-to-date with your latest FAQs. It's an automated process that runs whenever a new document is added.

1. Google Drive Trigger: The workflow starts when a new file is created in the designated Google Drive folder (named "FAQ" in this case). It runs every minute, checking for new content.
2. Download file: The newly created file is downloaded from Google Drive into the workflow.
3. Default Data Loader: This node reads the binary data of the downloaded file.
4. Recursive Character Text Splitter: The text from the document is broken down into smaller, manageable chunks. This is a crucial step for RAG, as it helps the vector database store and retrieve precise pieces of information.
5. Embeddings Google Gemini: Each text chunk is converted into a vector embedding (a numerical representation) using the powerful Google Gemini Embeddings model.
6. Pinecone Vector Store: The generated embeddings are then inserted into the Pinecone vector database under the namespace "FAQ" in the "demo" index, making them instantly searchable by the AI agent.

## it works: The Conversational AI Agent
This part of the workflow handles real-time questions and answers using the information stored in your vector database.

1. When chat message received: The workflow is triggered by an incoming chat message (like from Telegram, Slack, or any supported chat platform).
2. AI Agent: This is the core intelligence node. It orchestrates the entire response.
3. Google Gemini Chat Model: This node provides the large language model (LLM) that powers the conversation and generates human-like responses.
4. Pinecone Vector Store1 (Retrieve as Tool): This node is configured as a tool for the AI Agent. When the agent receives a question, it uses this tool to search the "FAQ" namespace in the "demo" Pinecone index.
5. It uses the Embeddings Google Gemini1 node to convert the user's question into an embedding for the search.
6. The agent then retrieves the most relevant document chunks (the "R" in RAG) from the Pinecone index.

Reply to User's query: The AI Agent takes the retrieved document chunks and the user's original question to generate a final, informed, and accurate answer (the "G" in RAG) back to the user via the chat platform.

This two-part design ensures a continuous loop: any new document automatically updates the knowledge base, and the chat agent can instantly use that new information to answer user questions.
