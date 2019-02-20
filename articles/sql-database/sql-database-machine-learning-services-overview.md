---
title: Introducción a Machine Learning Services (con R) en Azure SQL Database (versión preliminar)
description: En este tema se describe Machine Learning Services (con R) en Azure SQL Database y se explica cómo funciona.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning-services
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.reviewer: carlrab
manager: cgronlun
ms.date: 02/06/2019
ms.openlocfilehash: b50fd21e4d3325875134d2e2e9caeed9f8db75d0
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55875610"
---
# <a name="machine-learning-services-with-r-in-azure-sql-database-preview"></a>Machine Learning Services (con R) en Azure SQL Database (versión preliminar)

Machine Learning Services es una característica de Azure SQL Database que se usa para ejecutar scripts de R en bases de datos. Incluye paquetes de R de Microsoft para el análisis predictivos de alto rendimiento y el aprendizaje automático. Los datos relacionales pueden usarse en scripts de R mediante procedimientos almacenados, scripts de T-SQL que contienen instrucciones en R o código R que contiene T-SQL.

> [!IMPORTANT]
> Machine Learning Services (con R) en Azure SQL Database está actualmente en versión preliminar pública para las bases de datos únicas y los grupos elásticos que usan modelos de compra basados en núcleos virtuales de los niveles de servicio **de uso general** y **crítico para la empresa**. En esta versión preliminar pública inicial, no se admiten ni el nivel de servicio **hiperescala** ni la opción de implementación de **instancia administrada**. Actualmente, R es el único lenguaje que se admite. En este momento no hay ninguna compatibilidad con Python. 
>
> La versión preliminar está disponible actualmente en las siguientes regiones: Europa Occidental, Europa del Norte, Oeste de EE. UU. 2, Este de EE. UU., Centro-sur de EE. UU., Centro-norte de EE. UU., Centro de Canadá, Sudeste Asiático, India del Sur y Sudeste de Australia. 
>
> [Suscríbase a la versión preliminar](#signup) a continuación.

## <a name="what-you-can-do-with-r"></a>Qué puede hacer con R

Utilice la potencia del lenguaje R para ofrecer análisis avanzados y aprendizaje automático en la base de datos. Esta capacidad lleva los cálculos y el procesamiento al lugar donde residen los datos, con lo que se elimina la necesidad de extraer datos a través de la red. Además, aproveche la eficacia de los paquetes de R empresariales para ofrecer análisis avanzados a escala.

Machine Learning Services incluye una distribución básica de R, que se superpone con paquetes de R empresariales de Microsoft. Las funciones y los algoritmos de R de Microsoft están diseñados tanto para la escala como para la utilidad y ofrecen análisis predictivos, modelado estadístico, visualización de datos y algoritmos de aprendizaje automático de vanguardia.

### <a name="r-packages"></a>Paquetes de R

Los paquetes de R de código abierto más comunes están preinstalados en Machine Learning Services. También se incluyen los siguientes paquetes de R de Microsoft:

| Paquete de R | DESCRIPCIÓN|
|-|-|
| [Microsoft R Open](https://mran.microsoft.com/rro) | Microsoft R Open es la distribución mejorada de R de Microsoft. Es una plataforma completa de código abierto para la ciencia de datos y el análisis estadístico. Esta basado en R y es 100 % compatible con él. Además, incluye capacidades adicionales para mejorar el rendimiento y reproducibilidad. |
| [RevoScaleR](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-revoscaler) | RevoScaleR es la biblioteca principal para el uso escalable de R y sus funciones se encuentran entre las más usadas. En estas bibliotecas puede encontrar transformaciones y manipulación de datos, resúmenes estadísticos, visualización y varias formas de modelado y análisis. Además, las funciones de estas bibliotecas distribuyen automáticamente las cargas de trabajo entre los núcleos disponibles para el procesamiento en paralelo, con la capacidad de trabajar en fragmentos de datos que el motor de cálculo coordina y administra. |
| [MicrosoftML (R)](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-microsoftml) | MicrosoftML agrega algoritmos de aprendizaje automático para crear modelos personalizados para el análisis textos, imágenes y opiniones. |

Además de los paquetes preinstalados, puede [instalar paquetes adicionales](sql-database-connect-query-r.md#add-package).

<a name="signup"></a>

## <a name="sign-up-for-the-preview"></a>Suscríbase a la vista previa

Siga los pasos a continuación para registrarse para participar en la versión preliminar pública:

1. Si no tiene una suscripción a Azure, [cree una cuenta](https://azure.microsoft.com/free/) antes de empezar.

2. Envíe un correo electrónico a Microsoft en [sqldbml@microsoft.com](mailto:sqldbml@microsoft.com) para registrarse en la versión preliminar pública. De forma predeterminada, la versión preliminar pública de Machine Learning Services (con R) en SQL Database no está habilitada.

Cuando ya esté inscrito en el programa, Microsoft lo incorporará a la versión preliminar pública y habilitará R para su base de datos existente o para una nueva.

No use Machine Learning Services con R para cargas de trabajo de producción durante la versión preliminar pública.

## <a name="next-steps"></a>Pasos siguientes

- Consulte las [principales diferencias de Machine Learning Services en SQL Server](sql-database-machine-learning-services-differences.md).
- Para obtener información sobre cómo usar Machine Learning Services (con R) en Azure SQL Database, consulte la [guía de inicio rápido](sql-database-connect-query-r.md).
- Obtenga más información con los [tutoriales del lenguaje R de SQL Server](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sql-server-r-tutorials).
