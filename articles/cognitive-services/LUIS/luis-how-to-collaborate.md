---
title: Colaborar con otros colaboradores en aplicaciones de LUIS en Azure | Microsoft Docs
description: Obtenga información acerca de cómo colaborar con otros colaboradores en aplicaciones de Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/05/2018
ms.author: diberry
ms.openlocfilehash: 9ea0269439b3d00bf36186cf2fd5c73311526bec
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2018
ms.locfileid: "39225608"
---
# <a name="collaborate-with-others-on-language-understanding-luis-apps"></a>Colaborar con otros usuarios en aplicaciones de Language Understanding (LUIS)  

Puede colaborar con otros usuarios en su aplicación de LUIS. 

## <a name="owner-and-collaborators"></a>Propietario y colaboradores
Una aplicación tiene un único propietario, pero puede tener muchos colaboradores. 

## <a name="add-collaborator"></a>Agregar un colaborador

Para permitir a los colaboradores editar su aplicación de LUIS, en la página **Configuración** de la aplicación de LUIS, escriba el correo electrónico del colaborador y haga clic en **Add collaborator** (Agregar colaborador).

![Agregar un colaborador](./media/luis-how-to-collaborate/add-collaborator.png)

* Los colaboradores pueden iniciar sesión y editar su aplicación de LUIS mientras usted trabaja en la aplicación. <!--If a collaborator edits the LUIS app, you see a notification at the top of the browser.-->
* Los colaboradores no pueden agregar otros colaboradores.

Consulte [Usuario inquilino de Azure Active Directory](luis-how-to-account-settings.md#azure-active-directory-tenant-user) para más información sobre las cuentas de usuario de Active Directory. 

## <a name="set-application-as-public"></a>Establecer la aplicación como pública
Consulte [Public app endpoint access](luis-concept-security.md#public-app-endpoint-access) (Acceso al punto de conexión de la aplicación pública) para obtener más información.

### <a name="transfer-of-ownership"></a>Transferencia de propiedad
Aunque LUIS no admite actualmente la transferencia de propiedad, puede exportar la aplicación y otro usuario de LUIS puede importarla. Puede haber pequeñas diferencias en las puntuaciones de LUIS entre las dos aplicaciones. 
