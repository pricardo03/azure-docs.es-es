---
title: Descripción de los precios de Azure Data Factory a través de ejemplos | Microsoft Docs
description: En este artículo se explica y muestra el modelo de precios de Azure Data Factory con ejemplos detallados
documentationcenter: ''
author: shlo
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 09/25/2018
ms.author: shlo
ms.openlocfilehash: a825982532047f6e311c5508394df243310f02ab
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2019
ms.locfileid: "65233924"
---
# <a name="understanding-data-factory-pricing-through-examples"></a>Descripción de los precios de Data Factory a través de ejemplos

En este artículo se explica y muestra el modelo de precios de Azure Data Factory con ejemplos detallados.

> [!NOTE]
> Los precios usados en estos ejemplos siguientes son hipotéticos y no pretende implica precios reales.

## <a name="copy-data-from-aws-s3-to-azure-blob-storage-hourly"></a>Copia de datos desde AWS S3 a Azure Blob Storage cada hora

En este escenario, va a copiar datos desde AWS S3 a Azure Blob Storage en una programación por hora.

Para lograr el escenario, es preciso crear una canalización con los siguientes elementos:

1. Una actividad de copia con un conjunto de datos de entrada para los datos que se van a copiar desde AWS S3.

2. Un conjunto de datos de salida para los datos de Azure Storage.

3. Un desencadenador de programación para ejecutar la canalización cada hora.

   ![Escenario 1](media/pricing-concepts/scenario1.png)

| **Operaciones** | **Tipos y unidades** |
| --- | --- |
| Creación de un servicio vinculado | 2 entidades de lectura y escritura  |
| Creación de conjuntos de datos | 4 entidades de lectura y escritura (2 para la creación del conjunto de datos y 2 para las referencias del servicio vinculado) |
| Creación de una canalización | 3 entidades de lectura y escritura (1 para la creación de la canalización y 2 para las referencias del conjunto de datos) |
| Obtención de la canalización | 1 entidad de lectura y escritura |
| Ejecución de la canalización | 2 ejecuciones de actividad (1 para la ejecución del desencadenador y 1 para ejecuciones de la actividad) |
| Asunción de la copia de datos: tiempo de ejecución = 10 min | 10 \* 4 Azure Integration Runtime (el valor predeterminado de la unidad de integración de datos es 4). Para más información sobre las unidades de integración de datos y la optimización del rendimiento de la copia, consulte [este artículo](copy-activity-performance.md) |
| Asunción de la supervisión de la canalización: solo se produjo 1 ejecución | 2 registros de ejecución de supervisión reintentados (1 para la ejecución de la canalización y 1 para la ejecución de la actividad) |

**Precio total del escenario: 0,16811 $**

- Operaciones de Data Factory = **0,0001 $**
  - Lectura y escritura = 10\*00001 = 0,0001 $ [1 L/E = 0,50 $/50000 = 0,00001]
  - Supervisión = 2\*000005 = 0,00001 $ [1 supervisión = 0,25 $/50000 = 0,000005]
- Orquestación y ejecución la canalización = **0,168 $**
  - Ejecuciones de actividad = 001\*2 = 0,002 [1 ejecución = 1 $/1000 = 0,001]
  - Actividades de movimiento de datos = 0,166 $ (prorrateo durante 10 minutos de tiempo de ejecución. 0,25 $/hora en Azure Integration Runtime)

## <a name="copy-data-and-transform-with-azure-databricks-hourly"></a>Copiar datos y transformarlos con Azure Databricks por hora

En este escenario, quiere copiar datos desde AWS S3 a Azure Blob Storage y transformarlos con Azure Databricks en una programación por hora.

Para lograr el escenario, es preciso crear una canalización con los siguientes elementos:

1. Una actividad de copia con un conjunto de datos de entrada para los datos que desea copiar desde AWS S3 y un conjunto de datos de salida para los datos del almacenamiento de Azure.
2. Una actividad de Azure Databricks para la transformación de datos.
3. Un desencadenador de programación para ejecutar la canalización cada hora.

![Escenario 2](media/pricing-concepts/scenario2.png)

