# How to Create Knowledge Graphs for Documentation

## What is a Knowledge Graph?

A **knowledge graph** is a structured representation of entities and their relationships. It's like a web of interconnected facts.

**Structure**:
```
Subject → Predicate → Object

Examples:
"Authentication Module" → "contains" → "Auth API"
"Auth API" → "requires" → "Database Connection"
"Admin" → "can configure" → "Authentication Module"
"Biometric Auth" → "is type of" → "Authentication"
```

**Visual representation**:
```
[Authentication Module] ──contains──> [Auth API]
                                          │
                                     requires
                                          │
                                          ↓
                                [Database Connection]

[Admin] ──can configure──> [Authentication Module]

[Biometric Auth] ──is type of──> [Authentication]
```

---

## Why Knowledge Graphs for Documentation?

1. **Smart navigation**: "If user reads about Auth API, suggest Database setup"
2. **Prerequisite chains**: Auto-detect what user needs to know first
3. **Role-based recommendations**: Show relevant content for user's role
4. **Gap detection**: Find missing documentation
5. **Better RAG**: Query the graph to find contextually related content
6. **Intelligent content recommendations**: Surface related content beyond keyword matching
7. **Dependency visualization**: Show what needs to be configured/understood first
8. **Automated content organization**: Let the graph suggest navigation structure

---

## Approaches to Creating Knowledge Graphs

### 1. Manual (Small Scale)
Best for: Small doc sets, prototyping, learning
Effort: High upfront, low maintenance

### 2. Semi-Automated (From DITA/Structured Content)
Best for: Existing structured content, controlled vocabularies
Effort: Medium upfront, low maintenance

### 3. Fully Automated (AI Extraction from Unstructured Content)
Best for: Large unstructured doc sets, rapid prototyping
Effort: Low upfront, medium maintenance

### 4. Hybrid (Structured + AI Enhancement)
Best for: Production systems, evolving documentation
Effort: Medium upfront, medium maintenance

---

## Approach 1: Manual Knowledge Graph (Simple CSV/JSON)

### Step 1: Define Your Ontology

For MOSIP docs, you might have:

**Entity Types**:
- Module (UIN, Pre-Registration, Authentication, IDA)
- Component (API, Service, Database, UI)
- Role (Admin, Developer, Operator, Partner)
- Process (Registration, Authentication, ID Issuance)
- Resource (Guide, API Ref, Tutorial, How-to)

**Relationship Types**:
- contains, requires, depends_on
- used_by, configured_by
- is_type_of, part_of
- prerequisite_for, related_to

### Step 2: Create Knowledge Graph in CSV

**entities.csv**:
```csv
id,type,label,description
auth_module,Module,Authentication Module,Handles identity authentication
auth_api,Component,Authentication API,REST API for authentication
auth_service,Component,Auth Service,Backend authentication service
database,Component,Database,PostgreSQL database
admin,Role,Administrator,System administrator
developer,Role,Developer,Application developer
biometric_auth,Process,Biometric Authentication,Fingerprint/iris authentication
auth_guide,Resource,Authentication Guide,How to configure authentication
auth_api_ref,Resource,Auth API Reference,API documentation
```

**relationships.csv**:
```csv
source,relationship,target
auth_module,contains,auth_api
auth_module,contains,auth_service
auth_api,requires,database
auth_service,requires,database
auth_api,used_by,developer
auth_module,configured_by,admin
biometric_auth,is_type_of,authentication
auth_guide,explains,auth_module
auth_api_ref,documents,auth_api
auth_api,prerequisite_for,biometric_auth
```

### Step 3: Load into Graph Database (Neo4j)

**Install Neo4j**:
```bash
# Using Docker
docker run -p 7474:7474 -p 7687:7687 \
  -e NEO4J_AUTH=neo4j/password \
  neo4j:latest
```

