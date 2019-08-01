---
title: 'Recuperación de base de datos acelerada: Azure SQL Database | Microsoft Docs'
description: Azure SQL Database tiene una nueva característica que proporciona recuperación de bases de datos rápida y coherente, reversión de transacción instantánea y truncamiento de registros agresivo para bases de datos únicas y bases de datos agrupadas de Azure SQL Database, y bases de datos de Azure SQL Data Warehouse.
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: mashamsft
ms.author: mathoma
ms.reviewer: carlrab
ms.date: 01/25/2019
ms.openlocfilehash: d516dc51a25cbef92ff9fa22012773507b528a99
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68569620"
---
# <a name="accelerated-database-recovery"></a>Recuperación de base de datos acelerada

**Recuperación de base de datos acelerada (ADR)** es una nueva característica del motor de base de datos SQL que mejora considerablemente la disponibilidad de la base de datos, en especial en presencia de transacciones de ejecución prolongada, al rediseñar el proceso de recuperación del motor de base de datos SQL. ADR está actualmente disponible para bases de datos únicas y agrupadas de Azure SQL Database, así como para bases de datos de Azure SQL Data Warehouse (actualmente en versión preliminar pública). Las principales ventajas de ADR son:

- **Recuperación de bases de datos rápida y coherente**

  Con ADR, las transacciones de ejecución prolongada no afectan al tiempo de recuperación general, lo que permite una recuperación de bases de datos rápida y coherente independientemente del número de transacciones activas en el sistema o de sus tamaños.

- **Reversión de transacción instantánea**

  Con ADR, la reversión de transacción es instantánea, independientemente del tiempo que haya estado activa la transacción o del número de actualizaciones que haya realizado.

- **Truncamiento de registros agresivo**

  Con ADR, el registro de transacciones se trunca de forma agresiva, incluso en presencia de transacciones de ejecución prolongada activas, lo que evita que crezca sin control.

## <a name="the-current-database-recovery-process"></a>El proceso actual de recuperación de bases de datos

