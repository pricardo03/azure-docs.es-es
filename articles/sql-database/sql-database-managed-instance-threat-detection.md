---
title: 'Detección de amenazas: Instancia administrada de Azure SQL Database| Microsoft Docs'
description: Detección de amenazas detecta actividades anómalas en la base de datos que indican posibles amenazas de seguridad para la base de datos en una Instancia administrada.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: rmatchoro
ms.author: ronmat
ms.reviewer: vanto
manager: craigg
ms.date: 12/06/2018
ms.openlocfilehash: a456a214143f39ed9504af40129f9199b2535e46
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2018
ms.locfileid: "52997137"
---
# <a name="azure-sql-database-managed-instance-threat-detection-preview"></a>Detección de amenazas de Instancia administrada de Azure SQL Database (versión preliminar)

[Detección de amenazas](sql-database-threat-detection-overview.md) de Azure SQL para [Instancia administrada de SQL Database](sql-database-managed-instance-index.yml) detecta actividades anómalas que indican intentos inusuales y potencialmente peligrosos de acceder a las bases de datos o de vulnerar su seguridad. La detección de amenazas puede identificar **posible inyección SQL**, **acceso desde un centro de datos o una ubicación inusuales**, **acceso desde una aplicación potencialmente peligrosa o entidad de seguridad desconocida** y **credenciales SQL por fuerza bruta**; vea más detalles en [Alertas de detección de amenazas](sql-database-threat-detection-overview.md#azure-sql-database-threat-detection-alerts).

Puede recibir notificaciones sobre las amenazas detectadas mediante [notificaciones por correo electrónico](sql-database-threat-detection-overview.md#explore-anomalous-database-activities-upon-detection-of-a-suspicious-event) o en [Azure Portal](sql-database-threat-detection-overview.md#explore-threat-detection-alerts-for-your-database-in-the-azure-portal).

La [detección de amenazas](sql-database-threat-detection-overview.md) forma parte de la oferta de [protección contra amenazas avanzada de SQL](sql-advanced-threat-protection.md) (ATP), que es un paquete unificado para funcionalidades avanzadas de seguridad de SQL. Puede acceder a la característica de detección de amenazas y administrarla a través del portal central de SQL ATP. El servicio de detección de amenazas se cobra a 15 USD al mes por instancia administrada, siendo los primeros 30 días gratis.

## <a name="set-up-threat-detection-for-your-managed-instance-in-the-azure-portal"></a>Configuración de la detección de amenazas para instancias administradas en Azure Portal
1. Inicie Azure Portal en [https://portal.azure.com](https://portal.azure.com).
2. Vaya a la página de configuración de la instancia administrada que desea proteger. En la página **Configuración**, seleccione **Detección de amenazas**. 
3. En la página de configuración Detección de amenazas 
   - **Active** Detección de amenazas.
   - Configure la **lista de direcciones de correo electrónico** que recibirán alertas de seguridad cuando se detecten actividades anómalas en las bases de datos.
   - Seleccione la **cuenta de Azure Storage** donde se guardan los registros de anomalías de la auditoría de amenazas. 
4.  Haga clic en **Guardar** para guardar la directiva de detección de amenazas nueva o actualizada.

   ![detección de amenazas](./media/sql-database-managed-instance-threat-detection/threat-detection.png)

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre [Detección de amenazas](sql-database-threat-detection-overview.md) 
- Para más información acerca de Instancia administrada, vea [¿Qué es Instancia administrada de SQL Database (versión preliminar)?](sql-database-managed-instance.md) 
- Más información sobre la [detección de amenazas en base de datos única](sql-database-threat-detection.md). 
- Más información sobre la [auditoría de Instancia administrada](https://go.microsoft.com/fwlink/?linkid=869430) 
- Más información sobre [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)
