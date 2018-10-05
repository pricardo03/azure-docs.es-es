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
ms.date: 09/24/2018
ms.openlocfilehash: d84040dc440c373ae9bae6dbac7a95109a387ba7
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2018
ms.locfileid: "47162753"
---
# <a name="known-issues-and-troubleshooting-azure-machine-learning-service"></a>Problemas conocidos y solución de problemas del servicio Azure Machine Learning
 
En este artículo obtendrá ayuda para buscar y corregir los errores que se producen al usar el servicio Azure Machine Learning. 


## <a name="databricks"></a>Databricks

Problemas de Databricks y Azure Machine Learning.

1. Recomendación para el clúster de Databricks:
   
   Cree el clúster de Azure Databricks como v4.x con Python 3. Se recomienda un clúster de alta simultaneidad.
 
1. Error de instalación del SDK de AML en Databricks cuando se instalan varios paquetes.

   Algunos paquetes, como `psutil upgrade libs`, pueden provocar conflictos. Para evitar errores de instalación, inmovilice la versión lib para instalar paquetes. Este problema está relacionado con Databricks y no con el SDK de AML. Ejemplo:
   ```python
   pstuil cryptography==1.5 pyopenssl==16.0.0 ipython=2.2.0
   ```

## <a name="gather-diagnostics-information"></a>Recopilación de información de diagnóstico
A veces puede resultar útil proporcionar información de diagnóstico al solicitar ayuda. Aquí es donde se encuentran los archivos de registro:

## <a name="resource-quotas"></a>Cuotas de recursos

Obtenga información sobre la [cuotas de recursos](how-to-manage-quotas.md) que puede encontrar al trabajar con Azure Machine Learning.

## <a name="get-more-support"></a>Obtener más soporte técnico

Puede enviar las solicitudes de soporte técnico y obtener ayuda de soporte técnico, foros, etc. [Más información...](support-for-aml-services.md)
