---
title: Almacenamiento, administración y conservación de datos de aplicaciones en la nube con Visual Studio App Center y servicios de Azure
description: Conozca los servicios de App Center, que permiten almacenar, administrar y conservar datos de aplicaciones móviles en la nube.
author: elamalani
ms.assetid: 12344321-0123-4678-8588-ccff02097224
ms.service: vs-appcenter
ms.topic: article
ms.date: 10/22/2019
ms.author: emalani
ms.openlocfilehash: b7043c7e9bd944bfb3633397edfa3c35609bd8ec
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72795112"
---
# <a name="store-sync-and-query-mobile-application-data-from-the-cloud"></a>Almacenamiento, sincronización y consulta de datos de aplicaciones móviles desde la nube
Con independencia del tipo de aplicación que vaya a crear, es probable que genere y procese datos. Las expectativas de los usuarios son altas, quieren que la aplicación funcione de forma rápida y sin problemas, en todas las circunstancias. La mayoría de las aplicaciones funcionan en varios dispositivos, de modo que cuando se accede a la aplicación se puede hacer desde un dispositivo móvil o de escritorio. Es posible que varios usuarios usen la aplicación al mismo tiempo y que compartan datos con la expectativa de obtener acceso instantáneo y en tiempo real a ellos.

Además, los usuarios de las aplicaciones no siempre tendrán conectividad a Internet. Las aplicaciones están diseñadas para que funcionen con o sin conexión a Internet y eso es lo que se espera de ellas. Con todas estas complejidades cada vez mayores, supone una tarea enorme para los desarrolladores elegir la solución adecuada para almacenar sus datos en la nube y sincronizarlos con esta con el fin de proporcionar al cliente una excelente experiencia con su aplicación.

Microsoft proporciona una gran variedad de servicios que eliminan la necesidad de poner en marcha los servidores, de elegir la base de datos o de preocuparse por el escalado o la seguridad para ofrecer la mejor experiencia posible. Estos servicios ofrecen una gran experiencia de desarrollo que permite almacenar los datos de las aplicaciones en la nube mediante las API de SQL o NoSQL y sincronizar los datos de todos los dispositivos. También permiten que la aplicación funcione con o sin conexión de red de cara a crear aplicaciones escalables y robustas.

Use los servicios siguientes para administrar y almacenar datos de aplicaciones móviles en la nube.

## <a name="visual-studio-app-center"></a>Visual Studio App Center
[App Center Data](/appcenter/data/) es un servicio de administración de datos que permite a las aplicaciones administrar, conservar y sincronizar los datos en la nube entre distintos dispositivos y plataformas, tanto en escenarios en línea como sin conexión. **Basado en Cosmos DB**, el servicio que se escala a medida que crece la base de usuarios y el número de aplicaciones, este servicio garantiza una baja latencia, una alta disponibilidad y una gran escalabilidad para todos los datos.

**Características principales**
- **Aprovisione fácilmente una nueva base de datos de Cosmos DB** o **conéctese a una base de datos de Cosmos DB existente** desde el portal de App Center.
- **Compatibilidad con bases de datos NoSQL** para almacenar, sincronizar y consultar los datos de las aplicaciones fácilmente.
- **Basado en Cosmos DB**, este servicio hereda la mayoría de las características clave que ofrece Azure Cosmos DB y puede **escalarse globalmente** para satisfacer sus necesidades empresariales.
- Funcionalidades de **sincronización en línea y sin conexión** para sincronizar datos entre dispositivos.
- **SDK de cliente** para dispositivos móviles que permiten administrar fácilmente los datos privados de las aplicaciones.
- **Compatibilidad con plataformas**: iOS, Android, Xamarin y React Native.

