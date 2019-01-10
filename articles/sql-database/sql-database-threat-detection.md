---
title: Detección de amenazas de Azure SQL Database| Microsoft Docs
description: Detección de amenazas detecta actividades anómalas en la base de datos que indican posibles amenazas de seguridad para la base de datos en una sola base de datos o grupo elástico.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: rmatchoro
ms.author: ronmat
ms.reviewer: vanto, carlrab
manager: craigg
ms.date: 01/03/2019
ms.openlocfilehash: 3f4a120e2aaf2925805bec26f721d5cfb4194bf1
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "54041550"
---
# <a name="azure-sql-database-threat-detection-for-single-database"></a>Detección de amenazas de Azure SQL Database para una base de datos única

La [detección de amenazas](sql-database-threat-detection-overview.md) de Azure SQL para bases de datos únicas de [SQL Database](sql-database-technical-overview.md) detecta actividades anómalas que indican intentos inusuales y potencialmente peligrosos de acceder a las bases de datos o de vulnerar su seguridad. La detección de amenazas puede identificar **posible inyección SQL**, **acceso desde un centro de datos o una ubicación inusuales**, **acceso desde una aplicación potencialmente peligrosa o entidad de seguridad desconocida** y **credenciales SQL por fuerza bruta**; vea más detalles en [Alertas de detección de amenazas](sql-database-threat-detection-overview.md#azure-sql-database-threat-detection-alerts).

Puede recibir notificaciones sobre las amenazas detectadas mediante [notificaciones por correo electrónico](sql-database-threat-detection-overview.md#explore-anomalous-database-activities-upon-detection-of-a-suspicious-event) o en [Azure Portal](sql-database-threat-detection.md#explore-threat-detection-alerts-for-your-database-in-the-azure-portal).

La [detección de amenazas](sql-database-threat-detection-overview.md) forma parte de la oferta de [protección contra amenazas avanzada de SQL](sql-advanced-threat-protection.md) (ATP), que es un paquete unificado para funcionalidades avanzadas de seguridad de SQL. Puede acceder a la característica de detección de amenazas y administrarla a través del portal central de SQL ATP. El servicio de detección de amenazas se cobra a 15 USD al mes por servidor lógico, siendo los primeros 30 días gratis.

## <a name="set-up-threat-detection-for-your-database-in-the-azure-portal"></a>Configuración de la detección de amenazas en Azure Portal

1. Inicie Azure Portal en [https://portal.azure.com](https://portal.azure.com).
2. Vaya a la página de configuración del servidor de Azure SQL Database que quiere proteger. En la configuración de seguridad, seleccione **Protección contra amenazas avanzada**.
3. En la página de configuración **Protección contra amenazas avanzada**:

   - Habilite Protección contra amenazas avanzada en el servidor.
   - En **Configuración de detección de amenazas**, en el cuadro de texto **Send alerts to** (Enviar alertas a), proporcione la lista de correos electrónicos para recibir alertas de seguridad cuando se detecten actividades anómalas en la base de datos.
  
   ![Configurar la detección de amenazas](./media/sql-database-threat-detection/set_up_threat_detection.png)

## <a name="set-up-threat-detection-using-powershell"></a>Configuración de la detección de amenazas mediante PowerShell

Para ver un script de ejemplo, consulte [Configuración de la auditoría y detección de amenazas mediante PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre [Detección de amenazas](sql-database-threat-detection-overview.md)
- Más información sobre la [detección de amenazas en instancia administrada](sql-database-managed-instance-threat-detection.md).  
- Obtenga más información sobre [Protección contra amenazas avanzada de SQL](sql-advanced-threat-protection.md).
- Más información acerca de la [auditoría de Azure SQL Database](sql-database-auditing.md)
- Más información acerca de [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)
- Para más información sobre los precios, consulte la [página Precios de SQL Database](https://azure.microsoft.com/pricing/details/sql-database/)  
