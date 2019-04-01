---
title: Configurar opciones de herramienta de revisión - Content Moderator
titlesuffix: Azure Cognitive Services
description: Utilice la herramienta de revisión para configurar o recuperar su equipo, etiquetas, conectores, flujos de trabajo y las credenciales para Content Moderator.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 03/15/2019
ms.author: sajagtap
ms.openlocfilehash: f88ccbabc925b651abbc06f571a9d4220ed8aeb2
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2019
ms.locfileid: "58756513"
---
# <a name="configure-the-review-tool"></a>Configurar la herramienta de revisión

El [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com) tiene varias características importantes que puede tener acceso a través de la **configuración** menú en el panel.

![Content Moderator Review demasiado menú Configuración](images/settings-1.png)

## <a name="manage-team-and-subteams"></a>Administrar el equipo y subequipos

El **equipo** pestaña le permite administrar su equipo y subequipos&mdash;grupos de usuarios que pueden recibir una notificación cuando determinados [revisiones humanas](../review-api.md#reviews) se inician. Solo puede tener un equipo (que se crea al registrarse con la herramienta de revisión), pero puede crear varios subequipos. El administrador del equipo puede invitar a miembros, establezca sus permisos y asignarlos a distintos subequipos.

![Revise la configuración del equipo de herramientas](images/settings-2-team.png)

Los subequipos son útiles para crear equipos de escalamiento o equipos dedicados a revisar categorías específicas de contenido. Por ejemplo, podría enviar contenido para adultos en un equipo independiente para su revisión.

Esta sección explica cómo crear subequipos y asignar rápidamente las revisiones sobre la marcha. Sin embargo, puede usar [flujos de trabajo](workflows.md) para asignar revisiones según criterios específicos.

### <a name="create-a-subteam"></a>Creación de un subequipo

Vaya a la **subequipos** sección y haga clic en **Enreda agregar**. Escriba el nombre de equipo secundario en el cuadro de diálogo y haga clic en **guardar**.

![Nombre del subequipo](images/1-Teams-2.PNG)

#### <a name="invite-teammates"></a>Invitar a compañeros de equipo

No se puede asignar un usuario a un equipo secundario si aún no están un miembro del equipo de forma predeterminada, por lo que deberá agregar revisores al equipo de forma predeterminada, en primer lugar. Haga clic en **invitar** en el **equipo** ficha.

![Invitar a usuarios](images/invite-users.png)

#### <a name="assign-teammates-to-subteam"></a>Asignar los miembros del equipo para subteam

Haga clic en el **Add Member** botón para asignar los miembros del equipo de forma predeterminada a uno o más equipos secundarios. Solo puede agregar usuarios existentes a un subequipo. Para agregar nuevos usuarios que no están en la herramienta de revisión, invíteles mediante el botón "Invite" (Invitar) en la página de configuración del equipo.

![Asignar miembros del subequipo](images/1-Teams-3.PNG)

### <a name="assign-reviews-to-subteams"></a>Asignar las revisiones en los equipos secundarios

Una vez que ha creado sus equipos secundarios y asigna los miembros, puede comenzar a asignar contenido [revisa](../review-api.md#reviews) a esos equipos secundarios. Esto se realiza desde el **revisión** pestaña del sitio.
Para asignar el contenido a un equipo secundario, haga clic en el botón de puntos suspensivos en la esquina superior derecha, seleccione **mover a**y seleccione un equipo secundario.

![Asignar una revisión de imagen a un subequipo](images/3-review-image-subteam-1.png)

### <a name="switch-between-subteams"></a>Cambiar entre subequipos

Si es miembro de más de un grupo, puede cambiar entre los subequipos para cambiar qué revisiones de contenido se muestran al usuario. En el **revisión** pestaña, seleccione el menú desplegable con la etiqueta **predeterminado** y seleccione **Enreda elija**. Puede ver las revisiones de contenido para subequipos diferentes, pero solo aquellos de los cuales el miembro.

![Cambiar entre subequipos](images/3-review-image-subteam-2.png)

## <a name="tags"></a>Etiquetas

El **etiquetas** ficha permite definir las etiquetas de moderación personalizadas además de las etiquetas de moderación de dos valores predeterminados&mdash;**isadult** (**un**) y **isracy**  (**r**). Cuando se crea una etiqueta personalizada, estará disponible en las revisiones junto con las etiquetas predeterminadas. Puede cambiar qué etiquetas se mostrarán en las revisiones cambiando su configuración de visibilidad.

![Ver las etiquetas, incluidas las casillas de verificación "Está visible"](images/tags-4-disable.png)

### <a name="create-custom-tags"></a>Crear etiquetas personalizadas

Para crear una nueva etiqueta, debe escribir un código corto, nombre y descripción en los campos correspondientes.

- **Breve código**: Escriba un código de dos letras para la etiqueta. Ejemplo: **cb**
- **Nombre**: Escriba un nombre de etiqueta a corto y descriptivo en minúsculas, sin espacios en blanco. Ejemplo: **isbullying**.
- **Descripción**: (opcional) escriba una descripción del tipo de contenido que los destinos de la etiqueta. Ejemplo: **Representaciones o instancias de ciberseguridad pueden ser la intimidación**.

Haga clic en **agregar** para agregar una etiqueta y haga clic en **guardar** cuando haya terminado de crear etiquetas.

![Herramienta de revisión de crear el nuevo cuadro de diálogo de etiqueta](images/settings-3-tags.png)

### <a name="delete-tags"></a>Eliminar etiquetas

Puede eliminar etiquetas personalizadas, seleccione el icono de Papelera junto a sus entradas en la lista de etiquetas, pero no se puede eliminar las etiquetas predeterminadas.

## <a name="connectors"></a>Conectores

El **conectores** pestaña le permite administrar sus conectores, que son complementos específicos del servicio que pueden procesar el contenido de distintas maneras como parte del contenido [flujos de trabajo](../review-api.md#workflows).

El conector de forma predeterminada al crear un flujo de trabajo es el conector de Content Moderator, que puede marcar contenido como **para adultos** o **subido de tono**, busque palabras soeces y así sucesivamente. Sin embargo, puede usar otros conectores, se mencionan aquí, siempre que tengan credenciales de sus respectivos servicios (para usar el conector de Face API, por ejemplo, deberá obtener un [Face API](https://docs.microsoft.com/azure/cognitive-services/face/overview) clave de suscripción).

El [herramienta de revisión](./human-in-the-loop.md) incluye los conectores siguientes:

- Emotion API
- Face API
- PhotoDNA Cloud Service
- Text Analytics API

### <a name="add-a-connector"></a>Agregar un conector

Para agregar un conector (y que esté disponible para su uso en contenido [flujos de trabajo](../review-api.md#workflows)), seleccione la **Connect** botón. En el cuadro de diálogo siguiente, escriba la clave de suscripción para ese servicio. Cuando haya terminado, el conector nuevo debe aparecer en la parte superior de la página.

![Configuración de conectores de Content Moderator](images/settings-4-connectors.png)

## <a name="workflows"></a>Flujos de trabajo

El **flujos de trabajo** pestaña le permite administrar su [flujos de trabajo](../review-api.md#workflows). Flujos de trabajo son los filtros basados en la nube para el contenido, y trabajar con conectores para ordenar el contenido de varias maneras y tomar las medidas adecuadas. En este caso, puede definir, editar y probar los flujos de trabajo. Consulte [definir y usar flujos de trabajo](Workflows.md) para obtener instrucciones sobre cómo hacerlo.

![Configuración de flujos de trabajo de Content Moderator](images/settings-5-workflows.png)

## <a name="credentials"></a>Credenciales

El **credenciales** ficha proporciona acceso rápido a su clave de suscripción de Content Moderator, que necesitará para tener acceso a cualquiera de los servicios de moderación de una llamada de REST o SDK de cliente.

![Credenciales de Content Moderator](images/settings-6-credentials.png)

### <a name="use-external-credentials-for-workflows"></a>Uso de credenciales externas para los flujos de trabajo

El [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com) genera una clave de prueba gratuita para los servicios de Azure Content Moderator cuando se suscriba, pero también puede configurarlo para usar una existente de la clave de su cuenta de Azure. Esto se recomienda para escenarios a gran escala, como las claves de prueba gratuita poseen límites de uso estricto ([precios y límites](https://azure.microsoft.com/pricing/details/cognitive-services/content-moderator/)).

Si ha creado un [recursos Content Moderator](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator) en Azure, navegue hasta ella en Azure portal y seleccione el **claves** hoja. Copie una de las claves.

![Claves de Content Moderator en Azure Portal](images/credentials-azure-portal-keys.PNG)

En el [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com)del **credenciales** ficha, vaya a la **Workflow Settings** panel, seleccione **editar**y pegue la clave en el **Ocp-Apim-Subscription-Key** campo. Ahora, los flujos de trabajo que llaman a las API de moderación usará su credencial de Azure.

> [!NOTE]
> Los otros dos campos en el **Workflow Settings** panel son para las listas personalizadas de término y la imagen. Consulte la [términos personalizado](../try-terms-list-api.md) o [imágenes personalizadas](../try-image-list-api.md) guías para obtener información acerca de estos.

### <a name="use-your-azure-account-with-the-review-apis"></a>Use su cuenta de Azure con las API de revisión

Para usar la clave de Azure con las API de revisión, deberá recuperar su identificador de recurso. Vaya al recurso de Content Moderator en Azure portal y seleccione el **propiedades** hoja. Copie el valor de Id. de recurso y péguelo en el **los identificadores de recursos en la lista blanca** campo de la herramienta de revisión **credenciales** ficha.

![Id. de recurso de Content Moderator en Azure Portal](images/credentials-azure-portal-resourceid.PNG)

Si ha especificado la clave de suscripción en ambos lugares, no se usará la clave de prueba que se incluye con su cuenta de la herramienta de revisión, pero seguirá estando disponible.

## <a name="next-steps"></a>Pasos siguientes

Siga el [inicio rápido de herramienta de revisión](../quick-start.md) para empezar a usar la herramienta de revisión en escenarios de moderación de contenido.