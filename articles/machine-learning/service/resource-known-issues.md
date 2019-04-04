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
ms.topic: conceptual
ms.date: 03/29/2019
ms.custom: seodec18
ms.openlocfilehash: db0eccb542cb4253e6e891fa9fa51e60fb7951a1
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2019
ms.locfileid: "58892745"
---
# <a name="known-issues-and-troubleshooting-azure-machine-learning-service"></a>Problemas conocidos y solución de problemas del servicio Azure Machine Learning

En este artículo obtendrá ayuda para buscar y corregir los errores que se producen al usar el servicio Azure Machine Learning.

## <a name="sdk-installation-issues"></a>Problemas de instalación de SDK

**Mensaje de error: No se puede desinstalar 'PyYAML'**

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

## <a name="automated-machine-learning"></a>Automated Machine Learning

Aprendizaje automático de flujo automatizado de tensores no admite actualmente la versión del flujo de tensores 1.13. Instalar esta versión hará que las dependencias del paquete deje de funcionar. Estamos trabajando para corregir este problema en una versión futura. 


## <a name="databricks"></a>Databricks

Problemas de Databricks y Azure Machine Learning.

### <a name="failure-when-installing-packages"></a>Error al instalar paquetes

Instalación de Azure Machine Learning SDK se produce un error en Azure Databricks cuando se instalan varios paquetes. Algunos paquetes, como `psutil`, pueden provocar conflictos. Para evitar errores de instalación, instale los paquetes congelando la versión de la biblioteca. Este problema está relacionado con Databricks y no el SDK del servicio Azure Machine Learning. Puede experimentar este problema con otras bibliotecas, demasiado. Ejemplo:

```python
psutil cryptography==1.5 pyopenssl==16.0.0 ipython==2.2.0
```

Como alternativa, puede usar scripts de init si mantener enfrenta a problemas de instalación con bibliotecas de Python. Este enfoque no se admite oficialmente. Para obtener más información, consulte [scripts init centrada en el clúster](https://docs.azuredatabricks.net/user-guide/clusters/init-scripts.html#cluster-scoped-init-scripts).

### <a name="cancel-an-automated-machine-learning-run"></a>Cancelar una ejecución de aprendizaje de automático automatizada

Cuando usas automatizada de machine learning funcionalidades en Azure Databricks, para cancelar una ejecución e iniciar un nuevo experimento ejecutar, reinicie el clúster de Azure Databricks.

### <a name="10-iterations-for-automated-machine-learning"></a>> 10 iteraciones para el aprendizaje automático automatizadas

En la máquina automatizada aprendizaje configuración, si tiene más de 10 iteraciones, establezca `show_output` a `False` cuando se envía la ejecución.

### <a name="widget-for-the-azure-machine-learning-sdkautomated-machine-learning"></a>Widget para el aprendizaje automático de Azure Machine Learning/automatizado a la SDK

No se admite el widget de SDK de Azure Machine Learning en un cuaderno de Databricks porque los blocs de notas no pueden analizar los widgets HTML. Puede ver el widget en el portal mediante el uso de este código de Python en la celda del Bloc de notas de Azure Databricks:

```
displayHTML("<a href={} target='_blank'>Azure Portal: {}</a>".format(local_run.get_portal_url(), local_run.id))
```

### <a name="import-error-no-module-named-pandascoreindexes"></a>Error de importación: No hay ningún módulo denominado 'pandas.core.indexes'

Si ve este error cuando se usa había automatizada de aprendizaje automático:

1. Ejecute este comando para instalar dos paquetes en el clúster de Azure Databricks: 

   ```
   scikit-learn==0.19.1
   pandas==0.22.0
   ```

1. Separar y, a continuación, volver a adjuntar el clúster en el Bloc de notas. 

Si estos pasos no resuelven el problema, pruebe a reiniciar el clúster.

## <a name="azure-portal"></a>Azure Portal

Si ve directamente el área de trabajo desde un vínculo de recurso compartido desde el SDK o el portal, no podrá ver la página de información general normal con la información de suscripción en la extensión. Tampoco será capaz de cambiar a otra área de trabajo. Si necesita ver otra área de trabajo, la solución consiste en ir directamente a [Azure Portal](https://portal.azure.com) y buscar el nombre de la misma.

## <a name="diagnostic-logs"></a>Registros de diagnóstico

A veces puede resultar útil proporcionar información de diagnóstico al solicitar ayuda. Para ver algunos registros, visite [portal Azure](https://portal.azure.com) y vaya al área de trabajo y seleccione **área de trabajo > experimento > Ejecutar > registros**.

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
