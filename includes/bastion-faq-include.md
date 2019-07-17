---
title: archivo de inclusión
description: archivo de inclusión
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: include
ms.date: 06/17/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 29ab9b3c33aae6005510c34b207c7f87714149e5
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67608356"
---
### <a name="preview"></a>¿Cómo se participa en la versión preliminar pública?

Para participar en la versión preliminar pública es preciso incorporarla. Siga los pasos que se indican en [este artículo](../articles/bastion/bastion-create-host-portal.md) para crear un recurso de Azure Bastion. Actualmente, cuando se accede y se utiliza a este servicio, se debe usar la [versión preliminar de Azure Portal](https://aka.ms/BastionHost), en lugar de la versión normal.

### <a name="regions"></a>¿Qué regiones están disponibles durante la versión preliminar?

El recurso de Bastion se puede implementar y usar en cualquiera de estas regiones en versión preliminar a través del [vínculo de la versión preliminar de Azure Portal](https://aka.ms/BastionHost).

[!INCLUDE [region](bastion-regions-include.md)]

### <a name="portal"></a>No encuentro el recurso de Bastion en Azure Portal. ¿Cuál debo hacer?

Asegúrese de que usa el [vínculo la versión preliminar de Azure Portal](https://aka.ms/BastionHost), no la versión normal.

### <a name="publicip"></a>¿Necesito una dirección IP pública en mi máquina virtual?

NO necesita una dirección IP pública en la máquina virtual de Azure que va a conectar con el servicio Azure Bastion. El servicio Bastion abrirá la sesión o conexión RDP/SSH a la máquina virtual a través de la dirección IP privada de su máquina virtual dentro de su red virtual.

### <a name="rdpssh"></a>¿Necesito un cliente RDP o SSH?

No es necesario un cliente RDP o SSH para que el protocolo de escritorio remoto/Secure Shell accedan a su máquina virtual de Azure en Azure Portal. Use el [vínculo de la versión preliminar de Azure Portal](https://aka.ms/BastionHost) para acceder a ella. Esto le permitirá que RDP/SSH acceda a su máquina virtual directamente en el explorador.

### <a name="agent"></a>¿Es necesario que un agente se ejecute en la máquina virtual de Azure?

No es preciso instalar un agente ni ningún otro software en el explorador ni en la máquina virtual de Azure. El servicio Bastion no utiliza agentes y no requiere software adicional para RDP/SSH.

### <a name="browsers"></a>¿Qué exploradores se admiten?

Durante la versión preliminar pública, utilice el explorador Microsoft Edge o Google Chrome en Windows. Para Apple Mac, use Google Chrome. Microsoft Edge Chromium también es compatible tanto con Windows como con Mac.

### <a name="roles"></a>¿Se necesitan roles para acceder a una máquina virtual?

Para crear una conexión, se requieren los siguientes roles:

* Rol de lector en la máquina virtual
* Rol de lector en la tarjeta de interfaz de red con la dirección IP privada de la máquina virtual
* Rol de lector en el recurso de Azure Bastion

### <a name="previewbill"></a>Precios: ¿se me cobrará por participar en la versión preliminar?

Durante la versión preliminar pública solo se le cobrará parcialmente. Sin embargo, no hay ningún SLA asociado con su implementación. Consulte la [página de precios](https://aka.ms/BastionHostPricing)para obtener más información.

### <a name="previewbill"></a>¿Por qué obtengo el error "Su sesión ha expirado" antes de iniciar la sesión de Bastion?

Las sesiones deben iniciarse solo en Azure Portal. Inicie sesión en Azure Portal y vuelva a iniciar una sesión. Si va a la dirección URL directamente desde otra sesión del explorador o pestaña, este error es previsible. Ayuda a garantizar que la sesión sea más segura y que se solo se pueda acceder a la sesión desde Azure Portal.
