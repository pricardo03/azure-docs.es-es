---
title: Colaboración
titleSuffix: Language Understanding - Azure Cognitive Services
description: Las aplicaciones de LUIS requieren un único propietario y colaboradores opcionales, lo que permite a varias personas crear una sola aplicación.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 04/01/2019
ms.author: diberry
ms.openlocfilehash: 34ae47723762215fdea99362419c3875dc2e5942
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60812880"
---
# <a name="collaborating-with-other-authors"></a>Colaboración con otros creadores

Las aplicaciones de LUIS requieren un único propietario y colaboradores opcionales, lo que permite a varias personas crear una sola aplicación.

## <a name="luis-account"></a>Cuenta de LUIS
Se asocia una cuenta de LUIS con una cuenta de [Microsoft Live](https://login.live.com/). Cada cuenta de LUIS recibe una [clave de creación](luis-concept-keys.md#authoring-key) gratuita para crear todas las aplicaciones de LUIS a las que tiene acceso la cuenta. 

Una cuenta de LUIS puede tener muchas aplicaciones de LUIS.

Consulte [Usuario inquilino de Azure Active Directory](luis-how-to-collaborate.md#azure-active-directory-tenant-user) para más información sobre las cuentas de usuario de Active Directory. 

## <a name="luis-app-owner"></a>Propietario de la aplicación de LUIS

La cuenta que se crea una aplicación es el propietario y cada aplicación tiene un único propietario. El propietario se muestra en la aplicación **[configuración](luis-how-to-collaborate.md)** página. El propietario puede eliminar la aplicación recibir correo electrónico cuando llega a la cuota de punto de conexión 75% del límite mensual. 

## <a name="authorization-roles"></a>Roles de autorización
LUIS no es compatible con distintos roles para propietarios y colaboradores, salvo por una excepción. La cuenta de propietario es la única que puede eliminar la aplicación.

Si está interesado en el control del acceso al modelo, considere la posibilidad de segmentar el modelo en aplicaciones de LUIS más pequeñas, donde cada aplicación más pequeña tenga un conjunto más limitado de colaboradores. Use [Dispatch](https://aka.ms/dispatch-tool) para permitir que una aplicación principal de LUIS administre la coordinación entre las aplicaciones primarias y secundarias.

## <a name="transfer-ownership"></a>Transferencia de la propiedad
LUIS no proporciona transferencias de propiedad, aunque cualquier colaborador puede exportar la aplicación y, después, crear una aplicación importándola. Tenga en cuenta que la aplicación nueva tiene un identificador de aplicación diferente. La aplicación nueva se debe entrenar y publicar, y el punto de conexión nuevo se debe usar.

## <a name="luis-app-collaborators"></a>Colaboradores de aplicaciones de LUIS
Un propietario de aplicación puede agregar colaboradores a una aplicación. El propietario tiene que agregar la dirección de correo electrónico del colaborador en la **[configuración](luis-how-to-collaborate.md)** de la aplicación. El colaborador tiene acceso completo a la aplicación. Si el colaborador elimina la aplicación, esta se quitará de la cuenta del colaborador, pero permanecerá en la cuenta del propietario. 

Si quiere compartir varias aplicaciones con colaboradores, se debe agregar a cada aplicación el correo electrónico del colaborador. 

## <a name="managing-multiple-authors"></a>Administrar varios autores
El sitio web de [LUIS](luis-reference-regions.md#luis-website) actualmente no ofrece ninguna creación a nivel de transacción. Puede permitir que los autores trabajen con versiones independientes de una versión base. En las siguientes secciones se describen dos métodos diferentes.

## <a name="manage-multiple-versions-inside-the-same-app"></a>Administrar varias versiones dentro de la misma aplicación
Comience haciendo una [clonación](luis-how-to-manage-versions.md#clone-a-version), desde una versión base, para cada autor. 

Cada autor efectúa cambios en su propia versión de la aplicación. Cuando estén satisfechos con el modelo, deberá exportar las versiones nuevas a archivos JSON.  

Las aplicaciones exportadas son archivos con formato JSON, que se pueden comparar para efectuar cambios. Combine los archivos para crear un único archivo JSON de la nueva versión. Cambie la propiedad **versionId** del archivo JSON para indicar la nueva versión combinada. Importe esa versión en la aplicación original. 

Este método permite tener una versión activa, una versión de ensayo y una versión publicada. Puede comparar los resultados en el panel de pruebas interactivas en las tres versiones.

## <a name="manage-multiple-versions-as-apps"></a>Administrar varias versiones como aplicaciones
[Exporte](luis-how-to-manage-versions.md#export-version) la versión base. Cada autor importa la versión. La persona que importa la aplicación es el propietario de la versión. Cuando acabe de modificar la aplicación, deberá exportar la versión. 

Las aplicaciones exportadas son archivos con formato JSON, que se pueden comparar con la exportación base para efectuar cambios. Combine los archivos para crear un único archivo JSON de la nueva versión. Cambie la propiedad **versionId** del archivo JSON para indicar la nueva versión combinada. Importe esa versión en la aplicación original.

## <a name="collaborator-roles-vs-entity-roles"></a>Roles de colaborador roles frente a entidad

[Roles de la entidad](luis-concept-roles.md) se aplican al modelo de datos de la aplicación de LUIS. Roles de colaborador se aplican a los niveles de acceso de creación. 

## <a name="next-steps"></a>Pasos siguientes

Descripción de los conceptos de [control de versiones](luis-concept-version.md). 

Vea [App Settings](luis-how-to-collaborate.md) (Configuración de la aplicación) para obtener información sobre cómo administrar los colaboradores en la aplicación de LUIS.

Vea [Add email to access list](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58fcccdd5aca2f08a4104342) (Agregar un correo electrónico a la lista de acceso) con las API de creación.
