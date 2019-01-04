---
title: 'Capacidad de recursos para la implementación: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Antes de crear el servicio QnA Maker, debe decidir qué nivel de los servicios anteriores es conveniente en su caso.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 9e197929ce08f4e0c665f96d1c4ddbd382fdfb22
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53084469"
---
# <a name="choosing-capacity-for-your-qna-maker-deployment"></a>Elegir capacidad para la implementación de QnA Maker

El servicio QnA Maker depende de tres recursos de Azure:
1.  App Service (para el tiempo de ejecución)
2.  Azure Search (para almacenar elementos de QnA)
3.  App Insights (opcional, para almacenar registros de chat y telemetría)

Antes de crear el servicio QnA Maker, debe decidir qué nivel de los servicios anteriores es conveniente en su caso. 

Normalmente hay tres parámetros que necesita tener en cuenta:
1. **Rendimiento que se precisa del servicio**: seleccione el [plan de aplicación](https://azure.microsoft.com/pricing/details/app-service/plans/) apropiado para App Service en función de sus necesidades. Puede [escalar o reducir verticalmente](https://docs.microsoft.com/azure/app-service/web-sites-scale) la aplicación. Esto debería influir también en la selección de la SKU de Azure Search SKU; consulte más detalles [aquí](https://docs.microsoft.com/azure/search/search-sku-tier).

2. **Tamaño y número de bases de conocimiento**: Elija la [SKU de Azure Search](https://azure.microsoft.com/pricing/details/search/) adecuada para su escenario. Puede publicar N-1 bases de conocimiento en un nivel particular, donde N se corresponde con los índices máximos permitidos en el nivel. Compruebe también el tamaño y el número máximos de documentos permitidos por cada nivel.

3. **Número de documentos como orígenes**: la SKU gratuita del servicio de administración de QnA Maker limita el número de documentos que puede administrar mediante el portal y las API a 3 (de 1 MB cada uno). La SKU estándar no tiene ningún límite en relación con el número de documentos que puede administrar. Puede consultar más información [aquí](https://aka.ms/qnamaker-pricing).

En la tabla siguiente se proporcionan algunas directrices de alto nivel.

|                        | Administración de QnA Maker | App Service | Azure Search | Limitaciones                      |
| ---------------------- | -------------------- | ----------- | ------------ | -------------------------------- |
| Experimentación        | SKU gratuita             | Nivel Gratis   | Nivel Gratis    | Publicar hasta 2 KB; tamaño de 50 MB  |
| Entorno de Desarrollo/pruebas   | SKU Estándar         | Compartido      | Básica        | Publicaciones de hasta 14 KB; tamaño de 2 GB    |
| Entorno de producción | SKU Estándar         | Básica       | Estándar     | Publicar hasta 49 KB; tamaño de 25 GB |

Para actualizar la pila de QnA Maker, vea [Actualización del servicio QnA Maker](../How-To/upgrade-qnamaker-service.md).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Actualización del servicio QnA Maker](../How-To/upgrade-qnamaker-service.md)
