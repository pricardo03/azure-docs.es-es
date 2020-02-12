---
title: 'Preguntas frecuentes: Azure VMware Solutions (AVS)'
description: Preguntas frecuentes sobre Azure VMware Solutions (AVS)
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/15/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: c3808491c84f6c76a51c914aac6ee5e5ee370970
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77025068"
---
# <a name="frequently-asked-questions-about-vmware-solution-by-avs"></a>Preguntas frecuentes sobre VMware Solution by AVS

## <a name="avs-service"></a>Servicio de AVS

**¿Qué es Azure VMware Solutions (AVS)?**

Azure VMware Solutions (AVS) transforma y extiende las cargas de trabajo de VMware a nubes privadas y dedicadas en Azure en cuestión de minutos. AVS se encarga del aprovisionamiento, la administración de la infraestructura y la orquestación de las cargas de trabajo entre el entorno local y Azure. Como las aplicaciones se ejecutan de la misma forma en el entorno local y en Azure, se beneficia de la elasticidad y los servicios de la nube sin la complejidad de rediseñar las aplicaciones. AVS reduce el costo total de propiedad con un modelo de consumo en la nube que proporciona aprovisionamiento a petición, pagos en función del crecimiento y optimización de la capacidad. Para conocer los escenarios, las ventajas y las características, consulte [¿Qué es la solución de VMware en Azure de AVS?](cloudsimple-vmware-solutions-overview.md)

**¿Qué es una nube privada de AVS?**

Una nube privada de AVS es una nube privada y dedicada formada por un proceso de alto rendimiento, almacenamiento y un entorno de red implementado en la infraestructura de Microsoft Azure (hardware y espacio de centro de datos) en ubicaciones de Azure. Una nube privada de AVS proporciona una plataforma como servicio nativa de VMware. En términos de VMware, cada nube privada de AVS contiene exactamente una instancia de vCenter Server. vCenter Server administra varios nodos de ESXi incluidos en uno o varios clústeres de vSphere, junto con el almacenamiento virtual SAN (vSAN) correspondiente. Un servicio de AVS puede contener varias nubes privadas de AVS en la suscripción de Azure. Para más información, consulte [Introducción a la nube privada de AVS](cloudsimple-private-cloud.md).

**¿Dónde está disponible el servicio de AVS?**

AVS está disponible en las regiones Este de EE. UU., Oeste de EE. UU. y Oeste de Europa (y próximamente en otras regiones).

**¿Cómo habilito mi suscripción para AVS?**

Póngase en contacto con su representante de cuenta de Microsoft en [azurevmwaresales@microsoft.com](mailto:azurevmwaresales@microsoft.com) para habilitar la suscripción para el servicio de AVS. En el correo electrónico, especifique el identificador de suscripción para el que quiera habilitar el servicio de AVS. 

**¿Cómo puedo acceder al portal de AVS?**

Puede acceder al portal de AVS desde Azure Portal. Para más información, consulte [Acceso al portal de VMware Solutions (AVS) desde Azure Portal](access-cloudsimple-portal.md).

**¿Cómo puedo aumentar la capacidad de una nube privada de AVS?**

Para aumentar la capacidad, adquiera nodos adicionales desde Azure Portal y úselos para expandir la nube privada de AVS desde el portal de AVS. Puede agregar nodos adicionales a un clúster de vSphere existente o a uno nuevo. Para más información, consulte [Expandir una nube privada de AVS](expand-private-cloud.md).

**¿Qué ocurre con mi nube privada de AVS durante el mantenimiento?**

AVS proporciona una notificación varios días antes del intervalo de mantenimiento programado. El mantenimiento sin disrupciones para garantizar la disponibilidad de la nube privada de AVS. El mantenimiento puede ser de uno de los tipos siguientes:

* **Infraestructura de AVS**. La infraestructura de AVS está diseñada para tener alta disponibilidad. Durante este tipo de mantenimiento, los componentes redundantes se actualizan de uno en uno para evitar cualquier interrupción del servicio. Tendrá acceso continuado a la instancia de vCenter de la nube privada de AVS, a todas las máquinas virtuales, conexión a Internet desde la nube privada de AVS y conexiones al entorno local o a Azure.
* **Portal de AVS**. Durante este tipo de mantenimiento, algunas características del portal de AVS pueden estar deshabilitadas o inaccesibles. La notificación anterior al mantenimiento incluye detalles sobre las limitaciones de las características mientras este se realiza.

## <a name="connectivity"></a>Conectividad

**¿Cuáles son mis opciones de conectividad en la red de la región de AVS?**

AVS proporciona las siguientes opciones de conectividad para conectarse a la red de la región de AVS. Se pueden usar varias opciones al mismo tiempo.

