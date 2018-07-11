---
title: Colaborar con otros colaboradores en aplicaciones de LUIS en Azure | Microsoft Docs
description: Obtenga información acerca de cómo colaborar con otros colaboradores en aplicaciones de Language Understanding (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/05/2018
ms.author: v-geberr
ms.openlocfilehash: c0451f7621a3c18dbf365f3a03934924c030092f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35382095"
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

## <a name="set-application-as-public"></a>Establecer la aplicación como pública
Consulte [Public app endpoint access](luis-concept-security.md#public-app-endpoint-access) (Acceso al punto de conexión de la aplicación pública) para obtener más información.

### <a name="transfer-of-ownership"></a>Transferencia de propiedad
Aunque LUIS no admite actualmente la transferencia de propiedad, puede exportar la aplicación y otro usuario de LUIS puede importarla. Puede haber pequeñas diferencias en las puntuaciones de LUIS entre las dos aplicaciones. 
