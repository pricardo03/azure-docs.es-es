---
title: Aviso de migración del tráfico de puerta de enlace para Azure SQL Database | Microsoft Docs
description: El artículo proporciona un aviso a los usuarios sobre la migración de direcciones IP de puertas de enlace de Azure SQL Database
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: vanto
ms.date: 07/01/2019
ms.openlocfilehash: 1fc6c054b32c62fbebaa2af738e25ef0dec362ac
ms.sourcegitcommit: 6d2a147a7e729f05d65ea4735b880c005f62530f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2019
ms.locfileid: "69981288"
---
# <a name="azure-sql-database-traffic-migration-to-newer-gateways"></a>Migración de tráfico de Azure SQL Database a puertas de enlace más recientes

A media que la infraestructura de Azure mejora, Microsoft actualizará periódicamente el hardware para asegurarse de que ofrecemos la mejor experiencia de cliente posible. En los próximos meses, tenemos previsto agregar puertas de enlace basadas en generaciones de hardware más recientes, migrar el tráfico a ellas y retirar puertas de enlace basadas en hardware más antiguo en algunas regiones.  

Se enviará un aviso a los clientes por correo electrónico y en Azure Portal con antelación de cualquier cambios en las puertas de enlace disponibles en cada región. La información más actualizada se mantendrá en la tabla [Direcciones IP de la puerta de enlace de Azure SQL Database](sql-database-connectivity-architecture.md#azure-sql-database-gateway-ip-addresses).

## <a name="impact-of-this-change"></a>Impacto de este cambio

La primera ronda de migración del tráfico a las puertas de enlace más recientes está programada para el **14 de octubre de 2019** en las siguientes regiones:
- Sur de Brasil
- Oeste de EE. UU.
- Europa occidental
- East US
- Centro de EE. UU.
- Sudeste de Asia
- Centro-Sur de EE. UU
- Europa del Norte
- Centro-Norte de EE. UU
- Oeste de Japón
- Este de Japón
- Este de EE. UU. 2
- Asia oriental

La migración del tráfico cambiará la dirección IP pública que DNS resuelve para su instancia de SQL Database.
Se verá afectado en los siguientes casos:
- Ha codificado de forma rígida la dirección IP de una puerta de enlace determinada en el firewall local.
- Tiene subredes que usan Microsoft.SQL como punto de conexión de servicio, pero no se puede comunicar con las direcciones IP de puerta de enlace.

No se verá afectado en los siguientes casos: 
- La directiva de conexión tiene redireccionamiento.
- Tiene conexiones a SQL Database desde Azure y con etiquetas de servicio.
- Las conexiones realizadas mediante versiones compatibles de JDBC Driver para SQL Server no percibirán ningún impacto. Para las versiones compatibles de JDBC, consulte [Descarga de Microsoft JDBC Driver para SQL Server](/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server).

## <a name="what-to-do-you-do-if-youre-affected"></a>Qué puede hacer si se ve afectado

Es recomendable que permita el tráfico saliente a direcciones IP para todas las [direcciones IP de puerta de enlace de Azure SQL Database](sql-database-connectivity-architecture.md#azure-sql-database-gateway-ip-addresses) de la región en el puerto TCP 1433 y el intervalo de puertos de 11000 a 11999 en el dispositivo de firewall. Para obtener más información sobre los intervalos de puertos, consulte [Directiva de conexión](sql-database-connectivity-architecture.md#connection-policy).

Es posible que las conexiones realizadas desde aplicaciones que usan versiones de Microsoft JDBC Driver anteriores a 4.0 no superen la validación de certificados. Las versiones inferiores de Microsoft JDBC dependen del nombre común (CN) en el campo Asunto del certificado. La mitigación consiste en asegurarse de que la propiedad hostNameInCertificate esté establecida en *.database.windows.net. Para obtener más información sobre cómo establecer la propiedad hostNameInCertificate, consulte [Conectar con el cifrado SSL](/sql/connect/jdbc/connecting-with-ssl-encryption).

Si la mitigación anterior no funciona, envíe una solicitud de soporte técnico para SQL Database con la dirección URL siguiente: https://aka.ms/getazuresupport

## <a name="next-steps"></a>Pasos siguientes

- Obtener más información sobre la [Arquitectura de conectividad de Azure SQL](sql-database-connectivity-architecture.md)