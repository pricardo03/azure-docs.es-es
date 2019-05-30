---
title: 'Implementaciones de idioma no admitido: traductor personalizado'
titleSuffix: Azure Cognitive Services
description: Cómo implementar pares de idioma no admitido en convertidor personalizado.
services: cognitive-services
author: rajdeep-in
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 04/24/2019
ms.author: v-pawal
ms.openlocfilehash: 0938ba2e839be603c557cc2a87dd0c5aa1dfe1e3
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66390588"
---
# <a name="unsupported-language-deployments"></a>Implementaciones en lenguajes no compatibles

<!--Custom Translator provides the highest-quality translations possible using the latest techniques in neural machine learning. While Microsoft intends to make neural training available in all languages, there are some limitations that prevent us from being able to offer neural machine translation in all language pairs.-->  

Con la próxima retirada del concentrador del traductor de Microsoft, Microsoft se puede anular la implementación de todos los modelos implementados actualmente a través del concentrador. Muchos de ustedes tienen los modelos implementados en el concentrador cuyos pares de idiomas no se admiten en convertidor personalizado.  No se desea que los usuarios en esta situación no recurrir para convertir su contenido.

Ahora tenemos un proceso que le permite implementar los modelos no compatibles a través del traductor personalizado.  Este proceso le permite continuar traducir el contenido mediante la API de V3 más recientes.  Estos modelos se hospedará hasta que decide anular la implementación de ellas o que el par de idiomas esté disponible en convertidor personalizado.  En este artículo se explica el proceso para implementar modelos con pares de idioma no admitido.

## <a name="prerequisites"></a>Requisitos previos

Para los modelos para ser candidatos para la implementación, debe cumplir los siguientes criterios:
* El proyecto que contiene el modelo debe se han migrado desde el concentrador al traductor personalizado mediante la herramienta de migración.  Puede encontrar el proceso de migración de proyectos y áreas de trabajo [aquí](how-to-migrate.md).
* El modelo debe estar en estado implementado cuando se produce la migración.  
* El par de idioma del modelo debe ser un par de idioma no admitido en el traductor personalizado.  Los pares de idiomas en el que se admite un lenguaje desde o hacia el inglés, pero el par de sí mismo no incluye a inglés, son candidatos para las implementaciones de idioma no admitido.  Por ejemplo, un modelo de concentrador para francés al par de idioma alemán se considera un par de idioma compatible aunque francés para inglés y en alemán son incluso par de idioma no admitido.

## <a name="process"></a>Process
Una vez que se han migrado los modelos del concentrador que son candidatos para la implementación, se puede encontrar yendo a la **configuración** página para el área de trabajo y el desplazamiento hasta el final de la página donde podrá ver una **no compatible Traductor concentrador aprendizajes** sección.  En esta sección solo aparece si tiene proyectos que cumplen los requisitos previos mencionados anteriormente.

![Migración desde Microsoft Translator Hub](media/unsupported-language-deployments/unsupported-translator-hub-trainings.jpg)

Dentro de la **aprendizajes de concentrador no compatible de traductor** página de selección, el **no solicitado aprendizajes** pestaña contiene los modelos que sean aptos para la implementación.  Seleccione los modelos que desea implementar y enviar una solicitud.   Antes de la fecha límite de implementación del 30 de abril, puede seleccionar tantos modelos como desee para la implementación.
 
![Migración desde Microsoft Translator Hub](media/unsupported-language-deployments/unsupported-translator-hub-trainings-list.jpg)

Una vez enviado, el modelo ya no estará disponible en el **no solicitado aprendizajes** pestaña y aparecerá en su lugar en el **solicitado aprendizajes** ficha.  Puede ver los aprendizajes solicitadas en cualquier momento.

![Migración desde Microsoft Translator Hub](media/unsupported-language-deployments/request-unsupported-trainings.jpg) 

## <a name="whats-next"></a>Pasos siguientes

Una vez que se retira el concentrador y todos los modelos se anula, se guardan los modelos que seleccionó para la implementación.  Tiene hasta el 24 de mayo para enviar solicitudes para la implementación de modelos no admitidos.  Implementaremos estos modelos 15 de junio, momento en que sean accesibles a través de la API del traductor V3.  Además, estarán disponibles a través de la API V2 hasta el 1 de julio.  

Para obtener más información sobre las fechas importantes en la degradación de la comprobación de concentrador [aquí](https://www.microsoft.com/translator/business/hub/).
Una vez que se aplicarán los cargos de hospedaje implementados y normales.  Consulte [precios](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/) para obtener más información.  

A diferencia de los modelos estándar personalizado traductor, modelos de centro solo estará disponibles en una sola región, por lo que no se aplicarán cargos de hospedaje de varias regiones.  Una vez implementado, podrá anular la implementación del modelo de concentrador en cualquier momento a través del proyecto migrado de traductor personalizado.

## <a name="next-steps"></a>Pasos siguientes

- [Entrenamiento de un modelo](how-to-train-model.md).
- Comience a utilizar el modelo de traducción personalizado implementado mediante [Microsoft Translator Text API V3](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate?tabs=curl).
