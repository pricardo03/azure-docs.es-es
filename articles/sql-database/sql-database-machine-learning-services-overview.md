---
title: Azure SQL Database Machine Learning Services con información general de R (versión preliminar)
description: En este artículo se describe los servicios Azure SQL Database Machine Learning (con R) y explica cómo funciona.
services: sql-database
ms.service: sql-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.reviewer: carlrab
manager: cgronlun
ms.date: 03/01/2019
ms.openlocfilehash: b33561ad80207a5d649ac1fd497c9d6c7add78c6
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64723990"
---
# <a name="azure-sql-database-machine-learning-services-with-r-preview"></a>Azure SQL Database Machine Learning Services con R (versión preliminar)

Machine Learning Services es una característica de Azure SQL Database que se usa para ejecutar scripts de R en bases de datos. Incluye paquetes de R de Microsoft para el análisis predictivos de alto rendimiento y el aprendizaje automático. Los datos relacionales pueden usarse en scripts de R mediante procedimientos almacenados, scripts de T-SQL que contienen instrucciones en R o código R que contiene T-SQL.

> [!IMPORTANT]
> Azure SQL Database Machine Learning Services (con R) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>
> La versión preliminar pública está disponible para bases de datos únicas y grupos elásticos mediante el modelo de compra basado en núcleos virtuales en el **de propósito general** y **crítico para la empresa** niveles de servicio. En esta versión preliminar pública inicial, no se admiten ni el nivel de servicio **hiperescala** ni la opción de implementación de **instancia administrada**. Actualmente, R es el único lenguaje que se admite. En este momento no hay ninguna compatibilidad con Python.
>
> La vista previa está disponible actualmente en las siguientes regiones: Europa occidental, Europa del Norte, oeste de EE.UU. 2, East US, EE.UU. Central sur, North Central US, Canadá Central, sudeste asiático, India del Sur y sudeste de Australia.
>
> [Suscríbase a la versión preliminar](#signup) a continuación.

## <a name="what-you-can-do-with-r"></a>Qué puede hacer con R

Utilice la potencia del lenguaje R para ofrecer análisis avanzados y aprendizaje automático en la base de datos. Esta capacidad lleva los cálculos y el procesamiento al lugar donde residen los datos, con lo que se elimina la necesidad de extraer datos a través de la red. Además, puede aprovechar la eficacia de los paquetes de R enterprise para proporcionar análisis avanzado a escala.

Machine Learning Services incluye una distribución básica de R, que se superpone con paquetes de R empresariales de Microsoft. Las funciones y los algoritmos de R de Microsoft están diseñados tanto para la escala como para la utilidad y ofrecen análisis predictivos, modelado estadístico, visualización de datos y algoritmos de aprendizaje automático de vanguardia.

### <a name="r-packages"></a>Paquetes de R

Paquetes de R de código abierto más comunes están preinstalados en Machine Learning Services. También se incluyen los siguientes paquetes de R de Microsoft:

| Paquete de R | DESCRIPCIÓN|
|-|-|
| [Microsoft R Open](https://mran.microsoft.com/rro) | Microsoft R Open es la distribución mejorada de R de Microsoft. Es una plataforma completa de código abierto para la ciencia de datos y análisis estadística. Esta basado en R y es 100 % compatible con él. Además, incluye capacidades adicionales para mejorar el rendimiento y reproducibilidad. |
| [RevoScaleR](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-revoscaler) | RevoScaleR es la biblioteca principal para el uso escalable de R y sus funciones se encuentran entre las más usadas. En estas bibliotecas puede encontrar transformaciones y manipulación de datos, resúmenes estadísticos, visualización y varias formas de modelado y análisis. Además, las funciones de estas bibliotecas distribuyen automáticamente las cargas de trabajo entre los núcleos disponibles para el procesamiento en paralelo, con la capacidad de trabajar en fragmentos de datos que el motor de cálculo coordina y administra. |
| [MicrosoftML (R)](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-microsoftml) | MicrosoftML agrega algoritmos de aprendizaje automático para crear modelos personalizados para el análisis textos, imágenes y opiniones. |

<!-- Add this back when the new package-related article is written
In addition to the pre-installed packages, you can [install additional packages](sql-database-quickstart-r-create-script.md#add-a-package).
-->

<a name="signup"></a>

## <a name="sign-up-for-the-preview"></a>Suscríbase a la vista previa

Siga los pasos a continuación para registrarse para participar en la versión preliminar pública:

1. Si no tiene una suscripción a Azure, [cree una cuenta](https://azure.microsoft.com/free/) antes de empezar.

2. Envíe un correo electrónico a Microsoft en [sqldbml@microsoft.com](mailto:sqldbml@microsoft.com) para registrarse en la versión preliminar pública. De forma predeterminada, la versión preliminar pública de Machine Learning Services (con R) en SQL Database no está habilitada.

Una vez que esté inscrito en el programa, Microsoft en el que podrá incorporar a la versión preliminar pública y permitir R nuevos o existentes para la base de datos.

Servicios de Machine Learning con R no se recomienda para cargas de trabajo de producción durante la versión preliminar pública.

## <a name="next-steps"></a>Pasos siguientes

- Consulte la [las principales diferencias de SQL Server Machine Learning Services](sql-database-machine-learning-services-differences.md).
- Para obtener información sobre cómo usar R para consultar Azure SQL Database Machine Learning Services (versión preliminar), consulte el [Guía de inicio rápido](sql-database-connect-query-r.md).
- Para empezar a trabajar con algunos scripts de R, consulte [crear y ejecutados scripts de R en Machine Learning Services (versión preliminar) de Azure SQL Database](sql-database-quickstart-r-create-script.md).
