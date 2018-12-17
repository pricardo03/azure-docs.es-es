---
title: Límites y cuotas del servicio para Azure Batch AI | Microsoft Docs
description: Obtenga información sobre las restricciones, los límites y las cuotas de Azure Batch AI predeterminados y cómo solicitar aumentos de la cuota.
services: batch-ai
documentationcenter: ''
author: johnwu10
manager: jeconnoc
editor: ''
ms.service: batch-ai
ms.topic: article
ms.date: 08/08/2018
ms.author: danlep
ms.custom: mvc
ROBOTS: NOINDEX
ms.openlocfilehash: 16032ec5ba1e613462f92b86281ce93153b70923
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2018
ms.locfileid: "53409724"
---
# <a name="batch-ai-service-quotas-and-limits"></a>Límites y cuotas del servicio Batch AI

[!INCLUDE [batch-ai-retiring](../../includes/batch-ai-retiring.md)]

Al igual que en otros servicios de Azure, existen límites en determinados recursos asociados con el servicio Batch AI. En Batch AI, estos límites son cuotas predeterminadas que se aplica en el nivel de suscripción para cada región donde el servicio esté [disponible](https://azure.microsoft.com/global-infrastructure/services/). En este artículo se describen esos valores predeterminados y cómo solicitar un aumento de la cuota.

Tenga en cuenta estas cuotas al diseñar y escalar verticalmente las cargas de trabajo de Batch AI. Por ejemplo, si su clúster no alcanza el número objetivo de nodos especificado, es posible que se haya alcanzado el límite de núcleos de Batch AI de la suscripción.

Si planea ejecutar cargas de trabajo de producción en Batch AI, es posible que tenga que aumentar el valor predeterminado de una o varias de las cuotas.

> [!NOTE]
> Una cuota es un límite de crédito, no una garantía de capacidad. Si tiene necesidades de capacidad a gran escala, póngase en contacto con el soporte técnico de Azure.
> 
> 

## <a name="resource-quotas"></a>Cuotas de recursos

[!INCLUDE [azure-batch-ai-limits](../../includes/azure-batch-ai-limits.md)]

## <a name="other-limits"></a>Otros límites

Los siguientes son límites estrictos, que no se pueden superar una vez alcanzados.

| **Recurso** | **Límite máximo** |
| --- | --- |
| Número máximo de áreas de trabajo por grupo de recursos | 800 |
| Tamaño máximo del clúster | 100 nodos |
| Número máximo de procesos MPI de GPU por nodo | 1-4 |
| Número máximo de trabajos de GPU por nodo | 1-4 |
| Duración máxima del trabajo | 7 días<sup>1</sup> |
| Número máximo de servidores de parámetro por nodo | 1 |

<sup>1</sup> La duración máxima hace referencia a la hora de comienzo de un trabajo de ejecución y cuándo finaliza. Los trabajos completados se mantienen de forma indefinida; los datos de los trabajos no completados dentro de la duración máxima no son accesibles.

## <a name="view-batch-ai-quotas"></a>Visualización de las cuotas de Batch AI

Vea las cuotas de suscripción de Batch AI actuales en [Azure Portal][portal].

1. En el panel de la izquierda, haga clic en **Todos los servicios**. Después, busque **Batch AI** y haga clic para abrir el servicio.
2. Haga clic en **Uso y cuotas** en el menú de Batch AI.
3. Seleccione su suscripción para ver los límites de cuota.

## <a name="increase-a-batch-ai-cores-quota"></a>Aumento de la cuota de núcleos de Batch AI

Siga estos pasos para solicitar un aumento de la cuota para la suscripción de Batch AI con [Azure Portal][portal]. 

1. En el panel de la izquierda, haga clic en **Todos los servicios**. Después, busque **Batch AI** y haga clic para abrir el servicio.
2. Haga clic en **Nueva solicitud de soporte técnico** en el menú de Batch AI.
3. En **Aspectos básicos**:
   
     a. **Tipo de problema** > **Cuota**
   
    b. **Suscripción** > seleccione la suscripción.
   
    c. **Tipo de cuota** > **Batch AI**
   
    d. **Plan de soporte técnico** > seleccione el plan de soporte técnico.

    Haga clic en **Next**.
4. En **Problema**:
   
     a. Seleccione una de las opciones en **Gravedad** según su [impacto en el negocio][support_sev].
   
    b. En **Detalles de la cuota**, especifique la ubicación, el tipo de cuota y el tipo de recurso. Especifique el límite nuevo que quiere solicitar. Haga clic en **Guardar y continuar**.

    c. Opcional: cargue los archivos pertinentes con más información sobre el motivo del aumento.
   
    Haga clic en **Next**.
5. En **Información de contacto**:
   
     a. Seleccione un valor en **Método de contacto preferido**.
   
    b. Compruebe y especifique los detalles de contacto necesarios.
   
    Haga clic en **Crear** para enviar la solicitud de soporte técnico.

Una vez que haya enviado la solicitud de soporte técnico, el servicio de soporte técnico de Azure se comunicará con usted. Se puede tardar hasta dos días laborables en completar la solicitud.


## <a name="next-steps"></a>Pasos siguientes

Después de familiarizarse con los límites de cuota, consulte los artículos siguientes para comenzar a usar Batch AI.

> [!div class="nextstepaction"]
> [Batch AI quick start tutorial](quickstart-tensorflow-training-cli.md)(Tutorial de inicio rápido de Batch AI)
> [Batch AI recipes](https://github.com/Azure/BatchAI/tree/master/recipes)(Recetas de Batch AI)
> [Learn more about Batch AI resources](resource-concepts.md) (Más información sobre los recursos de Batch AI)

[portal]: https://portal.azure.com
[support_sev]: http://aka.ms/supportseverity