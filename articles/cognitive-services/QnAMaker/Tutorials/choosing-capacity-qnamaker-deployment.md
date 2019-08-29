---
title: 'Capacidad de recursos para la implementación: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Antes de crear el servicio QnA Maker, debe decidir qué nivel de los servicios anteriores es conveniente en su caso.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 08/20/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 2b2879a1ebcf01a0433873e8da77c4ce55a078cd
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2019
ms.locfileid: "69647010"
---
# <a name="choosing-capacity-for-your-qna-maker-deployment"></a>Elegir capacidad para la implementación de QnA Maker

El servicio QnA Maker depende de tres recursos de Azure:
1.  App Service (para el tiempo de ejecución)
2.  Azure Search (para almacenar y buscar Preguntas y respuestas)
3.  App Insights (opcional, para almacenar registros de chat y telemetría)

Antes de crear el servicio QnA Maker, debe decidir qué nivel de los servicios anteriores es conveniente en su caso. 

Normalmente hay tres parámetros que necesita tener en cuenta:

1. **Rendimiento que se precisa del servicio**: seleccione el [plan de aplicación](https://azure.microsoft.com/pricing/details/app-service/plans/) apropiado para App Service en función de sus necesidades. Puede [escalar o reducir verticalmente](https://docs.microsoft.com/azure/app-service/manage-scale-up) la aplicación. Esto debería influir también en la selección de la SKU de Azure Search SKU; consulte más detalles [aquí](https://docs.microsoft.com/azure/search/search-sku-tier).

1. **Tamaño y número de bases de conocimiento**: Elija la [SKU de Azure Search](https://azure.microsoft.com/pricing/details/search/) adecuada para su escenario. Puede publicar N-1 bases de conocimiento en un nivel particular, donde N se corresponde con los índices máximos permitidos en el nivel. Compruebe también el tamaño y el número máximos de documentos permitidos por cada nivel.

    Por ejemplo, si su nivel tiene permitidos 15 índices, puede publicar 14 bases de conocimiento (1 índice por cada base de conocimientos publicada). El decimoquinto índice se utiliza para crear y probar todas las bases de conocimiento. 

1. **Número de documentos como orígenes**: la SKU gratuita del servicio de administración de QnA Maker limita el número de documentos que puede administrar mediante el portal y las API a 3 (de 1 MB cada uno). La SKU estándar no tiene ningún límite en relación con el número de documentos que puede administrar. Puede consultar más información [aquí](https://aka.ms/qnamaker-pricing).

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
