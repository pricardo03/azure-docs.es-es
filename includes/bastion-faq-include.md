---
title: archivo de inclusión
description: archivo de inclusión
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: include
ms.date: 03/02/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 17d391a7e6b8ef0558fb73afe363cd96deb60a7d
ms.sourcegitcommit: e4c33439642cf05682af7f28db1dbdb5cf273cc6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2020
ms.locfileid: "78262232"
---
### <a name="regions"></a>¿En qué regiones está disponible?

[!INCLUDE [region](bastion-regions-include.md)]

### <a name="publicip"></a>¿Necesito una dirección IP pública en mi máquina virtual?

Si va a conectar una máquina virtual mediante Azure Bastion, NO necesitará ninguna dirección IP pública en la máquina virtual de Azure a la que se va a conectar. El servicio Bastion abrirá la sesión o conexión RDP/SSH a la máquina virtual a través de la dirección IP privada de su máquina virtual dentro de su red virtual.

### <a name="is-ipv6-supported"></a>¿Se admite IPv6?

Actualmente, IPv6 no se admite. Azure Bastion solo admite IPv4.

### <a name="rdpssh"></a>¿Necesito un cliente RDP o SSH?

No es necesario un cliente RDP o SSH para que el protocolo de escritorio remoto/Secure Shell accedan a su máquina virtual de Azure en Azure Portal. Use [Azure Portal](https://portal.azure.com) para que RDP/SSH acceda a su máquina virtual directamente en el explorador.

### <a name="rdscal"></a>¿Requiere Azure Bastion una CAL de RDS con fines administrativos en máquinas virtuales hospedadas en Azure?
No, el acceso a las máquinas virtuales Windows Server con Azure Bastion no requiere una [CAL de RDS](https://www.microsoft.com/en-us/p/windows-server-remote-desktop-services-cal/dg7gmgf0dvsv?activetab=pivot:overviewtab) cuando se utiliza únicamente con fines administrativos.

### <a name="agent"></a>¿Es necesario que un agente se ejecute en la máquina virtual de Azure?

No es preciso instalar un agente ni ningún otro software en el explorador ni en la máquina virtual de Azure. El servicio Bastion no utiliza agentes y no requiere software adicional para RDP/SSH.

### <a name="browsers"></a>¿Qué exploradores se admiten?

Utilice el explorador Microsoft Edge o Google Chrome en Windows. Para Apple Mac, use Google Chrome. Microsoft Edge Chromium también es compatible tanto con Windows como con Mac.

### <a name="roles"></a>¿Se necesitan roles para acceder a una máquina virtual?

Para crear una conexión, se requieren los siguientes roles:

* Rol de lector en la máquina virtual
* Rol de lector en la tarjeta de interfaz de red con la dirección IP privada de la máquina virtual
* Rol de lector en el recurso de Azure Bastion

### <a name="pricingpage"></a>¿Cuáles son los precios?

Consulte la [página de precios](https://aka.ms/BastionHostPricing)para obtener más información.

### <a name="session"></a>¿Por qué obtengo el error "Su sesión ha expirado" antes de iniciar la sesión de Bastion?

Las sesiones deben iniciarse solo en Azure Portal. Inicie sesión en Azure Portal y vuelva a iniciar una sesión. Si va a la dirección URL directamente desde otra sesión del explorador o pestaña, este error es previsible. Ayuda a garantizar que la sesión sea más segura y que se solo se pueda acceder a la sesión desde Azure Portal.

### <a name="keyboard"></a>¿Qué distribuciones de teclado se admiten durante la sesión remota Bastion?

Actualmente, Azure Bastion admite la distribución de teclado en-US-QWERTY dentro de la máquina virtual.  La compatibilidad con otras configuraciones regionales para la distribución del teclado está en curso.

### <a name="udr"></a>¿Se admite el enrutamiento definido por el usuario (UDR) en las subredes de Azure Bastion?

No. UDR no es compatible con las subredes de Azure Bastion.
En los escenarios que incluyen Azure Bastion y Azure Firewall/Aplicación virtual de red (NVA) en la misma red virtual, no es preciso forzar el tráfico de una subred de Azure Bastion a Azure Firewall, ya que la comunicación entre Azure Bastion y las máquinas virtuales es privada. Para más información, consulte [Acceso a las máquinas virtuales que están detrás de Azure Firewall con Bastion](https://azure.microsoft.com/blog/accessing-virtual-machines-behind-azure-firewall-with-azure-bastion/).

### <a name="filetransfer"></a>¿Se admite la transferencia de archivos con la sesión RDP de Azure Bastion?

Estamos trabajando para agregar nuevas características. Actualmente, no se admite la transferencia de archivos, pero forma parte de nuestro mapa de ruta. No dude en compartir sus comentarios sobre las nuevas características en la [página de comentarios de Azure Bastion](https://feedback.azure.com/forums/217313-networking?category_id=367303).
