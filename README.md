"# GraphRAG-with-Llama-3.1" 
Setup steps:

python3 -m venv .venv
source .venv/bin/activate
docker compose up

Setup new .env file with the following:
OPENAI_API_KEY=get_openai_api_key_and_replace_this_string
NEO4J_URI=bolt://localhost:7687
NEO4J_USERNAME=neo4j
NEO4J_PASSWORD=your_password

Ensure that your OpenAI API account has the supports the default embeddings model: "text-embedding-ada-002".
A better model to use is "text-embedding-3-large".  To change to this, edit line 121 in base.py

Run the cells in the notebook: enhancing_rag_with_graph.ipynb

Original source: https://github.com/Coding-Crashkurse/GraphRAG-with-Llama-3.1

Changes made:

Added in cell 1:
json-repair

Added in cell 12:
def create_fulltext_index(tx):
    driver = GraphDatabase.driver(
        uri = os.environ["NEO4J_URI"],
        auth = (os.environ["NEO4J_USERNAME"],
                os.environ["NEO4J_PASSWORD"]))
    tx.run("CREATE FULLTEXT INDEX entity FOR (n:Entity) ON EACH [n.name]")

    # Open a session and execute the index creation
    with driver.session() as session:
        session.write_transaction(create_fulltext_index)

    print("Full-text index 'entity' created successfully!")

    # Close the driver when done
    driver.close()