**Referencias**
- [Registro en App Center](https://appcenter.ms/signup?utm_source=Mobile%20Development%20Docs&utm_medium=Azure&utm_campaign=New%20azure%20docs)
- [Introducción a App Center Data](/appcenter/data/getting-started)

## <a name="azure-cosmos-db"></a>Azure Cosmos DB
[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) es un servicio de base de datos de varios modelos y distribuido globalmente de Microsoft que ayuda a crear aplicaciones a escala mundial. Con tan solo un clic, Cosmos DB permite escalar de forma elástica e individual el rendimiento y el almacenamiento en cualquier número de regiones de Azure a nivel mundial. Puede escalar de forma elástica el rendimiento y el almacenamiento, y sacar provecho del rápido acceso a los datos (menos de 10 milisegundos) mediante las superficies de API favoritas, entre las que se incluyen SQL, MongoDB, Cassandra, Tables o Gremlin. Cosmos DB proporciona de forma exclusiva completos Acuerdos de Nivel de Servicio (SLA) para el rendimiento, la latencia, la disponibilidad y la coherencia.

**Características principales**
- Admite un **amplio abanico de API**, como SQL (Core) API, Cassandra API, API de MongoDB, Gremlin API y Table API.
- La **distribución global inmediata** replica los datos allá donde estén sus usuarios, con el fin de que estos puedan interactuar con la réplica de los datos más cercana a ellos.
- **Sin administración de esquemas o índices** dado que el motor de base de datos es completamente independiente del esquema.
- **Presencia en todas las regiones** puesto que Cosmos DB está disponible en todas las regiones de Azure del mundo, incluidas las más de 54 regiones de la nube pública.
- **Varias opciones de coherencia definidas con precisión** puesto que el protocolo de replicación de varios maestros de Cosmos DB se ha diseñado cuidadosamente para ofrecer cinco opciones de coherencia bien definidas: fuerte, obsolescencia limitada, sesión, prefijo coherente y eventual.
- **Disponibilidad del 99,999 %** tanto en lectura como en escritura.
- **Invocación mediante programación (o a través del portal) la conmutación por error regional** de la cuenta de Cosmos para tener la seguridad de que la aplicación está diseñada para resistir un desastre regional.
- **Baja latencia garantizada en el percentil 99**, en todo el mundo.

**Referencias**
- [Azure Portal](https://portal.azure.com) 
- [Documentación](/azure/cosmos-db/introduction)   

## <a name="azure-sql-database"></a>Azure SQL Database
 [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) es un servicio administrado de base de datos relacional de uso general que le permite crear una capa de almacenamiento de datos de gran rendimiento y disponibilidad para las aplicaciones y las soluciones en la nube de Microsoft Azure.

**Características principales**
- **Herramientas y modelos de bases de datos elásticas**: una base de datos elástica ofrece a los desarrolladores la posibilidad de agrupar los recursos que se van a usar entre un grupo de bases de datos con el fin de escalarlos. Estos recursos se pueden gestionar administrativamente mediante el envío de un script como un trabajo, y SQL Database ejecuta el script en todas las bases de datos.
- **Alto rendimiento**: las aplicaciones de alto rendimiento pueden aprovechar la versión más reciente, que proporciona un 25 % más de potencia de la base de datos premium.
- **Copias de seguridad, replicación y alta disponibilidad**: la replicación integrada y un Acuerdo de Nivel de Servicio respaldado por Microsoft en el nivel de base de datos permiten la continuidad y la protección de las aplicaciones frente a eventos catastróficos. La replicación geográfica activa le permite configurar la conmutación por error y el autoservicio de restauración, lo que proporciona un control total sobre la recuperación de errores (restauración de datos desde copias de seguridad de datos disponibles de hasta 35 días).
- **Mantenimiento prácticamente inexistente**: el software automático es parte del servicio, y las réplicas del sistema integradas permiten protección de datos inherente, tiempo de actividad de la base de datos y estabilidad del sistema. Las réplicas del sistema se trasladan automáticamente a equipos nuevos que se aprovisionan sobre la marcha cuando se producen errores en los equipos antiguos.
- **Seguridad**: SQL Database ofrece un conjunto de características de seguridad que le ayudan a cumplir las directivas de cumplimiento impuestas por la organización o el sector.
    - La auditoría proporciona a los desarrolladores la posibilidad de realizar tareas relacionadas con el cumplimiento y obtener información sobre las actividades.
    - Los desarrolladores y los responsables de TI también pueden implementar directivas en el nivel de base de datos para limitar el acceso a datos confidenciales con seguridad de nivel de fila, enmascaramiento de datos dinámico y cifrado de datos transparente en Azure SQL Database.
    - SQL Database está verificado por importantes auditores de la nube como parte del proceso de certificaciones de conformidad y aprobaciones clave de Azure, como el acuerdo de asociación comercial de HIPAA, ISO/IEC 27001:2005, FedRAMP y las cláusulas según el modelo de la UE.

**Referencias**
- [Azure Portal](https://portal.azure.com) 
- [Documentación](/azure/sql-database/)
   