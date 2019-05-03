---
title: 'Advanced Threat Protection: Azure SQL Database | Microsoft Docs'
description: Protección contra amenazas avanzada detecta actividades anómalas de la base de datos que indican posibles amenazas de seguridad en Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: monhaber
ms.author: ronmat
ms.reviewer: vanto, carlrab
manager: craigg
ms.date: 03/31/2019
ms.openlocfilehash: 710a94c919f4262c3f572f28d03c79b77e658287
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60614559"
---
# <a name="advanced-threat-protection-for-azure-sql-database"></a>Advanced Threat Protection en Azure SQL Database

Protección contra amenazas para avanzada [Azure SQL Database](sql-database-technical-overview.md) y [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) detecta actividades anómalas que indican intentos inusuales y potencialmente dañinos de acceso o ataque a las bases de datos.

Protección contra amenazas avanzada forma parte de la [avanzadas de seguridad de los datos](sql-database-advanced-data-security.md) (ADS) oferta, que es un paquete unificado para funcionalidades avanzadas de seguridad SQL. Protección contra amenazas avanzada se puede acceder y administrar a través del portal central de anuncios de SQL.

> [!NOTE]
> Este tema se aplica al servidor de Azure SQL y tanto a las bases de datos de SQL Database como a SQL Data Warehouse que se crean en el servidor de Azure SQL. Para simplificar, SQL Database se utiliza cuando se hace referencia tanto a SQL Database como a SQL Data Warehouse.

