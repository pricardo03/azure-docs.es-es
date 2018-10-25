---
title: Problemas conocidos y solución de problemas del servicio Azure Machine Learning
description: Obtener una lista de los problemas conocidos, soluciones alternativas y solución de problemas
services: machine-learning
author: j-martens
ms.author: jmartens
ms.reviewer: mldocs
ms.service: machine-learning
ms.component: core
ms.topic: article
ms.date: 10/01/2018
ms.openlocfilehash: 02cee5a3e088c919ec94aee6f46ef6f428b9bb48
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2018
ms.locfileid: "48249424"
---
# <a name="known-issues-and-troubleshooting-azure-machine-learning-service"></a>Problemas conocidos y solución de problemas del servicio Azure Machine Learning
 
En este artículo obtendrá ayuda para buscar y corregir los errores que se producen al usar el servicio Azure Machine Learning. 

## <a name="sdk-installation-issues"></a>Problemas de instalación de SDK

**Mensaje de error: no se puede desinstalar 'PyYAML'** 

PyYAML es un proyecto instalado de Distutils. Por lo tanto, no podemos determinar con precisión qué archivos le pertenecen en caso de una desinstalación parcial. Para continuar con la instalación del SDK sin tener en cuenta este error, use:
```Python 
pip install --upgrade azureml-sdk[notebooks,automl] --ignore-installed PyYAML
```

## <a name="image-building-failure"></a>Error de creación de imagen

Error de creación de imagen al implementar el servicio web. La solución alternativa es agregar "pynacl==1.2.1" como una dependencia pip al archivo Conda para la configuración de la imagen.  

## <a name="pipelines"></a>Procesos
Se produce un error al llamar a PythonScriptStep varias veces seguidas sin cambiar el script o los parámetros. Una solución alternativa es volver a crear el objeto PipelineData.

## <a name="fpgas"></a>FPGA
No podrá implementar modelos en FPGA hasta que haya solicitado y se haya aprobado para la cuota FPGA. Para solicitar acceso, rellene el formulario de solicitud de cuota: https://aka.ms/aml-real-time-ai

## <a name="databricks"></a>Databricks

Problemas de Databricks y Azure Machine Learning.

1. Recomendación para el clúster de Databricks:
   
   Cree el clúster de Azure Databricks como v4.x con Python 3. Se recomienda un clúster de alta simultaneidad.
 
1. Error de instalación del SDK de AML en Databricks cuando se instalan varios paquetes.

   Algunos paquetes, como `psutil upgrade libs`, pueden provocar conflictos. Para evitar errores de instalación, inmovilice la versión lib para instalar paquetes. Este problema está relacionado con Databricks y no con el SDK de AML. Ejemplo:
   ```python
   pstuil cryptography==1.5 pyopenssl==16.0.0 ipython=2.2.0
   ```

## <a name="diagnostic-logs"></a>Registros de diagnóstico
A veces puede resultar útil proporcionar información de diagnóstico al solicitar ayuda. Aquí es donde se encuentran los archivos de registro:

## <a name="resource-quotas"></a>Cuotas de recursos

Obtenga información sobre la [cuotas de recursos](how-to-manage-quotas.md) que puede encontrar al trabajar con Azure Machine Learning.

## <a name="get-more-support"></a>Obtener más soporte técnico

Puede enviar las solicitudes de soporte técnico y obtener ayuda de soporte técnico, foros, etc. [Más información...](support-for-aml-services.md)
