---
title: Detección de amenazas en servicios de datos en Azure Security Center | Microsoft Docs
description: En este tema se presentan las alertas de servicios de datos disponibles en Azure Security Center.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: da960861-0b6c-4d80-932d-898cdebb4f83
ms.service: security-center
ms.topic: conceptual
ms.date: 07/24/2019
ms.author: memildin
ms.openlocfilehash: 7e45dffd15cdf2acf15c7d46ed0cf42fc5997a6a
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2019
ms.locfileid: "72244575"
---
# <a name="threat-detection-for-data-services-in-azure-security-center"></a>Detección de amenazas en servicios de datos en Azure Security Center

 Azure Security Center analiza los registros de los servicios de almacenamiento de datos y activa alertas cuando detecta una amenaza para los recursos de datos. En este tema se enumeran las alertas que Security Center genera para los siguientes servicios:

* [Azure SQL Database y Azure SQL Data Warehouse](#data-sql)
* [Azure Storage](#azure-storage)
* [Azure Cosmos DB](#cosmos-db)

## SQL Database y SQL Data Warehouse <a name="data-sql"></a>

La detección de amenazas de SQL identifica actividades anómalas que indican intentos inusuales y potencialmente peligrosos de obtener acceso a las bases de datos o de vulnerar su seguridad. 

|Alerta|DESCRIPCIÓN|
|---|---|
|**Vulnerabilidad a la inyección de código SQL**|Una aplicación ha generado una instrucción SQL errónea en la base de datos. Esto puede indicar una posible vulnerabilidad ante ataques por inyección de código SQL. Hay dos razones posibles para una instrucción errónea. Es posible que haya un defecto en el código de la aplicación que esté creando la instrucción SQL errónea. O bien, el código de la aplicación o los procedimientos almacenados no corrigen los datos que proporciona el usuario al construir la instrucción SQL errónea, lo que se puede aprovechar para ataques por inyección de código SQL.|
|**Posible ataque por inyección de código SQL**|Se ha producido una vulnerabilidad de la seguridad activa contra una aplicación identificada como vulnerable a la inyección de SQL. Esto significa que un atacante está intentando inyectar instrucciones SQL malintencionadas mediante el código de la aplicación vulnerable o procedimientos almacenados.|
|**Acceso desde una ubicación inusual**|Se ha producido un cambio en el patrón de acceso a SQL Server por el que alguien ha iniciado sesión en el servidor desde una ubicación geográfica inusual. En algunos casos, la alerta detecta una acción legítima (una nueva aplicación o el mantenimiento de un desarrollador). En otros casos, la alerta detecta una acción malintencionada (un antiguo empleado o un atacante externo).|
|**Acceso desde una entidad de seguridad desconocida**|Se produjo un cambio en el patrón de acceso a SQL Server. Alguien inició sesión en el servidor mediante una entidad de seguridad inusual (usuario). En algunos casos, la alerta detecta una acción legítima (una nueva aplicación o el mantenimiento de un desarrollador). En otros casos, la alerta detecta una acción malintencionada (un antiguo empleado o un atacante externo).|
|**Acceso desde una aplicación potencialmente dañina**|Se ha utilizado una aplicación potencialmente dañina para tener acceso a la base de datos. En algunos casos, la alerta detecta la realización de pruebas de seguridad. En otros casos, la alerta detecta un ataque que usa herramientas comunes.|
|**Credenciales de SQL por fuerza bruta**|Se produjo un número anormalmente elevado de inicios de sesión con error con distintas credenciales. En algunos casos, la alerta detecta la realización de pruebas de seguridad. En otros casos, la alerta detecta ataques por fuerza bruta.|

Para más información sobre las alertas de detección de amenazas de SQL, consulte [Detección de amenazas de Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview). En concreto, revise la sección sobre alertas de detección de amenazas. Consulte también [cómo Azure Security Center ayuda a desvelar un ciberataque](https://azure.microsoft.com/blog/how-azure-security-center-helps-reveal-a-cyberattack/) para ver un ejemplo de cómo Security Center usa la detección de actividad SQL malintencionada para detectar un ataque.

## Storage <a name="azure-storage"></a>

>[!NOTE]
> Advanced Threat Protection para Storage está actualmente disponible solo para Blob Storage.

Advanced Threat Protection para Storage proporciona un nivel adicional de inteligencia de seguridad que detecta intentos poco habituales y potencialmente peligrosos de acceder a las cuentas de almacenamiento o vulnerarlas. Esta capa de protección le permite afrontar las amenazas sin necesidad de ser un experto en seguridad ni administrar sistemas de supervisión de la seguridad.

Security Center analiza los registros de diagnóstico de solicitudes de lectura, escritura y eliminación en Blob Storage para detectar amenazas, y desencadena alertas cuando se producen anomalías en la actividad. Para más información, consulte el artículo sobre cómo [configurar el registro de Storage Analytics](https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account#configure-logging).

> [!div class="mx-tableFixed"]

|Alerta|DESCRIPCIÓN|
|---|---|
|**Anomalía de acceso desde ubicaciones inusuales**|El análisis de las muestras de tráfico ha detectado una comunicación del Protocolo de escritorio remoto (RDP) saliente anómala con origen en un recurso de su implementación. Esta actividad se considera anómala para este entorno. Esto puede indicar que el recurso está en peligro y ahora se usa para realizar un ataque por fuerza bruta en un punto de conexión RDP externo. Tenga en cuenta que este tipo de actividad puede dar lugar a que entidades externas marquen su dirección IP como malintencionada.|
|**Anomalía de acceso de aplicaciones**|Indica que una aplicación inusual ha accedido a esta cuenta de almacenamiento. Una posible causa es que un atacante ha accedido a la cuenta de almacenamiento mediante el uso de una aplicación nueva.|
|**Anomalía de acceso anónimo**|Indica que se ha producido un cambio en el patrón de acceso a una cuenta de almacenamiento. Por ejemplo, se ha accedido de forma anónima a la cuenta (sin autenticación), algo que resulta inesperado si se tiene en cuenta el patrón de acceso reciente en la cuenta. Una causa posible es que un atacante se haya aprovechado del acceso de lectura público a un contenedor que incluye almacenamiento en blobs.|
|**Anomalía de Tor**|Indica que se accedió a esta cuenta correctamente desde una dirección IP conocida como nodo de salida activo de Tor (proxy anónimo). La gravedad de esta alerta considera el tipo de autenticación usado (si existe) y si es la primera vez que se produce tal acceso. Las causas posibles pueden ser un atacante que haya tenido acceso a su cuenta de almacenamiento mediante Tor o un usuario legítimo que haya tenido acceso a su cuenta de almacenamiento mediante Tor.|
|**Anomalía de filtración de datos**|Indica que se ha extraído una cantidad de datos inusualmente grande en comparación con la actividad reciente en este contenedor de almacenamiento. Una causa posible es que un atacante haya extraído una gran cantidad de datos de un contenedor que incluye almacenamiento en blobs.|
|**Anomalía de eliminación inesperada**|Indica que se han producido una o varias operaciones de eliminación inesperadas en una cuenta de almacenamiento, en comparación con la actividad reciente en la cuenta. Una posible causa es que un atacante haya eliminado datos de la cuenta de almacenamiento.|
|**Carga de paquetes de Azure Cloud Services**|Indica que se ha cargado un paquete de Azure Cloud Services (archivo .cspkg) en una cuenta de almacenamiento de manera inusual, en comparación con la actividad reciente en esta cuenta. Una posible causa es que un atacante ha estado preparándose para implementar código malintencionado desde la cuenta de almacenamiento en un servicio en la nube de Azure.|
|**Anomalía de permisos de acceso**|Indica que los permisos de acceso a un contenedor de almacenamiento se han cambiado de un modo inusual. Una causa posible es que un atacante ha cambiado los permisos del contenedor para debilitar su posición de seguridad o para ganar persistencia.|
|**Anomalía de acceso de inspección**|Indica que los permisos de acceso de una cuenta de almacenamiento se han inspeccionado de un modo no habitual, en comparación con la actividad reciente de esta cuenta. Una posible causa es que un atacante ha realizado un reconocimiento para un ataque futuro.|
|**Anomalía de exploración de datos**|Indica que los blobs o los contenedores de una cuenta de almacenamiento se han enumerado de un modo anómalo, en comparación con la actividad reciente de esta cuenta. Una posible causa es que un atacante ha realizado un reconocimiento para un ataque futuro.|

>[!NOTE]
>Advanced Threat Protection para Storage no está disponible actualmente en Azure Government ni en regiones de nube soberana.

Para más información sobre las alertas de almacenamiento, [Advanced Threat Protection para Azure Storage](../storage/common/storage-advanced-threat-protection.md). En concreto, revise la sección sobre "Alertas de protección".

## Azure Cosmos DB<a name="cosmos-db"></a>

Las siguientes alertas las generan los intentos inusuales y potencialmente dañinos de acceso o aprovechamiento de cuentas de Azure Cosmos DB:

|Alerta|DESCRIPCIÓN|
|---|---|
|**Acceso desde una ubicación inusual**|Indica que se ha producido un cambio en el patrón de acceso a una cuenta de Azure Cosmos DB. Alguien ha tenido acceso a esta cuenta desde una dirección IP desconocida, en comparación con la actividad reciente. Un atacante ha tenido acceso a la cuenta, o bien un usuario legítimo ha accedido a ella desde una ubicación geográfica nueva e inusual. Un ejemplo de esto último es el mantenimiento remoto desde una nueva aplicación o desarrollador.|
|**Filtración de datos inusual**|Indica que se ha producido un cambio en el patrón de extracción de datos desde una cuenta de Azure Cosmos DB. Alguien ha extraído una cantidad de datos inusual en comparación con la actividad reciente. Un atacante podría haber extraído una gran cantidad de datos de una base de datos de Azure Cosmos DB (por ejemplo, la filtración de datos o una transferencia de datos no autorizada). O bien, una aplicación o un usuario legítimos podrían haber extraído una cantidad de datos inusual de un contenedor (por ejemplo, para la actividad de copia de seguridad de mantenimiento).|

Para más información, consulte [Advanced Threat Protection for Azure Cosmos DB](../cosmos-db/cosmos-db-advanced-threat-protection.md) (Protección contra amenazas avanzada para Azure Cosmos DB).
