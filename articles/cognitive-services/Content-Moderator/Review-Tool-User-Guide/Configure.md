---
title: 'Configuración de la herramienta de revisión de Content Moderator: Content Moderator'
titlesuffix: Azure Cognitive Services
description: Configure u recupere su equipo, etiquetas, conectores, flujos de trabajo y credenciales.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 32563b429764a45efbef952310ab6615567d42f0
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55210937"
---
# <a name="configure-review-tool-settings"></a>Configuración de la herramienta de revisión

Mediante la pestaña de configuración del panel de la herramienta de revisión, es fácil definir y cambiar muchos componentes.

![Configuración de revisión de Content Moderator](images/settings-1.png)

## <a name="team-and-subteams"></a>Equipo y subequipos

Administre su equipo y subequipos desde esta pestaña. Solo puede tener un equipo, pero puede [crear varios subequipos](subteams.md) y enviar invitaciones a futuros miembros. Después de que se han enviado las invitaciones, puede supervisarlas, cambiar los permisos de los miembros del equipo e invitar a más usuarios. Una vez que los miembros del equipo han aceptado la invitación, puede asignarlos a subequipos diferentes. Puede definir roles de administrador o revisor para los miembros del equipo: los administradores pueden invitar a otros usuarios, mientras que los revisores no.

![Configuración de equipos de Content Moderator](images/settings-2-team.png)

## <a name="tags"></a>Etiquetas

Aquí es donde puede [definir etiquetas personalizadas](tags.md); para ello, escriba un código, un nombre y una descripción cortos para las etiquetas. Después de haberlas creado, están disponible durante las revisiones. Puede usar etiquetas diferentes para diferentes revisiones con solo activar o desactivar la visibilidad.

![Configuración de etiquetas de Content Moderator](images/settings-3-tags.png)

## <a name="connectors"></a>Conectores

Los flujos de trabajo agregan funcionalidad por medio de conectores para comunicarse con la herramienta de revisión. La herramienta de revisión llama a las API de Content Moderator con el flujo de trabajo predeterminado para moderar el contenido. Cuando se registra en la herramienta de revisión, las credenciales de la API de Moderator se proporcionan automáticamente. También se admite la integración de otras API de conector, siempre y cuando haya un conector disponible. Algunos conectores están disponibles inmediatamente.

La pestaña [Connectors](connectors.md) (Conectores) es donde se administran los conectores. Puede agregar o eliminar conectores y buscar la clave de suscripción de un conector concreto. Haga clic en Connect (Conectar) para agregarlos a sus flujos de trabajo personalizados. 

![Configuración de conectores de Content Moderator](images/settings-4-connectors.png)

## <a name="workflows"></a>Flujos de trabajo

Administre flujos de trabajo desde la pestaña de flujos de trabajo. Puede probar los flujos de trabajo mediante la carga de un archivo de ejemplo. También puede [definir flujos de trabajo personalizados](workflows.md) de imagen y texto mediante los conectores de API disponibles (que se encuentran en la pestaña de conectores). 

![Configuración de flujos de trabajo de Content Moderator](images/settings-5-workflows.png)

## <a name="credentials"></a>Credenciales

En esta pestaña se proporciona acceso rápido a la clave de suscripción de Content Moderator. Esta clave la necesitará para usar las API que se incluye con Content Moderator (Image Moderation, Text Moderation, List Management, Workflow y Review).
 
![Credenciales de Content Moderator](images/settings-6-credentials.png)