La recuperación de bases de datos de SQL Server sigue el modelo de recuperación [ARIES](https://people.eecs.berkeley.edu/~brewer/cs262/Aries.pdf) y consta de tres fases, que se muestran en el diagrama siguiente y se explican con más detalle después del diagrama.

![Proceso de recuperación actual](./media/sql-database-accelerated-database-recovery/current-recovery-process.png)

- **Fase de análisis**

  Análisis hacia delante del registro de transacciones desde el principio del último punto de comprobación correcto (o el LSN de página desfasada más antiguo) hasta el final para determinar el estado de cada transacción en el momento en que SQL Server se ha detenido.

- **Fase de puesta al día**

  Análisis hacia delante del registro de transacciones desde la transacción más antigua sin confirmar hasta el final para actualizar la base de datos al estado que tenía en el momento del bloqueo al poner al día todas las operaciones confirmadas.

- **Fase de reversión**

  Para cada transacción activa en el momento del bloqueo, recorre el registro hacia atrás y deshace las operaciones realizadas por esta transacción.

En función de este diseño, el tiempo que tarda el motor de base de datos SQL en recuperarse de un reinicio inesperado es (aproximadamente) proporcional al tamaño de la transacción activa más larga del sistema en el momento del bloqueo. La recuperación requiere una reversión de todas las transacciones incompletas. El período de tiempo necesario es proporcional al trabajo que ha realizado la transacción y al tiempo que ha estado activa. Por lo tanto, el proceso de recuperación de SQL Server puede tardar mucho en presencia de transacciones de ejecución prolongada (por ejemplo, operaciones de inserción masivas de gran tamaño u operaciones de compilación de índices en una tabla grande).

Además, la cancelación o la reversión de una transacción grande en función de este diseño puede tardar mucho porque usa la misma fase de recuperación de reversión descrita anteriormente.

Además, el motor de base de datos SQL no puede truncar el registro de transacciones si hay transacciones de ejecución prolongada, ya que sus entradas de registro correspondientes son necesarias para los procesos de recuperación y reversión. Como resultado de este diseño del motor de base de datos SQL, algunos clientes se enfrentan al problema de que el tamaño del registro de transacciones crece mucho y consume cantidades enormes de espacio de la unidad.

## <a name="the-accelerated-database-recovery-process"></a>El proceso de recuperación de base de datos acelerada

ADR aborda los problemas anteriores al rediseñar por completo el proceso de recuperación del motor de base de datos SQL para:

- Que sea constante o instantáneo al evitar el análisis del registro desde o hacia el principio de la transacción activa más antigua. Con ADR, el registro de transacciones solo se procesa desde el último punto de comprobación correcto (o un número de secuencia de registro [LSN] de página desfasada más antiguo). Como resultado, el tiempo de recuperación no se ve afectado por las transacciones de ejecución prolongada.
- Minimizar el espacio de registro de transacciones necesario, ya que ya no es necesario procesar el registro de toda la transacción. Como resultado, se puede truncar el registro de transacciones de forma agresiva a medida que se producen los puntos de comprobación y las copias de seguridad.

En general, ADR consigue la recuperación rápida de bases de datos al controlar las versiones de todas las modificaciones de bases de datos físicas y deshacer únicamente las operaciones lógicas, que están limitadas y se pueden deshacer casi al instante. Cualquier transacción que estuviera activa en el momento de un bloqueo se marca como anulada y, por lo tanto, las consultas de usuario simultáneas pueden pasar por alto las versiones generadas por estas transacciones.

El proceso de recuperación ADR tiene las mismas tres fases que el proceso de recuperación actual. El funcionamiento de estas fases con ADR se muestra en el siguiente diagrama y se explica con más detalle después del diagrama.

![Proceso de recuperación ADR](./media/sql-database-accelerated-database-recovery/adr-recovery-process.png)

- **Fase de análisis**

  El proceso sigue siendo el mismo que hoy en día con la adición de que se reconstruye sLog y se copian las entradas del registro de las operaciones sin control de versiones.
  
- **Fase de puesta al día**

  Se divide en dos fases
  - Fase 1

      Puesta al día desde sLog (transacción sin confirmar más antigua hasta el último punto de comprobación). La puesta al día es una operación rápida, ya que solo es necesario procesar algunos registros de sLog.
      
  - Fase 2

     La puesta al día desde el registro de transacciones se inicia desde el último punto de comprobación (en lugar de la transacción sin confirmar más antigua)
     
- **Fase de reversión**

   La fase de reversión con ADR termina casi instantáneamente al usar sLog para deshacer las operaciones sin control de versiones y el almacén de versiones persistente (PVS) con reversión lógica para realizar la reversión basada en la versión del nivel de fila.

## <a name="adr-recovery-components"></a>Componentes de recuperación ADR

Los cuatro componentes clave de ADR son:

- **Almacén de versiones persistente (PVS)**

  El almacén de versiones persistente es un nuevo mecanismo de motor de base de datos SQL para conservar las versiones de fila generadas en la propia base de datos en lugar del almacén de versiones `tempdb` tradicional. PVS habilita el aislamiento de recursos, además de mejorar la disponibilidad de las réplicas secundarias legibles.

- **Reversión lógica**

  La reversión lógica es el proceso asincrónico responsable de realizar la reversión basada en la versión de nivel de fila, lo que proporciona una reversión de transacción instantánea y deshace todas las operaciones con control de versiones.

  - Realiza un seguimiento de todas las transacciones anuladas
  - Realiza la reversión mediante PVS de todas las transacciones de usuario
  - Libera todos los bloqueos inmediatamente después de la anulación de transacciones

- **sLog**

  sLog es una secuencia de registro en memoria secundaria que almacena las entradas de registro de las operaciones sin control de versiones (por ejemplo, la anulación de la caché de metadatos, las adquisiciones de bloqueo, etc.). SLog:

  - Es de bajo volumen y en memoria
  - Se conserva en el disco mediante su serialización durante el proceso de punto de comprobación
  - Se trunca periódicamente cuando se confirman las transacciones
  - Acelera la puesta al día y la reversión mediante el procesamiento de solo las operaciones sin control de versiones  
  - Permite el truncamiento agresivo del registro de transacciones al conservar solo las entradas del registro necesarias

- **Limpieza**

  La limpieza es el proceso asincrónico que se activa periódicamente y limpia las versiones de página que no son necesarias.

## <a name="who-should-consider-accelerated-database-recovery"></a>Quién debería considerar la recuperación de base de datos acelerada

Los siguientes tipos de clientes deberían considerar la habilitación de ADR:

- Clientes con cargas de trabajo con transacciones de ejecución prolongada.
- Clientes que han visto casos en que las transacciones activas hacen que el registro de transacciones crezca considerablemente.  
- Clientes que han experimentado largos períodos de falta de disponibilidad de una base de datos debido a la recuperación de ejecución prolongada de SQL Server (por ejemplo, un reinicio inesperado de SQL Server o una reversión de transacción manual).

