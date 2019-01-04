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
ms.date: 12/10/2018
ms.openlocfilehash: 964f91f412645e141ca003d511480f6f6eb438a3
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2018
ms.locfileid: "53343314"
---
# <a name="azure-sql-database-managed-instance-connectivity-architecture"></a>Arquitectura de conectividad de la Instancia administrada de Azure SQL Database

En este artículo se proporciona información general sobre la comunicación de la Instancia administrada de Azure SQL Database y se explica la arquitectura de conectividad y cómo funcionan los distintos componentes para dirigir el tráfico a la Instancia administrada.  

La Instancia administrada de Azure SQL Database está ubicada en la red virtual de Azure y la subred dedicada a las instancias administradas. Esta implementación permite los siguientes escenarios: 
- Protección de una dirección IP privada
- Conexión a Instancia administrada directamente desde una red local
- Conexión de Instancia administrada con un servidor vinculado o con otro almacén de datos local
- Conexión de Instancia administrada a los recursos de Azure

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

Los clientes se conectan a la Instancia administrada mediante un nombre de host que presenta la forma `<mi_name>.<dns_zone>.database.windows.net`. Este nombre de host se resuelve en la dirección IP privada aunque se registra en la zona DNS pública y puede resolverse públicamente. El `zone-id` se genera automáticamente cuando se crea el clúster. Si un clúster recién creado hospeda una instancia administrada secundaria, comparte su identificador de zona con el clúster principal. Para más información, consulte [Grupos de conmutación por error automática](sql-database-auto-failover-group.md##enabling-geo-replication-between-managed-instances-and-their-vnets).

Esta dirección IP privada pertenece al equilibrador de carga interno (ILB) de la Instancia administrada que dirige el tráfico a la puerta de enlace de la Instancia administrada. Como se podrían ejecutar varias instancias administradas dentro del mismo clúster, la puerta de enlace usa el nombre de host de la Instancia administrada para dirigir el tráfico al servicio correcto del motor de SQL.

Los servicios de administración e implementación se conectan a la Instancia administrada con el [punto de conexión de administración](#management-endpoint) que se asigna al equilibrador de carga externo. El tráfico se enruta a los nodos solo si se recibe en un conjunto predefinido de puertos que usan exclusivamente los componentes de administración de la Instancia administrada. El firewall integrado en los nodos está configurado para permitir el tráfico solo desde intervalos IP específicos de Microsoft. Toda la comunicación entre los componentes de administración y el plano de administración se autentican mutuamente mediante certificados.

## <a name="management-endpoint"></a>Administración de puntos de conexión

El clúster virtual de Instancia administrada de Azure SQL Database contiene un punto de conexión de administración que Microsoft utiliza para administrar la Instancia administrada. El punto de conexión de administración está protegido con un firewall integrado a nivel de red y con comprobación mutua de certificados a nivel de aplicación. Puede [buscar la dirección IP de punto de conexión de administración](sql-database-managed-instance-find-management-endpoint-ip-address.md).

Cuando las conexiones se inician desde dentro de la Instancia administrada (copia de seguridad, registro de auditoría), parece que el tráfico se origina desde la dirección IP pública del punto de conexión de administración. Puede limitar el acceso a los servicios públicos desde la Instancia administrada mediante el establecimiento de reglas de firewall para permitir únicamente la dirección IP de la Instancia administrada. Obtenga más información sobre el método que puede [comprobar el firewall integrado de la Instancia administrada](sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md).

> [!NOTE]
> Esto no se aplica a la configuración de reglas de firewall para los servicios de Azure que están en la misma región que la Instancia administrada, ya que la plataforma Azure tiene una optimización para el tráfico que pasa entre los servicios que se colocan.

## <a name="network-requirements"></a>Requisitos de red

Puede implementar una Instancia administrada en una subred dedicada (la subred de Instancia administrada) dentro de la red virtual que cumpla los requisitos siguientes:
- **Subred dedicada**: la subred de Instancia administrada no puede contener ningún otro servicio de nube asociado a ella y no puede ser la subred de puerta de enlace. No podrá crear ninguna Instancia administrada en una subred que contenga otros recursos que no sean los de la instancia administrada, ni podrá agregar otros recursos más adelante en la subred.
- **Grupo de seguridad de red (NSG) compatible**: un grupo de seguridad de red asociado a una subred de Instancia administrada debe contener las reglas que se muestran en las tablas siguientes (las reglas de seguridad de entrada y las reglas de seguridad de salida obligatorias) delante de cualquier otra regla. Puede usar un NSG para controlar totalmente el acceso al punto de conexión de datos de la Instancia administrada filtrando el tráfico en el puerto 1433. 
- **Tabla de rutas definida por el usuario (UDR) compatible**: la subred de la Instancia administrada debe tener una tabla de rutas de usuario con **Internet de próximo salto 0.0.0.0/0** como la UDR obligatoria asignada a ella. Además, puede agregar una UDR que dirija el tráfico que tiene intervalos IP privados locales como destino a través de una puerta de enlace de red local o de un dispositivo de red virtual (NVA). 
- **DNS personalizado opcional**: si se especifica un DNS personalizado en la red virtual, es necesario agregar a la lista la dirección IP de la resolución recursiva de Azure (por ejemplo, 168.63.129.16). Para más información, consulte [Configuración de DNS personalizado](sql-database-managed-instance-custom-dns.md). El servidor DNS personalizado debe ser capaz de resolver los nombres de host en los siguientes dominios y sus subdominios: *microsoft.com*, *windows.net*, *windows.com*, *msocsp.com*, *digicert.com*, *live.com*, *microsoftonline.com* y *microsoftonline-p.com*. 
- **No tener ningún punto de conexión de servicio**: la subred de Instancia administrada no puede tener un punto de conexión de servicio asociado a ella. Asegúrese de que la opción Puntos de conexión de servicio está deshabilitada cuando cree la red virtual.
- **Tener suficientes direcciones IP**: la subred de Instancia administrada debe tener el mínimo de 16 direcciones IP (el mínimo recomendado es 32 direcciones IP). Para más información, consulte la sección [Determinación del tamaño de subred para instancias administradas](sql-database-managed-instance-determine-size-vnet-subnet.md). Puede implementar las Instancias administradas en [la red existente](sql-database-managed-instance-configure-vnet-subnet.md) una vez que la configure para satisfacer [los requisitos de red de la Instancia administrada](#network-requirements) o crear una [nueva red y subred](sql-database-managed-instance-create-vnet-subnet.md).

> [!IMPORTANT]
> No podrá implementar una nueva Instancia administrada si la subred de destino no es compatible con todos estos requisitos. Cuando se crea una Instancia administrada, se aplica una *directiva de intención de red* en la subred para evitar cambios no compatibles con la configuración de red. Después de quitar la última instancia de la subred, también se quitará la *directiva de intención de red*.

### <a name="mandatory-inbound-security-rules"></a>Reglas de seguridad de entrada obligatorias 

| NOMBRE       |Port                        |Protocolo|Origen           |Destino|.|
|------------|----------------------------|--------|-----------------|-----------|------|
|management  |9000, 9003, 1438, 1440, 1452|TCP     |Cualquiera              |Cualquiera        |PERMITIR |
|mi_subnet   |Cualquiera                         |Cualquiera     |MI SUBNET        |Cualquiera        |PERMITIR |
|health_probe|Cualquiera                         |Cualquiera     |AzureLoadBalancer|Cualquiera        |PERMITIR |

### <a name="mandatory-outbound-security-rules"></a>Reglas de seguridad de salida obligatorias 

| NOMBRE       |Port          |Protocolo|Origen           |Destino|.|
|------------|--------------|--------|-----------------|-----------|------|
|management  |80, 443, 12000|TCP     |Cualquiera              |Cualquiera        |PERMITIR |
|mi_subnet   |Cualquiera           |Cualquiera     |Cualquiera              |MI SUBNET  |PERMITIR |

  > [!Note]
  > Aunque las reglas de seguridad de entrada obligatorias permiten el tráfico desde _cualquier_ origen en los puertos 9000, 9003, 1438, 1440, 1452, estos puertos están protegidos por un firewall integrado. Este [artículo](sql-database-managed-instance-find-management-endpoint-ip-address.md) muestra cómo puede detectar la dirección IP del punto de conexión de administración y comprobar las reglas del firewall. 

## <a name="next-steps"></a>Pasos siguientes

- Para información general, vea  [¿Qué es Instancia administrada de SQL Database (versión preliminar)?](sql-database-managed-instance.md)
- Aprenda cómo [configurar una nueva red virtual](sql-database-managed-instance-create-vnet-subnet.md) o [cómo configurar una red virtual existente](sql-database-managed-instance-configure-vnet-subnet.md) en la que pueda implementar Instancias administradas.
- [Calcule el tamaño de la subred](sql-database-managed-instance-determine-size-vnet-subnet.md) donde va a implementar Instancias administradas. 
- Para consultar una guía de inicio rápido, vea cómo crear la Instancia administrada:
  - En [Azure Portal](sql-database-managed-instance-get-started.md)
  - Con [PowerShell](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/27/quick-start-script-create-azure-sql-managed-instance-using-powershell/)
  - con la [plantilla de Azure Resource Manager](https://azure.microsoft.com/resources/templates/101-sqlmi-new-vnet/)
  - con la [plantilla de Azure Resource Manager (JumpBox con SSMS incluido)](https://portal.azure.com/)
