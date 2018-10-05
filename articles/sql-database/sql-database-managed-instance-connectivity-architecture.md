---
title: Arquitectura de conectividad de la Instancia administrada de Azure SQL Database | Microsoft Docs
description: En este artículo se proporciona información general sobre la comunicación de la Instancia administrada de Azure SQL Database y se explica la arquitectura de conectividad y cómo funcionan los distintos componentes para dirigir el tráfico a la Instancia administrada.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: bonova, carlrab
manager: craigg
ms.date: 08/16/2018
ms.openlocfilehash: 312425d3ea02d15a992b9a694f09cb2be73b6221
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2018
ms.locfileid: "47161597"
---
# <a name="azure-sql-database-managed-instance-connectivity-architecture"></a>Arquitectura de conectividad de la Instancia administrada de Azure SQL Database 

En este artículo se proporciona información general sobre la comunicación de la Instancia administrada de Azure SQL Database y se explica la arquitectura de conectividad y cómo funcionan los distintos componentes para dirigir el tráfico a la Instancia administrada.  

## <a name="communication-overview"></a>Información general sobre la comunicación 

En el siguiente diagrama se muestran las entidades que se conectan a la Instancia administrada, así como los recursos con los que la Instancia administrada tiene que comunicarse para funcionar correctamente. 

![Entidades de la arquitectura de conectividad](./media/managed-instance-connectivity-architecture/connectivityarch001.png)

La comunicación que se muestra en la parte inferior del diagrama representa las herramientas y aplicaciones de cliente que se conectan a la Instancia administrada como origen de datos.  

Como la Instancia administrada es una oferta de plataforma como servicio (PaaS), Microsoft administra este servicio con agentes automatizados (administración, implementación y mantenimiento) basándose en flujos de datos de telemetría. Como la administración de la Instancia administrada es responsabilidad de Microsoft únicamente, los clientes no pueden acceder a las máquinas de clúster virtual de la Instancia administrada a través de RDP. 

Algunas operaciones de SQL Server iniciadas por los usuarios finales o las aplicaciones pueden requerir la Instancia administrada para interactuar con la plataforma. Un caso es la creación de una base de datos de Instancia administrada: un recurso que se expone mediante el portal, PowerShell y la CLI de Azure. 

La Instancia administrada depende de otros servicios de Azure para su funcionamiento adecuado (por ejemplo, Azure Storage para copias de seguridad, Azure Service Bus para telemetría, Azure AD para autenticación, Azure Key Vault para TDE, etc.) e inicia conexiones con ellos según corresponda. 

Todas las comunicaciones indicadas anteriormente se cifran y firman con certificados. Para asegurarse de que las partes en comunicación son de confianza, la Instancia administrada verifica constantemente estos certificados mediante la entidad de certificación. En caso de revocación de los certificados o de que la Instancia administrada no pueda verificarlos, cierra la conexión para proteger los datos. 

## <a name="high-level-connectivity-architecture"></a>Arquitectura de conectividad de alto nivel 

En un nivel alto, la Instancia administrada es un conjunto de componentes de servicio hospedado en un conjunto dedicado de máquinas virtuales aisladas que se ejecutan dentro de la subred de la red virtual del cliente y forman un clúster virtual. 

Varias instancias administradas podrían hospedarse en un solo clúster virtual. El clúster se expande y contrae automáticamente si es necesario cuando el cliente cambia el número de instancias aprovisionadas en la subred. 

Las aplicaciones cliente podrían conectarse a la Instancia administrada, consultar y actualizar bases de datos solo si se ejecutan dentro de la red virtual o de la red virtual emparejada o de la red conectada VPN/ExpressRoute con el uso del punto de conexión mediante una dirección IP privada.  

![Diagrama de la arquitectura de conectividad](./media/managed-instance-connectivity-architecture/connectivityarch002.png)

Los servicios de implementación y administración de Microsoft se ejecutan fuera de la red virtual para que la conexión entre la Instancia administrada y los servicios de Microsoft se realice a través de los puntos de conexión con direcciones IP públicas. Cuando la Instancia administrada crea la conexión de salida, en el extremo receptor parece que procede de esta dirección IP pública debido a la traducción de direcciones de red (NAT). 

El tráfico de administración fluye a través de la red virtual del cliente. Esto significa que los elementos de la infraestructura de la red virtual afectan y podrían dañar potencialmente al tráfico de administración, provocando que la instancia adopte un estado defectuoso y deje de estar disponible. 

> [!IMPORTANT]
> Para mejorar la experiencia del cliente y la disponibilidad del servicio, Microsoft aplica la directiva de intención de red a los elementos de la infraestructura de Azure Virtual Network que podrían afectar al funcionamiento de la Instancia administrada. Se trata de un mecanismo de plataforma para comunicar los requisitos de red de forma transparente a los usuarios finales, con el objetivo principal de evitar errores de configuración de red y garantizar el funcionamiento normal de la Instancia administrada. Con la eliminación de la Instancia administrada también se elimina la directiva de intención de red. 

## <a name="virtual-cluster-connectivity-architecture"></a>Arquitectura de conectividad del clúster virtual 

Vamos a profundizar más en la arquitectura de conectividad de la Instancia administrada. En el siguiente diagrama se muestra el diseño conceptual del clúster virtual. 

![Clúster virtual en el diagrama de la arquitectura de conectividad](./media/managed-instance-connectivity-architecture/connectivityarch003.png)

Los clientes se conectan a la Instancia administrada mediante un nombre de host que presenta la forma <mi_nombre>.<clusterid>.database.windows.net. Este nombre de host se resuelve en la dirección IP privada aunque se registra en la zona DNS pública y puede resolverse públicamente. 

Esta dirección IP privada pertenece al equilibrador de carga interno (ILB) de la Instancia administrada que dirige el tráfico a la puerta de enlace de la Instancia administrada. Como se podrían ejecutar varias instancias administradas dentro del mismo clúster, la puerta de enlace usa el nombre de host de la Instancia administrada para dirigir el tráfico al servicio correcto del motor de SQL. 

Los servicios de administración e implementación se conectan a la Instancia administrada con el punto de conexión público que se asigna al equilibrador de carga externo. El tráfico se enruta a los nodos solo si se recibe en un conjunto predefinido de puertos que usan exclusivamente los componentes de administración de la Instancia administrada. Toda la comunicación entre los componentes de administración y el plano de administración se autentican mutuamente mediante certificados. 

## <a name="next-steps"></a>Pasos siguientes 

- Para información general, vea  [¿Qué es Instancia administrada de SQL Database (versión preliminar)?](sql-database-managed-instance.md) 
- Para más información acerca de la configuración de redes virtuales, consulte  [Configuración de una red virtual de instancia administrada](sql-database-managed-instance-vnet-configuration.md). 
- Para consultar una guía de inicio rápido, vea cómo crear la Instancia administrada: 
  - En [Azure Portal](sql-database-managed-instance-get-started.md) 
  - Con [PowerShell](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/27/quick-start-script-create-azure-sql-managed-instance-using-powershell/) 
  - con la [plantilla de Azure Resource Manager](https://azure.microsoft.com/resources/templates/101-sqlmi-new-vnet/) 
  - con la [plantilla de Azure Resource Manager (JumpBox con SSMS incluido)](https://portal.azure.com/) 

