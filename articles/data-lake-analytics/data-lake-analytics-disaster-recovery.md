---
title: Guía de recuperación ante desastres de Azure Data Lake Analytics
description: Obtenga información sobre cómo planear la recuperación ante desastres para las cuentas de Azure Data Lake Analytics.
services: data-lake-analytics
author: MikeRys
ms.author: mrys
ms.reviewer: jasonwhowell
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: ea1d4020aa9be23b4839690ae0b386d35bce8a23
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66498895"
---
# <a name="disaster-recovery-guidance-for-azure-data-lake-analytics"></a>Guía de recuperación ante desastres de Azure Data Lake Analytics

Azure Data Lake Analytics es un servicio de trabajos de análisis a petición que simplifican los macrodatos. En lugar de implementar, configurar y ajustar el hardware, escribirá consultas para transformar los datos y extraer ideas valiosas. El servicio de análisis puede administrar trabajos de cualquier escala al instante, simplemente estableciendo el ajuste adecuado. Solo tiene que pagar por su trabajo cuando se está ejecutando, lo que hace que sea una solución económica. Este artículo proporcionan instrucciones sobre cómo proteger los trabajos de poco frecuentes interrupciones de toda la región o eliminaciones accidentales.

## <a name="disaster-recovery-guidance"></a>Guía de recuperación ante desastres

Cuando se usa Azure Data Lake Analytics, es fundamental para preparar su propio plan de recuperación ante desastres. En este artículo le guía para crear un plan de recuperación ante desastres. Existen recursos adicionales que pueden ayudarle a crear su propio plan:
- [Recuperación ante desastres y errores para aplicaciones de Azure](/azure/architecture/reliability/disaster-recovery)
- [Guía técnica sobre resistencia en Azure](/azure/architecture/reliability)

## <a name="best-practices-and-scenario-guidance"></a>Procedimientos recomendados e instrucciones de escenario

Puede ejecutar un trabajo periódico de U-SQL en una cuenta de ADLA en una región que lee y escribe las tablas de U-SQL, así como datos no estructurados.  Prepararse para un desastre siguiendo estos pasos:

1. Crear cuentas de ADLA y ADLS en la región secundaria que se usará durante una interrupción.

   > [!NOTE]
   > Dado que los nombres de cuenta son globalmente únicos, utilice un esquema de nomenclatura coherente que indica qué cuenta es secundaria.

2. Para datos no estructurados, hacen referencia a [Guía de recuperación ante desastres para los datos en Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-disaster-recovery-guidance.md)

3. Para datos estructurados almacenados en bases de datos y tablas ADLA, crear copias de los artefactos de metadatos, como las bases de datos, tablas, funciones con valores de tabla y los ensamblados. Deberá volver a sincronizar estos artefactos periódicamente cuando se producen cambios en producción. Por ejemplo, datos recién insertados debe replicarse en la región secundaria copiando los datos y insertar en la tabla secundaria.

   > [!NOTE]
   > Estos nombres de objeto que se limitan a la cuenta secundaria y no son globalmente únicos, por lo que pueden tener los mismos nombres que en la cuenta de producción principal.

Durante una interrupción, deberá actualizar las secuencias de comandos para que las rutas de acceso de entrada de punto a punto de conexión secundario. A continuación, los usuarios envían sus trabajos a la cuenta ADLA en la región secundaria. A continuación, se escribirá la salida del trabajo a la cuenta ADLA y ADLS en la región secundaria.

## <a name="next-steps"></a>Pasos siguientes

[Guía de recuperación ante desastres para los datos en Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-disaster-recovery-guidance.md)
