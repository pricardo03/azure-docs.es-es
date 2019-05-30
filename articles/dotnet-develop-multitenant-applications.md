---
title: Patrón de aplicación web multiinquilino | Microsoft Docs
description: Encuentre información general de arquitectura y patrones de diseño que describan cómo implementar una aplicación web multiempresa en Azure.
services: ''
documentationcenter: .net
author: wadepickett
manager: wpickett
editor: ''
ms.assetid: 4f0281d2-1555-42b0-a99d-1222fade0b0f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/05/2015
ms.author: wpickett
ms.openlocfilehash: 92a0caedca34756228dbf57ec9099fd2ece3d84e
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66225983"
---
# <a name="multitenant-applications-in-azure"></a>Aplicaciones multiempresa en Azure
Una aplicación multiempresa es un recurso compartido que permite a usuarios independientes, o "inquilinos", ver la aplicación como si fuera propia. Un escenario típico de una aplicación multiempresa es aquel en que todos los usuarios de la aplicación pueden querer personalizar la experiencia de usuario pero, por otra parte, tienen los mismos requisitos empresariales básicos. Office 365, Outlook.com y visualstudio.com son ejemplos de grandes aplicaciones multiempresa.

Desde la perspectiva del proveedor de una aplicación, los beneficios de la arquitectura multiempresa se relacionan principalmente con la eficacia operativa y de costes. Una versión de la aplicación puede satisfacer las necesidades de muchos inquilinos/clientes, de manera que se permita la consolidación de las tareas de administración del sistema, como la supervisión, el ajuste del rendimiento, el mantenimiento del software y las copias de seguridad de los datos.

A continuación se ofrece una lista de los objetivos y los requisitos más importantes desde la perspectiva de un proveedor.

* **Aprovisionamiento**: Debe ser capaz de aprovisionar a nuevos inquilinos para la aplicación.  En aplicaciones multiempresa con un gran número de inquilinos, suele ser necesario automatizar este proceso mediante la habilitación del aprovisionamiento de autoservicio.
* **Mantenimiento**: Debe ser capaz de actualizar la aplicación y realizar otras tareas de mantenimiento mientras varios inquilinos la utilizan.
* **Supervisión**: Debe ser capaz de supervisar la aplicación en todo momento para identificar problemas y solucionarlos. Esto comprende supervisar el uso que cada inquilino hace de la aplicación.

Una aplicación multiempresa correctamente implementada ofrece los siguientes beneficios a los usuarios.

* **Aislamiento**: Las actividades de inquilinos individuales no afecta el uso de la aplicación de otros inquilinos. Los inquilinos no pueden tener acceso a los datos de los demás. Ante el inquilino, parecerán tener uso exclusivo de la aplicación.
* **Disponibilidad**: Los inquilinos individuales desean que la aplicación esté constantemente disponible, quizás con garantías definidas en un SLA. De nuevo, las actividades de otros inquilinos no deben afectar a la disponibilidad de la aplicación.
* **Escalabilidad**: La aplicación se escala para satisfacer la demanda de los inquilinos individuales. La presencia y las acciones de otros inquilinos no deben afectar al rendimiento de la aplicación.
* **Costos**: Los costos son menores que la ejecución de una aplicación dedicada y un único inquilino porque la arquitectura multiempresa permite el uso compartido de recursos.
* **Personalización**. la capacidad de personalizar la aplicación para un inquilino individual de varias formas, como agregar o eliminar características, cambiar el color y los logotipos o incluso agregar su propio código o script.

En resumen, aunque hay muchas consideraciones que debe tener en cuenta para proporcionar un servicio altamente escalable, también hay una serie de objetivos y requisitos que son comunes a muchas aplicaciones multiempresa. Algunos pueden no resultar pertinentes en escenarios específicos y la importancia de los objetivos y requisitos individuales variará en cada escenario. Como proveedor de la aplicación para varios inquilinos, también tendrá objetivos y requisitos como satisfacer del inquilino objetivos y requisitos, rentabilidad, facturación, varios niveles de servicio, aprovisionamiento, mantenimiento de supervisión y automatización.

Para obtener más información acerca de consideraciones de diseño adicionales de una aplicación multiinquilino, consulte [Hospedaje de una aplicación multiinquilino en Azure][Hosting a Multi-Tenant Application on Azure]. Para más información sobre los patrones comunes de la arquitectura de datos de aplicaciones de base de datos de software como servicio (SaaS) multiinquilino, consulte [Modelos de diseño para las aplicaciones SaaS multiinquilino y Azure SQL Database](sql-database/sql-database-design-patterns-multi-tenancy-saas-applications.md). 

Azure ofrece muchas características que le permiten solucionar los principales problemas detectados al diseñar un sistema multiempresa.

**Aislamiento**

