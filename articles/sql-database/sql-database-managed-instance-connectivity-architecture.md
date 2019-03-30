---
title: Arquitectura de conectividad de una instancia administrada de Azure SQL Database | Microsoft Docs
description: Obtenga información sobre la comunicación de instancia administrada de Azure SQL Database y la arquitectura de conectividad también cómo los componentes de dirigir el tráfico a la instancia administrada.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: fasttrack-edit
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: bonova, carlrab
manager: craigg
ms.date: 02/26/2019
ms.openlocfilehash: f08b22f24dfde41646f56dc1ecd9777f267620ee
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2019
ms.locfileid: "58651319"
---
# <a name="connectivity-architecture-for-a-managed-instance-in-azure-sql-database"></a>Arquitectura de conectividad de una instancia administrada de Azure SQL Database 

En este artículo se explica la comunicación en una instancia administrada de Azure SQL Database. También describe la arquitectura de conectividad y cómo los componentes de dirigir el tráfico a la instancia administrada.  

La instancia administrada de SQL Database se coloca dentro de la red virtual de Azure y la subred que está dedicada a las instancias administradas. Esta implementación proporciona:

- Una dirección IP privada segura.
- La capacidad de conectar una red local a una instancia administrada.
- La capacidad para conectarse a una instancia administrada en un servidor vinculado o en otro almacén de datos en local.
- La capacidad para conectarse a una instancia administrada a los recursos de Azure.

## <a name="communication-overview"></a>Información general sobre la comunicación

El siguiente diagrama muestra las entidades que se conectan a una instancia administrada. También muestra los recursos que necesitan para comunicarse con la instancia administrada. El proceso de comunicación en la parte inferior del diagrama representa las aplicaciones cliente y herramientas que se conectan a la instancia administrada como orígenes de datos.  

![Entidades de la arquitectura de conectividad](./media/managed-instance-connectivity-architecture/connectivityarch001.png)

Una instancia administrada es una plataforma como servicio (PaaS). Microsoft usa a automatizadas agents (administración, implementación y mantenimiento) para administrar este servicio basado en secuencias de datos de telemetría. Dado que Microsoft es responsable de administración, los clientes no pueden acceder a las máquinas virtuales del clúster de instancia administrada a través del protocolo de escritorio remoto (RDP).

Algunas operaciones a los usuarios finales o las aplicaciones pueden necesitar de SQL Server administra las instancias para interactuar con la plataforma. Un caso es la creación de una base de datos de instancia administrada. Este recurso se expone a través de la API de REST, PowerShell, CLI de Azure y Azure portal.

Las instancias administradas dependen de servicios de Azure como Azure Storage para copias de seguridad, Azure Service Bus para la telemetría, Azure Active Directory para la autenticación y Azure Key Vault para el cifrado de datos transparente (TDE). Las instancias administradas realizar conexiones a estos servicios.

Todas las comunicaciones utilizan certificados para cifrado y firma. Para comprobar la confiabilidad de comunicación partes, administra las instancias comprobación constantemente estos certificados ponerse en contacto con una entidad de certificación. Si los certificados se revocan o no se puede comprobar, la instancia administrada cierra la conexión para proteger los datos.

## <a name="high-level-connectivity-architecture"></a>Arquitectura de conectividad de alto nivel

En un nivel alto, una instancia administrada es un conjunto de componentes del servicio. Estos componentes se hospedan en un conjunto dedicado de máquinas virtuales aisladas que se ejecutan dentro de la subred de red virtual del cliente. Estas máquinas forman un clúster virtual.

Un clúster virtual puede hospedar varias instancias administradas. Si es necesario, el clúster automáticamente expande o contrae cuando el cliente cambia el número de instancias aprovisionados en la subred.

Las aplicaciones cliente pueden conectarse a las instancias administradas y pueden consultar y actualizar las bases de datos solo si se ejecutan dentro de la red virtual, emparejada virtual network o redes conectadas mediante VPN o ExpressRoute de Azure. Esta red debe usar un punto de conexión y una dirección IP privada.  

![Diagrama de arquitectura de conectividad](./media/managed-instance-connectivity-architecture/connectivityarch002.png)

Servicios de implementación y administración de Microsoft que se ejecutan fuera de la red virtual. Una instancia administrada y servicios de Microsoft conectan a través de los puntos de conexión que tienen direcciones IP públicas. Cuando una instancia administrada crea una conexión de salida, en el extremo receptor realiza traducción de direcciones de red (NAT) el aspecto de conexión procede de esta dirección IP pública.

