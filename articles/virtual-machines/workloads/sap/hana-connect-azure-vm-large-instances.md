---
title: Configuración de conectividad de máquinas virtuales en SAP HANA en Azure (instancias grandes) | Microsoft Docs
description: Configuración de conectividad de máquinas virtuales para usar SAP HANA en Azure (instancias grandes).
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f0e09e6dfce5d0ede525f461193866219b7f9429
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44167673"
---
# <a name="connecting-azure-vms-to-hana-large-instances"></a>Conexión de las máquinas virtuales de Azure a HANA Instancias grandes

Como se menciona en [Introducción y arquitectura de SAP HANA en Azure (Instancias grandes)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture), la implementación mínima de HANA Instancias grandes con la capa de aplicación de SAP en Azure es similar a:

![Red virtual de Azure conectada a SAP HANA en Azure (Instancias grandes) y localmente](./media/hana-overview-architecture/image3-on-premises-infrastructure.png)

Al examinar más de cerca el lado de la red virtual de Azure, somos conscientes de la necesidad de:

- La definición de una red virtual de Azure que se va a usar para implementar las máquinas virtuales de la capa de aplicación de SAP.
- Esto implica la definición de una subred predeterminada en la red virtual de Azure que es la utilizada realmente para implementar las máquinas virtuales.
- La red virtual de Azure que se crea debe tener al menos una subred de máquina virtual y una subred de puerta de enlace de ExpressRoute. A estas subredes se les deben asignar los intervalos de direcciones IP, como se especifica y trata en las siguientes secciones.

Por tanto, observemos más de cerca la creación de la red virtual de Azure para HANA Instancias grandes

## <a name="creating-the-azure-vnet-for-hana-large-instances"></a>Creación de la red virtual de Azure para HANA Instancias grandes

>[!Note]
>La red virtual de Azure para HANA Instancias grandes debe crearse mediante el modelo de implementación de Azure Resource Manager. La solución HANA (Instancias grandes) no admite el antiguo modelo de implementación de Azure, normalmente conocido como modelo de implementación clásica.

La red virtual puede crearse mediante Azure Portal, PowerShell, una plantilla de Azure o la CLI de Azure (vea [Creación de una red virtual (clásica) usando Azure Portal](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-virtual-network)). En el siguiente ejemplo se observa una red virtual creada mediante Azure Portal.

En él se explican las definiciones de una red virtual de Azure en Azure Portal y cómo se relacionan con lo que se indica sobre los distintos intervalos de direcciones IP. Por **espacio de direcciones** se entiende el espacio de direcciones que la red virtual de Azure puede usar. Este espacio de direcciones también es el intervalo de direcciones que usa la red virtual para la propagación de la ruta BGP. Este **espacio de direcciones** puede verse a continuación:

![Espacio de direcciones de la red virtual de Azure mostrado en Azure Portal](./media/hana-overview-connectivity/image1-azure-vnet-address-space.png)

En el caso anterior, con 10.16.0.0/16, se le asigna a la red virtual de Azure un amplio intervalo de direcciones IP para su uso. Esto significa que todos los intervalos de direcciones IP de subredes subsiguientes dentro de esta red virtual pueden tener sus intervalos dentro de ese “espacio de direcciones”. Normalmente no se recomienda un intervalo de direcciones tan grande para una única red virtual de Azure. Vayamos un poco más allá y observemos las subredes definidas en la red virtual de Azure:

![Subredes de la red virtual de Azure y sus intervalos de direcciones IP](./media/hana-overview-connectivity/image2b-vnet-subnets.png)

Como se puede ver, hay una primera subred de máquina virtual (denominada "default") y una subred llamada "GatewaySubnet".
En la sección siguiente se hace referencia al intervalo de direcciones IP de la subred denominada "default" en los gráficos como **Intervalo de direcciones IP de la subred de la máquina virtual de Azure**. En las secciones siguientes, nos referimos al intervalo de direcciones IP de la subred de la puerta de enlace como **Intervalo de direcciones IP de la subred de la puerta de enlace de la red virtual**. 

En el caso que muestran los dos gráficos anteriores, se observa que el **espacio de direcciones de la red virtual** abarca ambos, el **intervalo de direcciones IP de la subred de la máquina virtual de Azure** y el **intervalo de direcciones IP de la subred de la puerta de enlace de la red virtual**. 

En otros casos en los que necesite conservar o ser concreto con los intervalos de direcciones IP, puede restringir el **espacio de direcciones de red virtual** de una red virtual a los intervalos específicos usados por cada subred. En este caso, puede definir el **espacio de direcciones de la red virtual** de una red virtual como varios intervalos específicos, como se muestra a continuación:

![Espacio de direcciones de red virtual de Azure con dos espacios](./media/hana-overview-connectivity/image3-azure-vnet-address-space_alternate.png)

