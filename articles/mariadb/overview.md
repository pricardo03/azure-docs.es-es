---
title: Introducción al servicio de base de datos relacional Azure Database for MariaDB
description: Información general del servicio de base de datos relacional de Azure Database for MariaDB
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: overview
ms.date: 09/24/2018
ms.custom: mvc
ms.openlocfilehash: 3649a173d5707179ca8547a8169b7d308c4f7f1c
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2018
ms.locfileid: "48249169"
---
# <a name="what-is-azure-database-for-mariadb"></a>¿Qué es Azure Database for MariaDB?
Azure Database for MariaDB es un servicio de base de datos relacional de Microsoft Cloud basado en el motor de base de datos de [MariaDB Community Edition](https://mariadb.org/download/). Este servicio está en versión preliminar pública. Azure Database for MariaDB ofrece lo siguiente:

- Alta disponibilidad integrada sin coste adicional.
- Rendimiento predecible, con precios de pago por uso inclusivos.
- Escalado inmediato según las necesidades.
- Protección de información confidencial en reposo y en movimiento.
- Copias de seguridad automáticas y restauración a un momento dado durante un máximo de 35 días.
- Seguridad y cumplimiento de nivel empresarial.

Estas funcionalidades no requieren casi ninguna tarea de administración y todas se proporcionan sin ningún costo adicional. Le permiten centrarse en el desarrollo rápido de aplicaciones y en reducir el plazo de acceso al mercado, en lugar de tener que dedicar tiempo y recursos a la administración tanto de máquinas virtuales como de infraestructura. Además, puede seguir desarrollando su aplicación con las herramientas y la plataforma de código abierto de su elección, y entregar con la velocidad y eficacia que exige su negocio sin tener que aprender nuevas habilidades.

En este artículo se presentan los principales conceptos y características de Azure Database for MariaDB relacionados con el rendimiento, la escalabilidad y la manejabilidad, con vínculos para explorar los detalles. Consulte estas guías de inicio rápido para comenzar:
- [Creación de un servidor de Azure Database for MariaDB mediante Azure Portal](quickstart-create-mariadb-server-database-using-azure-portal.md)
- [Creación de un servidor de Azure Database for MariaDB mediante la CLI de Azure](quickstart-create-mariadb-server-database-using-azure-cli.md)

<!--
For a set of Azure CLI samples, see:
- [Azure CLI samples for Azure Database for MariaDB](sample-scripts-azure-cli.md) 
-->

## <a name="adjust-performance-and-scale-within-seconds"></a>Ajustar el rendimiento y la escala en cuestión de segundos
En la versión preliminar, el servicio Azure Database for MariaDB ofrece varios niveles de servicio: Básico, Uso general y Optimiza para memoria. Cada nivel ofrece diferentes niveles de rendimiento y funcionalidades para admitir cargas de trabajo de bases de datos, de ligeras a pesadas. Puede compilar su primera aplicación en una base de datos pequeña por poco dinero al mes y, después, ajustar la escala para satisfacer las necesidades de la solución. La escalabilidad dinámica permite a la base de datos responder de manera transparente a los cambiantes requisitos de recursos. Solo paga por los recursos que necesite y cuando los necesite. Vea los [Planes de tarifa](concepts-pricing-tiers.md) para obtener más información.

## <a name="monitoring-and-alerting"></a>Supervisión y alertas
¿Cómo se decide cuándo adaptar la velocidad? Utilice las características de supervisión y alertas de rendimiento integradas, combinadas con la clasificación del rendimiento basado en los núcleos virtuales. Con estas herramientas, puede evaluar rápidamente el impacto de ajustar la escala de los núcleos virtuales en función de sus necesidades de rendimiento actuales o futuras. <!--See [Alerts](howto-alert-on-metric.md) for details.-->

## <a name="keep-your-app-and-business-running"></a>Mantenimiento de la aplicación y el negocio en funcionamiento
El Acuerdo de Nivel de Servicio (SLA) de Azure, con una disponibilidad del 99,99 % líder del sector y con la tecnología de una red global de centros de datos administrados por Microsoft (no se ofrece durante la versión preliminar pública), ayuda a mantener las aplicaciones en funcionamiento de forma ininterrumpida. Con cada servidor de Azure Database for MariaDB, se saca provecho de las ventajas de la seguridad integrada, la tolerancia a errores y la protección de datos, algo que de lo contrario tendría que adquirir o diseñar, compilar y administrar. Con Azure Database for MariaDB puede usar la restauración a un momento dado para recuperar un servidor a un estado anterior, con un plazo máximo de 35 días.

## <a name="secure-your-data"></a>Protección de los datos
La tradicional seguridad de los datos de los servicios de bases de datos de Azure se mantiene en Azure Database for MariaDB con características que limitan el acceso, protegen los datos en reposo y en movimiento, y lo ayudan a supervisar la actividad. Para obtener más información sobre la seguridad de plataforma de Azure, vaya al [Centro de confianza de Azure](https://www.microsoft.com/en-us/trustcenter/security).

El servicio Azure Database for MariaDB usa el cifrado de almacenamiento de los datos en reposo. Los datos, incluidas las copias de seguridad, se cifran en el disco (a excepción de los archivos temporales creados por el motor durante la ejecución de consultas). El servicio usa el cifrado AES de 256 bits que se incluye en el cifrado de almacenamiento de Azure y las claves están administradas por el sistema. El cifrado de almacenamiento siempre está activado y no se puede deshabilitar.

De forma predeterminada, el servicio Azure Database for MariaDB está configurado para exigir la [seguridad de la conexión SSL](./concepts-ssl-connection-security.md) para los datos en movimiento a través de la red. La exigencia de conexiones SSL entre el servidor de base de datos y las aplicaciones cliente ayuda a proteger contra los ataques de tipo "man in the middle" dado que los datos se cifran entre el servidor y la aplicación. Tiene la opción de deshabilitar el requisito de SSL para conectar el servicio de base de datos en caso de que la aplicación cliente no admita la conectividad SSL.

## <a name="contacts"></a>Contactos
Para cualquier pregunta o sugerencia que pueda tener con respecto al uso de Azure Database for MariaDB, envíe un correo electrónico al equipo de Azure Database for MariaDB ([@Ask Azure DB for MariaDB](mailto:AskAzureDBforMariaDB@service.microsoft.com)). Tenga en cuenta que esto no es un alias de soporte técnico.

Además, tenga en cuenta los siguientes puntos de contacto según corresponda:
- Para ponerse en contacto con el servicio de soporte técnico de Azure, [presente una incidencia de soporte técnico en Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Para corregir un problema con la cuenta, envíe una [solicitud de soporte técnico](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) en Azure Portal.
- Para proporcionar comentarios o solicitar nuevas características, cree una entrada mediante [UserVoice](https://feedback.azure.com/forums/915439-azure-database-for-mariadb).

## <a name="next-steps"></a>Pasos siguientes
Ahora que ha leído una introducción a Azure Database for MariaDB y ha contestado a la pregunta "¿Qué es Azure Database for MariaDB?", ya está listo para:
- Consulte la página de precios para ver comparaciones de costos y calculadoras. [Precios](https://azure.microsoft.com/pricing/details/mariadb/)
- Comience por crear su primer servidor. [Creación de un servidor de Azure Database for MariaDB mediante Azure Portal](quickstart-create-mariadb-server-database-using-azure-portal.md)

<!--- - Build your first app using your preferred language: [Python](./connect-python.md) | [Node.JS](./connect-nodejs.md) | [Java](./connect-java.md) | [Ruby](./connect-ruby.md) | [PHP](./connect-php.md) | [.NET (C#)](./connect-csharp.md) | [Go](./connect-go.md) --->
