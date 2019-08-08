---
title: 'Preguntas más frecuentes: Solución de VMware de CloudSimple'
description: Preguntas frecuentes sobre la solución de Azure VMware de CloudSimple
author: sharaths-cs
ms.author: b-shsury
ms.date: 05/24/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 80380a1c33927029e000e59a5834f297340f5be3
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2019
ms.locfileid: "68816233"
---
# <a name="frequently-asked-questions-about-vmware-solution-by-cloudsimple"></a>Preguntas frecuentes sobre la solución de VMware de CloudSimple

Preguntas más frecuentes y respuestas sobre la Solución de Azure VMware de CloudSimple que le ayudarán a comprender el servicio y cómo usarlo. Las preguntas y respuestas se organizan en las categorías siguientes:

* Servicio CloudSimple
* Conectividad
* Redes
* Seguridad
* Proceso
* Storage
* VMware
* Integración de Azure
 
## <a name="cloudsimple-service"></a>Servicio CloudSimple

**¿Qué es Solución de Azure VMware de CloudSimple?**

Solución de Azure VMware de CloudSimple transforma y extiende las cargas de trabajo de VMware a nubes privadas y dedicadas en Azure en cuestión de minutos. La solución aprovisiona, administra la infraestructura y organiza las cargas de trabajo entre en el entorno local y Azure. Como las aplicaciones se ejecutan de la misma forma en el entorno local y en Azure, se beneficia de la elasticidad y los servicios de la nube sin la complejidad de rediseñar las aplicaciones. CloudSimple reduce el costo total de propiedad con un modelo de consumo en la nube que proporciona aprovisionamiento a petición, pagos en función del crecimiento y optimización de la capacidad. Para obtener escenarios, ventajas y características, vea [¿Qué es la Solución de VMware en Azure de CloudSimple?](cloudsimple-vmware-solutions-overview.md)

**¿Qué es una nube privada de CloudSimple?**

Una nube privada y dedicada que consta de un proceso de alto rendimiento, almacenamiento y un entorno de red implementado en la infraestructura de Microsoft Azure (hardware y espacio de centro de datos) se aprovisiona en las ubicaciones de Azure. La nube privada proporciona una plataforma nativa de VMware como un servicio. En términos de VMware, cada nube privada contiene exactamente una instancia de vCenter Server. vCenter Server administra varios nodos de ESXi incluidos en uno o varios clústeres de vSphere, junto con el almacenamiento vSAN correspondiente. Un servicio CloudSimple puede contener varias nubes privadas en la suscripción de Azure. Para más información sobre las nubes privadas, vea [Información general de las nubes privadas](cloudsimple-private-cloud.md).

**¿Dónde está disponible el servicio CloudSimple?**

CloudSimple está disponible en Este de EE. UU. y Oeste de EE. UU.

**¿Cómo habilito mi suscripción para CloudSimple?**

Póngase en contacto con su representante de cuenta de Microsoft en [azurevmwaresales@microsoft.com](mailto:azurevmwaresales@microsoft.com) para habilitar la suscripción para el servicio CloudSimple. En el correo electrónico, especifique el identificador de suscripción para el que quiera habilitar el servicio CloudSimple. 

**¿Cómo se accede al portal de CloudSimple?**

