---
title: Introducción al servicio de base de datos relacional Azure Database for PostgreSQL
description: Proporciona información general sobre el servicio de base de datos relacional de Azure Database for MySQL.
author: jonels-msft
ms.author: jonels
ms.custom: mvc
ms.service: postgresql
ms.topic: overview
ms.date: 05/06/2019
ms.openlocfilehash: f4023fa84215a0319669de0d812d8306b62278e3
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "65073281"
---
# <a name="what-is-azure-database-for-postgresql"></a>¿Qué es Azure Database for PostgreSQL?
Azure Database for PostgreSQL es un servicio de base de datos relacional en la nube de Microsoft diseñado para desarrolladores. Se basa en la versión de comunidad del motor de base de datos [PostgreSQL](https://www.postgresql.org/) de código abierto y está disponible en dos opciones de implementación: Un solo servidor y Citus (versión preliminar).

## <a name="azure-database-for-postgresql---single-server"></a>Azure Database for PostgreSQL: Un solo servidor
La opción de implementación Un solo servidor ofrece:

- Alta disponibilidad integrada sin coste adicional (99,99 % en contrato de nivel de servicio)
- Rendimiento predecible, con precios de pago por uso inclusivos.
- Escalado inmediato, cuando sea necesario, en cuestión de segundos
- Supervisión y alertas para evaluar rápidamente el impacto del escalado
- Protección de información confidencial en reposo y en movimiento.
- Copias de seguridad automáticas y restauración a un momento dado durante un máximo de 35 días.
- Seguridad y cumplimiento de nivel empresarial.

Todas estas funcionalidades apenas requieren tareas de administración y todas se proporcionan sin costo adicional. Le permiten centrarse en el desarrollo rápido de aplicaciones y en reducir el plazo de acceso al mercado, en lugar de tener que dedicar tiempo y recursos a la administración tanto de las máquinas virtuales como de la infraestructura. Puede seguir desarrollando la aplicación con las herramientas de código abierto y en la plataforma de que prefiera sin necesidad de adquirir nuevos conocimientos.

La opción de implementación Un solo servidor ofrece tres planes de tarifa: Básico, De uso general y Optimizado para memoria. Cada plan ofrece capacidades de recursos diferente para admitir sus cargas de trabajo de la base de datos. Puede compilar su primera aplicación en una base de datos pequeña por poco dinero al mes y, después, ajustar la escala para satisfacer las necesidades de la solución. La escalabilidad dinámica permite a la base de datos responder de manera transparente a los cambiantes requisitos de recursos. Solo paga por los recursos que necesite y cuando los necesite. Para obtener más información, consulte los  [planes de tarifa](concepts-pricing-tiers.md).

## <a name="azure-database-for-postgresql---hyperscale-citus-preview"></a>Azure Database for PostgreSQL: Citus (versión preliminar)
La opción Citus escala horizontalmente las consultas entre varias máquinas mediante particionamiento. Su motor de consultas paraleliza las consultas SQL entrantes en estos servidores para agilizar las respuestas en conjuntos de datos grandes. Sirve aplicaciones que requieren mayor escala y mejor rendimiento, por lo general cargas de trabajo que se aproximan a los 100 GB de datos (o que ya los superan).

La opción de implementación Citus ofrece:

- Escalado horizontal entre varias máquinas mediante particionamiento
- Paralelización de consultas entre estos servidores, lo que agiliza las respuestas en conjuntos de datos grandes
- Una excelente compatibilidad con aplicaciones de varios inquilinos, análisis operativos en tiempo real y cargas de trabajo transaccionales de alto rendimiento

Las aplicaciones compiladas para PostgreSQL puede ejecutar consultas distribuidas en Citus con las [bibliotecas de conexiones](./concepts-connection-libraries.md) estándar y unos cambios mínimos.

Tenga en cuenta que Citus está en versión preliminar pública y, por tanto, aún no ofrece un contrato de nivel de servicio.

## <a name="data-security"></a>Seguridad de los datos
Azure Database for PostgreSQL mantiene la tradicional seguridad de datos de los servicios de base de datos de Azure. Tiene características que limitan el acceso, protegen los datos en reposo y en movimiento, y le ayudan a supervisar la actividad. Para obtener más información sobre la seguridad de plataforma de Azure, vaya al [Centro de confianza de Azure](https://azure.microsoft.com/overview/trusted-cloud/).

El servicio Azure Database for PostgreSQL usa el cifrado de almacenamiento de los datos en reposo y cumple la norma FIPS 140-2. Los datos, incluidas las copias de seguridad, se cifran en el disco. El servicio usa el cifrado AES de 256 bits que se incluye en el cifrado de almacenamiento de Azure y las claves las administra el sistema. El cifrado de almacenamiento siempre está activado y no se puede deshabilitar. De forma predeterminada, el servicio Azure Database for PostgreSQL requiere conexiones seguras para los datos en movimiento tanto a través de la red como entre la aplicación cliente y la base de datos.

## <a name="contacts"></a>Contactos
Para cualquier pregunta o sugerencia acerca del uso de Azure Database for PostgreSQL, envíe un correo electrónico al equipo de Azure Database for PostgreSQL ([@Ask Azure DB for PostgreSQL](mailto:AskAzureDBforPostgreSQL@service.microsoft.com)). Esta dirección es para preguntas generales, no para incidencias de soporte técnico.

Además, tenga en cuenta estos puntos de contacto según corresponda:
- Para ponerse en contacto con el servicio de soporte técnico de Azure o solucionar cualquier problema de su cuenta, [presente una incidencia desde Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Para proporcionar comentarios o solicitar nuevas características, cree una entrada mediante [UserVoice](https://feedback.azure.com/forums/597976-azure-database-for-postgresql).

## <a name="next-steps"></a>Pasos siguientes
- Consulte la [página de precios](https://azure.microsoft.com/pricing/details/postgresql/) para ver comparaciones de costos y calculadoras.
- Comience por crear su primera base de datos de Azure Database for PostgreSQL [Un solo servidor](./quickstart-create-server-database-portal.md) o [Citus (versión preliminar)](./quickstart-create-hyperscale-portal.md)
- Compile la primera aplicación en Python, PHP, Ruby, C\#, Java, Node.js: [Bibliotecas de conexiones](./concepts-connection-libraries.md)