* **Conexión de ExpressRoute desde el centro de datos local a la red de la región de AVS**. Es una conexión privada segura de alta velocidad y latencia baja que usa Global Reach para conectar el circuito ExpressRoute local con el circuito ExpressRoute de AVS. Para instrucciones para establecer la conexión, consulte [Conexión desde el entorno local a AVS mediante ExpressRoute](on-premises-connection.md).
* **Conexión de ExpressRoute desde la red virtual de Azure a la red de la región de AVS**. Es una conexión privada segura de alta velocidad y latencia baja que usa puertas de enlace de red virtual para conectar la red virtual de Azure con el circuito ExpressRoute de AVS. Para obtener instrucciones para establecer la conexión, consulte [Conexión del entorno de nube privada de AVS a la red virtual de Azure mediante ExpressRoute](azure-expressroute-connection.md).
* **Conexión VPN de sitio a sitio desde el centro de datos local a la red de la región de AVS**. Es una red virtual privada segura desde el dispositivo VPN local hasta la región de la nube privada de AVS. Para más información, consulte [Configuración de puertas de enlace de VPN en la red de AVS](vpn-gateway.md).

**¿Cómo me conecto a una nube privada de AVS?**

Puede ver los detalles de la nube privada de AVS en el portal de AVS. Para conectarse a la instancia de vCenter correspondiente a la nube privada de AVS, primero asegúrese de que se establece una conexión de red mediante VPN de sitio a sitio, VPN de punto a sitio o ExpressRoute. A continuación, inicie el portal de AVS desde Azure Portal y haga clic en **Iniciar el cliente de vSphere** en la página principal o en la página de detalles de la nube privada de AVS.

**¿Qué ventaja tienen los circuitos ExpressRoute?**

Un circuito Azure ExpressRoute es una conexión segura de alta velocidad y baja latencia. AVS proporciona un circuito ExpressRoute dedicado por cada región y cliente. Con este circuito, puede establecer una conexión segura desde el entorno local o la suscripción de Azure.

**¿Cuáles son los costos de red para conectarse a AVS? ¿Se aplican cargos de salida entre AVS y Azure o entre regiones?**

No hay ningún cargo de AVS por la salida de red. Se aplican las tarifas estándar de Azure a todo el tráfico de salida desde la red virtual o desde el circuito ExpressRoute local.

## <a name="networking"></a>Redes

**¿Qué características de red están disponibles para la nube privada de AVS?**

Puede aprovisionar redes VLAN (y sus subredes) y tablas de firewall, y asignar direcciones IP públicas que a su vez se asignen a una máquina virtual que se ejecute en la nube privada de AVS. Para más información sobre las características de redes, consulte [Introducción a las redes VLAN y las subredes](cloudsimple-vlans-subnets.md), [Firewall tables overview](cloudsimple-firewall-tables.md) (Introducción a las tablas de firewall) e [Introducción a la dirección IP pública](cloudsimple-public-ip-address.md).

**¿Cómo configuro otras subredes para las aplicaciones en la nube privada de AVS?**

Puede crear redes VLAN en la nube privada de AVS desde el portal de AVS. Después de crear la VLAN, puede crear un grupo de puertos distribuido en la instancia de vCenter de la nube privada de AVS mediante la red VLAN y crear máquinas virtuales conectadas al grupo de puertos distribuido. Puede habilitar tablas de firewall para la subred o la red VLAN y definir reglas de firewall para proteger el tráfico.

**¿Qué configuración de firewall está disponible para las nubes privadas de AVS?**

Puede configurar reglas para el tráfico norte-sur y este-oeste. Las reglas se definen en una tabla de firewall. La tabla de firewall se puede conectar a redes VLAN de la nube privada de AVS. Para más información, consulte [Configuración de tablas de firewall y reglas para las nubes privadas de AVS](firewall.md).

**¿Puedo asignar direcciones IP públicas a las VM en mi entorno de nube privada de AVS?**

En el portal de AVS, puede asignar una nueva dirección IP pública y asociarla a una dirección IP privada de una máquina virtual o un dispositivo. En el portal también puede crear reglas de firewall o aplicar reglas de firewall existentes para permitir el tráfico desde puertos y direcciones IP específicos. Para más información, consulte [Asignación de direcciones IP públicas para un entorno de nube privada de AVS](public-ips.md).

## <a name="security"></a>Seguridad

**¿Cuáles son mis opciones de seguridad en AVS?**

AVS proporciona las siguientes características de seguridad para proteger el entorno de nube privada de AVS:

* **Cifrado de datos en reposo**. Puede cifrar los datos en reposo que residen en el almacenamiento vSAN en la nube privada de AVS. vSAN admite los servidores de administración de claves externas, que se puede implementar en el entorno local o la red virtual de Azure. Para más información, consulte [Configuración de cifrado de vSAN para la nube privada de AVS](vsan-encryption.md).
* **Seguridad de red**. Controle el flujo de tráfico de red con reglas de firewall que se apliquen entre la nube privada de AVS e Internet, la nube privada de AVS y el entorno local, o dentro de subredes de la nube privada de AVS.
* **Conexión privada y segura**. Se establece una conexión privada y segura entre la red local y la suscripción de Azure.

## <a name="compute"></a>Proceso

**¿Qué tipos de hosts están disponibles?**

AVS ofrece estos tipos de host:

