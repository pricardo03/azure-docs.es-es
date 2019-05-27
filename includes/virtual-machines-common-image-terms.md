---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines-windows, virtual-machines-linux
author: dlepow
ms.service: multiple
ms.topic: include
ms.date: 10/09/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 50d71a3967e61e5d531f4bfeae3582b56230a0d2
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66117303"
---
## <a name="terminology"></a>Terminología

Una imagen de Marketplace de Azure tiene los atributos siguientes:

* **Publicador**: Organización que ha creado la imagen. Ejemplos: Canonical, MicrosoftWindowsServer
* **Oferta**: nombre de un grupo de imágenes relacionadas creadas por un publicador. Ejemplos: UbuntuServer, WindowsServer
* **SKU**: Instancia de una oferta, por ejemplo, una versión principal de una distribución. Ejemplos: 18.04-LTS, 2019-Datacenter
* **Versión**: número de versión de una SKU de imagen. 

Para identificar una imagen de Marketplace cuando se implementa una máquina virtual mediante programación, brinde estos valores de manera individual como parámetros. Algunas herramientas aceptan un *URN* de imagen que combina estos valores separados por el carácter de dos puntos (:): *Publicador*:*Oferta*:*SKU*:*Versión*. En un URN, puede reemplazar el número de versión por "latest", que selecciona la versión más reciente de la imagen. 

Si el publicador de la imagen proporciona términos adicionales de licencia y compra, debe aceptar esos términos y habilitar la implementación mediante programación. También debe suministrar los parámetros de *plan de compra* al implementar una máquina virtual mediante programación. Consulte la sección [Implementación de una imagen con términos de Marketplace](#deploy-an-image-with-marketplace-terms).
