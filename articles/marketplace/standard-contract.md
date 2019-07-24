---
title: Contrato estándar | Azure
description: Contrato estándar en Azure Marketplace y AppSource
services: Azure, Marketplace, Compute, Storage, Networking
author: qianw211
ms.service: marketplace
ms.topic: article
ms.date: 07/05/2019
ms.author: ellacroi
ms.openlocfilehash: 80c157423572d356026f257e81d52650ce01d3e8
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2019
ms.locfileid: "67620381"
---
# <a name="standard-contract"></a>Contrato estándar

Para simplificar el proceso de adquisición para los clientes y reducir la complejidad legal para los proveedores de software, Microsoft ofrece una plantilla de Contrato estándar con el fin de ayudar a facilitar una transacción en Marketplace. En lugar de crear términos y condiciones personalizados, los editores de Azure Marketplace pueden ofrecer su software en virtud del Contrato estándar, que los clientes solo tienen que revisar y aceptar una vez. El Contrato estándar puede encontrarse aquí: [https://go.microsoft.com/fwlink/?linkid=2041178](https://go.microsoft.com/fwlink/?linkid=2041178). 

Los términos y condiciones de una oferta que se definen en la pestaña de Marketplace al crear una oferta en Cloud Partner Portal. Para habilitar la opción Contrato estándar, cambie la configuración a Sí.

![Habilitar la opción Contrato estándar](media/marketplace-publishers-guide/standard-contract.png)

>[!Note] 
>Si decide usar el Contrato estándar, siguen siendo necesarios términos y condiciones independientes para el canal del [Proveedor de soluciones en la nube](./cloud-solution-providers.md).

## <a name="standard-contract-amendments"></a>Modificaciones del Contrato estándar

Las modificaciones del Contrato estándar permiten a los anunciantes seleccionar el contrato estándar por motivos de simplicidad y personalizar los términos para su producto o empresa.  Los clientes solo necesitan revisar las modificaciones del contrato, si ya han revisado y aceptado el Contrato estándar de Microsoft.

Hay dos tipos de modificaciones para los anunciantes de Azure Marketplace:

* Modificaciones universales: Estas modificaciones se aplican universalmente al Contrato estándar para todos los clientes. Las modificaciones universales se muestran a cada cliente del producto en el flujo de compra.

![Modificaciones universales](media/marketplace-publishers-guide/universal-amendaments.png)

* Modificaciones personalizadas: Azure Marketplace también tiene una disposición para enmiendas personalizadas dirigidas a los inquilinos. Son modificaciones especiales al Contrato estándar que están dirigidas únicamente a ciertos clientes. Los anunciantes pueden elegir al inquilino al que desean dirigirse. Los clientes de ese inquilino podrían comprar el producto de acuerdo con el Contrato estándar y las modificaciones específicas.

![Modificaciones personalizadas](media/marketplace-publishers-guide/custom-amendaments.png)

>[!Note] 
>Los clientes específicos con modificaciones personalizadas también obtendrán la modificación universal a los términos estándar durante la compra.

>[!Note]
>Los siguientes tipos de oferta admiten las modificaciones de Contrato estándar: Las aplicaciones de Azure (plantillas de solución y aplicaciones administradas), máquinas virtuales, contenedores, aplicaciones de contenedor.

### <a name="customer-experience"></a>Experiencia de los clientes

Durante el proceso de compra en Azure Portal, los clientes podrán ver las condiciones asociadas con el producto como el Contrato estándar de Microsoft y las modificaciones.

![Experiencia de cliente de Azure Portal.](media/marketplace-publishers-guide/ibiza-customer-experience.png)

### <a name="api"></a>API

Los clientes pueden usar `Get-AzureRmMarketplaceTerms` para recuperar las condiciones de una oferta y aceptarla. El Contrato estándar y las modificaciones asociadas se devolverán en la salida del cmdlet.

---
