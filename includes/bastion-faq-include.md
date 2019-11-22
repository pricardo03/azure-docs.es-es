---
title: archivo de inclusión
description: archivo de inclusión
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: include
ms.date: 10/15/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: b4e479405a9606a8353785828d0c9c94ef8c32ed
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2019
ms.locfileid: "73850484"
---
### <a name="regions"></a>¿En qué regiones está disponible?

[!INCLUDE [region](bastion-regions-include.md)]

### <a name="publicip"></a>¿Necesito una dirección IP pública en mi máquina virtual?

NO necesita una dirección IP pública en la máquina virtual de Azure que va a conectar con el servicio Azure Bastion. El servicio Bastion abrirá la sesión o conexión RDP/SSH a la máquina virtual a través de la dirección IP privada de su máquina virtual dentro de su red virtual.

### <a name="rdpssh"></a>¿Necesito un cliente RDP o SSH?

No es necesario un cliente RDP o SSH para que el protocolo de escritorio remoto/Secure Shell accedan a su máquina virtual de Azure en Azure Portal. Use [Azure Portal](https://portal.azure.com) para que RDP/SSH acceda a su máquina virtual directamente en el explorador.

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

