---
title: Solución de problemas de flujos de datos de Azure Data Factory | Microsoft Docs
description: Obtenga información acerca de la solución de problemas relacionados con flujos de datos en Azure Data Factory.
services: data-factory
author: kromerm
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 10/08/2019
ms.author: makromer
ms.openlocfilehash: b5895b061426066d265d3ff68dc948014e641322
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2019
ms.locfileid: "72242265"
---
# <a name="troubleshoot-azure-data-factory-data-flows"></a>Solución de problemas de flujos de datos en Azure Data Factory

En este artículo se exploran métodos comunes de solución de problemas de flujos de datos en Azure Data Factory.

## <a name="common-errors-and-messages"></a>Errores habituales y mensajes

### <a name="error-message-df-sys-01-shadeddatabricksorgapachehadoopfsazureazureexception-commicrosoftazurestoragestorageexception-the-specified-container-does-not-exist"></a>Mensaje de error: DF-SYS-01: shaded.databricks.org.apache.hadoop.fs.azure.AzureException: com.microsoft.azure.storage.StorageException: El contenedor especificado no existe.

- **Síntomas**: error en la ejecución del flujo de datos de la vista previa de datos, depuración y canalización porque el contenedor no existe

- **Causa**: cuando el conjunto de datos contiene un contenedor que no existe en el almacenamiento

- **Resolución**: asegúrese de que el contenedor al que se hace referencia en el conjunto de datos exista

### <a name="error-message-df-sys-01-javalangassertionerror-assertion-failed-conflicting-directory-structures-detected-suspicious-paths"></a>Mensaje de error: DF-SYS-01: java.lang.AssertionError: error de aserción: se han detectado estructuras de directorio en conflicto. Rutas de acceso sospechosas

- **Síntomas**: al usar caracteres comodín en la transformación de origen con archivos Parquet

- **Causa**: la sintaxis de los comodines es incorrecta o no es válida

- **Resolución**: compruebe la sintaxis de los caracteres comodín que se usan en las opciones de transformación de origen

### <a name="error-message-df-src-002-container-container-name-is-required"></a>Mensaje de error: DF-SRC-002: se requiere "container" (nombre de contenedor)

- **Síntomas**: error en la ejecución del flujo de datos de la vista previa de datos, depuración y canalización porque el contenedor no existe

- **Causa**: cuando el conjunto de datos contiene un contenedor que no existe en el almacenamiento

- **Resolución**: asegúrese de que el contenedor al que se hace referencia en el conjunto de datos exista

### <a name="error-message-df-uni-001-primarykeyvalue-has-incompatible-types-integertype-and-stringtype"></a>Mensaje de error: DF-UNI-001: PrimaryKeyValue tiene tipos incompatibles IntegerType y StringType

- **Síntomas**: error en la ejecución del flujo de datos de la vista previa de datos, depuración y canalización porque el contenedor no existe

- **Causa**: tiene lugar cuando se intenta insertar un tipo de clave principal incorrecto en los receptores de base de datos

- **Resolución**: use una columna derivada para convertir la columna que está usando para que la clave principal del flujo de datos coincida con el tipo de datos de la base de datos de destino

### <a name="error-message-df-sys-01-commicrosoftsqlserverjdbcsqlserverexception-the-tcpip-connection-to-the-host-xxxxxdatabasewindowsnet-port-1433-has-failed-error-xxxxdatabasewindowsnet-verify-the-connection-properties-make-sure-that-an-instance-of-sql-server-is-running-on-the-host-and-accepting-tcpip-connections-at-the-port-make-sure-that-tcp-connections-to-the-port-are-not-blocked-by-a-firewall"></a>Mensaje de error: DF-SYS-01: com.microsoft.sqlserver.jdbc.SQLServerException: error en la conexión TCP/IP al puerto 1433 del host xxxxx.database.windows.net. Error: "xxxx.database.windows.net. Compruebe las propiedades de conexión. Asegúrese de que se esté ejecutando una instancia de SQL Server en el host y se acepten las conexiones TCP/IP en el puerto. Asegúrese de que un firewall no bloquee las conexiones TCP al puerto."

- **Síntomas**: no se puede obtener una vista previa de los datos o ejecutar la canalización con el origen de base de datos o el receptor

- **Causa**: la base de datos está protegida por un firewall

- **Resolución**: abra el acceso del firewall a la base de datos

### <a name="error-message-df-sys-01-commicrosoftsqlserverjdbcsqlserverexception-there-is-already-an-object-named-xxxxxx-in-the-database"></a>Mensaje de error: DF-SYS-01: com.microsoft.sqlserver.jdbc.SQLServerException: ya existe un objeto denominado "xxxxxx" en la base de datos.

- **Síntomas**: el receptor no puede crear la tabla

- **Causa**: ya existe un nombre de tabla existente en la base de datos de destino con el mismo nombre definido en el origen o en el conjunto de datos

- **Resolución**: cambie el nombre de la tabla que está intentando crear

## <a name="general-troubleshooting-guidance"></a>Guía de solución de problemas generales

1. Compruebe el estado de las conexiones del conjunto de datos. En cada transformación de origen y de receptor, visite el servicio vinculado para cada conjunto de datos que use y pruebe las conexiones.
2. Compruebe el estado de las conexiones de archivos y tablas desde el diseñador de flujo de datos. Cambie al modo de depuración y haga clic en Vista previa de datos en las transformaciones de origen para asegurarse de que puede acceder a los datos.
3. Si todo parece correcto desde la vista previa de los datos, vaya al diseñador de canalizaciones y coloque el flujo de datos en una actividad de canalización. Depure la canalización para realizar una prueba de un extremo a otro.

## <a name="next-steps"></a>Pasos siguientes

Para obtener ayuda para solucionar problemas, pruebe estos recursos:

*  [Blog de Data Factory](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Solicitud de características de Data Factory](https://feedback.azure.com/forums/270578-data-factory)
*  [Vídeos de Azure](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Foro de MSDN](https://social.msdn.microsoft.com/Forums/home?sort=relevancedesc&brandIgnore=True&searchTerm=data+factory)
*  [Foro de Stack Overflow para Data Factory](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Información de Twitter sobre Data Factory](https://twitter.com/hashtag/DataFactory)
*  [Guía de optimización y rendimiento de la asignación de instancias de Data Flow](concepts-data-flow-performance.md)