Puede acceder al portal de CloudSimple desde Azure Portal. Para obtener información sobre cómo acceder al portal CloudSimple, vea [Acceso al portal de la solución de VMware de CloudSimple desde Azure Portal](https://docs.azure.cloudsimple.com/access-cloudsimple-portal).

**¿Cómo se puede aumentar la capacidad de una nube privada?**

Se aprovisionan nodos desde Azure Portal y se expande la nube privada desde el portal de CloudSimple. Puede expandir la nube privada mediante la adición de nodos a un clúster de vSphere existente o mediante la creación de uno. Para obtener información sobre el procedimiento, vea [Expansión de una nube privada de CloudSimple](https://docs.azure.cloudsimple.com/expand-private-cloud).

**¿Qué ocurre con mi nube privada durante el mantenimiento?**

CloudSimple proporciona notificaciones periódicas días antes del mantenimiento programado. El mantenimiento se realiza de forma no disruptiva para garantizar la disponibilidad de la nube privada. El mantenimiento puede ser de uno de los tipos siguientes:

- **Infraestructura de CloudSimple**: la infraestructura de CloudSimple está diseñada para tener alta disponibilidad. Durante el mantenimiento, se garantizan la conectividad y la disponibilidad de la nube privada mediante la actualización individual de los componentes redundantes sin ningún impacto. Tendrá acceso a la instancia de vCenter de la nube privada, a todas las máquinas virtuales, conexión a Internet desde la nube privada y conexiones al entorno local o Azure.
- **Portal de CloudSimple**: durante el mantenimiento, es posible que algunas características del portal de CloudSimple no sean accesibles o estén deshabilitadas. La notificación de mantenimiento incluye información sobre lo que se puede realizar en el portal.

## <a name="connectivity"></a>Conectividad

**¿Cuáles son mis opciones de conectividad en la red de mi región de CloudSimple?**

CloudSimple proporciona tres opciones de conectividad diferentes para conectarse a la red de la región de CloudSimple. Las tres opciones se pueden usar de forma conjunta:

- Conexión de Azure ExpressRoute desde el centro de datos local a la red de la región de CloudSimple: una conexión privada segura de alta velocidad y latencia baja que conecta el circuito ExpressRoute local con el circuito ExpressRoute de CloudSimple mediante Global Reach. Para establecer la conexión, vea [Conexión desde el entorno local a CloudSimple mediante ExpressRoute](https://docs.azure.cloudsimple.com/on-premises-connection).
- Conexión de ExpressRoute desde la red virtual de Azure a la red de la región de CloudSimple: una conexión privada segura de alta velocidad y latencia baja que conecta la red virtual de Azure con el circuito ExpressRoute de CloudSimple mediante puertas de enlace de red virtual. Para establecer la conexión, vea [Conexión del entorno de nube privada de CloudSimple a la red virtual de Azure mediante ExpressRoute](https://docs.azure.cloudsimple.com/azure-expressroute-connection).
- Conexión VPN de sitio a sitio desde el centro de datos local a la red de la región de CloudSimple: una red virtual privada segura desde el dispositivo VPN local a la región de la nube privada de CloudSimple. Para establecer la conexión, vea [Establecimiento de una conexión VPN entre la red local y la nube privada de CloudSimple](https://docs.azure.cloudsimple.com/set-up-vpn).

**¿Cómo me conecto a una nube privada?**

Puede ver los detalles de la nube privada en el portal de CloudSimple. Para conectarse a la instancia de vCenter que se corresponde a la nube privada, asegúrese de que se establece una conexión de red mediante sitio a sitio, punto a sitio o ExpressRoute. Después, inicie el portal de CloudSimple desde Azure Portal. Seleccione **Launch vSphere Client** (Iniciar cliente vSphere) en la página principal o en la página de detalles de la nube privada.

**¿Qué ventaja tiene un circuito ExpressRoute?**

Un circuito Azure ExpressRoute proporciona una conexión segura de alta velocidad y baja latencia. CloudSimple proporciona un circuito ExpressRoute dedicado por cada región y cliente. Con este circuito, puede establecer una conexión segura desde el entorno local y la suscripción de Azure.

**¿Cuáles son los costos de red para conectarse a y desde CloudSimple? ¿Existen cargos de salida hacia y desde CloudSimple a Azure? ¿Existen cargos de salida entre regiones?**

No hay ningún cargo por la salida de red. Se aplican las tarifas estándar de Azure a todo el tráfico de salida desde la red virtual o desde un circuito ExpressRoute local.

## <a name="networking"></a>Redes

**¿Qué características de red están disponibles para la nube privada?**

Puede aprovisionar VLAN y sus subredes, y tablas de firewall. Puede asignar direcciones IP públicas y asignar a una máquina virtual que se ejecute en la nube privada. Para más información, vea [Información general de las VLAN y subredes](cloudsimple-vlans-subnets.md), [Información general de tablas de firewall](cloudsimple-firewall-tables.md) e [Introducción a la dirección IP pública](cloudsimple-public-ip-address.md).

**¿Cómo configuro otras subredes para las aplicaciones en la nube privada?**

Puede crear redes VLAN en la nube privada desde el portal de CloudSimple. Después de crear la VLAN, puede crear un grupo de puertos distribuido en la instancia de vCenter de la nube privada mediante la VLAN y crear máquinas virtuales conectadas al grupo de puertos distribuido. Puede habilitar una tabla de firewall para la subred o VLAN, y definir reglas de firewall para proteger el tráfico de red.

**¿Qué configuración de firewall está disponible para las nubes privadas?**

Puede configurar reglas para el tráfico norte-sur y este-oeste. Las reglas se definen en una tabla de firewall. La tabla de firewall se puede conectar a redes VLAN en la nube privada. Para el procedimiento de instalación, vea [Configuración de tablas de firewall y reglas para las nubes privadas](https://docs.azure.cloudsimple.com/firewall).

**¿Puedo asignar direcciones IP públicas para las máquinas virtuales en el entorno de nube privada?**

En el portal de CloudSimple, puede asignar con facilidad una nueva dirección IP pública y asociarla a una dirección IP privada de la máquina virtual o un dispositivo. En el portal también puede crear reglas de firewall o aplicar reglas de firewall existentes para permitir el tráfico desde puertos y conjuntos de direcciones IP específicos. Para el procedimiento de instalación, vea [Asignación de direcciones IP públicas para un entorno de nube privada](https://docs.azure.cloudsimple.com/public-ips).

## <a name="security"></a>Seguridad

**¿Cuáles son mis opciones de seguridad en CloudSimple?**

Una nube privada de CloudSimple proporciona las características de seguridad siguientes para proteger el entorno de nube privada:

- **Cifrado de datos en reposo:** puede cifrar los datos en reposo que residen en el almacenamiento vSAN en la nube privada. vSAN admite un servidor de administración de claves externas, que se puede implementar en el entorno local o la red virtual de Azure. Para más información, vea [Configuración de cifrado de vSAN para la nube privada de CloudSimple](https://docs.azure.cloudsimple.com/vsan-encryption).
- **Seguridad de redes:** controle el flujo de tráfico desde y hacia la nube privada desde Internet, en el entorno local y en las subredes de la nube privada mediante reglas de firewall.
- **Conexión privada y segura:** conexión privada y segura entre la red local y la suscripción de Azure.

## <a name="compute"></a>Proceso

**¿Qué tipos de hosts están disponibles?**

CloudSimple ofrece dos tipos de host:

* **Nodo CS28**: CPU: 2x 2,2 GHz, 28 núcleos en total, 48 HT. RAM: 256 GB. Almacenamiento: Caché NVMe de 1600 GB, 5760 GB de datos (All-Flash). Red: NIC de 2x25Gbe.
* **Nodo CS36**: CPU 2x 2,3 GHz, 36 núcleos en total, 72 HT. RAM: 512 GB. Almacenamiento: Caché NVMe de 3200 GB, 11.520 GB de datos (All-Flash). Red: NIC de 2x25Gbe.

**¿Cómo se controlan los errores de hardware?**

Toda la infraestructura de CloudSimple se supervisa de forma continua mediante la plataforma CloudSimple y sus equipos de operaciones de servicio. Si se detecta un error de hardware, se agrega un nodo nuevo a la nube privada. El nodo con error se quita para garantizar la alta disponibilidad de la nube privada.

## <a name="storage"></a>Storage

**¿Qué tipo de almacenamiento se admite en una nube privada?**

CloudSimple ofrece **almacenamiento vSAN de VMware All-flash** con cada nube privada. Cada instancia de vSphere se crea con su propio almacén de datos vSAN. Para más información, vea [Componentes de VMware de nube privada: almacenamiento vSAN](https://docs.azure.cloudsimple.com/vmware-components/#vsan-storage).

**¿Se admite el cifrado de datos?**
Sí. Puede configurar el almacenamiento de vSAN en la nube privada para usar un servidor de administración de claves (KMS), que se implementa en el entorno local o en Azure para cifrar los datos almacenados en vSAN.

**¿Cómo se controlan los discos con errores?**

CloudSimple supervisa de forma continua todos los componentes de hardware de la nube privada. Si se detecta un error de disco o se identifica un disco con errores a partir de la heurística, se agrega automáticamente un nodo nuevo a la nube privada. El nodo con un disco con errores se quita de la nube privada.

## <a name="vmware"></a>VMware

**¿Cómo se realiza la carga y la migración a gran escala de aplicaciones y datos desde el entorno local?**

CloudSimple proporciona una solución VMware vSphere nativa. Cualquier herramienta que se use para la migración de datos de forma masiva se puede utilizar con una nube privada de CloudSimple. Algunas de las opciones disponibles son las siguientes:

- VMware HCX para la migración masiva de datos.
- Migración en frío de datos mediante Storage vMotion desde el entorno local a CloudSimple.

**¿Puedo instalar cualquier herramienta de VMware?**

CloudSimple proporciona una solución VMware vSphere nativa. Cualquier herramienta que se usa para administrar un entorno de vSphere local se puede usar en CloudSimple. CloudSimple admite un modelo traiga su propia licencia (BYOL) para la instalación de herramientas de VMware.

**¿Cómo se administran las actualizaciones?**

CloudSimple administra y actualiza todos los componentes de infraestructura de la nube privada de forma transparente y sin interrupciones. Cualquier actualización o revisión de seguridad publicada por VMware o los proveedores de infraestructura se programa para la actualización tan pronto como sea calificada por CloudSimple.

CloudSimple no realiza actualizaciones de las aplicaciones instaladas en la nube privada.

## <a name="azure-integration"></a>Integración de Azure

**¿Qué servicios de Azure se admiten?**

CloudSimple ofrece conexión de Azure ExpressRoute a la suscripción en Azure. Todos los servicios que se ejecutan en la suscripción tienen conectividad de red a la nube privada y se pueden conectar a ella. Ejemplos:

- **Azure Active Directory**: use Azure Active Directory como un origen de identidades para la instancia de vCenter de CloudSimple.
- **Azure Storage**: use Storage para almacenar copias de seguridad, imágenes y otros datos de la nube privada.
- **Aplicaciones híbridas**: puede crear una arquitectura de aplicaciones que abarque nubes públicas y privadas. Por ejemplo, puede crear servidores web en Azure que accedan a servidores de aplicaciones y base de datos en una nube privada de CloudSimple.
- **Azure Monitor** y **Azure Security Center**: la carga de trabajo que se ejecuta en VMware puede usar Monitor y Security Center para el registro, las métricas de rendimiento y la administración de seguridad.

**¿Cómo se asignan los inquilinos de VMware a Azure?**

CloudSimple proporciona la capacidad única de administrar las máquinas virtuales de VMware en una nube privada desde Azure Portal. El administrador global puede asignar a la suscripción un grupo de recursos de vCenter configurado con las restricciones de recursos que quiera. 

**¿Qué ventajas de licencia obtengo con Azure?**

Con CloudSimple, puede aprovechar la ventaja híbrida de Azure y ahorrar hasta un 90 por ciento en las licencias para conservar la inversión en licencias de Microsoft y reducir el costo total de propiedad en comparación con otras nubes. También obtiene actualizaciones de seguridad ampliadas para Windows Server 2008 y Microsoft SQL Server 2008. Los costos se mantienen bajos mediante BYOL para la nube para aplicaciones comunes, como Veeam, Zerto y otras. 
