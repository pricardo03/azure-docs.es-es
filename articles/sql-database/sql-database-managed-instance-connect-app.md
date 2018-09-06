---
title: Conexión de la aplicación a Instancia administrada de Azure SQL Database | Microsoft Docs
description: En este artículo se describe la conexión de la aplicación a Instancia administrada de Azure SQL Database.
ms.service: sql-database
author: srdan-bozovic-msft
manager: craigg
ms.custom: managed instance
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.openlocfilehash: 82e8836892b033ccbb3c3ad9806257348afe3702
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42818409"
---
# <a name="connect-your-application-to-azure-sql-database-managed-instance"></a>Conexión de la aplicación a Instancia administrada de Azure SQL Database

Hoy en día puede elegir entre varias opciones sobre cuándo y cómo hospedar la aplicación. 
 
Puede elegir hospedar la aplicación en la nube mediante Azure App Service o algunas de las opciones de red virtual integradas de Azure, como Azure App Service Environment, Virtual Machine o Virtual Machine Scale Set. También podría elegir un enfoque de nube híbrida y mantener las aplicaciones en local. 
 
Sea cual sea su opción, se podrá realizar una conexión a Instancia administrada (versión preliminar).  

![alta disponibilidad](./media/sql-database-managed-instance/application-deployment-topologies.png)  

## <a name="connect-an-application-inside-the-same-vnet"></a>Conexión de una aplicación dentro de la misma red virtual 

Este escenario es el más sencillo. Las máquinas virtuales dentro de la red virtual pueden conectarse directamente entre sí aunque se encuentren en subredes diferentes. Esto significa que para conectar la aplicación dentro de una instancia de Azure Application Environment o de Virtual Machine solo tiene que establecer correctamente la cadena de conexión.  
 
Si no puede establecer la conexión, compruebe si hay algún grupo de seguridad de red establecido en la subred de la aplicación. Si es así, abra una conexión saliente en el puerto 1433 de SQL, así como en los puertos 11000-12000 para el redireccionamiento. 

## <a name="connect-an-application-inside-a-different-vnet"></a>Conexión de una aplicación dentro de una red virtual distinta 

Este escenario es un poco más complejo, ya que Instancia administrada tiene la dirección IP privada en su propia red virtual. Para conectarse, una aplicación necesita acceso a la red virtual donde Instancia administrada está implementada. Por lo tanto, primero debe establecer una conexión entre la aplicación y la red virtual de Instancia administrada. Las redes virtuales no tienen por qué estar en la misma suscripción para que este escenario funcione. 
 
Para conectar redes virtuales hay dos opciones: 
- [Emparejamiento de Azure Virtual Network](../virtual-network/virtual-network-peering-overview.md) 
- Puerta de enlace VPN de red virtual a red virtual ([Azure Portal](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md), [PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md), [CLI de Azure](../vpn-gateway/vpn-gateway-howto-vnet-vnet-cli.md)) 
 
La opción de emparejamiento es la preferida, porque se usa la red troncal de Microsoft y, por lo tanto, desde la perspectiva de la conectividad, no hay diferencias notables de latencia entre máquinas virtuales de la red virtual emparejada y la propia. El emparejamiento de VNet está restringido a las redes de la misma región.  
 
> [!IMPORTANT]
> El escenario de emparejamiento de VNet de la instancia administrada está restringido a las redes de la misma región debido a las [restricciones de emparejamiento de redes virtuales globales](../virtual-network/virtual-network-manage-peering.md#requirements-and-constraints). 

## <a name="connect-an-on-premises-application"></a>Conexión de una aplicación local 

A Instancia administrada solo se puede acceder mediante dirección IP privada. Para acceder a la instancia desde la máquina local, debe establecer una conexión de sitio a sitio entre la aplicación y la red virtual de Instancia administrada. 
 
Hay dos opciones de conexión local a una red virtual de Azure: 
- Conexión VPN de sitio a sitio ([Azure Portal](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md), [PowerShell](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md), [CLI de Azure](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md)) 
- Conexión de [ExpressRoute](../expressroute/expressroute-introduction.md)  
 
Si ha establecido la conexión local a Azure correctamente y no puede establecer la conexión con Instancia administrada, compruebe si el firewall tiene abierta la conexión saliente en el puerto 1433 de SQL, así como los puertos 11000-12000 para el redireccionamiento. 

## <a name="connect-an-azure-app-service-hosted-application"></a>Conexión de una aplicación hospedada en Azure App Service 

A Instancia administrada se puede acceder solo mediante una dirección IP privada, por ello, para acceder desde Azure App Service, primero debe realizar una conexión entre la aplicación y la red virtual de Instancia administrada. Consulte [Integración de su aplicación con una instancia de Azure Virtual Network](../app-service/web-sites-integrate-with-vnet.md).  
 
Para la solución de problemas, consulte la sección de [solución de problemas con redes virtuales y aplicaciones](../app-service/web-sites-integrate-with-vnet.md#troubleshooting). Si no se puede establecer una conexión, intente [sincronizar la configuración de red](sql-database-managed-instance-sync-network-configuration.md). 
 
Un caso especial de conexión de Azure App Service a Instancia administrada es cuando la instancia de Azure App Service se ha integrado en una red virtual emparejada a otra red virtual de Instancia administrada. Ese caso requiere la siguiente configuración: 

- La red virtual de Instancia administrada NO debe tener puerta de enlace  
- La red virtual de Instancia administrada debe tener la opción Usar puertas de enlace remotas establecida 
- La red virtual emparejada debe tener la opción Permitir tránsito de puerta de enlace establecida 
 
Este escenario se ilustra en el diagrama siguiente:

![emparejamiento de aplicación integrada](./media/sql-database-managed-instance/integrated-app-peering.png)
 
## <a name="connect-an-application-on-the-developers-box"></a>Conexión de una aplicación en el cuadro de desarrollo 

A Instancia administrada se puede acceder solo mediante una dirección IP privada, por ello, para acceder desde el cuadro de desarrollo, primero debe realizar una conexión entre el cuadro de desarrollo y la red virtual de Instancia administrada.  
 
En la configuración de una conexión de punto a sitio a una red virtual en los artículos de autenticación con certificados de Azure nativos ([Azure Portal](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md), [PowerShell](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md), [CLI de Azure](../vpn-gateway/vpn-gateway-howto-point-to-site-classic-azure-portal.md)) se muestran los detalles de cómo se hace. 

## <a name="required-versions-of-drivers-and-tools"></a>Versiones necesarias de los controladores y las herramientas

Si desea conectarse a Instancia administrada, se recomiendan las siguientes versiones mínimas de las herramientas y los controladores:

| Controlador/Herramienta | Versión |
| --- | --- |
|.NET Framework | 4.6.1 o .NET Core | 
|Controlador ODBC    | v17 |
|Controlador PHP | 5.2.0 |
|Controlador JDBC    | 6.4.0 |
|Controlador de Node.js | 2.1.1 |
|Controlador de OLEDB   | 18.0.2.0 |
|SSMS   | 17.8.1 o [posterior](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017) |

## <a name="next-steps"></a>Pasos siguientes

- Para más información acerca de Instancia administrada, consulte [¿Qué es Instancia administrada de SQL Database (versión preliminar)?](sql-database-managed-instance.md).
- Para un tutorial que muestra cómo crear una instancia administrada, consulte el artículo de [creación de instancias administradas](sql-database-managed-instance-get-started.md).