En este caso, el **espacio de direcciones de red virtual** tiene dos espacios definidos. Estos dos espacios son idénticos a los intervalos de direcciones IP definidos para el **intervalo de direcciones IP de la subred de la máquina virtual de Azure** y el **intervalo de direcciones IP de la subred de la puerta de enlace de la red virtual**.

Puede usar la nomenclatura que desee para estas subredes de inquilino (subredes de máquina virtual). Pero **siempre debe haber una, y solo una, subred de puerta de enlace para cada red virtual** que se conecte al circuito ExpressRoute de SAP HANA en Azure (Instancias grandes). Y **esta subred de puerta de enlace siempre debe denominarse "GatewaySubnet"** para garantizar la correcta colocación de la puerta de enlace de ExpressRoute.

> [!WARNING] 
> Es fundamental que la subred de la puerta de enlace siempre se denomine "GatewaySubnet".

Se pueden utilizar varias subredes de máquina virtual, incluso con intervalos de direcciones no contiguas. Pero como se mencionó anteriormente, estos intervalos de direcciones deben ser abarcados por el **espacio de direcciones de la red virtual** de la red virtual, bien en forma agregada o como una lista de los intervalos exactos de las subredes de máquina virtual y de la subred de la puerta de enlace.

Resumen de los hechos importantes sobre una red virtual de Azure que se conecta HANA Instancias grandes:

- Tiene que enviar a Microsoft el **espacio de direcciones de la red virtual** al realizar una implementación inicial de HANA (Instancias grandes). 
- El **espacio de direcciones de la red virtual** puede ser un intervalo mayor que abarque los intervalos de direcciones IP de las subredes de la máquina virtual de Azure y el intervalo de direcciones IP de la subred de la puerta de enlace de la red virtual.
- O puede enviar como **espacio de direcciones de la red virtual** varios intervalos que cubran los diferentes intervalos de direcciones IP de subredes de la máquina virtual y el intervalo de direcciones IP de la subred de la puerta de enlace de la red virtual.
- El **espacio de direcciones de la red virtual** definido se utiliza en la propagación del enrutamiento BGP.
- El nombre de la subred de la puerta de enlace debe ser: **"GatewaySubnet"**.
- El **espacio de direcciones de la red virtual** se usa como un filtro en el lado de HANA Instancias grandes para permitir o impedir el tráfico a las unidades de HANA Instancias grandes desde Azure. Si la información de enrutamiento de BGP de la red virtual de Azure y los intervalos de direcciones IP configurados para el filtrado en el lado de HANA (Instancias grandes) no coinciden, se pueden producir problemas de conectividad.
- Hay algunos detalles acerca de la subred de la puerta de enlace que se describen más adelante en la sección 'Conexión de una red virtual a ExpressRoute de HANA Instancias grandes'



## <a name="different-ip-address-ranges-to-be-defined"></a>Distintos intervalos de direcciones IP para definir 

En secciones anteriores ya se han presentado algunos de los intervalos de direcciones IP necesarios para implementar HANA (Instancias grandes). Pero hay algunos otros intervalos de direcciones IP que son importantes. Analicemos algunos detalles adicionales. Se necesita definir las siguientes direcciones IP (algunas de las cuales no es necesario enviar a Microsoft) antes de enviar una solicitud de implementación inicial:

- **Espacio de direcciones de la red virtual:** como se ha explicado antes, es el intervalo de direcciones IP que ha asignado (o planea asignar) al parámetro de espacio de direcciones de las redes virtuales de Azure que se conectan al entorno de SAP HANA (Instancias grandes). Se recomienda que el parámetro de espacio de direcciones sea un valor de varias líneas que comprenda los intervalos de las subredes de máquina virtual de Azure y el intervalo de la subred de la puerta de enlace de Azure, como muestran los gráficos anteriores. Este intervalo no se debe solapar con los intervalos de direcciones locales, ni con los del grupo de direcciones IP de servidor, ni con los de ER-P2P. ¿Cómo obtener estos intervalos de direcciones IP? El equipo de redes corporativas o el proveedor de servicios deben proporcionar uno o varios intervalos de direcciones IP que no se usen en la red. Ejemplo: si la subred de la máquina virtual de Azure (ver arriba) es 10.0.1.0/24 y la subred de la puerta de enlace de Azure (ver a continuación) es 10.0.2.0/28, se recomienda que el espacio de direcciones de la red virtual de Azure tenga dos líneas: 10.0.1.0/24 y 10.0.2.0/28. Aunque se pueden agregar valores al espacio de direcciones, se recomienda asociarlos a los intervalos de la subred para evitar una futura reutilización accidental de intervalos de direcciones IP sin usar en espacios de direcciones mayores en alguna parte de la red. **El espacio de direcciones de la red virtual es un intervalo de direcciones IP que se debe enviar a Microsoft al solicitar una implementación inicial**

