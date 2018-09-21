---
title: Conexión a Azure Cosmos DB como origen de datos en Azure Machine Learning Workbench | Microsoft Docs
description: En este documento se proporciona un ejemplo de cómo conectarse a Azure Cosmos DB mediante Azure Machine Learning Workbench
services: machine-learning
author: cforbe
ms.author: cforbe
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 09/11/2017
ms.openlocfilehash: 2d9562582a94b7c7aac5b972a7ef57b84eb9aa9e
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2018
ms.locfileid: "35642392"
---
# <a name="connecting-to-azure-cosmos-db-as-a-data-source"></a>Conexión a Azure Cosmos DB como un origen de datos
Este artículo contiene un ejemplo de Python que le permite conectarse a Cosmos DB en Azure Machine Learning Workbench.

## <a name="load-azure-cosmos-db-data-into-data-preparation"></a>Carga de los datos de Azure Cosmos DB en la preparación de datos

Cree un flujo de datos basado en script y use el script siguiente para cargar los datos de Azure Cosmos DB. 

```python
import pydocumentdb
import pydocumentdb.document_client as document_client

import pandas as pd

config = { 
    'ENDPOINT': '<Endpoint>',
    'MASTERKEY': '<Key>',
    'DOCUMENTDB_DATABASE': '<DBName>',
    'DOCUMENTDB_COLLECTION': '<collectionname>'
};

# Initialize the Python DocumentDB client.
client = document_client.DocumentClient(config['ENDPOINT'], {'masterKey': config['MASTERKEY']})

# Read databases and take first since id should not be duplicated.
db = next((data for data in client.ReadDatabases() if data['id'] == config['DOCUMENTDB_DATABASE']))

# Read collections and take first since id should not be duplicated.
coll = next((coll for coll in client.ReadCollections(db['_self']) if coll['id'] == config['DOCUMENTDB_COLLECTION']))

docs = client.ReadDocuments(coll['_self'])

df = pd.DataFrame(list(docs))
```

## <a name="other-data-source-connections"></a>Otras conexiones de origen de datos
Para otros ejemplos, lea [Conexiones de datos de origen adicionales de ejemplo](data-prep-appendix8-sample-source-connections-python.md)
