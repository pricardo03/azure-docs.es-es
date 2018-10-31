---
title: Pestaña Configuración de oferta de máquina virtual en Cloud Partner Portal para Azure Marketplace | Microsoft Docs
description: Se describe la pestaña Configuración de oferta que se usa en la creación de una oferta de máquina virtual de Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal, virtual machine
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
ms.date: 10/19/2018
ms.author: pbutlerm
ms.openlocfilehash: 43c830bea57a4c6f3b6c56552dbcacaccc75d54e
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2018
ms.locfileid: "49639337"
---
# <a name="virtual-machine-offer-settings-tab"></a>Pestaña Configuración de oferta de máquina virtual

La página **Nueva oferta** para las máquinas virtuales se abre por la primera pestaña denominada **Configuración de oferta**.  Un asterisco (*) anexo al nombre del campo indica que es obligatorio. 

![Página Nueva oferta para máquinas virtuales](./media/publishvm_004.png)

En la pestaña **Configuración de oferta**, debe proporcionar los siguientes campos obligatorios.

|  **Campo**       |     **Descripción**                                                          |
|  ---------       |     ---------------                                                          |
| **Id. de oferta**       | Identificador único (en el perfil de un publicador) de la oferta. Este identificador será visible en las direcciones URL de producto, las plantillas de Azure Resource Manager y los informes de facturación. Tiene una longitud máxima de 50 caracteres, solo puede estar formado por caracteres alfanuméricos en minúscula y guiones (-), pero no puede terminar con un guión. Este campo no se puede modificar una vez lanzada una oferta. <br> Por ejemplo, si Contoso publica una oferta con el Id. de oferta **sample-vm**, se le asigna la dirección URL de Azure Marketplace `https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sample-vm?tab=Overview`. |
| **Publicador**     | Identificador único de la organización en Azure Marketplace. Todas las ofertas se deben asociar al identificador de publicador. Este valor no se puede modificar una vez que se haya guardado la oferta. |
| **Nombre**          | Nombre para mostrar de la oferta. Este nombre se mostrará en Azure Marketplace y en Cloud Partner Portal. Puede tener un máximo de 50 caracteres. Aquí se recomienda incluir un nombre de marca que identifique el producto. No incluya aquí el nombre de la organización a menos que sea así como se comercializa. Si comercializa esta oferta en otros sitios web y publicaciones, asegúrese de que el nombre sea exactamente el mismo en todas las publicaciones. |
|  |  |
 
Haga clic en **Guardar** para guardar lo que ha hecho hasta ahora. En la pestaña siguiente, agregará las [SKU](./cpp-skus-tab.md) a la oferta.