- **Intervalo de direcciones IP de la subred de la máquina virtual de Azure:** este intervalo de direcciones IP, como se ha explicado anteriormente, es el que ha asignado (o planea asignar) al parámetro de la subred de la red virtual de Azure que va a conectar al entorno de SAP HANA (Instancias grandes). Este intervalo de direcciones IP se usa para asignar direcciones IP a las máquinas virtuales de Azure. Las direcciones IP fuera de este intervalo pueden conectarse a los servidores de SAP HANA (Instancias grandes). Si es necesario, se pueden utilizar varias subredes de máquina virtual de Azure. Microsoft recomienda un bloque CIDR /24 para cada subred de máquina virtual de Azure. Este intervalo de direcciones debe formar parte de los valores utilizados en el espacio de direcciones de red virtual de Azure. ¿Cómo obtener este intervalo de direcciones IP? El equipo de redes corporativas o el proveedor de servicios deben proporcionar un intervalo de direcciones IP no utilizado actualmente en la red.

- **Intervalo de direcciones IP de la subred de la puerta de enlace de la red virtual:** dependiendo de las características que se van a utilizar, el tamaño recomendado sería:
   - Puerta de enlace de ExpressRoute de ultrarrendimiento: bloque de direcciones /26: se necesita para la clase de tipo II de SKU
   - Coexistencia con VPN y ExpressRoute mediante una puerta de enlace de ExpressRoute de alto rendimiento (o inferior): bloque de direcciones /27
   - Resto de situaciones: bloque de direcciones /28. Este intervalo de direcciones debe formar parte de los valores utilizados en "Espacio de direcciones de la red virtual". Este intervalo de direcciones debe formar parte de los valores utilizados en los valores del espacio de direcciones de la red virtual de Azure que debe enviar a Microsoft. ¿Cómo obtener este intervalo de direcciones IP? El equipo de redes corporativas o el proveedor de servicios deben proporcionar un intervalo de direcciones IP no utilizado actualmente en la red. 

- **Intervalo de direcciones para conectividad ER-P2P:** es el intervalo de direcciones IP para la conexión ExpressRoute (ER) P2P de SAP HANA (Instancias grandes). Este intervalo de direcciones IP debe ser un intervalo de direcciones IP CIDR /29. Este intervalo no se debe superponer con los intervalos de direcciones locales ni con otros intervalos de direcciones IP de Azure. Este intervalo de direcciones IP se usa para configurar la conectividad ER entre la puerta de enlace ExpressRoute de la red virtual de Azure y los servidores de SAP HANA (Instancias grandes). ¿Cómo obtener este intervalo de direcciones IP? El equipo de redes corporativas o el proveedor de servicios deben proporcionar un intervalo de direcciones IP no utilizado actualmente en la red. **Se debe enviar a Microsoft este intervalo de direcciones IP al solicitar una implementación inicial**
  
- **Intervalo de direcciones del grupo de direcciones IP de servidor:** este intervalo de direcciones IP se usa para asignar direcciones IP individuales a los servidores de HANA Instancias grandes. El tamaño de subred recomendado es un bloque CIDR /24, aunque si es necesario, puede ser menor hasta un mínimo de 64 direcciones IP. De este intervalo, las primeras 30 direcciones IP están reservadas para uso de Microsoft. Asegúrese de tenerlo en cuenta al elegir el tamaño del intervalo. Este intervalo no se debe superponer con las direcciones IP locales ni con otras direcciones IP de Azure. ¿Cómo obtener este intervalo de direcciones IP? El equipo de redes corporativas o el proveedor de servicios deben proporcionar un intervalo de direcciones IP no utilizado actualmente en la red. Un bloque CIDR /24 único (recomendado) que se usará para asignar las direcciones IP específicas necesarias para SAP HANA en Azure (Instancias grandes). **Se debe enviar a Microsoft este intervalo de direcciones IP al solicitar una implementación inicial**
 
Aunque tiene que definir y planear los intervalos de direcciones IP anteriores, no es necesario transmitirlos todos a Microsoft. Para resumir lo anterior, los intervalos de direcciones IP que es necesario notificar a Microsoft son:

- Espacios de direcciones de la red virtual de Azure
- Intervalo de direcciones para conectividad ER-P2P
- Intervalo de direcciones del grupo de direcciones IP de servidor

Al agregar redes virtuales adicionales que necesitan conectarse a HANA Instancias grandes, es necesario enviar a Microsoft el nuevo espacio de direcciones de la red virtual de Azure que está agregando. 

