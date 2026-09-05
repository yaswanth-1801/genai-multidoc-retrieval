## Design and Implementation of a Multidocument Retrieval Agent Using LlamaIndex

### AIM:
To design and implement a multidocument retrieval agent using LlamaIndex to extract and synthesize information from multiple research articles, and to evaluate its performance by testing it with diverse queries, analyzing its ability to deliver concise, relevant, and accurate responses.

### PROBLEM STATEMENT:

### DESIGN STEPS:

STEP 1: Data Collection and Preprocessing
Gather a set of research articles or documents relevant to the topic. Preprocess the data by converting the articles into a suitable format (e.g., plain text or structured format). Tokenize the content and remove any irrelevant or noisy information.

STEP 2: Index Construction with LlamaIndex
Use LlamaIndex (formerly known as GPT Index) to create an index for the documents. LlamaIndex will help build an optimized index for efficient retrieval, making it easy to query multiple documents at once. Incorporate features like semantic search to improve relevance and accuracy of retrieval.

STEP 3: Query Handling and Response Generation
Develop the query interface where users can input questions related to the research articles. Integrate the query interface with the LlamaIndex-powered retrieval system. Process the retrieved documents to extract relevant information and synthesize a concise response, potentially using additional techniques like summarization.

STEP 4: Evaluation and Testing
Test the system with a range of diverse queries to evaluate its performance in terms of accuracy, relevance, and conciseness of responses. Collect feedback and refine the system based on test results.

### PROGRAM:
```
from helper import get_openai_api_key
OPENAI_API_KEY = get_openai_api_key()
import nest_asyncio
nest_asyncio.apply()

urls = [
    "https://openreview.net/pdf?id=C9AhjL8aUZ",
    "https://openreview.net/pdf?id=FN20iuPnEU",
    "https://openreview.net/pdf?id=3ZFvXHJwmB",
]

papers = [
    "semantics.pdf",
    "helix.pdf",
    "hdflow.pdf",
]


from utils import get_doc_tools
from pathlib import Path

paper_to_tools_dict = {}
for paper in papers:
    print(f"Getting tools for paper: {paper}")
    vector_tool, summary_tool = get_doc_tools(paper, Path(paper).stem)
    paper_to_tools_dict[paper] = [vector_tool, summary_tool]
initial_tools = [t for paper in papers for t in paper_to_tools_dict[paper]]
from llama_index.llms.openai import OpenAI

llm = OpenAI(model="gpt-3.5-turbo")
len(initial_tools)
from llama_index.core.agent import FunctionCallingAgentWorker
from llama_index.core.agent import AgentRunner

agent_worker = FunctionCallingAgentWorker.from_tools(
    initial_tools, 
    llm=llm, 
    verbose=True
)
agent = AgentRunner(agent_worker)


response = agent.query(
    "Tell me about the evaluation dataset used in semantics, "
    "and then tell me about the evaluation results"

response = agent.query("Give me a summary of helix, semantics and hdflow")
print(str(response))
```

### OUTPUT:
<img width="977" height="160" alt="image" src="https://github.com/user-attachments/assets/e02cb41f-e5bf-41fe-8495-c2e91cab83b2" />
<img width="1300" height="870" alt="image" src="https://github.com/user-attachments/assets/7fef775e-b7b0-49fc-b5ad-3c390b0c5996" />
<img width="1365" height="810" alt="image" src="https://github.com/user-attachments/assets/2fe83c8c-96b2-4d55-8b8c-93fc6457b933" />


### RESULT:
The system successfully retrieves and synthesizes relevant information from multiple documents, providing concise and relevant answers to the user's query. Performance is evaluated based on the accuracy, relevance, and coherence of the responses. The system successfully retrieves and synthesizes relevant information from multiple documents, providing concise and relevant answers to the user's query.