## <a name="what-is-advanced-threat-protection"></a>¿Qué es la protección contra amenazas avanzada

 Protección contra amenazas avanzada ofrece un nuevo nivel de seguridad, que permite a los clientes a detectar y responder a amenazas potenciales, cuando se producen proporcionando alertas de seguridad sobre actividades anómalas. Los usuarios reciben una alerta sobre actividades sospechosas en las bases de datos, posibles vulnerabilidades y ataques por inyección de código SQL, así como sobre los patrones de acceso y consultas a las bases de datos anómalos. Protección contra amenazas avanzada integra alertas con [Azure Security Center](https://azure.microsoft.com/services/security-center/), que incluyen los detalles de actividad sospechosa y recomiendan acciones sobre cómo investigar y mitigar la amenaza. Protección contra amenazas avanzada simplifica la dirección de las posibles amenazas a la base de datos sin necesidad de ser un experto en seguridad ni administrar sistemas de supervisión de seguridad avanzada.

Para obtener una experiencia de investigación, se recomienda habilitar la [auditoría de SQL Database](sql-database-auditing.md), que escribe los eventos de las bases de datos en un registro de auditoría en su cuenta de Azure Storage.  

## <a name="advanced-threat-protection-alerts"></a>Alertas de Advanced Threat Protection

Protección contra amenazas avanzada de Azure SQL Database detecta actividades anómalas que indican intentos inusuales y potencialmente dañinos de acceso o ataque a las bases de datos y puede desencadenar las siguientes alertas:

- **Vulnerabilidad a la inyección de código SQL**: esta alerta se desencadena cuando una aplicación genera una instrucción SQL errónea en la base de datos. Esta alerta puede indicar una posible vulnerabilidad a los ataques de inyección de código SQL. Hay dos posibles razones para la generación de una instrucción errónea:

  - Existe un defecto en el código de la aplicación que crea la instrucción SQL errónea
  - El código de la aplicación o los procedimientos almacenados no corrigen los datos que proporciona el usuario al construir la instrucción SQL errónea, lo que se puede aprovechar para ataques por inyección de código SQL
- **Potencial inyección de código SQL**: esta alerta se desencadena cuando se produce una vulnerabilidad de seguridad activa contra una vulnerabilidad de la aplicación identificada ante inyección de código SQL. Esto significa que el atacante intentan inyectar instrucciones SQL malintencionadas mediante el código de la aplicación vulnerable o procedimientos almacenados.
- **Acceso desde una ubicación inusual**: esta alerta se desencadena cuando se produce un cambio en el patrón de acceso a SQL Server, donde alguien ha iniciado sesión en el servidor SQL Server desde una ubicación geográfica inusual. En algunos casos, la alerta detecta una acción legítima (una nueva aplicación o el mantenimiento de un desarrollador). En otros casos, la alerta detecta una acción malintencionada (por ejemplo, un antiguo empleado o un atacante externo).
- **Acceso desde un centro de datos de Azure inusual**: esta alerta se desencadena cuando se produce un cambio en el patrón de acceso a SQL Server, donde alguien ha iniciado sesión en el servidor SQL Server desde un centro de datos de Azure inusual que se vio en el servidor recientemente. En algunos casos, la alerta detecta una acción legítima (una aplicación nueva en Azure, Power BI o Azure SQL Query Editor). En otros casos, la alerta detecta una acción malintencionada procedente de un recurso o servicio de Azure (por ejemplo, un antiguo empleado o un atacante externo).
- **Acceso desde una entidad de seguridad desconocida**: esta alerta se desencadena cuando se produce un cambio en el patrón de acceso a SQL Server, donde alguien ha iniciado sesión en el servidor SQL Server mediante una entidad de seguridad inusual (usuario de SQL). En algunos casos, la alerta detecta una acción legítima (una nueva aplicación o el mantenimiento de un desarrollador). En otros casos, la alerta detecta una acción malintencionada (por ejemplo, un antiguo empleado o un atacante externo).
- **Acceso desde una aplicación potencialmente dañina**: esta alerta se desencadena cuando una aplicación potencialmente dañina se utiliza para tener acceso a la base de datos. En algunos casos, la alerta detecta la realización de pruebas de seguridad. En otros casos, la alerta detecta un ataque que se realiza con herramientas de ataque comunes.
- **Credenciales de SQL por fuerza bruta**: esta alerta se desencadena cuando hay un número anormalmente elevado de inicios de sesión infructuosos con distintas credenciales. En algunos casos, la alerta detecta la realización de pruebas de seguridad. En otros casos, la alerta detecta ataques por fuerza bruta.

## <a name="explore-anomalous-database-activities-upon-detection-of-a-suspicious-event"></a>Exploración de las actividades anómalas en la base de datos cuando se detecta un evento sospechoso

Cuando se detecten actividades anómalas en las bases de datos, recibirá una notificación por correo electrónico. El correo electrónico proporciona información sobre el evento de seguridad sospechoso, en la que se incluyen la naturaleza de las actividades anómalas, el nombre de la base de datos, el nombre del servidor, el nombre de la aplicación y la hora del evento. Además, el correo electrónico proporciona información sobre las posibles causas y las medidas recomendadas para investigar y mitigar la amenaza potencial para la base de datos.

![Informes de actividades anómalas](./media/sql-database-threat-detection/anomalous_activity_report.png)

1. Haga clic en el vínculo **View recent SQL alerts** (Ver las alertas recientes de SQL) del correo electrónico para iniciar Azure Portal y mostrar la página de alertas de Azure Security Center, que proporciona información general sobre amenazas activas detectadas en la base de datos de SQL.

   ![Amenazas de actividad](./media/sql-database-threat-detection/active_threats.png)

2. Haga clic en una alerta específica para obtener detalles y acciones adicionales para investigar esta amenaza y solucionar amenazas futuras.

   Por ejemplo, la inyección de código SQL es uno de los problemas de seguridad habituales entre las aplicaciones web en Internet y se usa para atacar aplicaciones controladas por datos. Los atacantes aprovechan las vulnerabilidades de la aplicación para inyectar instrucciones SQL malintencionadas en los campos de entrada de la aplicación, con el fin de infringir la seguridad o modificar datos en la base de datos. Para las alertas de inyección de código SQL, los detalles de la alerta incluyen la instrucción SQL vulnerable que se ha aprovechado.

   ![Alerta específica](./media/sql-database-threat-detection/specific_alert.png)

## <a name="explore-advanced-threat-protection-alerts-for-your-database-in-the-azure-portal"></a>Explore las alertas de protección contra amenazas avanzada de la base de datos en Azure portal

Protección contra amenazas avanzada integra las alertas con [de Azure security center](https://azure.microsoft.com/services/security-center/). Los iconos dinámicos de protección contra amenazas avanzada de SQL dentro de la base de datos y hojas de anuncios de SQL en el portal de Azure un seguimiento del estado de las amenazas activas.

Haga clic en **alerta de protección contra amenazas avanzada** para iniciar el centro de seguridad de Azure página alertas y obtenga una introducción a las amenazas SQL activas detectadas en el almacén de datos o base de datos.

   ![Alerta de protección contra amenazas avanzada](./media/sql-database-threat-detection/threat_detection_alert.png)

   ![Advanced Threat Protection real2](./media/sql-database-threat-detection/threat_detection_alert_atp.png)

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre [avanzada de protección contra amenazas en bases de datos únicas y agrupados](sql-database-threat-detection.md).
- Obtenga más información sobre [protección de amenazas avanzada en instancia administrada](sql-database-managed-instance-threat-detection.md).
- Obtenga más información sobre [avanzadas de seguridad de los datos](sql-database-advanced-data-security.md).
- Más información sobre las [auditorías de Azure SQL Database](sql-database-auditing.md)
- Más información sobre [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)
- Para más información sobre los precios, consulte la [página Precios de SQL Database](https://azure.microsoft.com/pricing/details/sql-database/)  
