---
title: Oferta de máquina virtual en Azure Marketplace | Microsoft Docs
description: Información general del proceso de publicación de una oferta de máquina virtual en Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 09/28/2018
ms.author: pbutlerm
ms.openlocfilehash: d3682d18fb849b2d851bae0986f9e61f216aaf2c
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2018
ms.locfileid: "49639380"
---
# <a name="virtual-machine-offer"></a>Oferta de máquina virtual

En esta sección se describen los elementos de la publicación de una máquina virtual (VM), y está pensada como una guía para el publicador en [Azure Marketplace](https://azuremarketplace.microsoft.com).  Desde este punto de vista, se divide en las partes principales siguientes:

- [Requisitos previos](./cpp-prerequisites.md): se enumeran los requisitos técnicos y empresariales antes de crear o publicar una oferta de máquina virtual
- [Creación de oferta de máquina virtual](./cpp-create-offer.md): se enumeran los pasos necesarios para crear una entrada de oferta de máquina virtual mediante [Cloud Partner Portal](https://cloudpartner.azure.com).
- [Creación de recursos técnicos de máquina virtual](./cpp-create-technical-assets.md): cómo crear los recursos técnicos para una solución de máquina virtual y cómo configurar este paquete como una oferta de máquina virtual en Azure Marketplace.
- [Publicación de la oferta de máquina virtual](./cpp-publish-offer.md): cómo enviar la oferta para su publicación en Azure Marketplace.


## <a name="vm-publishing-process-flow"></a>Flujo de proceso de publicación de máquina virtual

En el diagrama siguiente se ilustran los pasos generales de la publicación de una oferta de máquina virtual. 

![Proceso de publicación de máquina virtual](./media/publishvm_001.png)

1. Creación de la oferta: se configuran todos los detalles y la información sobre la oferta, incluida la descripción de la oferta, los materiales de marketing, la información legal y de soporte técnico, y las especificaciones de los recursos.

2. Creación de los recursos técnicos y empresariales: se crean los recursos empresariales (documentos legales y materiales de marketing) y los recursos técnicos de la solución asociada (en este caso, las máquinas virtuales y los discos conectados). 

3. Creación de la SKU: se crean las SKU asociadas con la oferta y se envían.  Se necesita una SKU única para cada imagen que se vaya a publicar. 
 
4. Certificación y publicación de la oferta: después de completar la oferta y los recursos técnicos, se puede enviar. Este envío iniciará el proceso de publicación, en el que la solución se evalúa, se valida, se certifica y después se "publica" en Marketplace.  

## <a name="next-steps"></a>Pasos siguientes

Antes de considerar estos pasos, debe cumplir los [requisitos técnicos y empresariales](./cpp-prerequisites.md) para publicar una máquina virtual en Microsoft Azure Marketplace. 
