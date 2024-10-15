This is a report detailing my approach to this project as well as the challenges I faced and how I solved them.

1. Data Handling

PDF Extraction: 
Used libraries like PyPDF2 to extract text from PDF documents.

2. Embedding Generation

Model Selection:
I used the SentenceTransformer model, specifically the all-MiniLM-L6-v2 variant, which is a pre-trained model designed for generating sentence embeddings. This model is efficient and captures semantic information from text.

Text Extraction:
Text is extracted from PDF documents using the get_pdf_text function. This function reads each PDF file and concatenates the extracted text into a single string, making it ready for processing.

Advanced Chunking:
The advanced_chunking function is responsible for dividing the extracted text into semantically meaningful chunks.
The text is split into individual sentences using spaCy’s NLP capabilities. Each sentence is extracted and stored in a list.
NER is applied to identify important entities within the text, such as names, dates, and locations. This helps ensure that the final chunks contain contextually relevant information.

Generating Sentence Embeddings:
Each sentence is transformed into a dense vector representation that captures its semantic meaning. The output is a NumPy array where each row corresponds to a sentence.

Cosine Similarity Calculation:
A similarity matrix is computed using cosine similarity to assess how similar each sentence is to the others. This matrix enables the grouping of sentences based on their semantic similarity

Chunking Based on Similarity:
The sentences are grouped into chunks based on a predefined similarity threshold (in this case, 0.8). If the cosine similarity between the current chunk’s embedding and a new sentence exceeds this threshold, the sentence is added to the current chunk.
When the threshold is not met, the current chunk is finalized, and a new chunk begins

Filtering Chunks with NER:
After forming the initial chunks, the code checks whether any named entities identified earlier are present in each chunk. This ensures that the final output is contextually relevant

The function returns a list of final text chunks, each represented as a string. These chunks are then used to generate embeddings for storage in a vector database (FAISS)

3. RAG Pipeline Development

Pipeline Architecture:
Developed a Retrieval-Augmented Generation (RAG) pipeline that combined a retrieval component and a generative language model.

Retrieval:
Implemented a conversational retrieval mechanism using LangChain, which utilized the FAISS vector store to retrieve relevant text chunks based on user queries.

Response Generation:
Integrated a language model (e.g., ChatGoogleGenerativeAI) to generate coherent responses based on the retrieved chunks. The model generated text by conditioning on the retrieved context.


CHALLENGES & SOLUTIONS

-> Limited Resources: I could not use GPT models or OpenAI Embeddings as I did not have any credits left. So, I used the the Gemini LLM Model and got the Gemini API Key but this also has a rate limit. The Resource Exhausted error crops up in case limit or quota has been exceeded when using Google's APIs.

-> Long Embedding Process: Earlier I was working with a Kaggle dataset. I pre-loaded the dataset and subsequently created chunks for it and generated embeddings but this process took too long. Hence, I choose a functionality where the user will upload the documents. This proved to be a better approach to demonstrate the functioning of the application built.

-> REST API: Although I could have created a REST API to expose the pipeline, but I didn't do it. The simple reason is that Streamlit apps can handle user interactions directly within the app interface, making it unnecessary to create a separate API layer for the same functionality.