* **Nodo CS28:** CPU: 2x 2,2 GHz, 28 núcleos en total, 48 HT.  RAM: 256 GB.  Almacenamiento: Caché NVMe de 1600 GB, 5760 GB de datos (All-Flash). Red: NIC de 4x25 Gbe
* **Nodo CS36:** CPU 2x 2,3 GHz, 36 núcleos en total, 72 HT.  RAM: 512 GB.  Almacenamiento: Caché NVMe de 3200 GB, 11520 GB de datos (All-Flash).  Red: NIC de 4x25 Gbe
* **Nodo CS36m:** CPU 2x 2,3 GHz, 36 núcleos en total, 72 HT.  RAM: 576 GB.  Almacenamiento: Caché NVMe de 3200 GB, 13360 GB de datos (All-Flash).  Red: NIC de 4x25 Gbe

**¿Cómo se controlan los errores de hardware?**

Toda la infraestructura de AVS se supervisa de forma continua mediante la plataforma AVS y los equipos de operaciones de servicio. Si se detecta un error de hardware, se agrega un nodo nuevo a la nube privada de AVS y se elimina el nodo con el error.

## <a name="storage"></a>Storage

**¿Qué tipo de almacenamiento se admite en una nube privada de AVS?**

AVS ofrece almacenamiento vSAN de VMware All-Flash con cada nube privada de AVS. Cada instancia de vSphere se crea con su propio almacén de datos vSAN. Para más información, consulte [Componentes de VMware para la nube privada de AVS: almacenamiento de vSAN](vmware-components.md#vsan-storage).

**¿Se admite el cifrado de datos?**
Sí. Puede configurar el almacenamiento de vSAN en la nube privada de AVS para usar un servidor de administración de claves (KMS) que se implemente en el entorno local o en Azure para cifrar los datos almacenados en vSAN.

**¿Cómo se controlan los discos con errores?**

AVS supervisa de forma continua todos los componentes de hardware de la nube privada de AVS. Si se detecta un error de disco o se identifica un disco con errores a partir de la heurística, se agrega automáticamente un nodo nuevo a la nube privada de AVS. El nodo con el disco con errores se quita de la nube privada de AVS.

## <a name="vmware"></a>VMware

**¿Cómo se realiza la carga y la migración a gran escala de aplicaciones y datos desde el entorno local?**

AVS proporciona una solución VMware vSphere nativa. Todas las herramientas de VMware para la migración de datos de forma masiva se pueden utilizar con la nube privada de AVS. Las opciones incluyen:

* VMware HCX para la migración masiva de datos.
* Migración en frío de datos mediante Storage vMotion desde el entorno local a AVS.

**¿Puedo instalar cualquier herramienta de VMware?**

AVS proporciona una solución VMware vSphere nativa. Todas las herramientas de VMware que se usan para administrar el entorno de vSphere local se pueden usar en AVS. AVS admite un modelo BYOL (traiga su propia licencia) para la instalación de herramientas de VMware.

**¿Cómo se administran las actualizaciones?**

AVS administra y actualiza todos los componentes de infraestructura de la nube privada de AVS de forma transparente y sin interrupciones. Las actualizaciones y revisiones de seguridad publicadas por VMware o los proveedores de infraestructura se programan para la actualización en cuanto cumplan las condiciones de AVS.

AVS no realiza actualizaciones de las aplicaciones instaladas en la nube privada de AVS.

## <a name="azure-integration"></a>Integración de Azure

**¿Qué servicios de Azure se admiten?**

AVS ofrece conexión de Azure ExpressRoute a la suscripción en Azure. Todos los servicios que se ejecutan en la suscripción pueden conectarse a la nube privada de AVS. Algunos ejemplos son:

* **Azure Active Directory** como origen de identidades para la instancia de vCenter de AVS.
* **Azure Storage** para almacenar copias de seguridad, imágenes y otros datos de la nube privada de AVS.
* **Aplicaciones híbridas** con una arquitectura de aplicación que abarque las nubes públicas y privadas de AVS. Por ejemplo, puede crear servidores web en Azure que accedan a servidores de aplicaciones y base de datos en la nube privada de AVS.
* **Azure Monitor** y **Azure Security Center** para las cargas de trabajo que se ejecutan en VMware admiten el registro, las métricas de rendimiento y la administración de seguridad.

**¿Cómo se asignan los inquilinos de VMware a Azure?**

AVS proporciona la capacidad única de administrar las máquinas virtuales de VMware en la nube privada de AVS desde Azure Portal. El administrador global puede asignar a la suscripción un grupo de recursos de vCenter configurado con las restricciones de recursos que quiera. 

**¿Qué ventajas de licencia obtengo con Azure?**

Con AVS puede aprovechar la ventaja del uso híbrido de Azure y ahorrar hasta un 90 % en las licencias. Esta ventaja conserva su inversión en las licencias de Microsoft y reduce el costo total de propiedad de otras soluciones en la nube. También obtiene actualizaciones de seguridad ampliadas para Windows Server 2008 y Microsoft SQL Server 2008. El modelo BYOL (traiga su propia licencia) ayuda a mantener los costos reducidos para las aplicaciones comunes como Veeam y Zerto. 
