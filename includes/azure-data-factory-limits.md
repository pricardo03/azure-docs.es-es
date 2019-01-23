---
title: archivo de inclusión
description: archivo de inclusión
services: data-factory
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 1/10/2019
ms.author: jingwang
ms.custom: include file
ms.openlocfilehash: 2e9e4fe06f4d3802f6c0b3ff13e6cf6cdc218050
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/10/2019
ms.locfileid: "54211906"
---
Data Factory es un servicio multiinquilino que tiene los siguientes límites predeterminados para asegurarse de que las suscripciones de cliente están protegidas frente a las cargas de trabajo de los demás. Se pueden aumentar muchos de los límites para su suscripción fácilmente hasta el límite máximo; para ello, póngase en contacto con el soporte técnico.

### <a name="version-2"></a>versión 2

| Recurso | Límite predeterminado | Límite máximo |
| -------- | ------------- | ------------- |
| Factorías de datos en una suscripción de Azure | 50 | [Ponerse en contacto con soporte técnico](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Número total de entidades (canalización, conjuntos de datos, desencadenadores, servicios vinculados y tiempos de ejecución de integración) dentro de una factoría de datos | 5000 | [Ponerse en contacto con soporte técnico](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Núcleos de CPU totales para Integration Runtime de SSIS de Azure en una suscripción | 256 | [Ponerse en contacto con soporte técnico](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Ejecuciones de canalizaciones simultáneas por factoría de datos (que se comparten entre todas las canalizaciones de la fábrica) | 10 000  | [Ponerse en contacto con soporte técnico](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Número máximo de actividades por canalización (incluye las actividades internas para contenedores) | 40 | 40 |
| Número máximo de entornos vinculados de ejecución de integración que pueden crearse en un único entorno de ejecución de integración autohospedado. | 20 | [Ponerse en contacto con soporte técnico](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Parámetros máximos por canalización | 50 | 50 |
| Elementos ForEach | 100 000 | 100 000 |
| Paralelismo de ForEach | 20 | 50 |
| Caracteres por expresión | 8192 | 8192 |
| Intervalo mínimo del desencadenador de ventana de saltos de tamaño constante | 15 minutos | 15 minutos |
| Número máximo de tiempos de expiración de ejecuciones de actividad de canalización | 7 días | 7 días |
| Bytes por objeto para objetos de canalización <sup>1</sup> | 200 KB | 200 KB |
| Bytes por objeto para objetos de conjunto de datos y de servicio vinculados <sup>1</sup> | 100 KB | 2000 KB |
| Unidades de integración de datos por ejecución de la actividad de copia <sup>3</sup> | 256 | [Ponerse en contacto con soporte técnico](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Llamadas API de escritura | 2500/h<br/><br/> Este límite lo impone Azure Resource Manager, no Azure Data Factory. | [Póngase en contacto con el servicio de soporte técnico](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Llamadas API de lectura | 12 500/h<br/><br/> Este límite lo impone Azure Resource Manager, no Azure Data Factory. | [Ponerse en contacto con soporte técnico](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Supervisión de consultas por minuto | 1000 | [Ponerse en contacto con soporte técnico](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Operaciones CRUD de entidad por minuto | 50 | [Ponerse en contacto con soporte técnico](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |


### <a name="version-1"></a>versión 1

| **Recurso** | **Límite predeterminado** | **Límite máximo** |
| --- | --- | --- |
| Factorías de datos en una suscripción de Azure |50 |[Ponerse en contacto con soporte técnico](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Canalizaciones dentro de una factoría de datos |2.500 |[Ponerse en contacto con soporte técnico](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Conjuntos de datos dentro de una factoría de datos |5000 |[Ponerse en contacto con soporte técnico](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Fragmentos simultáneos por conjunto de datos |10 |10 |
| Bytes por objeto para objetos de canalización <sup>1</sup> |200 KB |200 KB |
| Bytes por objeto para objetos de conjunto de datos y de servicio vinculados <sup>1</sup> |100 KB |2000 KB |
| Núcleos de clúster a petición de HDInsight con una suscripción <sup>2</sup> |60 |[Ponerse en contacto con soporte técnico](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Unidades de movimiento de datos en la nube por ejecución de actividad de copia <sup>3</sup> |32 |[Ponerse en contacto con soporte técnico](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Número de reintentos de ejecuciones de la actividad Canalización |1000 |MaxInt (32 bits) |

<sup>1</sup> Los objetos de canalización, de conjunto de datos y de servicio vinculado representan una agrupación lógica de la carga de trabajo. Los límites de estos objetos no se corresponden relacionados con la cantidad de datos que se pueden mover y procesar con el servicio Azure Data Factory. Factoría de datos está diseñado para poder escalarse para manejar petabytes de datos.

<sup>2</sup>Los núcleos de HDInsight a petición se asignan fuera de la suscripción que contiene Data Factory. En consecuencia, el límite superior es el límite de núcleos aplicados de Data Factory para los núcleos de HDInsight a petición y es diferente del límite de núcleos asociado a su suscripción de Azure.

<sup>3</sup> Unidad de integración de datos (DIU) para v2 o Unidad de movimiento de datos de nube (DMU) para v1 se usa en una operación de copia de la nube a la nube. Es una medida que representa la eficacia (una combinación de CPU, memoria y asignación de recursos de red) de una única unidad en Data Factory. Poder usar más DMU en algunos escenarios permite lograr una mayor capacidad de procesamiento de copia. Consulte las secciones [Unidades de integración de datos (V2)](../articles/data-factory/copy-activity-performance.md#data-integration-units) y [Unidades de movimiento de datos (V1) en la nube](../articles/data-factory/v1/data-factory-copy-activity-performance.md#cloud-data-movement-units) para obtener más detalles, y la [página de precios de Azure Data Factory](https://azure.microsoft.com/pricing/details/data-factory/) para obtener implicación de facturación.

<sup>4</sup> Integration Runtime (IR) es la infraestructura de proceso que usa Azure Data Factory para proporcionar las siguientes funcionalidades de integración de datos en distintos entornos de red: movimiento de datos, distribución de actividades a Compute Services y ejecución de paquetes SSIS. Para más información, vea la [información general sobre Integration Runtime](../articles/data-factory/concepts-integration-runtime.md).

| **Recurso** | **Límite inferior predeterminado** | **Límite mínimo** |
| --- | --- | --- |
| Intervalo de programación |15 minutos |15 minutos |
| Intervalo entre reintentos |1 segundo |1 segundo |
| Valor de tiempo de espera de reintento |1 segundo |1 segundo |

#### <a name="web-service-call-limits"></a>Límites de llamadas de servicio web
Azure Resource Manager tiene límites para las llamadas de API. Se pueden realizar llamadas API a un ritmo que esté dentro de los [límites de la API del Administrador de recursos de Azure](../articles/azure-subscription-service-limits.md#resource-group-limits).
