---
title: Problemas conocidos y soluciones
titleSuffix: Azure Machine Learning service
description: Obtenga una lista de los problemas conocidos, soluciones alternativas y soluciones para Azure Machine Learning Service.
services: machine-learning
author: j-martens
ms.author: jmartens
ms.reviewer: mldocs
ms.service: machine-learning
ms.subservice: core
ms.topic: article
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: b10e434aece0ac214a0fd397ea94cbeccca4e44a
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2019
ms.locfileid: "55746497"
---
# <a name="known-issues-and-troubleshooting-azure-machine-learning-service"></a>Problemas conocidos y solución de problemas del servicio Azure Machine Learning

En este artículo obtendrá ayuda para buscar y corregir los errores que se producen al usar el servicio Azure Machine Learning.

## <a name="sdk-installation-issues"></a>Problemas de instalación de SDK

**Mensaje de error: no se puede desinstalar 'PyYAML'**

SDK de Azure Machine Learning para Python: PyYAML es un proyecto instalado de Distutils. Por lo tanto, no se puede determinar con precisión qué archivos le pertenecen en caso de una desinstalación parcial. Para continuar con la instalación del SDK sin tener en cuenta este error, use:

```Python
pip install --upgrade azureml-sdk[notebooks,automl] --ignore-installed PyYAML
```

## <a name="trouble-creating-azure-machine-learning-compute"></a>Problemas al crear la instancia de Proceso de Azure Machine Learning

Es posible que algunos usuarios que crearon su área de trabajo de Azure Machine Learning en Azure Portal antes de la versión disponible de forma general no puedan crear la instancia de Proceso de Azure Machine Learning en esa área de trabajo. Puede generar una solicitud de soporte técnico en el servicio o crear una nueva área de trabajo mediante el portal o el SDK para desbloquearse a sí mismo inmediatamente.

## <a name="image-building-failure"></a>Error de creación de imagen

Error de creación de imagen al implementar el servicio web. La solución alternativa es agregar "pynacl==1.2.1" como una dependencia pip al archivo Conda para la configuración de la imagen.

## <a name="deployment-failure"></a>Error de implementación

Si observa `['DaskOnBatch:context_managers.DaskOnBatch', 'setup.py']' died with <Signals.SIGKILL: 9>`, cambie la SKU de las máquinas virtuales usadas en la implementación por otra que tenga más memoria.

## <a name="fpgas"></a>FPGA
No podrá implementar modelos en FPGA hasta que haya solicitado y se haya aprobado para la cuota FPGA. Para solicitar acceso, rellene el formulario de solicitud de cuota: https://aka.ms/aml-real-time-ai

## <a name="databricks"></a>Databricks

Problemas de Databricks y Azure Machine Learning.

1. Errores de instalación del SDK de Azure Machine Learning en Databricks cuando se instalan más paquetes.

   Algunos paquetes, como `psutil`, pueden provocar conflictos. Para evitar errores de instalación, inmovilice la versión lib para instalar paquetes. Este problema está relacionado con Databricks y no con el SDK de Azure Machine Learning Service, aunque también se puede encontrar en otras bibliotecas. Ejemplo:
   ```python
   pstuil cryptography==1.5 pyopenssl==16.0.0 ipython==2.2.0
   ```
   Como alternativa, puede usar scripts de init si sigue experimentando problemas de instalación con las bibliotecas de Python. Este no es un enfoque oficialmente compatible. Puede hacer referencia a [este documento](https://docs.azuredatabricks.net/user-guide/clusters/init-scripts.html#cluster-scoped-init-scripts).

2. Mientras usa Automated Machine Learning en Databricks, si quiere cancelar la ejecución de un experimento e iniciar una nueva, reinicie el clúster de Azure Databricks.

3. En la configuración de Automated Machine Learning, si tiene más de 10 iteraciones, establezca `show_output` en `False` cuando envíe la ejecución.


## <a name="azure-portal"></a>Azure Portal
Si ve directamente el área de trabajo desde un vínculo de recurso compartido desde el SDK o el portal, no podrá ver la página de información general normal con la información de suscripción en la extensión. Tampoco será capaz de cambiar a otra área de trabajo. Si necesita ver otra área de trabajo, la solución consiste en ir directamente a [Azure Portal](https://portal.azure.com) y buscar el nombre de la misma.

## <a name="diagnostic-logs"></a>Registros de diagnóstico
A veces puede resultar útil proporcionar información de diagnóstico al solicitar ayuda.
Aquí es donde se encuentran los archivos de registro:

## <a name="resource-quotas"></a>Cuotas de recursos

Obtenga información sobre la [cuotas de recursos](how-to-manage-quotas.md) que puede encontrar al trabajar con Azure Machine Learning.

## <a name="authentication-errors"></a>Errores de autenticación

Si realiza una operación de administración en un destino de proceso desde un trabajo remoto, recibirá uno de los siguientes errores:

```json
{"code":"Unauthorized","statusCode":401,"message":"Unauthorized","details":[{"code":"InvalidOrExpiredToken","message":"The request token was either invalid or expired. Please try again with a valid token."}]}
```

```json
{"error":{"code":"AuthenticationFailed","message":"Authentication failed."}}
```

Por ejemplo, si intenta crear o asociar un destino de proceso desde una canalización de aprendizaje automático que se envía para ejecución remota, recibirá un error.

## <a name="get-more-support"></a>Obtener más soporte técnico

Puede enviar las solicitudes de soporte técnico y obtener ayuda de soporte técnico, foros, etc. [Más información...](support-for-aml-services.md)