**Import script** (Python):
```python
from neo4j import GraphDatabase
import csv

class KnowledgeGraphBuilder:
    def __init__(self, uri, user, password):
        self.driver = GraphDatabase.driver(uri, auth=(user, password))

    def close(self):
        self.driver.close()

    def create_entities(self, csv_file):
        with self.driver.session() as session:
            with open(csv_file, 'r') as f:
                reader = csv.DictReader(f)
                for row in reader:
                    session.run(
                        """
                        CREATE (n:{type} {{
                            id: $id,
                            label: $label,
                            description: $description
                        }})
                        """.format(type=row['type']),
                        id=row['id'],
                        label=row['label'],
                        description=row['description']
                    )

    def create_relationships(self, csv_file):
        with self.driver.session() as session:
            with open(csv_file, 'r') as f:
                reader = csv.DictReader(f)
                for row in reader:
                    session.run(
                        """
                        MATCH (a {{id: $source}})
                        MATCH (b {{id: $target}})
                        CREATE (a)-[:{rel}]->(b)
                        """.format(rel=row['relationship'].upper()),
                        source=row['source'],
                        target=row['target']
                    )

# Usage
kg = KnowledgeGraphBuilder("bolt://localhost:7687", "neo4j", "password")
kg.create_entities("entities.csv")
kg.create_relationships("relationships.csv")
kg.close()
```

### Step 4: Query the Graph