* Segmentación de inquilinos de sitios web mediante encabezados host con o sin comunicación de SSL
* Segmentar inquilinos de sitio web según parámetros de consulta
* Servicios web en roles de trabajo
  * Roles de trabajo que se suelen procesan datos en el back-end de una aplicación.
  * Roles web que suelen actuar como el front-end de las aplicaciones

**Storage**

Administración de datos como servicios de Azure SQL Database o Azure Storage como el servicio de tabla, que proporciona servicios de almacenamiento de grandes cantidades de datos no estructurados y Blob service, que proporciona servicios para almacenar grandes cantidades de texto no estructurado o datos binarios, como vídeo, audio e imágenes.

* Proteger los datos en SQL Database multiempresa inicios de sesión de SQL Server por inquilino.
* Usar tablas de Azure para recursos de la aplicación mediante la especificación de una directiva de acceso de nivel de contenedor, puede tener la capacidad de ajustar permisos sin tener que emitir la nueva URL para los recursos protegidos con firmas de acceso compartido.
* Las colas de Azure para recursos de la aplicación se suelen usar para dirigir el procesamiento en nombre de los inquilinos, pero también se pueden usar para distribuir el trabajo necesario para el aprovisionamiento o la administración.
* Colas de Service Bus para recursos de aplicaciones que insertan el trabajo en un servicio compartido, donde puede usar una única cola en la que cada remitente inquilino solo tiene permisos (según se deriva de las notificaciones emitidas desde ACS) para insertar en dicha cola, mientras que solo los receptores del servicio tienen permiso para extraer de la cola los datos procedentes de varios inquilinos.

**Servicios de conexión y seguridad**

* Azure Service Bus, una infraestructura de mensajería que se ubica entre las aplicaciones, de manera que les permite intercambiar mensajes sin conexión directa, a fin de ofrecer mayor escalabilidad y resistencia.

**Servicios de red**

Azure ofrece varios servicios de red que admiten la autenticación y mejoran la manejabilidad de las aplicaciones hospedadas. Estos servicios incluyen lo siguiente:

* Azure Virtual Network le permite aprovisionar y administrar redes privadas virtuales (VPN) en Azure, así como vincularlas de forma segura con la infraestructura de TI local.
* Traffic Manager de Virtual Network le permite cargar tráfico entrante equilibrado entre varios servicios de Azure hospedados, independientemente de que se ejecuten en el mismo centro de datos o en diferentes centros de datos en todo el mundo.
* Azure Active Directory (Azure AD) es un servicio moderno basado en REST que ofrece capacidades de administración de identidades y control de acceso para las aplicaciones en la nube. Uso de Azure AD para recursos de aplicaciones proporciona una manera fácil de autenticar y autorizar a los usuarios obtener acceso a los servicios y aplicaciones web al tiempo que permite a las características de autenticación y autorización queden excluidas del código.
* Azure Service Bus ofrece funcionalidades de flujo de datos y mensajería segura para aplicaciones distribuidas e híbridas, como la comunicación entre aplicaciones hospedadas de Azure y aplicaciones y servicios locales, sin requerir infraestructuras de seguridad y firewall complejas. Con Service Bus Relay para recursos de la aplicación para tener acceso a los servicios que se exponen como extremos puede pertenecer al inquilino (por ejemplo, hospedados fuera del sistema, como en el entorno local) o pueden ser servicios aprovisionados específicamente para el inquilino porque ( datos confidenciales específicos del inquilino viajan a través de ellos).

**Aprovisionamiento de recursos**

Azure proporciona varias maneras de aprovisionar a nuevos inquilinos para la aplicación. En aplicaciones multiempresa con un gran número de inquilinos, suele ser necesario automatizar este proceso mediante la habilitación del aprovisionamiento de autoservicio.

* Los roles de trabajo le permiten aprovisionar y desaprovisionar recursos por inquilino (como cuando un nuevo inquilino inicia sesión o la cancela), recopilar métricas para medir el uso y administrar la escalación conforme a un programa determinado o para responder al cruce de umbrales de indicadores de rendimiento claves. Este mismo rol se puede usar también para realizar actualizaciones de la solución.
* Los blobs de Azure se pueden usar para aprovisionar recursos de almacenamiento informáticos o preinicializados para nuevos inquilinos, a la vez que se ofrecen directivas de acceso de nivel de contenedor para proteger los paquetes de servicios informáticos, las imágenes VHD y otros recursos.
* Las opciones para aprovisionar los recursos de la SQL Database para un inquilino incluyen:
  
  * DDL en scripts o insertados como recursos en ensamblados.
  * Paquetes SQL Server 2008 R2 DAC implementados mediante programación
  * Copiando desde una base de datos de referencia principal.
  * Usar la importación y exportación de base de datos para aprovisionar bases de datos nuevas desde un archivo

<!--links-->

[Hosting a Multi-Tenant Application on Azure]: https://msdn.microsoft.com/library/hh534480.aspx
[Designing Multitenant Applications on Azure]: https://msdn.microsoft.com/library/windowsazure/hh689716
