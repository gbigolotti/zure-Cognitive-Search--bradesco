# zure-Cognitive-Search--bradesco
Criar uma solução para indexar documentos financeiros (ex: relatórios, contratos, análises) usando o Azure Cognitive Search com capacidades de AI Search (enriquecimento cognitivo, compreensão de linguagem natural) para consultas avançadas.

azure-cognitive-search-finance/

├── README.md

├── LICENSE

├── .gitignore

├── data/

│  └── financial-documents.json

├── scripts/

│  ├── deploy.bicep

│  ├── upload-data.py

│  └── search-sample.py

├── docs/

│  └── instructions.md

└── templates/

  └── azuresearch.bicep



📄 Passo a passo resumido
Criar recurso Azure Cognitive Search no portal ou via Bicep
Preparar e carregar dados financeiros (JSON, CSV, etc)
Definir index com campos, incluindo campos enriquecidos (ex: AI Key Phrases, Sentiment)
Criar Skillset para enriquecer documentos via AI (Language, OCR, etc)
Ingerir dados na index com skillset aplicado
Consultar e testar buscas avançadas usando Python SDK
📄 README.md exemplo
markdown
CopiarEditar
# Azure Cognitive Search para Setor Financeiro - AI Search Demo

## Objetivo

Este projeto demonstra o uso do Azure Cognitive Search com AI Search para indexação e consulta de documentos financeiros.

## Pré-requisitos

- Conta Azure ativa
- Azure CLI instalado
- Python 3.8+
- Pacotes Python: azure-search-documents, requests

## Passos

1. Criar recurso Azure Cognitive Search:
```bash
az deployment group create --resource-group rg-search-finance --template-file templates/azuresearch.bicep
Preparar dados no arquivo data/financial-documents.json
Rodar o script de upload:
bash
CopiarEditar
python scripts/upload-data.py
Testar buscas com:
bash
CopiarEditar
python scripts/search-sample.py
Arquivos
deploy.bicep - Deploy infraestrutura
upload-data.py - Script para upload
search-sample.py - Exemplo de consulta
Referências
Azure Cognitive Search Docs
yaml
CopiarEditar

---

## Código Bicep para deploy (`templates/azuresearch.bicep`):

```bicep
param location string = resourceGroup().location
param searchServiceName string

resource searchService 'Microsoft.Search/searchServices@2021-04-01-preview' = {
  name: searchServiceName
  location: location
  sku: {
    name: 'standard'
    capacity: 1
  }
  properties: {
    replicaCount: 1
    partitionCount: 1
    hostingMode: 'default'
  }
}
Exemplo de upload e busca (scripts/upload-data.py):
python
CopiarEditar
import os
import json
from azure.core.credentials import AzureKeyCredential
from azure.search.documents import SearchClient

endpoint = os.getenv("SEARCH_ENDPOINT")
key = os.getenv("SEARCH_API_KEY")
index_name = "finance-index"

client = SearchClient(endpoint=endpoint, index_name=index_name, credential=AzureKeyCredential(key))

with open('../data/financial-documents.json', 'r') as f:
    documents = json.load(f)

result = client.upload_documents(documents=documents)
print(f"Uploaded {len(result)} documents.")
