---
title: 'Exportación o eliminación de datos: Content Moderator'
titlesuffix: Azure Cognitive Services
description: Obtenga información acerca de cómo exportar o eliminar datos en Content Moderator.
services: cognitive-services
author: v-jaswel
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: conceptual
ms.date: 05/25/2018
ms.author: v-jaswel
ms.openlocfilehash: 9e671fa9735406ec425b105bb121d3a029ef19dc
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47227238"
---
# <a name="export-or-delete-user-data-in-content-moderator"></a>Exportación o eliminación de datos de usuario en Content Moderator

Content Moderator recopila los datos del usuario para prestar el servicio, pero los clientes tienen control total sobre la visualización, la exportación y la eliminación de sus datos mediante la [interfaz de usuario de revisión](http://contentmoderator.cognitive.microsoft.com/) y las [API](https://docs.microsoft.com/azure/cognitive-services/content-moderator/api-reference).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Para más información acerca de cómo exportar y eliminar datos del usuario en Content Moderator, consulte la siguiente tabla.

| Datos | Operación de exportación | Elimina una operación |
| ---- | ---------------- | ---------------- |
| Información de cuenta (claves de suscripción) | N/D | Eliminación mediante Azure Portal (suscripciones de Azure). O utilice el botón **Eliminar equipo** en la página de configuración de equipos de la [interfaz de usuario de revisión](http://contentmoderator.cognitive.microsoft.com/). |
| Imágenes para coincidencia personalizada | [Obtener identificadores de imagen](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f676). Las imágenes se almacenan en un formato de algoritmo hash propietario unidireccional, y no hay manera de extraer las imágenes reales. | [Eliminar todas las imágenes](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f686). O bien, elimine el recurso de Content Moderator con Azure Portal. |
| Condiciones para la coincidencia personalizada | [Obtener todos los términos](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67e) | [Eliminar todos los términos](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67d). O bien, elimine el recurso de Content Moderator con Azure Portal. |
| Etiquetas | N/D | Utilice el icono **Eliminar** disponible para cada etiqueta en la página de configuración de etiquetas de la interfaz de usuario de revisión. O utilice el botón **Eliminar equipo** en la página de configuración de equipos de la [interfaz de usuario de revisión](http://contentmoderator.cognitive.microsoft.com/). |
| Revisiones | [Obtener la revisión](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c2) | Use el botón **Eliminar equipo** en la página de configuración de equipos de la [interfaz de usuario de revisión](http://contentmoderator.cognitive.microsoft.com/).
| Usuarios | N/D | Utilice el icono **Eliminar** disponible para cada usuario en la página de configuración de equipos de la [interfaz de usuario de revisión](http://contentmoderator.cognitive.microsoft.com/). O utilice el botón **Eliminar equipo** en la página de configuración de equipos de la [interfaz de usuario de revisión](http://contentmoderator.cognitive.microsoft.com/). |

