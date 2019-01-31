---
title: 'Administración de quipos y subequipos en la API de Content Moderator: Content Moderator'
titlesuffix: Azure Cognitive Services
description: Aprenda a usar equipos y subequipos en la API de Content Moderator de Cognitive Services.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: a36da1787d5abd5f7dc1455823be55f1880c7ba5
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55227138"
---
# <a name="manage-review-teams-and-subteams"></a>Administración de equipos y subequipos de revisión

Para usar Content Moderator, es necesario crear un equipo. Al registrarse y asignar un nombre a su equipo, este se convierte en el equipo predeterminado. Solo puede tener un equipo en la herramienta de revisión. Sin embargo, puede crear varios subequipos. Los subequipos son útiles para crear equipos de escalamiento o equipos dedicados a revisar categorías específicas de contenido. Por ejemplo, puede que quiera enviar contenido para adultos a un equipo diferente para una revisión más profunda.

En este tema se explica cómo crear subequipos y asignar rápidamente revisiones sobre la marcha. Sin embargo, puede usar [flujos de trabajo](workflows.md) para asignar revisiones según criterios específicos.

## <a name="go-to-the-teams-setting"></a>Ir a la configuración del equipo

Para comenzar a crear un subequipo, seleccione la opción **Teams** (Equipos) en Settings (Configuración).

![Configuración del equipo](images/0-teams-1.png)

## <a name="create-subteams"></a>Crear subequipos

El equipo predeterminado contiene todos los posibles revisores; los subequipos son subconjuntos del equipo predeterminado. No puede asignar a nadie a un subequipo si no es ya miembro del equipo predeterminado, así que ahora lo que debe hacer es agregar revisores al equipo predeterminado. Haga clic en el botón Invite (Invitar) en la página del equipo.

![Invitar a usuarios](images/invite-users.png)

### <a name="create-a-subteam"></a>Creación de un subequipo
Desplácese hacia abajo de la página Team (Equipo) hasta la sección Subteam (Subequipo). Haga clic en el botón Add Subteam (Agregar subequipo). 

![Agregar subequipo](images/1-teams-1.png)

### <a name="name-your-subteam"></a>Asignación de un nombre al subequipo
Escriba el nombre del subequipo en el cuadro de diálogo y, a continuación, haga clic en Save (Guardar).

![Nombre del subequipo](images/1-Teams-2.PNG)

### <a name="assign-members-from-your-default-team"></a>Asigne miembros del equipo predeterminado.
Haga clic en el botón Add Member (Agregar miembro) para asignar miembros del equipo predeterminado a uno o varios subequipos. Solo puede agregar usuarios existentes a un subequipo. Para agregar nuevos usuarios que no están en la herramienta de revisión, invíteles mediante el botón "Invite" (Invitar) en la página de configuración del equipo.

![Asignar miembros del subequipo](images/1-Teams-3.PNG)

## <a name="assign-reviews-to-your-subteams"></a>Asignar revisiones a los subequipos

Una vez que ha creado los subequipos y asignado los miembros del equipo, puede empezar a asignar las revisiones de texto e imagen a esos subequipos. Para ello, se usa la ventana de revisión.
Si quiere asignar una imagen individual a un subequipo, haga clic en los puntos suspensivos en la esquina superior derecha de la imagen, seleccione Move to (Mover a) y elija un subequipo.

![Asignar una revisión de imagen a un subequipo](images/3-review-image-subteam-1.png)

## <a name="switch-between-subteams-to-review-assigned-content"></a>Cambiar entre subequipos para revisar contenido asignado

Si es miembro de uno o varios subequipos, puede cambiar entre ellos desde el panel de la herramienta de revisión. Para ver todas revisiones pendientes actuales que pertenecen a un subequipo, seleccione Choose Subteam (Elegir subequipo) en la pestaña Image (Imagen).

![Cambiar entre subequipos](images/3-review-image-subteam-2.png)
