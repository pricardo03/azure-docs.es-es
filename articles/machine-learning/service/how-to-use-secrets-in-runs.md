---
title: Uso de secretos en ejecuciones de entrenamiento
titleSuffix: Azure Machine Learning
description: Paso de secretos a ejecuciones de entrenamiento de manera segura mediante la instancia de Key Vault del área de trabajo
services: machine-learning
author: rastala
ms.author: roastala
ms.reviewer: larryfr
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 08/23/2019
ms.custom: seodec18
ms.openlocfilehash: e2074cec65ea4c1df803999c6a995f73ea4227ee
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2019
ms.locfileid: "73796685"
---
# <a name="use-secrets-in-training-runs"></a>Uso de secretos en ejecuciones de entrenamiento
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

En este artículo, aprenderá a usar secretos en las ejecuciones de entrenamiento de forma segura. Por ejemplo, para conectarse a una base de datos externa y consultar datos de entrenamiento, deberá pasar el nombre de usuario y la contraseña al contexto de ejecución remoto. La codificación de estos valores en los scripts de entrenamiento en texto no cifrado no es segura, ya que expondría el secreto. 

En su lugar, el área de trabajo de Azure Machine Learning tiene [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview) como recurso asociado. Esta instancia de Key Vault se puede usar para pasar secretos a ejecuciones remotas de forma segura mediante un conjunto de API en el SDK de Python de Azure Machine Learning.

El flujo básico para el uso de secretos es:
 1. En el equipo local, inicie sesión en Azure y conéctese al área de trabajo.
 2. En el equipo local, establezca un secreto en el almacén de claves del área de trabajo.
 3. Envíe una ejecución remota.
 4. Dentro de la ejecución remota, obtenga el secreto de Valor de clave y úselo.

## <a name="set-secrets"></a>Establecimiento de secretos

En el SDK de Python de Azure Machine Learning, la clase [Keyvault](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py) contiene métodos para establecer secretos. En la sesión de Python local, obtenga primero una referencia al área de trabajo de Key Vault y, a continuación, use el método [set_secret](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py#set-secret-name--value-) para establecer un secreto por nombre y valor.

```python
from azureml.core import Workspace
import os

ws = Workspace.from_config()
my_secret = os.environ.get("MY_SECRET")
keyvault = ws.get_default_keyvault()
keyvault.set_secret(name="mysecret", value = my_secret)
```

No ponga el valor del secreto en código de Python porque no es seguro almacenarlo en un archivo como texto no cifrado. En su lugar, obtenga el valor del secreto de la variable de entorno, por ejemplo, el secreto de compilación de Azure DevOps o de la entrada interactiva del usuario.

Puede enumerar los nombres de secretos mediante el método [list_secrets](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py#list-secrets--). El método __set_secret__ actualiza el valor del secreto si el nombre ya existe.

## <a name="get-secrets"></a>Obtención de secretos

En el código local, puede usar el método [Keyvault.get_secret](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py#get-secret-name-) para obtener el valor del secreto por nombre.

En ejecuciones enviadas con [Experiment.submit](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py#submit-config--tags-none----kwargs-), utilice el método [Run.get_secret](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#get-secret-name-). Como la ejecución enviada conoce su área de trabajo, este método establece un acceso directo a la creación de instancias del área de trabajo y devuelve el valor del secreto directamente.

```python
# Code in submitted run
from azureml.core import Run

run = Run.get_context()
secret_value = run.get_secret(name="mysecret")
```

Tenga cuidado de no exponer el valor del secreto escribiéndolo o imprimiéndolo.

Los métodos set y get también tienen versiones por lotes [set_secrets](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py#set-secrets-secrets-batch-) y [get_secrets](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#get-secrets-secrets-) para tener acceso a varios secretos a la vez.

## <a name="next-steps"></a>Pasos siguientes

 * [Visualización de cuaderno de ejemplo](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/manage-azureml-service/authentication-in-azureml/authentication-in-azureml.ipynb)
 * [Información sobre seguridad de empresa con Azure Machine Learning](concept-enterprise-security.md)
