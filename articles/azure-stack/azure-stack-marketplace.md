---
title: Publicar un elemento de Marketplace personalizado en Azure Stack (operador de nube) | Microsoft Docs
description: Como operador de Azure Stack, aprenda cómo publicar un elemento de Marketplace personalizado en Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 60871cbb-eed2-433c-a76d-d605c7aec06c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2018
ms.author: sethm
ms.reviewer: jeffgo
ms.openlocfilehash: 12310c088777d65bef211747806f942433857e40
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/15/2018
ms.locfileid: "45632355"
---
# <a name="the-azure-stack-marketplace-overview"></a>Información general de Azure Stack Marketplace

*Se aplica a: sistemas integrados de Azure Stack y Kit de desarrollo de Azure Stack*

Marketplace es una colección de servicios, aplicaciones y recursos personalizados para Azure Stack. Los recursos incluyen redes, máquinas virtuales, almacenamiento, etc. Es el lugar donde los usuarios vienen para crear nuevos recursos e implementar nuevas aplicaciones. Piense que es como un catálogo de compras donde los usuarios pueden examinar y elegir los elementos que se quieren usar. Los usuarios, para utilizar un elemento de Marketplace, deben suscribirse a una oferta que les concede acceso al elemento.

Como operador Azure Stack, usted decide qué elementos quiere agregar (publicar) en Marketplace. Puede publicar elementos, como bases de datos, App Services, etc. Esto hace que sean visibles para todos los usuarios. Puede publicar elementos personalizados creados por usted. También puede publicar elementos de una creciente [lista de elementos de Azure Marketplace](azure-stack-marketplace-azure-items.md). Al publicar un elemento en Marketplace, los usuarios pueden verlo a los cinco minutos.

> [!Caution]  
> Todos los elementos de galería, conocidos como imágenes y archivos json, son accesibles sin autenticación después de publicarlos en Azure Stack Marketplace. Consulte otros aspectos que se deben tener en cuenta al publicar elementos en Marketplace en [Creación y publicación de un producto en Marketplace](azure-stack-create-and-publish-marketplace-item.md).

Para abrir Marketplace, en la consola de administración, seleccione **+Crear un recurso**.

![](media/azure-stack-publish-custom-marketplace-item/image1.png)

## <a name="marketplace-items"></a>Elementos de Marketplace
Un elemento de Azure Stack Marketplace es un servicio, una aplicación o un recurso que los usuarios pueden descargar y usar. Todos los elementos de Marketplace para Azure Stack son visibles para todos los usuarios, incluidos los elementos administrativos como planes y ofertas. Estos elementos no requieren una suscripción pero no están en funcionamiento para los usuarios.

Cada elemento de Marketplace tiene:

* Una plantilla del Administrador de recursos de Azure para el aprovisionamiento de recursos
* Metadatos, como cadenas, iconos y otra documentación y material adjunto de marketing
* Información de formato para mostrar el elemento en el portal

Cada elemento publicado en Marketplace utiliza un formato denominado paquete de galería de Azure (.azpkg). Agregue recursos de implementación o de entorno en tiempo de ejecución (por ejemplo, código, archivos comprimidos con software o imágenes de máquinas virtuales) a Azure Stack por separado, no como parte del elemento de Marketplace. 

En la versión 1803 y versiones posteriores, Azure Stack convierte las imágenes en archivos dispersos cuando se descargan de Azure o al cargar imágenes personalizadas. Este proceso supone más tiempo a la hora de agregar una imagen, pero ahorra espacio y acelera la implementación de esas imágenes. La conversión solo se aplica a las nuevas imágenes.  Las imágenes existentes no cambian. 

## <a name="next-steps"></a>Pasos siguientes
[Descarga de elementos de Marketplace](azure-stack-download-azure-marketplace-item.md)  
[Creación y publicación de un producto en Marketplace](azure-stack-create-and-publish-marketplace-item.md)

