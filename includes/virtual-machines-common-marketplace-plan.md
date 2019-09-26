---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines-windows, virtual-machines-linux
author: cynthn
ms.service: multiple
ms.topic: include
ms.date: 10/09/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 8e0e549f88caf4a541642bab77faf54b5e536b29
ms.sourcegitcommit: f2771ec28b7d2d937eef81223980da8ea1a6a531
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2019
ms.locfileid: "71174990"
---
## <a name="deploy-an-image-with-marketplace-terms"></a>Implementación de una imagen con términos de Marketplace

Algunas imágenes de máquina virtual en Azure Marketplace tienen términos adicionales de licencia y compra que debe aceptar antes de poder implementarlas mediante programación.  

Para implementar una máquina virtual a partir de ese tipo de imagen, deberá aceptar los términos de la imagen y habilitar la implementación mediante programación. Solo tendrá que hacerlo una vez por suscripción. Luego, cada vez que implementa una máquina virtual mediante programación a partir de la imagen, también deberá especificar los parámetros de *plan de compra*.

En las secciones siguientes se muestra cómo:

* Averiguar si una imagen de Marketplace tiene términos de licencia adicionales 
* Aceptar los términos mediante programación
* Proporcionar los parámetros de plan de compra al implementar una máquina virtual mediante programación

