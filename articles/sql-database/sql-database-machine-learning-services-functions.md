---
title: Escribir funciones avanzadas de R
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Obtenga información sobre cómo escribir una función de R para el cálculo estadístico avanzada en Azure SQL Database mediante Machine Learning Services (versión preliminar).
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: r
ms.topic: conceptual
author: garyericson
ms.author: garye
ms.reviewer: davidph
manager: cgronlun
ms.date: 04/11/2019
ms.openlocfilehash: 939798d5d9eb2843d7bbbbe74680342e4ce6ce95
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "60014787"
---
# <a name="write-advanced-r-functions-in-azure-sql-database-using-machine-learning-services-preview"></a>Escribir funciones avanzadas de R en Azure SQL Database mediante Machine Learning Services (versión preliminar)

En este artículo se describe cómo insertar R matemática y funciones de utilidad en una instancia de SQL de procedimiento almacenan. Funciones estadísticas avanzadas que son difíciles de implementar en T-SQL se pueden hacer en R con una sola línea de código.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Requisitos previos

- Si no tiene una suscripción a Azure, [cree una cuenta](https://azure.microsoft.com/free/) antes de empezar.

- Para ejecutar el código de ejemplo en estos ejercicios, primero debe tener una base de datos de SQL Azure con Machine Learning Services (con R) habilitado. Durante la versión preliminar pública, Microsoft le incorporará y habilitará el aprendizaje automático para la base de datos nueva o existente. Siga los pasos que se indican en [Suscríbase a la versión preliminar](sql-database-machine-learning-services-overview.md#signup).

- Asegúrese de que ha instalado la versión más reciente [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS). Puede ejecutar scripts de R mediante otra administración de base de datos o las herramientas de consulta, pero en este tutorial usará SSMS.

## <a name="create-a-stored-procedure-to-generate-random-numbers"></a>Crear un procedimiento almacenado para generar números aleatorios

Por motivos de simplicidad, vamos a usar R `stats` paquete que ha instalado y se cargan de forma predeterminada con Azure SQL Database mediante Machine Learning Services (versión preliminar). El paquete contiene cientos de funciones para tareas estadísticas comunes, entre ellos el `rnorm` función. Esta función genera un número especificado de números aleatorios mediante la distribución normal, dada una desviación estándar y medios.

Por ejemplo, el siguiente código R devuelve 100 números en una media de 50, dada una desviación estándar de 3.

```R
as.data.frame(rnorm(100, mean = 50, sd = 3));
```

Para llamar a esta línea de R desde T-SQL, ejecute `sp_execute_external_script` y agregue la función de R en el parámetro de script de R, similar al siguiente:

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
OutputDataSet <- as.data.frame(rnorm(100, mean = 50, sd =3));
'
    , @input_data_1 = N'   ;'
WITH RESULT SETS(([Density] FLOAT NOT NULL));
```

¿Qué ocurre si desea que sea más fácil generar un conjunto diferente de números aleatorios?

Eso es sencillo cuando se combina con SQL. Defina un procedimiento almacenado que obtiene los argumentos del usuario, a continuación, pasa esos argumentos al script de R como variables.

```sql
CREATE PROCEDURE MyRNorm (
    @param1 INT
    , @param2 INT
    , @param3 INT
    )
AS
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
OutputDataSet <- as.data.frame(rnorm(mynumbers, mymean, mysd));
'
    , @input_data_1 = N'   ;'
    , @params = N' @mynumbers int, @mymean int, @mysd int'
    , @mynumbers = @param1
    , @mymean = @param2
    , @mysd = @param3
WITH RESULT SETS(([Density] FLOAT NOT NULL));
```

- La primera línea define cada uno de los parámetros de entrada de SQL que son necesarios cuando se ejecuta el procedimiento almacenado.

- La línea que comienza con `@params` define todas las variables usadas por el código de R y los correspondientes tipos de datos SQL.

- Las líneas que siguen inmediatamente asignan los nombres de parámetro SQL a los nombres de variable de R correspondientes.

Ahora que ha ajustado la función de R en un procedimiento almacenado, puede llamar a la función fácilmente y pasar distintos valores, similar al siguiente:

```sql
EXECUTE MyRNorm @param1 = 100
    , @param2 = 50
    , @param3 = 3
```

## <a name="use-r-utility-functions-for-troubleshooting"></a>Usar funciones de utilidad de R para solucionar problemas

El `utils` paquete, se instala de forma predeterminada, proporciona una variedad de funciones de utilidad para investigar el entorno actual de R. Estas funciones pueden ser útiles si se detecta discrepancias en la forma en que el código de R se realiza en SQL y en entornos exteriores. Por ejemplo, podría usar la R `memory.limit()` función para obtener memoria para el entorno actual de R.

Dado que el `utils` paquete está instalado pero no se cargan de forma predeterminada, debe usar el `library()` función para cargarlo en primer lugar.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
library(utils);
mymemory <- memory.limit();
OutputDataSet <- as.data.frame(mymemory);
'
    , @input_data_1 = N' ;'
WITH RESULT SETS(([Col1] INT NOT NULL));
```

> [!TIP]
> Al igual que muchos usuarios usar las funciones de control de tiempo del sistema en R, como `system.time` y `proc.time`, para capturar el tiempo utilizado por los procesos de R y analizar problemas de rendimiento.
