---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines-windows, virtual-machines-linux
author: dlepow
ms.service: multiple
ms.topic: include
ms.date: 02/28/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: a3c5290eb0179fe5842c495c2e08f22580d02bda
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2018
ms.locfileid: "38944883"
---
## <a name="deploy-an-image-with-marketplace-terms"></a>Implementación de una imagen con términos de Marketplace

Ciertas imágenes de máquina virtual en Azure Marketplace tienen términos adicionales de licencia y compra que debe aceptar antes de poder implementarlas mediante programación.  

Para implementar una máquina virtual a partir de ese tipo de imagen, debe aceptar los términos de la imagen y habilitar la implementación mediante programación. Solo tiene que hacerlo una vez para la suscripción. Luego, cada vez que implementa una máquina virtual mediante programación a partir de la imagen, también debe especificar los parámetros de *plan de compra*.

En las secciones siguientes se muestra cómo:

* Averiguar si una imagen de Marketplace tiene términos de licencia adicionales 
* Aceptar los términos mediante programación
* Proporcionar los parámetros de plan de compra al implementar una máquina virtual mediante programación