Tráfico de administración fluye a través de la red virtual del cliente. Esto significa que los elementos de la infraestructura de la red virtual pueden dañar el tráfico de administración mediante la realización de la instancia de un error y dejan de estar disponibles.

> [!IMPORTANT]
> Para mejorar la experiencia del cliente y la disponibilidad del servicio, Microsoft aplica una directiva de red intención en elementos de la infraestructura de red virtual de Azure. La directiva puede afectar al funcionamiento de la instancia administrada. Este mecanismo de plataforma transparente comunica los requisitos de red a los usuarios. Objetivo principal de la directiva es para evitar errores de configuración de red y para garantizar un funcionamiento normal de instancia administrada. Cuando se elimina una instancia administrada, también se quita la directiva de intención de la red.

## <a name="virtual-cluster-connectivity-architecture"></a>Arquitectura de conectividad del clúster virtual

Vamos a profundizar más en la arquitectura de conectividad para las instancias administradas. En el siguiente diagrama se muestra el diseño conceptual del clúster virtual.

![Arquitectura de conectividad del clúster virtual](./media/managed-instance-connectivity-architecture/connectivityarch003.png)

Los clientes se conectan a una instancia administrada mediante el uso de un nombre de host que tiene el formato `<mi_name>.<dns_zone>.database.windows.net`. Este nombre de host se resuelve en una dirección IP privada aunque se registra en una zona de sistema de nombres de dominio (DNS) de dominio público y es que pueda resolverse públicamente. El `zone-id` se genera automáticamente al crear el clúster. Si un clúster recién creado hospeda una instancia administrada secundaria, comparte su Id. de zona con el clúster principal. Para obtener más información, consulte [usar grupos de conmutación por error automática para permitir una recuperación de varias bases de datos transparente y coordinada](sql-database-auto-failover-group.md##enabling-geo-replication-between-managed-instances-and-their-vnets).

Esta dirección IP privada pertenece al equilibrador de carga interno de la instancia administrada. El equilibrador de carga dirige el tráfico a la puerta de enlace de la instancia administrada. Ya pueden ejecutar varias instancias administradas dentro del mismo clúster, la puerta de enlace usa el nombre de host de la instancia administrada para redirigir el tráfico al servicio de motor SQL correcto.

Servicios de implementación y administración de conexión a una instancia administrada mediante un [extremo de administración](#management-endpoint) que se asigna a una referencia externa del equilibrador de carga. Tráfico se enruta a los nodos solo si se hubiera recibido en un conjunto predefinido de puertos que utilizan sólo los componentes de administración de la instancia administrada. Un firewall integrado en los nodos está configurado para permitir el tráfico solo desde los intervalos IP de Microsoft. Todas las comunicaciones entre los componentes de administración y el plano de administración autentican mutuamente con certificados.

## <a name="management-endpoint"></a>Administración de puntos de conexión

Microsoft administra la instancia administrada mediante el uso de un extremo de administración. Este punto de conexión está dentro del clúster virtual de la instancia. El extremo de administración está protegido por un firewall integrado en el nivel de red. En el nivel de aplicación, está protegido mediante la comprobación del certificado mutuo. Para buscar la dirección IP del punto de conexión, consulte [determinar la dirección IP de la administración del punto de conexión](sql-database-managed-instance-find-management-endpoint-ip-address.md).

Al iniciar las conexiones dentro de la instancia administrada (como ocurre con las copias de seguridad y los registros de auditoría), el tráfico aparece iniciar desde la dirección IP pública de la administración del punto de conexión. Puede limitar el acceso a servicios públicos desde una instancia administrada mediante el establecimiento de reglas de firewall para permitir solo la dirección IP de la instancia administrada. Para obtener más información, consulte [Compruebe firewall integrado de la instancia administrada](sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md).

> [!NOTE]
> A diferencia de firewall para las conexiones que se inicie dentro de la instancia administrada, los servicios de Azure que están dentro de la región de la instancia administrada tienen un firewall que está optimizado para el tráfico que pasa entre estos servicios.

## <a name="network-requirements"></a>Requisitos de red

Implementar una instancia administrada en una subred dentro de la red virtual dedicada. La subred debe tener estas características:

- **Subred dedicada:** Subred de la instancia administrada no puede contener cualquier otro servicio en la nube que está asociado con él, y no puede ser una subred de puerta de enlace. La subred no puede contener cualquier recurso, pero la instancia administrada y, posteriormente, no puede agregar los recursos en la subred.
- **Grupo de seguridad de red (NSG):** Debe definir un NSG asociado a la red virtual [reglas de seguridad de entrada](#mandatory-inbound-security-rules) y [reglas de seguridad de salida](#mandatory-outbound-security-rules) antes de cualquier otra regla. Puede usar un NSG para controlar el acceso al punto de conexión de datos de la instancia administrada filtrando el tráfico en el puerto 1433.
- **Tabla de usuario (UDR) de ruta definida por:** Una tabla UDR que está asociado con la red virtual debe incluir específico [entradas](#user-defined-routes).
- **Ningún punto de conexión de servicio:** Ningún extremo de servicio se debe asociar con la subred de la instancia administrada. Asegúrese de que la opción de puntos de conexión de servicio está deshabilitada cuando se crea la red virtual.
- **Suficientes direcciones IP:** La subred de instancia administrada debe tener al menos 16 direcciones IP. El mínimo recomendado es 32 direcciones IP. Para obtener más información, consulte [determinar el tamaño de la subred para instancias administradas](sql-database-managed-instance-determine-size-vnet-subnet.md). Puede implementar las instancias administradas en [la red existente](sql-database-managed-instance-configure-vnet-subnet.md) después de configurarlo para satisfacer [los requisitos de red para las instancias administradas](#network-requirements). De lo contrario, cree un [red y subred nuevas](sql-database-managed-instance-create-vnet-subnet.md).

> [!IMPORTANT]
> No se puede implementar una nueva instancia administrada si la subred de destino no tiene estas características. Cuando se crea una instancia administrada, se aplica una directiva de intención de red en la subred para evitar cambios no compatibles en el programa de instalación de red. Después de quita la última instancia de la subred, también se quita la directiva de intención de la red.

### <a name="mandatory-inbound-security-rules"></a>Reglas de seguridad de entrada obligatorias

| NOMBRE       |Port                        |Protocolo|Origen           |Destino|.|
|------------|----------------------------|--------|-----------------|-----------|------|
|management  |9000, 9003, 1438, 1440, 1452|TCP     |Cualquiera              |Cualquiera        |PERMITIR |
|mi_subnet   |Cualquiera                         |Cualquiera     |MI SUBNET        |Cualquiera        |PERMITIR |
|health_probe|Cualquiera                         |Cualquiera     |AzureLoadBalancer|Cualquiera        |PERMITIR |

### <a name="mandatory-outbound-security-rules"></a>Reglas de seguridad de salida obligatorias

| NOMBRE       |Port          |Protocolo|Origen           |Destino|.|
|------------|--------------|--------|-----------------|-----------|------|
|management  |80, 443, 12000|TCP     |Cualquiera              |AzureCloud  |PERMITIR |
|mi_subnet   |Cualquiera           |Cualquiera     |Cualquiera              |MI SUBRED *  |PERMITIR |

> [!IMPORTANT]
> Asegúrese de que hay solo una regla de entrada para los puertos 9000, 9003, 1438, 1440, 1452 y una regla de salida de los puertos 80, 443, 12000. Aprovisionamiento de instancia administrada a través de implementaciones de ARM generará un error si las reglas de entrada y salidas se configuran por separado para cada puerto. Si estos puertos están siendo reglas independientes, se producirá un error en la implementación con código de error `VnetSubnetConflictWithIntendedPolicy`

\* MI subred hace referencia al intervalo de direcciones IP para la subred en el formulario de 10.x.x.x/y. Puede encontrar esta información en el portal de Azure, en Propiedades de la subred.

> [!IMPORTANT]
> Aunque las reglas de seguridad de entrada necesario permiten el tráfico desde _cualquier_ de código fuente en los puertos 9000, 9003, 1438, 1440 y 1452, estos puertos están protegidos por un firewall integrado. Para obtener más información, consulte [determinar la dirección del punto de conexión de administración](sql-database-managed-instance-find-management-endpoint-ip-address.md).

> [!NOTE]
> Si utiliza la replicación transaccional en una instancia administrada y usar cualquier instancia de la base de datos como un publicador o un distribuidor, abra el puerto 445 (salida TCP) en las reglas de seguridad de la subred. Este puerto permitirá el acceso al recurso compartido de archivos de Azure.

### <a name="user-defined-routes"></a>rutas definidas por el usuario

|NOMBRE|Prefijo de dirección|Próximo salto|
|----|--------------|-------|
|subnet_to_vnetlocal|[mi_subnet]|Virtual network|
|mi-0-5-next-hop-internet|0.0.0.0/5|Internet|
|mi-11-8-nexthop-internet|11.0.0.0/8|Internet|
|mi-12-6-nexthop-internet|12.0.0.0/6|Internet|
|mi-128-3-nexthop-internet|128.0.0.0/3|Internet|
|mi-16-4-nexthop-internet|16.0.0.0/4|Internet|
|mi-160-5-nexthop-internet|160.0.0.0/5|Internet|
|mi-168-6-nexthop-internet|168.0.0.0/6|Internet|
|mi-172-12-nexthop-internet|172.0.0.0/12|Internet|
|mi-172-128-9-nexthop-internet|172.128.0.0/9|Internet|
|mi-172-32-11-nexthop-internet|172.32.0.0/11|Internet|
|mi-172-64-10-nexthop-internet|172.64.0.0/10|Internet|
|mi-173-8-nexthop-internet|173.0.0.0/8|Internet|
|mi-174-7-nexthop-internet|174.0.0.0/7|Internet|
|mi-176-4-nexthop-internet|176.0.0.0/4|Internet|
|mi-192-128-11-nexthop-internet|192.128.0.0/11|Internet|
|mi-192-160-13-nexthop-internet|192.160.0.0/13|Internet|
|mi-192-169-16-nexthop-internet|192.169.0.0/16|Internet|
|mi-192-170-15-nexthop-internet|192.170.0.0/15|Internet|
|mi-192-172-14-nexthop-internet|192.172.0.0/14|Internet|
|mi-192-176-12-nexthop-internet|192.176.0.0/12|Internet|
|mi-192-192-10-nexthop-internet|192.192.0.0/10|Internet|
|mi-192-9-nexthop-internet|192.0.0.0/9|Internet|
|mi-193-8-nexthop-internet|193.0.0.0/8|Internet|
|mi-194-7-nexthop-internet|194.0.0.0/7|Internet|
|mi-196-6-nexthop-internet|196.0.0.0/6|Internet|
|mi-200-5-nexthop-internet|200.0.0.0/5|Internet|
|mi-208-4-nexthop-internet|208.0.0.0/4|Internet|
|mi-224-3-nexthop-internet|224.0.0.0/3|Internet|
|mi-32-3-nexthop-internet|32.0.0.0/3|Internet|
|mi-64-2-nexthop-internet|64.0.0.0/2|Internet|
|mi-8-7-nexthop-internet|8.0.0.0/7|Internet|
||||

Además, puede agregar entradas a la tabla de enrutamiento para enrutar el tráfico que tiene intervalos de IP privadas de forma local como un destino a través de la puerta de enlace de red virtual o un dispositivo de red virtual (NVA).

Si la red virtual incluye un DNS personalizado, agregue una entrada para la dirección IP de resolución recursiva de Azure (por ejemplo, 168.63.129.16). Para obtener más información, consulte [configurar un DNS personalizado](sql-database-managed-instance-custom-dns.md). El servidor DNS personalizado debe ser capaz de resolver los nombres de host en estos dominios y sus subdominios: *microsoft.com*, *windows.net*, *windows.com*,  *msocsp.com*, *digicert.com*, *live.com*, *microsoftonline.com*, y *microsoftonline-p.com*.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información general, consulte [avanzadas de seguridad de los datos de SQL Database](sql-database-managed-instance.md).
- Obtenga información sobre cómo [configurar una nueva red virtual Azure](sql-database-managed-instance-create-vnet-subnet.md) o un [red virtual de Azure existente](sql-database-managed-instance-configure-vnet-subnet.md) donde puede implementar las instancias administradas.
- [Calcular el tamaño de la subred](sql-database-managed-instance-determine-size-vnet-subnet.md) donde desea implementar las instancias administradas.
- Obtenga información sobre cómo crear una instancia administrada:
  - Desde [Azure Portal](sql-database-managed-instance-get-started.md).
  - Mediante el uso de [PowerShell](scripts/sql-database-create-configure-managed-instance-powershell.md).
  - Mediante el uso de [una plantilla de Azure Resource Manager](https://azure.microsoft.com/resources/templates/101-sqlmi-new-vnet/).
  - Mediante el uso de [una plantilla de Azure Resource Manager (con el JumpBox, con SSMS incluidos)](https://portal.azure.com/).
