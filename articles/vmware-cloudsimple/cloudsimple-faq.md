---
title: 'Preguntas más frecuentes: solución de VMware CloudSimple'
description: Solución de VMware de Azure por CloudSimple preguntas más frecuentes
author: sharaths-cs
ms.author: b-shsury
ms.date: 05/24/19
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: e727398e1b7bfa406166574ab40320c68dac5709
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66358534"
---
# <a name="frequently-asked-questions-about-vmware-solution-by-cloudsimple"></a>Preguntas más frecuentes acerca de la solución VMware CloudSimple

Preguntas más frecuentes y respuestas acerca de la solución VMware CloudSimple que le ayudarán a comprenden el servicio y cómo usarlo.  Las preguntas y respuestas se organizan en las siguientes categorías.

* CloudSimple Service
* Conectividad
* Redes
* Seguridad
* Proceso
* Almacenamiento
* VMware
* Azure Integration
  
## <a name="cloudsimple-service"></a>CloudSimple Service

**¿Qué es la solución VMware CloudSimple?**

**Solución de VMware CloudSimple** transforma y extiende las cargas de trabajo de VMware a nubes privadas, dedicadas en Azure en cuestión de minutos. Se encargará de aprovisionar, administrar la infraestructura y orquestar las cargas de trabajo entre en el entorno local y Azure. Dado que las aplicaciones ejecutan exactamente la misma red local y en Azure, beneficiarse de la elasticidad y servicios de la nube sin la complejidad de la necesidad de rediseñar las aplicaciones. CloudSimple reduce el costo total de propiedad con un modelo de consumo en la nube que proporciona el aprovisionamiento, pague a medida crece bajo demanda y optimización de capacidad.  Consulte [¿qué es la solución de VMware en Azure por CloudSimple](cloudsimple-vmware-solutions-overview.md) para escenarios, ventajas y características.

**¿Qué es una nube privada CloudSimple?**

Aprovisionar una nube privada y dedicada que consta de proceso de alto rendimiento, almacenamiento y de red de entorno que está implementado en la infraestructura de Microsoft Azure (espacio de hardware y el centro de datos) en las ubicaciones de Azure.  Nube privada proporciona una plataforma de VMware nativa 'como servicio'. En términos de VMware, cada nube privada contiene exactamente una instancia del servidor vCenter. VCenter Server administra varios nodos de ESXi que contiene uno o varios clústeres de vSphere, junto con el almacenamiento de SAN Virtual (vSAN) correspondiente. Un CloudSimple Service puede contener varias nubes privadas en su suscripción de Azure.  Para obtener más información acerca de las nubes privadas, consulte [información general de la nube privada](cloudsimple-private-cloud.md).

**¿Dónde está disponible service CloudSimple?**

CloudSimple está disponible en las regiones este de EE.UU. y oeste de Estados Unidos.

**¿Cómo habilito CloudSimple mi suscripción?**

Puede ponerse en contacto con su representante de cuenta de Microsoft en [ azurevmwaresales@microsoft.com ](mailto:azurevmwaresales@microsoft.com) para habilitar la suscripción para el servicio CloudSimple. Especifique el identificador de suscripción en el correo electrónico que se desea servicio CloudSimple habilitado.  

**¿Cómo se puede acceder al portal de CloudSimple?**

