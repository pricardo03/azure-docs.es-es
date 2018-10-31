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
ms.openlocfilehash: 4d2235eaea457c89d01a632afa5dd5a862bec344
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2018
ms.locfileid: "49437044"
---
## <a name="deploy-an-image-with-marketplace-terms"></a>Implementación de una imagen con términos de Marketplace

Algunas imágenes de máquina virtual en Azure Marketplace tienen términos adicionales de licencia y compra que debe aceptar antes de poder implementarlas mediante programación.  

Para implementar una máquina virtual a partir de ese tipo de imagen, deberá aceptar los términos de la imagen y habilitar la implementación mediante programación. Solo tendrá que hacerlo una vez por suscripción. Luego, cada vez que implementa una máquina virtual mediante programación a partir de la imagen, también deberá especificar los parámetros de *plan de compra*.

En las secciones siguientes se muestra cómo:

* Averiguar si una imagen de Marketplace tiene términos de licencia adicionales 
* Aceptar los términos mediante programación
* Proporcionar los parámetros de plan de compra al implementar una máquina virtual mediante programación

