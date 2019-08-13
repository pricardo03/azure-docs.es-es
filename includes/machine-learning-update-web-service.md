---
author: larryfr
ms.service: machine-learning
ms.topic: include
ms.date: 07/26/2019
ms.author: larryfr
ms.openlocfilehash: fef6225812980900ad55944644310433a6105de2
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68556867"
---
Cuando cree un nuevo modelo, debe actualizar manualmente cada servicio que quiera que use el nuevo modelo. Para actualizar el servicio web, utilice el método `update`. El código siguiente muestra cómo usar el SDK para actualizar el modelo para un servicio Web:

```python
from azureml.core.webservice import Webservice
from azureml.core.model import Model

# register new model
new_model = Model.register(model_path="outputs/sklearn_mnist_model.pkl",
                           model_name="sklearn_mnist",
                           tags={"key": "0.1"},
                           description="test",
                           workspace=ws)

service_name = 'myservice'
# Retrieve existing service
service = Webservice(name=service_name, workspace=ws)

# Update to new model(s)
service.update(models=[new_model])
print(service.state)
print(service.get_logs())
```

También puede actualizar un servicio Web con la CLI de ML. En el ejemplo siguiente se muestra cómo registrar un nuevo modelo y, a continuación, actualizar el servicio web para usarlo:

```azurecli
az ml model register -n sklearn_mnist  --asset-path outputs/sklearn_mnist_model.pkl  --experiment-name myexperiment --output-metadata-file modelinfo.json
az ml service update -n myservice --model-metadata-file modelinfo.json
```

> [!TIP]
> En este ejemplo, se usa un documento JSON para pasar la información del modelo del comando de registro al comando de actualización.