Tener acceso a CloudSimple portal de Azure portal.  Consulte [acceso a la solución de VMware CloudSimple portal de Azure portal](https://docs.azure.cloudsimple.com/access-cloudsimple-portal) artículo para obtener más información sobre cómo acceder a portal CloudSimple. 

**¿Cómo se puede aumentar la capacidad para una nube privada?**

Adquirir los nodos de Azure portal y expanda su propia nube privada desde el portal de CloudSimple.  Puede expandir su propia nube privada mediante la adición de nodos adicionales al clúster de vSphere existente o creando nuevo clúster de vSphere.  Consulte [expandir una nube privada CloudSimple](https://docs.azure.cloudsimple.com/expand-private-cloud) artículo para conocer el procedimiento.

**¿Qué ocurre con mi nube privada durante el mantenimiento?**

CloudSimple proporciona notificaciones periódicas días antes de mantenimiento programado.  Mantenimiento se realiza de forma que no son perjudiciales para garantizar la disponibilidad de su nube privada.  Puede ser el mantenimiento de los siguientes tipos:

1. **Infraestructura CloudSimple:**  Infraestructura CloudSimple está diseñado para ser altamente disponible.  Durante el mantenimiento, conectividad y la disponibilidad de su propia nube privada se garantiza mediante la actualización de componentes redundantes de uno a la vez sin que ello influya.  Tendrá acceso a vCenter de la nube privada, todas las máquinas virtuales, conexión a internet desde su propia nube privada y conexiones en un entorno local o en Azure.
2. **CloudSimple Portal:** Durante el mantenimiento, algunas características en el portal de CloudSimple no pueden ser accesibles o deshabilitado.  Notificación de mantenimiento incluirá los detalles de lo que puede realizarse en el portal.

## <a name="connectivity"></a>Conectividad

**¿Cuáles son Mis opciones de conectividad a red de área CloudSimple?**

CloudSimple proporciona tres opciones de conectividad diferentes para conectarse a la red de área CloudSimple.  Las tres se pueden usar conjuntamente.

1. Conexión de ExpressRoute del centro de datos local a la región CloudSimple red - conexión privada segura una latencia baja a alta velocidad el circuito de ExpressRoute en el entorno local con el circuito de CloudSimple ExpressRoute con alcance Global de protocolo de puente. Consulte [conectarse localmente a CloudSimple mediante ExpressRoute](https://docs.azure.cloudsimple.com/on-premises-connection) artículo para configurar la conexión.
2. Conexión de ExpressRoute desde la red virtual de Azure a la red de área CloudSimple - conexión privada segura una latencia baja a alta velocidad puentes de la red virtual en Azure con el circuito de CloudSimple ExpressRoute mediante puertas de enlace de red virtual.  Consulte [conectar el entorno de nube privada CloudSimple a la red virtual de Azure mediante ExpressRoute](https://docs.azure.cloudsimple.com/azure-expressroute-connection) artículo para configurar la conexión.
3. Conexión de VPN de sitio a sitio desde su centro de datos local a la red de área CloudSimple - proteger la red privada virtual del dispositivo VPN local a la región de la nube privada CloudSimple.  Consulte [configurar la conexión VPN entre su red local y nube privada CloudSimple] el artículo para configurar la conexión VPN.

**¿Cómo me conecto a una nube privada?**

Puede ver los detalles de su propia nube privada en el CloudSimple Portal. Para conectarse a vCenter correspondiente a su propia nube privada, asegúrese de conexión de red está establecida mediante sitio a sitio, punto a sitio o ExpressRoute. A continuación, inicie el portal de CloudSimple desde Azure portal y haga clic en *iniciar cliente vSphere* en la página principal o en la página de detalles de la nube privada.

**¿Qué es la ventaja del circuito de ExpressRoute?**

Circuito de ExpressRoute de Azure proporciona la conexión segura de alta velocidad baja latencia.  CloudSimple proporciona un circuito dedicado de ExpressRoute por región por cada cliente.  Con este circuito, puede establecer una conexión segura de su suscripción de Azure o locales.

**¿Cuáles son los costos de red para conectarse desde CloudSimple. ¿Los cargos de salida hacia y desde CloudSimple a Azure? ¿En las regiones?**

No hay ningún cargo por la salida de cualquier red.  Las tarifas estándares de Azure se aplican a cualquier tráfico de salida de la red virtual o de circuito de ExpressRoute en el entorno local.

## <a name="networking"></a>Redes

**¿Qué características de red están disponibles para la nube privada?**

Puede aprovisionar las VLAN (y sus subredes), tablas, de firewall y asignar direcciones IP públicas y se asignan a una máquina virtual que se ejecuta en su propia nube privada.  Para obtener más información, consulte [información general de VLAN y subredes](cloudsimple-vlans-subnets.md), [información general de tablas de Firewall](cloudsimple-firewall-tables.md), y [información general de dirección IP pública](cloudsimple-public-ip-address.md) artículos.

**¿Cómo configuro subredes diferentes para mis aplicaciones en la nube privada?**

Puede crear redes VLAN en su propia nube privada desde el Portal de CloudSimple.  Una vez creada la VLAN, puede crear un grupo de puertos distribuido en el vCenter de nube privada mediante la VLAN y crear máquinas virtuales conectadas al grupo de puertos distribuida.  Puede habilitar la tabla de firewall para la subred/VLAN y definir las reglas de firewall para proteger el tráfico de red.

**¿Qué configuración de firewall está disponibles para mi nubes privadas?**

Puede configurar reglas para el tráfico norte sur y este oeste.  Las reglas se definen en una tabla de firewall.  La tabla de firewall puede asociarse a VLAN(s) en su propia nube privada.  Consulte [configurar tablas de firewall y reglas para nubes privadas](https://docs.azure.cloudsimple.com/firewall) artículo para el procedimiento de instalación.

**¿Puedo asignar direcciones IP públicas para las máquinas virtuales en mi entorno de nube privada?**

En el portal CloudSimple, fácilmente puede asignar una dirección IP pública nueva y asociarla a una dirección IP privada de la máquina virtual o un dispositivo.  También puede crear nuevas reglas de firewall o aplicar reglas de firewall existente para permitir el tráfico de puertos específicos o específicas del conjunto de direcciones IP en el Portal. Consulte [asignar direcciones IP públicas para el entorno de nube privada](https://docs.azure.cloudsimple.com/public-ips) para el procedimiento de instalación.

## <a name="security"></a>Seguridad

**¿Cuáles son Mis opciones de seguridad en CloudSimple?**

Nube privada CloudSimple proporciona las siguientes características de seguridad para proteger su entorno de nube privada:

1. **Datos en cifrado en reposo**: Puede cifrar los datos en reposo que residen en el almacenamiento de la vSAN en su propia nube privada. vSAN es compatible con el servidor de administración de claves externas, que se puede implementar en su entorno de red virtual local o en Azure.  Consulte [configurar cifrado vSAN para su propia nube privada CloudSimple](https://docs.azure.cloudsimple.com/vsan-encryption) para obtener más detalles.
2. **Seguridad de red**: Control flujo de tráfico desde y hacia la nube privada desde Internet, de forma local y en las subredes de su nube privada mediante reglas de firewall.
3. **Conexión privada y segura**: Conexión privada segura entre su red local y la suscripción de Azure.

## <a name="compute"></a>Proceso

**¿Qué tipo de hosts están disponibles?**

CloudSimple ofrece dos tipos de host:

* **Nodo CS28:** CPU:2x 2.2 GHz, Total 28 Cores, 48 HT.  RAM: 256 GB.  Storage: Caché de 1600 GB NVMe, 5760 GB de datos (memoria Flash). Red: 2x25Gbe NIC
* **Nodo CS36:** CPU 2 x 2.3 GHz, 36 Total de núcleos, 72 HT.  RAM: 512 GB.  Storage: 3200 GB NVMe caché 11.520 GB de datos (memoria Flash).  Red: 2x25Gbe NIC

**¿Cómo se controlan los errores de hardware?**

Toda la infraestructura de CloudSimple se supervisa de forma continua la plataforma CloudSimple y nuestros equipos de operaciones de servicio.  Si se detecta un error de hardware, se agrega un nuevo nodo en la nube privada y se quita un nodo con error garantizar la alta disponibilidad de su nube privada.

## <a name="storage"></a>Almacenamiento

**¿Qué tipo de almacenamiento se admite en una nube privada?**

Ofrece CloudSimple **almacenamiento vSAN de VMware de memoria flash** con cada nube privada.  Se crea cada vSphere con su propio almacén de datos vSAN.  Consulte [componentes de VMware de la nube privada: almacenamiento vSAN](https://docs.azure.cloudsimple.com/vmware-components/#vsan-storage) artículo para obtener más detalles.

**¿Es el cifrado de datos admitidos?**
Sí.  Puede configurar el almacenamiento de la vSAN en su propia nube privada para usar un servidor de administración de claves (KMS) que está implementado en local o en Azure para cifrar datos almacenados en la vSAN

**¿Cómo se administran los discos con errores?**

Supervisión de CloudSimple supervisa continuamente todos los componentes de hardware de la nube privada.  Si se detecta cualquier error de disco o cualquier disco que se identifica como con errores (basada en heurística), se agrega automáticamente un nuevo nodo en la nube privada.  El nodo con el disco con errores o con errores se quita de la nube privada.

## <a name="vmware"></a>VMware

**¿Cómo se realizan a gran escala cargar o migrar aplicaciones y datos locales?**

CloudSimple proporciona la solución nativa de vSphere de VMware.  Puede utilizarse cualquier herramienta que se usa para la migración de datos de forma masiva con la nube privada CloudSimple.  Algunas de las opciones disponibles son:

1. VMware HCX migración masiva de datos.
2. Migración en frío de datos con vMotion de almacenamiento local para CloudSimple.

**¿Puedo instalar las herramientas de VMware?**

CloudSimple proporciona la solución nativa de vSphere de VMware.  Cualquier herramienta que se usa para administrar el entorno vSphere local puede usarse en CloudSimple.  CloudSimple admite modelo Bring-Your-propia licencia (BYOL) para instalar las herramientas de VMware.

**¿Cómo se administran las actualizaciones?**

CloudSimple administra y actualiza todos los componentes de infraestructura de su nube privada en una conexión directa sin interrupciones.  Cualquier actualización o revisión de seguridad publicada por los proveedores de infraestructura o de VMware se programará para actualización tan pronto como lo está calificada por CloudSimple.

CloudSimple no lleva a cabo actualizaciones o las actualizaciones de las aplicaciones instaladas en la nube privada.

## <a name="azure-integration"></a>Azure Integration

**¿Se admiten los servicios de Azure?**

CloudSimple ofrece conexión Azure ExpressRoute a la suscripción en Azure.  Los servicios que se ejecutan en su suscripción tiene conectividad de red con su propia nube privada y puede conectarse a su propia nube privada.  Ejemplos:

1. **Azure Active Directory** como un origen de identidad para el vCenter CloudSimple
2. **Almacenamiento de Azure** para almacenar las copias de seguridad, imágenes y otros datos de su propia nube privada
3. **Aplicaciones híbridas** -puede crear la arquitectura de aplicaciones que abarca las nubes públicas y privadas.  Por ejemplo, puede crear servidores Web en Azure que tienen acceso a aplicaciones y servidores de base de datos en la nube privada CloudSimple.
4. **Azure monitor** y **de Azure security center** -carga de trabajo que se ejecutan en VMware puede usarlas para la administración de seguridad, las métricas de rendimiento y registro.

**¿Cómo se puede asignar Mis inquilinos de VMware a Azure?**

CloudSimple proporciona la capacidad única para administrar las máquinas virtuales de VMware en la nube privada de Azure portal.  Un grupo de recursos de vCenter (configurado con las restricciones de recursos deseado) puede asignarse a su suscripción mediante el administrador global.  

**¿Qué ventajas de las licencias obtengo con Azure?**

Con CloudSimple, puede aprovechar la ventaja de uso híbrido de Azure y ahorre hasta un 90% en licencias manteniendo su inversión en Microsoft Licenses, reduciendo el TCO en comparación con otras nubes. Además, extiendan las actualizaciones de seguridad para Windows Server 2008 y Microsoft SQL Server 2008.  Mantener los costos bajos con traiga su propia licencias (BYOL) a la nube para aplicaciones comunes, como Veeam, Zerto y otros.  