**Cypher queries** (Neo4j's query language):

```cypher
// Find all components in Authentication Module
MATCH (m:Module {id: 'auth_module'})-[:CONTAINS]->(c:Component)
RETURN c.label

// Find prerequisites for Biometric Authentication
MATCH (p)-[:PREREQUISITE_FOR]->(b:Process {id: 'biometric_auth'})
RETURN p.label

// Find what an Admin can configure
MATCH (admin:Role {id: 'admin'})-[:CONFIGURED_BY]->(m)
RETURN m.label

// Find all resources that explain Authentication
MATCH (r:Resource)-[:EXPLAINS]->(auth:Module {id: 'auth_module'})
RETURN r.label

// Find dependency chain (recursive)
MATCH path = (start:Component {id: 'auth_api'})-[:REQUIRES*]->(dep)
RETURN path

// Find all content relevant to a role
MATCH (role:Role {id: 'developer'})-[:USED_BY]->(content)
RETURN content.label, content.type
```

---

## Approach 2: Semi-Automated from Frontmatter/DITA

### Extract from Markdown Frontmatter

**Your docs with enhanced frontmatter**:
```markdown
---
title: Authentication API
type: component
category: api
module: authentication
requires: [database, keymanager]
used_by: [developer, partner]
related:
  - path: /auth/service.md
    type: sibling
  - path: /troubleshooting/auth-errors.md
    type: troubleshooting
---

# Authentication API

This API handles authentication requests...
```

**Extraction script**:
```python
import os
import yaml
import networkx as nx
from pathlib import Path

class DocsKnowledgeGraphBuilder:
    def __init__(self):
        self.graph = nx.DiGraph()

    def extract_from_docs(self, docs_dir):
        """Extract entities and relationships from markdown frontmatter"""
        for md_file in Path(docs_dir).rglob("*.md"):
            with open(md_file, 'r') as f:
                content = f.read()

                # Extract frontmatter
                if content.startswith('---'):
                    parts = content.split('---', 2)
                    if len(parts) >= 3:
                        frontmatter = yaml.safe_load(parts[1])
                        self._add_to_graph(md_file, frontmatter)

    def _add_to_graph(self, file_path, metadata):
        """Add node and relationships to graph"""
        doc_id = metadata.get('title', file_path.stem)

        # Add node
        self.graph.add_node(
            doc_id,
            type=metadata.get('type'),
            category=metadata.get('category'),
            path=str(file_path),
            **metadata
        )

        # Add relationships
        # Module relationship
        if 'module' in metadata:
            self.graph.add_edge(metadata['module'], doc_id, rel='contains')

        # Requirements
        if 'requires' in metadata:
            for req in metadata['requires']:
                self.graph.add_edge(doc_id, req, rel='requires')

        # Used by
        if 'used_by' in metadata:
            for user in metadata['used_by']:
                self.graph.add_edge(user, doc_id, rel='uses')

        # Related content
        if 'related' in metadata:
            for rel in metadata['related']:
                rel_type = rel.get('type', 'related')
                self.graph.add_edge(doc_id, rel['path'], rel=rel_type)

    def export_to_neo4j(self, uri, user, password):
        """Export NetworkX graph to Neo4j"""
        from neo4j import GraphDatabase

        driver = GraphDatabase.driver(uri, auth=(user, password))

        with driver.session() as session:
            # Create nodes
            for node, attrs in self.graph.nodes(data=True):
                session.run(
                    """
                    CREATE (n:Doc {
                        id: $id,
                        type: $type,
                        title: $title,
                        path: $path
                    })
                    """,
                    id=node,
                    type=attrs.get('type'),
                    title=attrs.get('title', node),
                    path=attrs.get('path')
                )

            # Create relationships
            for source, target, attrs in self.graph.edges(data=True):
                rel_type = attrs.get('rel', 'RELATED_TO').upper()
                session.run(
                    f"""
                    MATCH (a:Doc {{id: $source}})
                    MATCH (b:Doc {{id: $target}})
                    CREATE (a)-[:{rel_type}]->(b)
                    """,
                    source=source,
                    target=target
                )

        driver.close()

    def export_to_json(self, output_file):
        """Export as JSON for web visualization"""
        import json
        from networkx.readwrite import json_graph

        data = json_graph.node_link_data(self.graph)

        with open(output_file, 'w') as f:
            json.dump(data, f, indent=2)

# Usage
builder = DocsKnowledgeGraphBuilder()
builder.extract_from_docs('./docs')
builder.export_to_neo4j("bolt://localhost:7687", "neo4j", "password")
builder.export_to_json("knowledge_graph.json")
```

---

## Approach 3: Fully Automated with AI (LLM Extraction)

Extract entities and relationships from unstructured content using LLMs.

```python
from langchain.chat_models import ChatOpenAI
from langchain.prompts import PromptTemplate
from langchain.output_parsers import PydanticOutputParser
from pydantic import BaseModel, Field
from typing import List

class Entity(BaseModel):
    name: str = Field(description="Entity name")
    type: str = Field(description="Entity type: Module, Component, Role, Process")
    description: str = Field(description="Brief description")

class Relationship(BaseModel):
    source: str = Field(description="Source entity")
    relation: str = Field(description="Relationship type")
    target: str = Field(description="Target entity")

class KnowledgeExtraction(BaseModel):
    entities: List[Entity] = Field(description="List of entities")
    relationships: List[Relationship] = Field(description="List of relationships")

class AIKnowledgeGraphExtractor:
    def __init__(self, openai_api_key):
        self.llm = ChatOpenAI(
            model="gpt-4",
            temperature=0,
            openai_api_key=openai_api_key
        )
        self.parser = PydanticOutputParser(pydantic_object=KnowledgeExtraction)

    def extract_from_text(self, text):
        """Extract entities and relationships from text"""

        prompt = PromptTemplate(
            template="""
            Extract entities and relationships from the following documentation text.

            Entity types: Module, Component, Role, Process, Resource
            Relationship types: contains, requires, used_by, configured_by, is_type_of,
                               prerequisite_for, depends_on

            Text:
            {text}

            {format_instructions}
            """,
            input_variables=["text"],
            partial_variables={"format_instructions": self.parser.get_format_instructions()}
        )

        chain = prompt | self.llm | self.parser
        result = chain.invoke({"text": text})

        return result

    def build_graph_from_docs(self, docs_dir):
        """Process all docs and build knowledge graph"""
        import networkx as nx
        from pathlib import Path

        graph = nx.DiGraph()

        for md_file in Path(docs_dir).rglob("*.md"):
            with open(md_file, 'r') as f:
                content = f.read()

                # Extract using AI
                extraction = self.extract_from_text(content)

                # Add to graph
                for entity in extraction.entities:
                    graph.add_node(
                        entity.name,
                        type=entity.type,
                        description=entity.description,
                        source_file=str(md_file)
                    )

                for rel in extraction.relationships:
                    graph.add_edge(
                        rel.source,
                        rel.target,
                        relation=rel.relation
                    )

        return graph

# Usage
extractor = AIKnowledgeGraphExtractor("your-openai-key")

# Extract from single document
text = """
The Authentication Module contains the Auth API and Auth Service.
The Auth API requires a Database connection. Administrators can
configure the Authentication Module. Developers use the Auth API
to integrate authentication into their applications.
"""
result = extractor.extract_from_text(text)

print("Entities:", result.entities)
print("Relationships:", result.relationships)

# Extract from entire docs folder
graph = extractor.build_graph_from_docs('./docs')
```

---

## Approach 4: Use RDF/JSON-LD (Semantic Web Standards)

**Create knowledge graph in JSON-LD format**:

```json
{
  "@context": {
    "@vocab": "https://mosip.io/ontology#",
    "contains": {"@type": "@id"},
    "requires": {"@type": "@id"},
    "usedBy": {"@type": "@id"}
  },
  "@graph": [
    {
      "@id": "auth_module",
      "@type": "Module",
      "name": "Authentication Module",
      "description": "Handles identity authentication",
      "contains": ["auth_api", "auth_service"]
    },
    {
      "@id": "auth_api",
      "@type": "Component",
      "subType": "API",
      "name": "Authentication API",
      "requires": ["database"],
      "usedBy": ["developer", "partner"]
    },
    {
      "@id": "database",
      "@type": "Component",
      "subType": "Database",
      "name": "PostgreSQL Database"
    },
    {
      "@id": "developer",
      "@type": "Role",
      "name": "Developer"
    }
  ]
}
```

**Load into graph processing library**:

```python
from rdflib import Graph, URIRef, Literal, Namespace
from rdflib.namespace import RDF, RDFS

# Create RDF graph
g = Graph()

# Define namespace
MOSIP = Namespace("https://mosip.io/ontology#")
g.bind("mosip", MOSIP)

# Add triples
g.add((MOSIP.auth_module, RDF.type, MOSIP.Module))
g.add((MOSIP.auth_module, RDFS.label, Literal("Authentication Module")))
g.add((MOSIP.auth_module, MOSIP.contains, MOSIP.auth_api))
g.add((MOSIP.auth_api, MOSIP.requires, MOSIP.database))

# Query with SPARQL
query = """
PREFIX mosip: <https://mosip.io/ontology#>

SELECT ?component
WHERE {
    mosip:auth_module mosip:contains ?component .
}
"""

results = g.query(query)
for row in results:
    print(row.component)

# Export as JSON-LD
print(g.serialize(format='json-ld'))
```

---

## Practical Example: Complete MOSIP Knowledge Graph Builder

```python
import yaml
import json
from pathlib import Path
from typing import Dict, List
import networkx as nx

class MOSIPKnowledgeGraph:
    def __init__(self):
        self.graph = nx.MultiDiGraph()  # Allow multiple edge types between nodes
        self.ontology = self._define_ontology()

    def _define_ontology(self):
        """Define MOSIP documentation ontology"""
        return {
            "entity_types": [
                "Module", "Component", "API", "Service", "Database",
                "Role", "Process", "Use Case", "Guide", "Reference"
            ],
            "relationship_types": {
                "contains": "hierarchical",
                "requires": "dependency",
                "usedBy": "usage",
                "configuredBy": "permission",
                "isTypeOf": "classification",
                "prerequisiteFor": "sequence",
                "relatedTo": "association",
                "documents": "documentation",
                "troubleshoots": "support"
            }
        }

    def add_from_frontmatter(self, md_file: Path):
        """Extract knowledge from markdown frontmatter"""
        with open(md_file, 'r', encoding='utf-8') as f:
            content = f.read()

            if not content.startswith('---'):
                return

            parts = content.split('---', 2)
            if len(parts) < 3:
                return

            try:
                metadata = yaml.safe_load(parts[1])
            except:
                return

            # Add node
            node_id = metadata.get('id', md_file.stem)
            self.graph.add_node(
                node_id,
                label=metadata.get('title', node_id),
                type=metadata.get('type', 'Guide'),
                path=str(md_file.relative_to(Path.cwd())),
                **{k: v for k, v in metadata.items() if k not in ['id', 'title', 'type']}
            )

            # Add relationships
            self._add_relationships(node_id, metadata)

    def _add_relationships(self, node_id: str, metadata: Dict):
        """Add edges based on metadata"""

        # Module containment
        if 'module' in metadata:
            self.graph.add_edge(metadata['module'], node_id, type='contains')

        # Dependencies
        if 'requires' in metadata:
            for dep in metadata['requires']:
                self.graph.add_edge(node_id, dep, type='requires')

        # Usage by roles
        if 'audience' in metadata:
            if isinstance(metadata['audience'], list):
                for role in metadata['audience']:
                    self.graph.add_edge(role, node_id, type='usedBy')

        # Prerequisites
        if 'prerequisites' in metadata:
            for prereq in metadata['prerequisites']:
                self.graph.add_edge(prereq, node_id, type='prerequisiteFor')

        # Related content
        if 'related' in metadata:
            for rel in metadata['related']:
                if isinstance(rel, dict):
                    target = rel.get('path', rel.get('id'))
                    rel_type = rel.get('type', 'relatedTo')
                    self.graph.add_edge(node_id, target, type=rel_type)
                else:
                    self.graph.add_edge(node_id, rel, type='relatedTo')

    def build_from_directory(self, docs_dir: str):
        """Build knowledge graph from entire docs directory"""
        for md_file in Path(docs_dir).rglob("*.md"):
            self.add_from_frontmatter(md_file)

        print(f"Built knowledge graph with {self.graph.number_of_nodes()} nodes "
              f"and {self.graph.number_of_edges()} edges")

    def query_dependencies(self, node_id: str) -> List[str]:
        """Find all dependencies of a node"""
        if node_id not in self.graph:
            return []

        deps = []
        for _, target, data in self.graph.out_edges(node_id, data=True):
            if data.get('type') == 'requires':
                deps.append(target)
        return deps

    def query_prerequisites_chain(self, node_id: str) -> List[str]:
        """Find prerequisite chain for a node"""
        if node_id not in self.graph:
            return []

        chain = []
        for source, _, data in self.graph.in_edges(node_id, data=True):
            if data.get('type') == 'prerequisiteFor':
                chain.append(source)
                # Recursively find prerequisites
                chain.extend(self.query_prerequisites_chain(source))

        return list(set(chain))  # Remove duplicates

    def query_by_role(self, role: str) -> List[Dict]:
        """Find all content usable by a role"""
        content = []
        for source, target, data in self.graph.edges(data=True):
            if source == role and data.get('type') == 'usedBy':
                node_data = self.graph.nodes[target]
                content.append({
                    'id': target,
                    'title': node_data.get('label'),
                    'type': node_data.get('type'),
                    'path': node_data.get('path')
                })
        return content

    def query_module_contents(self, module: str) -> List[Dict]:
        """Find all content in a module"""
        content = []
        for source, target, data in self.graph.edges(data=True):
            if source == module and data.get('type') == 'contains':
                node_data = self.graph.nodes[target]
                content.append({
                    'id': target,
                    'title': node_data.get('label'),
                    'type': node_data.get('type')
                })
        return content

    def find_related_content(self, node_id: str, depth: int = 2) -> List[str]:
        """Find related content up to specified depth"""
        if node_id not in self.graph:
            return []

        related = set()

        # BFS to find related nodes
        from collections import deque

        queue = deque([(node_id, 0)])
        visited = {node_id}

        while queue:
            current, current_depth = queue.popleft()

            if current_depth >= depth:
                continue

            # Get all neighbors (both incoming and outgoing)
            neighbors = set(self.graph.successors(current)) | set(self.graph.predecessors(current))

            for neighbor in neighbors:
                if neighbor not in visited:
                    visited.add(neighbor)
                    related.add(neighbor)
                    queue.append((neighbor, current_depth + 1))

        return list(related)

    def export_for_visualization(self, output_file: str):
        """Export as JSON for D3.js or similar"""
        nodes = []
        edges = []

        for node, attrs in self.graph.nodes(data=True):
            nodes.append({
                'id': node,
                'label': attrs.get('label', node),
                'type': attrs.get('type'),
                **{k: v for k, v in attrs.items() if k not in ['label', 'type']}
            })

        for source, target, data in self.graph.edges(data=True):
            edges.append({
                'source': source,
                'target': target,
                'type': data.get('type', 'relatedTo')
            })

        with open(output_file, 'w') as f:
            json.dump({'nodes': nodes, 'edges': edges}, f, indent=2)

    def export_to_cypher(self, output_file: str):
        """Export as Cypher statements for Neo4j"""
        statements = []

        # Create nodes
        for node, attrs in self.graph.nodes(data=True):
            node_type = attrs.get('type', 'Doc')
            props = {k: v for k, v in attrs.items() if k != 'type' and v is not None}
            props_str = ', '.join([f"{k}: '{v}'" for k, v in props.items()])

            statements.append(
                f"CREATE (:{node_type} {{id: '{node}', {props_str}}})"
            )

        # Create relationships
        for source, target, data in self.graph.edges(data=True):
            rel_type = data.get('type', 'RELATED_TO').upper()
            statements.append(
                f"MATCH (a {{id: '{source}'}}), (b {{id: '{target}'}}) "
                f"CREATE (a)-[:{rel_type}]->(b)"
            )

        with open(output_file, 'w') as f:
            f.write(';\n'.join(statements) + ';')

    def export_statistics(self):
        """Print graph statistics"""
        print(f"\n=== Knowledge Graph Statistics ===")
        print(f"Total nodes: {self.graph.number_of_nodes()}")
        print(f"Total edges: {self.graph.number_of_edges()}")

        # Count by type
        type_counts = {}
        for _, attrs in self.graph.nodes(data=True):
            node_type = attrs.get('type', 'Unknown')
            type_counts[node_type] = type_counts.get(node_type, 0) + 1

        print(f"\nNodes by type:")
        for node_type, count in sorted(type_counts.items()):
            print(f"  {node_type}: {count}")

        # Count relationships
        rel_counts = {}
        for _, _, data in self.graph.edges(data=True):
            rel_type = data.get('type', 'unknown')
            rel_counts[rel_type] = rel_counts.get(rel_type, 0) + 1

        print(f"\nRelationships by type:")
        for rel_type, count in sorted(rel_counts.items()):
            print(f"  {rel_type}: {count}")

# Usage examples
kg = MOSIPKnowledgeGraph()
kg.build_from_directory('./docs')
kg.export_statistics()

# Query examples
deps = kg.query_dependencies('auth_api')
print(f"\nAuth API depends on: {deps}")

prereqs = kg.query_prerequisites_chain('biometric_auth')
print(f"Prerequisites for biometric auth: {prereqs}")

admin_content = kg.query_by_role('admin')
print(f"Content for admins: {admin_content}")

auth_contents = kg.query_module_contents('authentication')
print(f"Authentication module contains: {auth_contents}")

related = kg.find_related_content('auth_api', depth=2)
print(f"Content related to Auth API: {related}")

# Export
kg.export_for_visualization('knowledge_graph.json')
kg.export_to_cypher('import.cypher')
```

---

## Integration with RAG

### Enhance RAG with Knowledge Graph

```python
from langchain.vectorstores import Chroma
from langchain.embeddings import OpenAIEmbeddings
from langchain.chat_models import ChatOpenAI
from langchain.chains import ConversationalRetrievalChain

class KnowledgeGraphEnhancedRAG:
    def __init__(self, vectorstore, knowledge_graph):
        self.vectorstore = vectorstore
        self.kg = knowledge_graph
        self.llm = ChatOpenAI(temperature=0, model="gpt-4")

    def query_with_context(self, question, user_role=None):
        """Query with knowledge graph context"""

        # 1. Get initial RAG results
        retriever = self.vectorstore.as_retriever(search_kwargs={"k": 3})
        initial_docs = retriever.get_relevant_documents(question)

        # 2. Expand using knowledge graph
        expanded_docs = []
        for doc in initial_docs:
            doc_id = self._extract_doc_id(doc)

            # Get prerequisites
            prereqs = self.kg.query_prerequisites_chain(doc_id)

            # Get dependencies
            deps = self.kg.query_dependencies(doc_id)

            # Get related content
            related = self.kg.find_related_content(doc_id, depth=1)

            # Add context to metadata
            doc.metadata['prerequisites'] = prereqs
            doc.metadata['dependencies'] = deps
            doc.metadata['related'] = related

            expanded_docs.append(doc)

        # 3. Filter by role if specified
        if user_role:
            role_content = self.kg.query_by_role(user_role)
            role_ids = {c['id'] for c in role_content}
            expanded_docs = [d for d in expanded_docs
                           if self._extract_doc_id(d) in role_ids]

        # 4. Generate answer with enhanced context
        context = self._build_context(expanded_docs)

        prompt = f"""
        Answer the following question based on the documentation provided.

        Context: {context}

        Question: {question}

        If prerequisites are needed, mention them.
        If dependencies exist, explain them.
        Suggest related content if relevant.
        """

        response = self.llm.predict(prompt)

        return {
            'answer': response,
            'sources': [d.metadata.get('path') for d in expanded_docs],
            'prerequisites': [p for d in expanded_docs for p in d.metadata.get('prerequisites', [])],
            'related': [r for d in expanded_docs for r in d.metadata.get('related', [])]
        }

    def _extract_doc_id(self, doc):
        """Extract document ID from doc metadata"""
        # Implement based on your metadata structure
        return doc.metadata.get('id', doc.metadata.get('title'))

    def _build_context(self, docs):
        """Build context string from documents"""
        context_parts = []
        for doc in docs:
            context_parts.append(f"Content: {doc.page_content}")
            if doc.metadata.get('prerequisites'):
                context_parts.append(f"Prerequisites: {', '.join(doc.metadata['prerequisites'])}")
            if doc.metadata.get('dependencies'):
                context_parts.append(f"Dependencies: {', '.join(doc.metadata['dependencies'])}")
        return "\n\n".join(context_parts)

# Usage
kg = MOSIPKnowledgeGraph()
kg.build_from_directory('./docs')

vectorstore = Chroma.from_documents(docs, OpenAIEmbeddings())

rag = KnowledgeGraphEnhancedRAG(vectorstore, kg)
result = rag.query_with_context(
    "How do I configure biometric authentication?",
    user_role="admin"
)

print(f"Answer: {result['answer']}")
print(f"Prerequisites: {result['prerequisites']}")
print(f"Related content: {result['related']}")
```

---

## Tools & Technologies Comparison

| Tool | Best For | Pros | Cons | Learning Curve |
|------|----------|------|------|----------------|
| **Neo4j** | Production, complex queries | Powerful Cypher, visualization, mature | Commercial for enterprise | Medium |
| **NetworkX** | Python scripts, analysis | Simple, integrated with Python | In-memory only, no persistence | Low |
| **RDFLib** | Semantic web, standards | W3C standards, SPARQL | Steeper learning curve | High |
| **JSON-LD** | Web integration | Web-friendly, schema.org compatible | Limited query capabilities | Low |
| **Apache Jena** | Java apps, enterprise | Robust, scalable, reasoning | Java-centric | High |
| **Stardog** | Enterprise knowledge graphs | Commercial support, reasoning, ACID | Expensive | Medium |
| **Amazon Neptune** | Cloud, serverless | Managed service, scalable | AWS lock-in, cost | Medium |
| **ArangoDB** | Multi-model needs | Graph + document + key-value | Smaller community | Medium |

---

## Visualization Options

### 1. D3.js (Web)

```javascript
// Load knowledge graph JSON
d3.json('knowledge_graph.json').then(data => {
    const svg = d3.select('svg');
    const width = 800, height = 600;

    // Create force simulation
    const simulation = d3.forceSimulation(data.nodes)
        .force('link', d3.forceLink(data.edges).id(d => d.id))
        .force('charge', d3.forceManyBody().strength(-100))
        .force('center', d3.forceCenter(width / 2, height / 2));

    // Draw edges
    const link = svg.append('g')
        .selectAll('line')
        .data(data.edges)
        .enter().append('line')
        .attr('stroke', '#999');

    // Draw nodes
    const node = svg.append('g')
        .selectAll('circle')
        .data(data.nodes)
        .enter().append('circle')
        .attr('r', 5)
        .attr('fill', d => colorByType(d.type));

    // Update positions
    simulation.on('tick', () => {
        link
            .attr('x1', d => d.source.x)
            .attr('y1', d => d.source.y)
            .attr('x2', d => d.target.x)
            .attr('y2', d => d.target.y);

        node
            .attr('cx', d => d.x)
            .attr('cy', d => d.y);
    });
});
```

### 2. Cytoscape.js (Interactive)

```javascript
const cy = cytoscape({
  container: document.getElementById('cy'),

  elements: {
    nodes: data.nodes.map(n => ({ data: n })),
    edges: data.edges.map(e => ({ data: e }))
  },

  style: [
    {
      selector: 'node',
      style: {
        'label': 'data(label)',
        'background-color': 'data(color)',
        'width': 20,
        'height': 20
      }
    },
    {
      selector: 'edge',
      style: {
        'width': 2,
        'line-color': '#ccc',
        'target-arrow-color': '#ccc',
        'target-arrow-shape': 'triangle',
        'label': 'data(type)'
      }
    }
  ],

  layout: {
    name: 'cose',
    idealEdgeLength: 100,
    nodeOverlap: 20
  }
});
```

### 3. Graphviz (Static Diagrams)

```python
import networkx as nx
from networkx.drawing.nx_agraph import to_agraph

# Convert to Graphviz format
A = to_agraph(kg.graph)
A.layout('dot')
A.draw('knowledge_graph.png')

# Or generate DOT file
nx.drawing.nx_pydot.write_dot(kg.graph, 'knowledge_graph.dot')
```

---

## Quick Start Recommendation

### For MOSIP Docs

**Phase 1: Foundation (Week 1-2)**
1. ✅ Add structured frontmatter to existing Markdown
2. ✅ Define your ontology (entity types & relationships)
3. ✅ Start with 20-30 key documents

**Phase 2: Build Graph (Week 3-4)**
4. ✅ Use NetworkX to build the graph in Python
5. ✅ Export to JSON for web visualization
6. ✅ Create basic queries (dependencies, prerequisites)

**Phase 3: Integration (Week 5-6)**
7. ✅ Integrate with RAG for enhanced retrieval
8. ✅ Add role-based filtering
9. ✅ Build prerequisite warning system

**Phase 4: Production (Month 2+)**
10. ✅ Migrate to Neo4j for complex queries
11. ✅ Add web visualization
12. ✅ Automate graph updates from git commits

---

## Best Practices

### 1. Ontology Design
- **Start simple**: Don't over-engineer entity types
- **Be consistent**: Use controlled vocabulary for relationships
- **Document it**: Maintain ontology documentation
- **Iterate**: Refine based on actual usage patterns

### 2. Data Quality
- **Validate**: Check for orphaned nodes, broken relationships
- **Deduplicate**: Merge similar entities
- **Enrich**: Add descriptions, metadata progressively
- **Version**: Track ontology changes

### 3. Maintenance
- **Automate**: Extract from frontmatter automatically
- **CI/CD**: Rebuild graph on doc updates
- **Monitor**: Track graph growth, detect anomalies
- **Prune**: Remove obsolete entities

### 4. Performance
- **Index**: Index frequently queried properties
- **Cache**: Cache common queries
- **Pagination**: Limit result sets
- **Batch**: Bulk operations for imports

---

## Common Pitfalls

1. **Over-engineering the ontology**: Start simple, add complexity as needed
2. **Manual maintenance**: Automate extraction and updates
3. **Ignoring data quality**: Garbage in, garbage out
4. **No validation**: Broken relationships break queries
5. **Choosing wrong tool**: NetworkX for exploration, Neo4j for production
6. **Not integrating with docs workflow**: Graph becomes stale

---

## Next Steps

1. **Define your ontology** for MOSIP documentation
2. **Add frontmatter** to 10-20 key documents
3. **Run the extraction script** to build initial graph
4. **Visualize** to validate structure
5. **Query** to test usefulness
6. **Iterate** based on findings

---

## Further Reading

- **Neo4j Graph Academy**: Free courses on graph databases
- **Knowledge Graphs (book)** by Aidan Hogan et al.
- **NetworkX Documentation**: Graph algorithms in Python
- **Semantic Web Primer**: RDF, OWL, SPARQL
- **Building Knowledge Graphs** (O'Reilly)

---

*Last Updated: February 2026*
*Related: RAG, Ontologies, Semantic Search, Documentation Systems*
