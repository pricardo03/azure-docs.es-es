---
title: Consideraciones de redes para Azure Files | Microsoft Docs
description: Una introducción a las opciones de red para Azure Files.
author: roygara
ms.service: storage
ms.topic: overview
ms.date: 10/19/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 596479652478bffb6d18a90fc53d5972b3839408
ms.sourcegitcommit: b45ee7acf4f26ef2c09300ff2dba2eaa90e09bc7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2019
ms.locfileid: "73126541"
---
# <a name="azure-files-networking-considerations"></a>Consideraciones de redes para Azure Files 
Puede conectarse a un recurso compartido de archivos de Azure de dos maneras:

- Acceda de forma directa al recurso compartido a través de los protocolos SMB o FileREST. Este patrón de acceso se emplea principalmente cuando se quieren eliminar tantos servidores locales como sea posible.
- Cree una memoria caché del recurso compartido de archivos de Azure en un servidor local con Azure File Sync y acceda a los datos del recurso compartido de archivos desde el servidor local con el protocolo que elija (SMB, NFS, FTPS, etc.) para su caso de uso. Este patrón de acceso es útil porque combina lo mejor de la escala en la nube y el rendimiento local junto con los servicios conectables sin servidor, como Azure Backup.

Este artículo se centra en cómo configurar las funciones de red cuando el caso de uso necesita acceder al recurso compartido de archivos de Azure directamente, en lugar de usar Azure File Sync. Para obtener más información acerca de las consideraciones de red para una implementación de Azure File Sync, consulte [Configuración de los ajustes de proxy y firewall de Azure File Sync](storage-sync-files-firewall-and-proxy.md).

## <a name="storage-account-settings"></a>Configuración de cuentas de almacenamiento
Una cuenta de almacenamiento es una construcción de administración que representa un grupo compartido de almacenamiento en el que puede implementar varios recursos compartidos de archivos u otros recursos de almacenamiento, como contenedores de blobs o colas. Las cuentas de Azure Storage exponen dos conjuntos básicos de configuraciones para proteger la red: el cifrado en tránsito y los firewalls y redes virtuales.

### <a name="encryption-in-transit"></a>Cifrado en tránsito
De forma predeterminada, todas las cuentas de Azure Storage tienen habilitado el cifrado en tránsito. Esto significa que al montar un recurso compartido de archivos a través de SMB o acceder a él a través del protocolo de FileREST (por ejemplo, a través de Azure Portal, la CLI o PowerShell, o los SDK de Azure), Azure Files solo permitirá la conexión si se realiza con una versión posterior a SMB 3.0 con cifrado o HTTPS. Los clientes que no admiten SMB 3.0, o los clientes que admiten SMB 3.0, pero no al cifrado SMB no podrán montar el recurso compartido de archivos de Azure si está habilitado el cifrado en tránsito. Para obtener más información sobre qué sistemas operativos admiten SMB 3.0 con cifrado, consulte nuestra documentación detallada para [Windows](storage-how-to-use-files-windows.md), [macOS](storage-how-to-use-files-mac.md) y [Linux](storage-how-to-use-files-linux.md). Todas las versiones actuales de PowerShell, la CLI y los SDK admiten HTTPS.  

Puede deshabilitar el cifrado en tránsito para una cuenta de almacenamiento de Azure. Cuando el cifrado está deshabilitado, Azure Files también permite el uso de SMB 2.1, SMB 3.0 sin cifrado y las llamadas API de FileREST sin cifrar a través de HTTP. La razón principal para deshabilitar el cifrado en tránsito es admitir una aplicación heredada que debe ejecutarse en un sistema operativo anterior, como Windows Server 2008 R2 o una distribución de Linux anterior. Azure Files solo permite conexiones SMB 2.1 dentro de la misma región de Azure del recurso compartido de archivos de Azure. Un cliente SMB 2.1 fuera de la región de Azure del recurso compartido de archivos de Azure (por ejemplo, en un entorno local o en una región de Azure diferente) no podrá acceder al recurso compartido de archivos.