A continuación se muestra un ejemplo de los diferentes intervalos y algunos intervalos de ejemplo que se deben configurar y, en algunos casos, proporcionar a Microsoft. Como se puede observar, en el primer ejemplo no se agrega el valor del espacio de direcciones de la red virtual de Azure; en su lugar, se define a partir de los intervalos del primer intervalo de direcciones IP de la subred de la máquina virtual de Azure y del intervalo de direcciones IP de la subred de la puerta de enlace de la red virtual. El uso de varias subredes de máquina virtual en la red virtual de Azure funcionaría mediante la configuración y el envío de los intervalos de direcciones IP adicionales de las subredes de máquina virtual adicionales como parte del espacio de direcciones de la red virtual de Azure.

![Intervalos de direcciones IP necesarios en la implementación mínima de SAP HANA en Azure (Instancias grandes)](./media/hana-overview-connectivity/image4b-ip-addres-ranges-necessary.png)

También tiene la posibilidad de agregar los datos que envíe a Microsoft. En ese caso, el espacio de direcciones de la red virtual de Azure solo incluiría un espacio. Con los intervalos de direcciones IP usados en el ejemplo anterior. Este espacio de direcciones de red virtual agregado podría tener un aspecto como este:

![Segunda posibilidad de intervalos de direcciones IP necesarios en la implementación mínima de SAP HANA en Azure (Instancias grandes)](./media/hana-overview-connectivity/image5b-ip-addres-ranges-necessary-one-value.png)

Como puede ver más arriba, en lugar de dos intervalos más pequeños que definían el espacio de direcciones de la red virtual de Azure, tenemos un intervalo mayor que abarca 4096 direcciones IP. Una definición de gran tamaño del espacio de direcciones deja algunos intervalos bastante grandes sin usar. Puesto que los valores del espacio de direcciones de la red virtual se usan para la propagación de la ruta BGP, la utilización de los intervalos no usados en instancias locales o en otra parte de la red puede causar problemas de enrutamiento. Por este motivo, se recomienda mantener el espacio de direcciones estrechamente alineado con el espacio de direcciones de la subred realmente utilizado. En caso necesario, siempre puede agregar nuevos valores al espacio de direcciones más adelante sin incurrir en tiempo de inactividad en la red virtual.
 
> [!IMPORTANT] 
> Los intervalos de direcciones IP de ER-P2P, del grupo de direcciones IP de servidor y del espacio de direcciones de la red virtual de Azure **NO** deben superponerse entre sí ni con ningún otro intervalo usado en la red; cada uno de ellos debe ser diferente y, como muestran los dos gráficos anteriores, no pueden ser una subred de cualquier otro intervalo. Si se producen superposiciones entre los intervalos, la red virtual de Azure podría no conectarse al circuito de ExpressRoute.

## <a name="next-steps-after-address-ranges-have-been-defined"></a>Pasos siguientes después de definir los intervalos de direcciones
Después de definir los intervalos de direcciones IP, son necesarias las siguientes actividades:

1. Enviar los intervalos de direcciones IP del espacio de nombres de la red virtual de Azure, de la conectividad ER-P2P y del grupo de direcciones IP de servidor, junto con los otros datos indicados al principio del documento. En este momento también podría empezar a crear la red virtual y las subredes de la máquina virtual. 
2. Microsoft crea un circuito de ExpressRoute entre la suscripción de Azure y el sello de HANA Instancias grandes.
3. Microsoft crea una red de inquilino en el sello de HANA Instancias grandes.
4. Microsoft configura las redes en la infraestructura de SAP HANA en Azure (Instancias grandes) de forma que acepten las direcciones IP del espacio de direcciones de la red virtual de Azure que se comunica con HANA (Instancias grandes).
5. Según la SKU concreta de SAP HANA en Azure (Instancias grandes) que se haya adquirido, Microsoft asigna una unidad de proceso en una red de inquilino, asigna y monta almacenamiento e instala el sistema operativo (SUSE o Red Hat Linux). Las direcciones IP para estas unidades se toman del intervalo de direcciones del grupo de direcciones IP de servidor que envió a Microsoft.

Al final del proceso de implementación, Microsoft le proporciona los siguientes datos:
- Información necesaria para conectar las redes virtuales de Azure al circuito de ExpressRoute que conecta las redes virtuales de Azure a HANA Instancias grandes:
     - Claves de autorización
     - PeerID de ExpressRoute
- Datos de acceso a HANA Instancias grandes después de establecer el circuito de ExpressRoute y la red virtual de Azure.

También puede buscar la secuencia de conexión de HANA (Instancias grandes) en el documento [End to End Setup for SAP HANA Large Instances (Configuración de extremo a extremo para SAP HANA (Instancias grandes))](https://azure.microsoft.com/resources/sap-hana-on-azure-large-instances-setup/). Muchos de los pasos siguientes se muestran en una implementación de ejemplo en ese documento. 

**Pasos siguientes**

- Consulte [Conexión de una red virtual a ExpressRoute de HANA (instancias grandes)](hana-connect-vnet-express-route.md).