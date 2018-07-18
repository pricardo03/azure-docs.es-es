---
title: Usar conectores en Azure Content Moderator para obtener acceso a otras API | Microsoft Docs
description: Obtenga información acerca de cómo obtener acceso a otras API para los flujos de trabajo de Content Moderator mediante conectores.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 06/22/2017
ms.author: sajagtap
ms.openlocfilehash: d8114457e7079ca8772cab830bd011dcddf372f5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380159"
---
# <a name="connectors"></a>Conectores

Los flujos de trabajo de Azure Content Moderator pueden usar otras API, además de Content Moderator API. Se obtiene acceso a otras API mediante un conector en Content Moderator. El conector proporciona un vínculo a las otras API.

Content Moderator incluye estos conectores predeterminados:

* Emotion API
* Face API
* PhotoDNA Cloud Service

![Conectores disponibles en Content Moderator](images/connectors-1.png)

## <a name="verify-your-credentials"></a>Comprobar las credenciales 

Antes de definir un flujo de trabajo, asegúrese de que tiene credenciales válidas para la API de conector que quiera usar:

1.  En el panel de la herramienta de revisión, seleccione **Configuración** > **Conectores**.

  ![Selección de conectores en Content Moderator](images/connectors-2.png)

2.  Seleccione el símbolo **Editar** junto al conector para el que quiera comprobar las credenciales.

  ![Selección del símbolo de editar en Content Moderator](images/connectors-3.png)

3.  Aparece la clave de suscripción. Si realiza ediciones, seleccione **Guardar** cuando haya terminado.

  ![Página de edición de conectores de Content Moderator](images/connectors-4-1.png)
 
## <a name="add-a-connector"></a>Agregar un conector

1.  Antes de agregar un conector, se necesita una clave de suscripción. En el panel de la herramienta de revisión, seleccione **Configuración** > **Credenciales**. Seleccione y copie el valor que se encuentra en el cuadro **Ocp-Admin-Subscription-Key**.

2.  Seleccione **Conectores**. Seleccione uno de los conectores disponibles que se muestran en el panel de la herramienta de revisión. A continuación, seleccione **Conectar**. 

  ![Página para agregar conectores en Content Moderator](images/connectors-5.png)

3.  En el cuadro **Ocp-Admin-Subscription-Key**, pegue la clave que copió. Después, seleccione **Guardar**.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información acerca de cómo usar conectores para [definir flujos de trabajo personalizados](workflows.md).
