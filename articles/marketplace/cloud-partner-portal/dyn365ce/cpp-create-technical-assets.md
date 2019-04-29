---
title: 'Creación de recursos técnicos de Dynamics 365 for Customer Engagement: Azure Marketplace | Microsoft Docs'
description: Cree los recursos técnicos para una oferta de aplicación de Dynamics 365 for Customer Engagement.
services: Dynamics 365 for Customer Engagement, Azure, Marketplace, Cloud Partner Portal, AppSource
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
ms.date: 12/29/2018
ms.author: pbutlerm
ms.openlocfilehash: 50c4fd512206cdf17ebb555acb88de2a3f74c2bd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61472675"
---
# <a name="create-technical-assets-for-azure-application-offer"></a>Creación de recursos técnicos para una oferta de aplicación de Azure

Normalmente, se desarrollan soluciones mediante el [SDK para aplicaciones de Dynamics 365 for Customer Engagement](https://docs.microsoft.com/dynamics365/customer-engagement/developer/get-started-sdk).  Las soluciones adoptan una variedad de formas, como se describe en [Modelos de programación de Dynamics 365 for Customer Engagement](https://docs.microsoft.com/dynamics365/customer-engagement/developer/programming-models).  Elija el formato que mejor se ajusta a sus requisitos de la solución.  Al desarrollar una solución, hay una serie de problemas que debe afrontar como, por ejemplo, las opciones de extensibilidad, los componentes de la solución y la compatibilidad entre versiones.  Para más información, consulte [Introducción a soluciones](https://docs.microsoft.com/dynamics365/customer-engagement/developer/introduction-solutions).

La mayoría de las soluciones de Dynamics 365 publicadas en AppSource son aplicaciones administradas que se distribuyen como archivos de paquete.


## <a name="creating-and-storing-the-package"></a>Creación y almacenamiento del paquete

La documentación paralela sobre creación de ofertas de Dynamics 365 for Customer Engagement se encuentra en la sección [Publicación de la aplicación en AppSource](https://docs.microsoft.com/dynamics365/customer-engagement/developer/publish-app-appsource).  Los siguientes temas contenidos detallan cómo crear el archivo de paquete de la solución y cómo cargarlo en el almacenamiento de Azure:

- [Paso 4: Creación de un paquete de AppSource para la aplicación](https://docs.microsoft.com/dynamics365/customer-engagement/developer/create-package-app-appsource): explica cómo crear un archivo comprimido (zip) que representa la aplicación administrada y que contiene una carpeta de recursos de la solución, código DLL personalizado, archivo de información tipo MIME, icono del paquete de AppSource y el archivo de contenido (XML).
- [Paso 5: Almacenamiento del paquete de AppSource en Azure Storage y generación de una dirección URL con una clave SAS](https://docs.microsoft.com/dynamics365/customer-engagement/developer/store-appsource-package-azure-storage): explica cómo almacenar un archivo de paquete de AppSource en una cuenta de Microsoft Azure Blob Storage y cómo usar una clave de firma de acceso compartido (SAS) para compartir el archivo de paquete. El archivo de paquete se recupera desde su ubicación de Azure Storage para la certificación y, posteriormente, para las evaluaciones gratuitas y la publicación en AppSource.


## <a name="next-steps"></a>Pasos siguientes

Si aún no lo ha hecho, [cree su oferta de Dynamics 365 for Customer Engagement](./cpp-create-offer.md).  Después, estará listo para [publicar su oferta](./cpp-publish-offer.md).