Para obtener más información sobre el cifrado en tránsito, consulte [Requerir transferencia segura en Azure Storage](../common/storage-require-secure-transfer.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

### <a name="firewalls-and-virtual-networks"></a>Firewalls y redes virtuales 
Un firewall es una directiva de red cuyas solicitudes pueden acceder a los recursos compartidos de archivos de Azure y a otros recursos de almacenamiento de la cuenta de almacenamiento. Cuando se crea una cuenta de almacenamiento con la configuración de red predeterminada, no está restringida a una red específica y, por lo tanto, se puede acceder a ella desde Internet. Esto no significa que cualquier usuario de Internet puede acceder a los datos almacenados en los recursos compartidos de archivos de Azure hospedados en su cuenta de almacenamiento, sino que la cuenta de almacenamiento aceptará las solicitudes autorizadas de cualquier red. Las solicitudes se pueden autorizar con una clave de cuenta de almacenamiento, un token de firma de acceso compartido (SAS) (solo FileREST) o una entidad de seguridad de usuario de Active Directory. 

La directiva de firewall para una cuenta de almacenamiento puede usarse para restringir el acceso a determinadas direcciones IP o intervalos de direcciones, o a una red virtual. En general, la mayoría de las directivas de firewall para una cuenta de almacenamiento restringirán el acceso de red a una red virtual. 

Una [red virtual](../../virtual-network/virtual-networks-overview.md) es similar a la red tradicional en la que puede trabajar en su propio centro de recursos. Permite crear un canal de comunicación seguro para que los recursos de Azure (como los recursos compartidos de archivos de Azure, las máquinas virtuales, las bases de datos SQL, etc.) se comuniquen entre sí. Al igual que una cuenta de almacenamiento de Azure o una máquina virtual de Azure, una red virtual es un recurso de Azure que se implementa en un grupo de recursos. Con la configuración de red adicional, las redes virtuales de Azure también pueden conectarse a sus redes locales.

Cuando se agregan recursos como una máquina virtual de Azure a una red virtual, una interfaz de red virtual conectada a la máquina virtual se restringe de forma concreta a solo esa red virtual. Esto es posible porque las máquinas virtuales de Azure son equipos virtualizados que, por supuesto, tienen interfaces de red virtual. Las máquinas virtuales se ofertan como parte de la línea de productos de infraestructura como servicio, o IaaS, de Azure. Dado que los recursos compartidos de archivos de Azure son recursos compartidos de archivos sin servidor, no tienen una interfaz de red virtual para que la agregue a una red virtual. Dicho de otra forma, Azure Files se oferta como parte de la línea de productos de la plataforma como servicio, o PaaS, de Azure. Para habilitar una cuenta de almacenamiento a fin de que pueda formar parte de una red virtual, Azure admite un concepto de servicios PaaS denominados puntos de conexión de servicio. Un punto de conexión de servicio permite que los servicios PaaS formen parte de una red virtual. Para obtener más información sobre los puntos de conexión de servicio, consulte [Puntos de conexión de servicio de red virtual](../../virtual-network/virtual-network-service-endpoints-overview.md).

Se puede agregar una cuenta de almacenamiento a una o varias redes virtuales. Para obtener más información sobre cómo agregar su cuenta de almacenamiento a una red virtual o cómo configurar otras opciones de firewall, consulte [Configuración de firewalls y redes virtuales de Azure Storage](../common/storage-network-security.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

## <a name="azure-networking"></a>Redes de Azure
De forma predeterminada, se puede acceder a los servicios de Azure, incluido Azure Files, a través de Internet. Dado que, de forma predeterminada, el tráfico a la cuenta de almacenamiento está cifrado (y los montajes de SMB 2.1 nunca se permiten fuera de una región de Azure), el acceso a los recursos compartidos de archivos de Azure a través de Internet no es inseguro en esencia. En función de la directiva de la organización o de los requisitos normativos únicos, es posible que necesite una comunicación más restrictiva con Azure y, por lo tanto, Azure proporciona varias maneras de restringir el modo en que el tráfico fuera de Azure se comunica con Azure Files. Puede proteger aún más las redes al acceder al recurso compartido de archivos de Azure mediante las siguientes ofertas de servicio:

- [Azure VPN Gateway](../../vpn-gateway/vpn-gateway-about-vpngateways.md): Una puerta de enlace de VPN es un tipo específico de puerta de enlace de red virtual que se usa para enviar tráfico cifrado entre una red virtual de Azure y una ubicación alternativa (por ejemplo, en un entorno local) a través de Internet. Una instancia de Azure VPN Gateway es un recurso de Azure que se puede implementar en un grupo de recursos junto con una cuenta de almacenamiento u otros recursos de Azure. Las puertas de enlace de VPN exponen dos tipos de conexión distintos:
    - Las conexiones de puerta de enlace de [VPN de punto a sitio](../../vpn-gateway/point-to-site-about.md), que son conexiones VPN entre Azure y un cliente individual. Esta solución es especialmente útil para los dispositivos que no forman parte de la red local de su organización, como los trabajadores remotos que quieren montar su recurso compartido de archivos de Azure desde casa, una cafetería o un hotel mientras viajan. Para usar una conexión VPN de punto a sitio con Azure Files, se debe configurar una conexión VPN de punto a sitio para cada cliente que quiera conectarse. 
    - Las [VPN de sitio a sitio](../../vpn-gateway/vpn-gateway-about-vpngateways.md#s2smulti), que son conexiones VPN entre Azure y la red de su organización. Una conexión de VPN de sitio a sitio le permite configurar una conexión VPN una vez, para un servidor VPN o un dispositivo hospedado en la red de su organización, en lugar de hacerlo para cada dispositivo cliente que necesite tener acceso al recurso compartido de archivos de Azure.
- [ExpressRoute](../../expressroute/expressroute-introduction.md), que permite crear una ruta definida entre Azure y la red local que no atraviesa Internet. Como ExpressRoute proporciona una ruta de acceso dedicada entre el centro de recursos local y Azure, ExpressRoute puede resultar útil cuando se tiene en cuenta el rendimiento de la red. ExpressRoute también es una buena opción cuando la directiva o los requisitos normativos de la organización requieren una ruta de acceso determinista a los recursos en la nube.

## <a name="securing-access-from-on-premises"></a>Protección del acceso desde entornos locales 
Al migrar recursos compartidos de archivos de uso general (por ejemplo, documentos de Office, archivos PDF, documentos CAD, etc.) a Azure Files, los usuarios seguirán necesitando acceder a sus archivos desde dispositivos locales como sus estaciones de trabajo, equipos portátiles y tabletas. La consideración principal para un recurso compartido de archivos de uso general es cómo los usuarios locales pueden acceder de forma segura a sus recursos compartidos de archivos a través de Internet o WAN.

La forma más fácil de acceder al recurso compartido de archivos de Azure desde el entorno local es abrir la red local para el puerto 445 (el que usa SMB) y montar la ruta de acceso UNC proporcionada por Azure Portal. Esto no requiere de ninguna red especial. Muchos clientes son reacios a abrir el puerto 445 debido a las instrucciones de seguridad obsoletas basadas en SMB 1.0, que Microsoft no considera un protocolo seguro de Internet. Azure Files no implementa SMB 1.0. 

SMB 3.0 se diseñó con el requisito explícito de ser un protocolo de recursos compartidos de archivos seguro para Internet. Por lo tanto, cuando se usan versiones posteriores a SMB 3.0, desde el punto de vista de la red del equipo, la apertura del puerto 445 no es diferente de la apertura del puerto 443 (el que se usa para las conexiones HTTPS). En lugar de bloquear el puerto 445 para evitar el tráfico de SMB 1.0 inseguro, Microsoft recomienda los siguientes pasos:

> [!Important]  
> Incluso si decide dejar el puerto 445 cerrado para el tráfico saliente, Microsoft le recomienda seguir estos pasos para quitar SMB 1.0 de su entorno.

1. Asegúrese de que se ha quitado o deshabilitado SMB 1.0 en los dispositivos de la organización. Todas las versiones de Windows y Windows Server compatibles actualmente admiten la eliminación o desactivación de SMB 1.0 y, a partir de Windows 10, versión 1709, SMB 1.0 no se instala en Windows de forma predeterminada. Para obtener más información sobre cómo deshabilitar SMB 1.0, consulte nuestras páginas específicas para cada sistema operativo:
    - [Protección de Windows y Windows Server](storage-how-to-use-files-windows.md#securing-windowswindows-server)
    - [Protección de Linux](storage-how-to-use-files-linux.md#securing-linux)
1. Asegúrese de que ningún producto de la organización requiere SMB 1.0 y quite los que sí lo requieran. Mantenemos un [Centro de enrutamiento de productos de SMB1](https://aka.ms/stillneedssmb1), que contiene todos los productos propios y de terceros que Microsoft sabe que requieren de SMB 1.0. 
1. (Opcional) Use un firewall de terceros con la red local de su organización para evitar el tráfico SMB 1.0.

Si su organización requiere que el puerto 445 se bloquee debido a directivas o reglas, puede usar Azure VPN Gateway o ExpressRoute para tunelizar el tráfico en el puerto 443. Para obtener más información sobre los pasos específicos de implementación, consulte nuestras páginas de instrucciones específicas:
- [Configuración de una VPN de sitio a sitio (S2S) para su uso con Azure Files](storage-files-configure-s2s-vpn.md)
- [Configuración de una VPN de punto a sitio (P2S) en Windows para su uso con Azure Files](storage-files-configure-p2s-vpn-windows.md)
- [Configuración de una VPN de punto a sitio (P2S) en Linux para su uso con Azure Files](storage-files-configure-p2s-vpn-linux.md)

Su organización puede tener el requisito adicional de que el tráfico saliente desde el sitio local debe seguir una ruta de acceso determinista hacia los recursos en la nube. De ser así, ExpressRoute es capaz de cumplir este requisito.

## <a name="securing-access-from-cloud-resources"></a>Protección del acceso desde recursos en la nube
Por lo general, cuando una aplicación local se transfiere mediante lift-and-shift a la nube, la aplicación y los datos de la aplicación se mueven al mismo tiempo. Esto significa que la consideración principal de una migración mediante lift-and-shift es bloquear el acceso al recurso compartido de archivos de Azure para las máquinas virtuales o servicios de Azure específicos que requieren de dicho acceso para funcionar. 

Quizá quiera usar redes virtuales para limitar qué máquinas virtuales u otros recursos de Azure pueden realizar conexiones de red (montajes SMB o llamadas de API REST a su recurso compartido de archivos de Azure). Siempre se recomienda colocar el recurso compartido de archivos de Azure en una red virtual si permite el flujo de tráfico no cifrado hacia la cuenta de almacenamiento. De lo contrario, independientemente de si usa redes virtuales o no, se trata de una decisión que debe basarse en los requisitos empresariales y la directiva de la organización.

La razón principal para permitir el tráfico no cifrado hacia el recurso compartido de archivos de Azure es la compatibilidad con Windows Server 2008 R2, Windows 7 u otro sistema operativo anterior que acceda al recurso compartido de archivos de Azure mediante SMB 2.1 (o SMB 3.0 sin cifrado para algunas distribuciones de Linux). No se recomienda usar SMB 2.1 o SMB 3.0 sin cifrado en los sistemas operativos que admiten las versiones posteriores a SMB 3.0 con cifrado.

## <a name="see-also"></a>Otras referencias
- [Introducción a Azure Files](storage-files-introduction.md)
- [Planeamiento de una implementación de Azure Files](storage-files-planning.md)