| **Operaciones** | **Tipos y unidades** |
| --- | --- |
| Creación de un servicio vinculado | 3 entidades de lectura y escritura  |
| Creación de conjuntos de datos | 4 entidades de lectura y escritura (2 para la creación del conjunto de datos y 2 para las referencias del servicio vinculado) |
| Creación de una canalización | 3 entidades de lectura y escritura (1 para la creación de la canalización y 2 para las referencias del conjunto de datos) |
| Obtención de la canalización | 1 entidad de lectura y escritura |
| Ejecución de la canalización | 3 ejecuciones de actividad (1 para la ejecución del desencadenador, 2 para ejecuciones de actividad) |
| Asunción de la copia de datos: tiempo de ejecución = 10 min | 10 \* 4 Azure Integration Runtime (el valor predeterminado de la unidad de integración de datos es 4). Para más información sobre las unidades de integración de datos y la optimización del rendimiento de la copia, consulte [este artículo](copy-activity-performance.md) |
| Asunción de la supervisión de la canalización: solo se produjo 1 ejecución | 3 registros de supervisión de la ejecución reintentados (1 para la ejecución de la canalización y 2 para la ejecución de la actividad) |
| Asunción de la ejecución de la actividad de Databricks: tiempo de ejecución = 10 min | Ejecución de la actividad de canalización externa de 10 min |

**Precio total del escenario: 0,16916 $**

- Operaciones de Data Factory = **0,00012 $**
  - Lectura y escritura = 11\*00001 = 0,00011 $ [1 L/E = 0,50 $/50000 = 0,00001]
  - Supervisión = 3\*000005 = 0,00001 $ [1 supervisión = 0,25 $/50000 = 0,000005]
- Orquestación y ejecución de la canalización = **0,16904 $**
  - Ejecuciones de actividad = 001\*3 = 0,003 [1 ejecución = 1 $/1000 = 0,001]
  - Actividades de movimiento de datos = 0,166 $ (prorrateo durante 10 minutos de tiempo de ejecución. 0,25 $/hora en Azure Integration Runtime)
  - Actividad de canalización externa = 0,000041 $ (prorrateo durante 10 minutos de tiempo de ejecución. 0,00025 $/hora en Azure Integration Runtime)

## <a name="copy-data-and-transform-with-dynamic-parameters-hourly"></a>Copiar datos y transformarlos con parámetros dinámicos por hora

En este escenario, quiere copiar datos desde AWS S3 a Azure Blob Storage y transformarlos con Azure Databricks (con parámetros dinámicos en el script) en una programación por hora.

Para lograr el escenario, es preciso crear una canalización con los siguientes elementos:

1. Una actividad de copia con un conjunto de datos de entrada para los datos que desea copiar desde AWS S3 y un conjunto de datos de salida para los datos del almacenamiento de Azure.
2. Una actividad de búsqueda para pasar parámetros de forma dinámica al script de transformación.
3. Una actividad de Azure Databricks para la transformación de datos.
4. Un desencadenador de programación para ejecutar la canalización cada hora.

![Escenario 3](media/pricing-concepts/scenario3.png)

| **Operaciones** | **Tipos y unidades** |
| --- | --- |
| Creación de un servicio vinculado | 3 entidades de lectura y escritura  |
| Creación de conjuntos de datos | 4 entidades de lectura y escritura (2 para la creación del conjunto de datos y 2 para las referencias del servicio vinculado) |
| Creación de una canalización | 3 entidades de lectura y escritura (1 para la creación de la canalización y 2 para las referencias del conjunto de datos) |
| Obtención de la canalización | 1 entidad de lectura y escritura |
| Ejecución de la canalización | 4 ejecuciones de actividad (1 para la ejecución del desencadenador, 3 para ejecuciones de actividad) |
| Asunción de la copia de datos: tiempo de ejecución = 10 min | 10 \* 4 Azure Integration Runtime (el valor predeterminado de la unidad de integración de datos es 4). Para más información sobre las unidades de integración de datos y la optimización del rendimiento de la copia, consulte [este artículo](copy-activity-performance.md) |
| Asunción de la supervisión de la canalización: solo se produjo 1 ejecución | 4 registros de ejecución de supervisión reintentados (1 para la ejecución de la canalización y 3 para la ejecución de la actividad) |
| Asunción de la ejecución de actividad de búsqueda: tiempo de ejecución = 1 min | Ejecución de la actividad de canalización de 1 min |
| Asunción de la ejecución de la actividad de Databricks: tiempo de ejecución = 10 min | Ejecución de la actividad de canalización externa de 10 min |

**Precio total del escenario: 0,17020 $**

- Operaciones de Data Factory = **0,00013 $**
  - Lectura y escritura = 11\*00001 = 0,00011 $ [1 L/E = 0,50 $/50000 = 0,00001]
  - Supervisión = 4\*000005 = 0,00002 $ [1 supervisión = 0,25 $/50000 = 0,000005]
