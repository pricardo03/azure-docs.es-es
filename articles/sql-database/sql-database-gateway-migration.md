---
title: Aviso de migración de puerta de enlace de Azure SQL Database de Gen2 a Gen3 | Microsoft Docs
description: El artículo proporciona un aviso a los usuarios sobre la migración de direcciones IP de puertas de enlace de Azure SQL Database
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: vanto
manager: craigg
ms.date: 07/01/2019
ms.openlocfilehash: 5894579c62c524394c7fea044b96885f7c8e8204
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538152"
---
# <a name="azure-sql-database-traffic-migration-to-newer-gateways"></a>Migración de tráfico de Azure SQL Database a puertas de enlace más recientes

A media que la infraestructura de Azure mejora, Microsoft actualizará periódicamente el hardware para asegurarse de que ofrecemos la mejor experiencia de cliente posible. En los próximos meses, tenemos previsto agregar puertas de enlace basadas en generaciones de hardware más recientes y retirar puertas de enlace basadas en hardware más antiguo en algunas regiones.  

Se enviará un aviso a los clientes por correo electrónico y en Azure Portal con antelación de cualquier cambios en las puertas de enlace disponibles en cada región. La información más actualizada se mantendrá en la tabla [Direcciones IP de la puerta de enlace de Azure SQL Database](sql-database-connectivity-architecture.md#azure-sql-database-gateway-ip-addresses).

## <a name="impact-of-this-change"></a>Impacto de este cambio

La primera ronda de retirada de la puerta de enlace está programada para el 1 de septiembre de 2019 en las siguientes regiones:

- Oeste de EE. UU.
- Europa occidental
- Este de EE. UU
- Centro de EE. UU.
- Sudeste de Asia
- Centro-Sur de EE. UU
- Europa del Norte
- Centro-Norte de EE. UU
- Oeste de Japón
- Este de Japón
- Este de EE. UU. 2
- Asia oriental

La dirección IP retirada dejará de aceptar tráfico, y cualquier nuevo intento de conexión se enrutará a una de las puertas de enlace de la región.

Dónde no se verá el impacto de este cambio:

- Los clientes que usan el redireccionamiento como directiva de conexión no verán ningún impacto.
- Las conexiones a SQL Database desde Azure y la utilización de etiquetas de servicio no se verán afectadas.
- Las conexiones realizadas mediante versiones compatibles de JDBC Driver para SQL Server no percibirán ningún impacto. Para las versiones compatibles de JDBC, consulte [Descarga de Microsoft JDBC Driver para SQL Server](/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server).

## <a name="what-to-do-you-do-if-youre-affected"></a>Qué puede hacer si se ve afectado

Es recomendable que permita el tráfico saliente a direcciones IP para todas las [direcciones IP de puerta de enlace de Azure SQL Database](sql-database-connectivity-architecture.md#azure-sql-database-gateway-ip-addresses) de la región en el puerto TCP 1433 y el intervalo de puertos de 11000 a 11999 en el dispositivo de firewall. Para obtener más información sobre los intervalos de puertos, consulte [Directiva de conexión](sql-database-connectivity-architecture.md#connection-policy).

Es posible que las conexiones realizadas desde aplicaciones que usan versiones de Microsoft JDBC Driver anteriores a 4.0 no superen la validación de certificados. Las versiones inferiores de Microsoft JDBC dependen del nombre común (CN) en el campo Asunto del certificado. La mitigación consiste en asegurarse de que la propiedad hostNameInCertificate esté establecida en *.database.windows.net. Para obtener más información sobre cómo establecer la propiedad hostNameInCertificate, consulte [Conectar con el cifrado SSL](/sql/connect/jdbc/connecting-with-ssl-encryption).

Si la mitigación anterior no funciona, envíe una solicitud de soporte técnico para SQL Database con la dirección URL siguiente: https://aka.ms/getazuresupport

## <a name="next-steps"></a>Pasos siguientes

- Obtener más información sobre la [Arquitectura de conectividad de Azure SQL](sql-database-connectivity-architecture.md)