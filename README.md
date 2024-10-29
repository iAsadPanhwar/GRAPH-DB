# GraphDB with LangChain, Neo4j, and Groq
This project integrates Neo4j for graph database management, Groq for language model operations, and LangChain for handling document conversions. The setup allows for semantic processing of text documents and visual representation of relationships within a Neo4j graph database.

## Project Overview
Using LangChain and Groq, this project extracts and visualizes structured data (nodes and relationships) from unstructured text. Neo4j serves as the backend graph database, enabling graph queries and visualizations. The language model used, Gemma2-9b-It, identifies entities and defines relationships, storing them as graph nodes and relationships in Neo4j.

## Dependencies
Ensure the following dependencies are installed:
```bash
pip install --upgrade langchain langchain-community langchain-groq neo4j langchain_experimental
```
# Configuration
## Neo4j Setup
To connect to your Neo4j database, configure the URI, username, and password in NEO4J_URI, NEO4J_USERNAME, and NEO4J_PASSWORD:
```bash
import os

NEO4J_URI = 'neo4j+s://<your-database-uri>'
NEO4J_USERNAME = 'neo4j'
NEO4J_PASSWORD = '<your-password>'

os.environ['NEO4J_URI'] = NEO4J_URI
os.environ['NEO4J_USERNAME'] = NEO4J_USERNAME
os.environ['NEO4J_PASSWORD'] = NEO4J_PASSWORD
```
## Groq API Key
For LLM functionality, set up the Groq API key as follows:
```bash
os.environ['GROQ_API_KEY'] = "GROQ_API_KEY"
```
## Usage
1. **Initialize Neo4j Graph:**
```bash
from langchain_community.graphs import Neo4jGraph

graph = Neo4jGraph(
    url=NEO4J_URI,
    username=NEO4J_USERNAME,
    password=NEO4J_PASSWORD
)
```
2. **Load and Convert Text to Document:**
Define your input text and transform it into a LangChain Document format.
```bash
from langchain_core.documents import Document

text = """
Elon Reeve Musk (born June 28, 1971) is a businessman and investor known for his key roles...
"""
documents = [Document(page_content=text)]
```
3. **Initialize Language Model and Transformer:**
Load the language model (LLM) and initialize the graph transformer.
```bash
from langchain_groq import ChatGroq
from langchain_experimental.graph_transformers import LLMGraphTransformer

llm = ChatGroq(model_name='Gemma2-9b-It')
llm_transformer = LLMGraphTransformer(llm=llm)
```
4. **Transform Documents into Graph Nodes and Relationships**
Convert the text documents into graph nodes and relationships, then inspect the output:
```bash
graph_documents = llm_transformer.convert_to_graph_documents(documents)
```
5. **View Graph Structure**
Each node and relationship can be visualized or accessed as shown below:
```bash
nodes = graph_documents[0].nodes
relationships = graph_documents[0].relationships
```

## Example Output
The graph_documents object will contain structured nodes and relationships based on the entities and associations identified in the text. Example output:

```bash
[Node(id='Elon Reeve Musk', type='Person'),
 Node(id='Tesla, Inc.', type='Company'),
 Relationship(source=Node(id='Elon Reeve Musk', type='Person'), target=Node(id='Tesla, Inc.', type='Company'), type='KEY_ROLE')]

```