- Orquestación y ejecución de canalizaciones = **0,17007 $**
  - Ejecuciones de actividad = 001\*4 = 0,004 [1 ejecución = 1 $/1000 = 0,001]
  - Actividades de movimiento de datos = 0,166 $ (prorrateo durante 10 minutos de tiempo de ejecución. 0,25 $/hora en Azure Integration Runtime)
  - Actividad de canalización = 0,00003 $ (prorrateo durante 1 minuto de tiempo de ejecución. 0,002 $/hora en Azure Integration Runtime)
  - Actividad de canalización externa = 0,000041 $ (prorrateo durante 10 minutos de tiempo de ejecución. 0,00025 $/hora en Azure Integration Runtime)

## <a name="using-mapping-data-flow-debug-for-a-normal-workday-preview-pricing"></a>Con la depuración de flujo de datos de asignación para un día laboral normal (precios de vista previa)

Como ingeniero de datos, usted es responsable de diseñar, compilar y probar la asignación de flujos de datos cada día. Inicie sesión en la UI ADF por la mañana y habilitar el modo de depuración de flujos de datos. El TTL predeterminado para las sesiones de depuración es de 60 minutos. Funcionan a lo largo del día durante 10 horas, por lo que nunca expira la sesión de depuración. Por lo tanto, el precio del día será:

**10 (horas) x 8 (núcleos) x $0.112 = $8.96**

## <a name="transform-data-in-blob-store-with-mapping-data-flows-preview-pricing"></a>Transformación de datos en el almacén de blobs con la asignación de los flujos de datos (precios de vista previa)

En este escenario, desea transformar los datos de Blob Store visualmente en ADF asignación fluyen los datos en una programación por hora.

Para lograr el escenario, es preciso crear una canalización con los siguientes elementos:

1. Una actividad de flujo de datos con la lógica de transformación.

2. Un conjunto de datos de entrada para los datos en Azure Storage.

3. Un conjunto de datos de salida para los datos de Azure Storage.

4. Un desencadenador de programación para ejecutar la canalización cada hora.

| **Operaciones** | **Tipos y unidades** |
| --- | --- |
| Creación de un servicio vinculado | 2 entidades de lectura y escritura  |
| Creación de conjuntos de datos | 4 entidades de lectura y escritura (2 para la creación del conjunto de datos y 2 para las referencias del servicio vinculado) |
| Creación de una canalización | 3 entidades de lectura y escritura (1 para la creación de la canalización y 2 para las referencias del conjunto de datos) |
| Obtención de la canalización | 1 entidad de lectura y escritura |
| Ejecución de la canalización | 2 ejecuciones de actividad (1 para la ejecución del desencadenador y 1 para ejecuciones de la actividad) |
| Tiempo de ejecución de flujo de datos las suposiciones son: = 10 min + TTL de 10 minutos | 10 \* 8 núcleos de proceso General con TTL de 10 |
| Asunción de la supervisión de la canalización: solo se produjo 1 ejecución | 2 registros de ejecución de supervisión reintentados (1 para la ejecución de la canalización y 1 para la ejecución de la actividad) |

**Total de los precios de escenario: $0.3011**

- Operaciones de Data Factory = **0,0001 $**
  - Lectura y escritura = 10\*00001 = 0,0001 $ [1 L/E = 0,50 $/50000 = 0,00001]
  - Supervisión = 2\*000005 = 0,00001 $ [1 supervisión = 0,25 $/50000 = 0,000005]
- Orquestación de canalizaciones &amp; ejecución = **0.301 $**
  - Ejecuciones de actividad = 001\*2 = 0,002 [1 ejecución = 1 $/1000 = 0,001]
  - Actividades de flujo de datos = $0.299 prorrateo durante 20 minutos (10 minutos el tiempo de ejecución + TTL de 10 minutos). proceso 0.112 $/ hora en tiempo de ejecución de integración de Azure con 8 núcleos generales

## <a name="next-steps"></a>Pasos siguientes

¡Ahora que comprende los precios de Azure Data Factory, puede empezar a trabajar!

- [Creación de una factoría de datos con la interfaz de usuario de Azure Data Factory](quickstart-create-data-factory-portal.md)

- [Introducción al servicio Azure Data Factory](introduction.md)

- [Creación visual en Azure Data Factory](author-visually.md)
