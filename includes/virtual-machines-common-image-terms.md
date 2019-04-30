---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines-windows, virtual-machines-linux
author: rockboyfor
ms.service: multiple
ms.topic: include
origin.date: 10/09/2018
ms.date: 04/01/2019
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 50d71a3967e61e5d531f4bfeae3582b56230a0d2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60880569"
---
## <a name="terminology"></a>Terminología

Una imagen de Marketplace de Azure tiene los atributos siguientes:

* **Publicador**: Organización que ha creado la imagen. Ejemplos: Canonical, MicrosoftWindowsServer
* **Oferta**: nombre de un grupo de imágenes relacionadas creadas por un publicador. Ejemplos: UbuntuServer, WindowsServer
* **SKU**: Instancia de una oferta, por ejemplo, una versión principal de una distribución. Ejemplos: 18.04-LTS, 2019-Datacenter
* **Versión**: número de versión de una SKU de imagen. 

Para identificar una imagen de Marketplace cuando se implementa una máquina virtual mediante programación, brinde estos valores de manera individual como parámetros. Algunas herramientas aceptan un *URN* de imagen que combina estos valores separados por el carácter de dos puntos (:): *Publicador*:*Oferta*:*SKU*:*Versión*. En un URN, puede reemplazar el número de versión por "latest", que selecciona la versión más reciente de la imagen. 

Si el publicador de la imagen proporciona términos adicionales de licencia y compra, debe aceptar esos términos y habilitar la implementación mediante programación. También debe suministrar los parámetros de *plan de compra* al implementar una máquina virtual mediante programación. Consulte la sección [Implementación de una imagen con términos de Marketplace](#deploy-an-image-with-marketplace-terms).

<!-- Update_Description: wording update -->