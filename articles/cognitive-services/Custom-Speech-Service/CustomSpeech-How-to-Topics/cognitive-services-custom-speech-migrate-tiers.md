---
title: Migración de los planes de tarifa de puntos de Custom Speech Service en Azure | Microsoft Docs
description: Obtenga información sobe cómo migrar las implementaciones de planes S0 y S1 a S2 de puntos de conexión de Custom Speech Service en Cognitive Services.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 07/05/2017
ms.author: panosper
ms.openlocfilehash: 6d92459deb3464cd97c215cbf9a8320628b6da80
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380710"
---
# <a name="migrate-deployments-to-the-new-pricing-model"></a>Migración de implementaciones al nuevo modelo de precios
A partir de julio de 2017, Custom Speech Service ofrece un [nuevo modelo de precios](https://azure.microsoft.com/pricing/details/cognitive-services/custom-speech-service/). El nuevo modelo es *más fácil de entender*, *más sencillo para calcular los costos* y *más flexible* en cuanto al escalado. Para ajustar la escala, Microsoft ha introducido el concepto de unidad de escalado. Cada unidad de escalado puede controlar cinco solicitudes simultáneas. El escalado de solicitudes simultáneas en el modelo anterior estaba establecido en 5 solicitudes simultáneas para el plan S0, y estaba establecido en 12 solicitudes simultáneas para el plan S1. Hemos ampliado estos límites para ofrecer una mayor flexibilidad a sus requisitos de casos de uso.

Si ejecuta un plan S0 o S1 anterior, se recomienda migrar las implementaciones existentes al nuevo plan S2. El nuevo plan S2 cubre los planes S0 y S1. Puede ver las opciones disponibles en la ilustración siguiente:

![Página Elegir plan de tarifa](../../../media/cognitive-services/custom-speech-service/custom-speech-pricing-tier.png)

Microsoft controla la migración de una manera parcialmente automatizada. En primer lugar, para desencadenar la migración, seleccione el nuevo plan de tarifa. A continuación, la implementación se migrará automáticamente.

En la tabla siguiente se muestra la asignación de los planes antiguos para unidades de escalado:

| Nivel: | Solicitudes simultáneas (modelo anterior) | Migración | Solicitudes simultáneas |
|----- | ----- | ---- | ---- |
| S0 |  5   |   => **S2** con 1 unidad de escalado |   5 |
| S1 |  12  |   => **S2** con 3 unidades de escalado |  15 |

Para migrar al nuevo plan, haga lo siguiente:

## <a name="step-1-check-your-existing-deployment"></a>Paso 1: Comprobar la implementación existente
Vaya al [portal de Custom Speech Service](http://cris.ai) y compruebe las implementaciones existentes. En el ejemplo, hay dos implementaciones. Una implementación se ejecuta en un plan S0, y la otra implementación se ejecuta en un plan S1. Las implementaciones se muestran en la columna **Opciones de implementación** de la tabla siguiente:

![Página Implementaciones](../../../media/cognitive-services/custom-speech-service/custom-speech-deployments.png)

## <a name="step-2-select-your-new-pricing-tier-in-the-azure-portal"></a>Paso 2: Seleccionar el nuevo plan de tarifa en Azure Portal
1. Abra una nueva pestaña del explorador e inicie sesión en [Azure Portal](http://ms.portal.azure.com/). 

2. En el panel **Cognitive Services**, en la lista **Suscripciones**, seleccione la suscripción a Custom Speech. 

3. En el panel de su suscripción, seleccione **Plan de tarifa**.

    ![Vínculo Plan de tarifa](../../../media/cognitive-services/custom-speech-service/custom-speech-update-tier.png)

4. En la página **Elegir plan de tarifa**, seleccione **S2 Estándar**. Este es el nuevo plan de tarifa simplificado y más flexible.

5. Seleccione el botón **Seleccionar**.

    ![Página Elegir plan de tarifa](../../../media/cognitive-services/custom-speech-service/custom-speech-update-pricing.png)

## <a name="step-3-check-the-migration-status-in-the-custom-speech-service-portal"></a>Paso 3: Comprobar el estado de migración en el portal de Custom Speech Service
Vuelva al portal de Custom Speech Service y compruebe las implementaciones. (Si la ventana del explorador todavía está abierta, actualícela). 

El estado de la implementación relacionada debe haber cambiado a *Procesando*. Para validar la migración, también puede comprobar la columna **Opciones de implementación**. Allí ahora puede encontrar información sobre las unidades de escalado y el registro. Las unidades de escalado deben reflejar el plan de tarifa anterior. El registro también debe estar activado, como se muestra en la tabla:

![Columna Opciones de implementación](../../../media/cognitive-services/custom-speech-service/custom-speech-deployments-new.png)


> [!NOTE]
> Si tiene problemas durante la migración, póngase en contacto con nosotros.
>

## <a name="next-steps"></a>Pasos siguientes
Para obtener más tutoriales, vea:
* [Crear un modelo acústico personalizado](cognitive-services-custom-speech-create-acoustic-model.md)
* [Crear un modelo de lenguaje personalizado](cognitive-services-custom-speech-create-language-model.md)
* [Creación de un punto de conexión personalizado de voz a texto](cognitive-services-custom-speech-create-endpoint.md)
