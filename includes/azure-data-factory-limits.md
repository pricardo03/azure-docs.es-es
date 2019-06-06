---
title: archivo de inclusión
description: archivo de inclusión
services: data-factory
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 5/30/2019
ms.author: jingwang
ms.custom: include file
ms.openlocfilehash: ac0747dcff4e74363a58dc9aaf6da4dbd4c6a1c7
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66427581"
---
Azure Data Factory es un servicio multiempresa que tiene los siguientes límites predeterminados en su lugar para asegurarse de que las suscripciones de cliente están protegidas frente a cargas de trabajo de los demás. Para aumentar los límites hasta el máximo de su suscripción, póngase en contacto con soporte técnico.

### <a name="version-2"></a>versión 2

| Recurso | Límite predeterminado | Límite máximo |
| -------- | ------------- | ------------- |
| Factorías de datos en una suscripción de Azure | 50 | [Póngase en contacto con el servicio de soporte técnico](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Número total de entidades, como canalizaciones, conjuntos de datos, desencadenadores, servicios vinculados y los tiempos de ejecución de integración, dentro de una factoría de datos | 5.000 | [Póngase en contacto con el servicio de soporte técnico](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Total de núcleos de CPU para Azure-SSIS Integration Runtime en una suscripción | 256 | [Póngase en contacto con el servicio de soporte técnico](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Ejecuciones de canalizaciones simultáneas por la factoría de datos que se comparte entre todas las canalizaciones en la fábrica | 10 000  | [Póngase en contacto con el servicio de soporte técnico](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Ejecuta actividades externas simultáneas por suscripción por [región de Azure Integration Runtime](../articles/data-factory/concepts-integration-runtime.md#integration-runtime-location)<br><small>Actividades externas se administran en tiempo de ejecución de integración pero que se ejecutan en servicios vinculados, incluidos Databricks, procedimiento almacenado, HDInsights y otros.</small> | 3000 | [Póngase en contacto con el servicio de soporte técnico](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Se ejecuta la actividad de canalización simultánea por suscripción por [región de Azure Integration Runtime](../articles/data-factory/concepts-integration-runtime.md#integration-runtime-location) <br><small>Actividades de canalización se ejecutan en tiempo de ejecución de integración, incluidos la búsqueda, GetMetadata y eliminarán. </small>| 1000 | [Póngase en contacto con el servicio de soporte técnico](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Simultáneas de creación de operaciones por suscripción por [región de Azure Integration Runtime](../articles/data-factory/concepts-integration-runtime.md#integration-runtime-location)<br><small>Incluido Probar conexión, examinar carpeta listas y tabla, vista previa de datos. | 200 | [Póngase en contacto con el servicio de soporte técnico](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Unidades de integración de datos simultáneas<sup>1</sup> consumo por suscripción por [región de Azure Integration Runtime](../articles/data-factory/concepts-integration-runtime.md#integration-runtime-location)| Grupo de regiones 1<sup>2</sup>: 6000<br>Grupo de regiones 2<sup>2</sup>: 3000<br>Grupo de regiones de 3<sup>2</sup>: 1.500 | [Póngase en contacto con el servicio de soporte técnico](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Actividades máximas por la canalización, que incluye las actividades internas de contenedores | 40 | 40 |
| Número máximo de los tiempos de ejecución de integración vinculado que se pueden crear en un único entorno integration runtime autohospedado | 100 | [Póngase en contacto con el servicio de soporte técnico](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Parámetros máximos por canalización | 50 | 50 |
| Elementos ForEach | 100 000 | 100 000 |
| Paralelismo de ForEach | 20 | 50 |
| Caracteres por expresión | 8192 | 8192 |
| Intervalo de desencadenador de ventana de saltos de tamaño constante mínimo | 15 minutos | 15 minutos |
| Tiempo de espera máximo para la actividad de canalización se ejecuta | 7 días | 7 días |
| Bytes por objeto para objetos de canalización<sup>3</sup> | 200 KB | 200 KB |
| Bytes por objeto de conjunto de datos y objetos de servicio vinculado<sup>3</sup> | 100 KB | 2.000 KB |
| Unidades de datos de integración<sup>1</sup> por la ejecución de la actividad de copia | 256 | [Póngase en contacto con el servicio de soporte técnico](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Llamadas API de escritura | 2500/h<br/><br/> Este límite lo impone Azure Resource Manager, no Azure Data Factory. | [Póngase en contacto con el servicio de soporte técnico](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Llamadas API de lectura | 12.500/h<br/><br/> Este límite lo impone Azure Resource Manager, no Azure Data Factory. | [Póngase en contacto con el servicio de soporte técnico](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Supervisión de consultas por minuto | 1000 | [Póngase en contacto con el servicio de soporte técnico](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Operaciones CRUD de entidad por minuto | 50 | [Póngase en contacto con el servicio de soporte técnico](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |

<sup>1</sup> la unidad de integración de datos (DIU) se usa en una operación de copia de nube a nube, obtenga más información en [unidades de integración de datos (versión 2)](../articles/data-factory/copy-activity-performance.md#data-integration-units). Para obtener información sobre la facturación, consulte [precios de Azure Data Factory](https://azure.microsoft.com/pricing/details/data-factory/).

<sup>2</sup> [azure Integration Runtime](../articles/data-factory/concepts-integration-runtime.md#azure-integration-runtime) es [disponible globalmente](https://azure.microsoft.com/global-infrastructure/services/) para garantizar el cumplimiento de datos, la eficiencia y la red reducido los costos de salida. 

| Grupo de regiones | Regiones | 
| -------- | ------ |
| Grupo de regiones de 1 | EE. UU., este de Estados Unidos, East US2, Europa del Norte, Europa occidental, oeste de Estados Unidos, oeste de EE.UU. 2 |
| Grupo de regiones de 2 | Este de Australia, sudeste de Australia, sur de Brasil, India Central, este de Japón, Northcentral EE. UU., Centro-Sur de EE. UU., sudeste asiático, centro occidental de EE.UU |
| Grupo de regiones de 3 | Canadá Central, Asia oriental, Francia Central, Corea Central, sur de Reino Unido |

<sup>3</sup> canalización, conjunto de datos y objetos de servicio vinculado representan una agrupación lógica de la carga de trabajo. Los límites de estos objetos no están relacionados con la cantidad de datos, puede mover y procesar con Azure Data Factory. Factoría de datos está diseñada para escalarse para manejar petabytes de datos.

### <a name="version-1"></a>versión 1

| **Recurso** | **Límite predeterminado** | **Límite máximo** |
| --- | --- | --- |
| Factorías de datos en una suscripción de Azure |50 |[Póngase en contacto con el servicio de soporte técnico](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Canalizaciones dentro de una factoría de datos |2500 |[Póngase en contacto con el servicio de soporte técnico](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Conjuntos de datos dentro de una factoría de datos |5.000 |[Póngase en contacto con el servicio de soporte técnico](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Fragmentos simultáneos por conjunto de datos |10 |10 |
| Bytes por objeto para objetos de canalización<sup>1</sup> |200 KB |200 KB |
| Bytes por objeto de datos establecido y los objetos de servicio vinculados<sup>1</sup> |100 KB |2.000 KB |
| Núcleos de clúster y a petición de Azure HDInsight dentro de una suscripción<sup>2</sup> |60 |[Póngase en contacto con el servicio de soporte técnico](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Unidades de movimiento de datos por la ejecución de la actividad de copia de nube<sup>3</sup> |32 |[Póngase en contacto con el servicio de soporte técnico](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Número de reintentos de ejecuciones de la actividad Canalización |1000 |MaxInt (32 bits) |

<sup>1</sup> canalización, conjunto de datos y objetos de servicio vinculado representan una agrupación lógica de la carga de trabajo. Los límites de estos objetos no están relacionados con la cantidad de datos, puede mover y procesar con Azure Data Factory. Factoría de datos está diseñada para escalarse para manejar petabytes de datos.

<sup>2</sup>Los núcleos de HDInsight a petición se asignan fuera de la suscripción que contiene Data Factory. Como resultado, el límite anterior es el límite de núcleos aplicada por Data Factory para los núcleos de HDInsight y a petición. Es diferente del límite de núcleos que esté asociada con su suscripción de Azure.

<sup>3</sup> la unidad de movimiento de datos en la nube (DMU) para la versión 1 se utiliza en una operación de copia de nube a nube, obtenga más información en [unidades de movimiento de datos (versión 1) en la nube](../articles/data-factory/v1/data-factory-copy-activity-performance.md#cloud-data-movement-units). Para obtener información sobre la facturación, consulte [precios de Azure Data Factory](https://azure.microsoft.com/pricing/details/data-factory/).

| **Recurso** | **Límite inferior predeterminado** | **Límite mínimo** |
| --- | --- | --- |
| Intervalo de programación |15 minutos |15 minutos |
| Intervalo entre reintentos |1 segundo |1 segundo |
| Valor de tiempo de espera de reintento |1 segundo |1 segundo |

#### <a name="web-service-call-limits"></a>Límites de llamadas de servicio web
Azure Resource Manager tiene límites para las llamadas de API. Se pueden realizar llamadas API a un ritmo que esté dentro de los [límites de la API del Administrador de recursos de Azure](../articles/azure-subscription-service-limits.md#resource-group-limits).
