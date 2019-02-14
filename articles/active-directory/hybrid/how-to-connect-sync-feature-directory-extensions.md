---
title: 'Sincronización de Azure AD Connect: Extensiones de directorio | Microsoft Docs'
description: En este tema se describe la característica de extensiones de directorio en Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 995ee876-4415-4bb0-a258-cca3cbb02193
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/05/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: ff0fd4d01eab739b79685c1de67cb8fe28873961
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56167208"
---
# <a name="azure-ad-connect-sync-directory-extensions"></a>Sincronización de Azure AD Connect: Sincronización de Azure AD Connect: Extensiones de directorio
Puede usar extensiones de directorio para ampliar el esquema de Azure Active Directory (Azure AD) con sus propios atributos desde Active Directory local. Esta característica le permite compilar aplicaciones de LOB mediante el consumo de atributos que sigue administrando de forma local. Estos atributos se pueden consumir mediante [extensiones de directorio de API de Azure AD Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) o [Microsoft Graph](https://developer.microsoft.com/graph/). Puede ver los atributos disponibles mediante el [Explorador de Azure AD Graph](https://graphexplorer.azurewebsites.net/) y el [Probador de Microsoft Graph](https://developer.microsoft.com/graph/graph-explorer), respectivamente.

En la actualidad, ninguna carga de trabajo de Office 365 consume estos atributos.

Configure qué atributos adicionales desea sincronizar en la ruta de acceso de configuración personalizada en el Asistente para instalación.

>[!NOTE]
>El cuadro de atributos disponibles distingue mayúsculas de minúsculas.

![Asistente para la extensión de esquema](./media/how-to-connect-sync-feature-directory-extensions/extension2.png)  

 La instalación muestra los atributos siguientes, que son candidatos válidos:

* Tipos de objetos de usuario y de grupo
* Atributos de valor único: cadena, booleano, entero, binario
* Atributos con varios valores: cadena, binario


>[!NOTE]
> Aunque Azure AD Connect admite la sincronización con varios valores de atributos de Active Directory con Azure AD como extensiones de directorio con varios valores, actualmente no hay forma de recuperar o consumir los datos cargados en los atributos de extensiones de directorio con varios valores.

La lista de atributos se lee de la memoria caché de esquemas creada durante la instalación de Azure AD Connect. Si ha ampliado el esquema de Active Directory con atributos adicionales, se debe [actualizar el esquema](how-to-connect-installation-wizard.md#refresh-directory-schema) para que los nuevos atributos estén visibles.

Un objeto de Azure AD puede tener hasta 100 atributos para las extensiones de directorio. La longitud máxima es de 250 caracteres. Si un valor de atributo es mayor, el motor de sincronización lo trunca.

Durante la instalación de Azure AD Connect, se registra una aplicación donde estos atributos estén disponibles. Puede ver esta aplicación en el Portal de Azure.

![Aplicación de extensión de esquema](./media/how-to-connect-sync-feature-directory-extensions/extension3new.png)

Los atributos tienen el prefijo de extensión \_{AppClientId}\_. AppClientId tiene el mismo valor en todos los atributos del inquilino de Azure AD.

Estos atributos ahora están disponibles en Azure AD Graph API. Puede consultarlos mediante el [Explorador de Azure AD Graph](https://graphexplorer.azurewebsites.net/).

![Explorador de Azure AD Graph](./media/how-to-connect-sync-feature-directory-extensions/extension4.png)

O bien puede consultar los atributos a través de Microsoft Graph API, mediante el uso del [Probador de Microsoft Graph](https://developer.microsoft.com/graph/graph-explorer#).

>[!NOTE]
> Debe pedir que se devuelvan los atributos. Seleccione los atributos de forma explícita del modo siguiente: https://graph.microsoft.com/beta/users/abbie.spencer@fabrikamonline.com?$select=extension_9d98ed114c4840d298fad781915f27e4_employeeID,extension_9d98ed114c4840d298fad781915f27e4_division. 
>
> Para más información, consulte [Microsoft Graph: Usar parámetros de consulta](https://developer.microsoft.com/graph/docs/concepts/query_parameters#select-parameter).

## <a name="next-steps"></a>Pasos siguientes
Obtenga más información sobre la configuración de la [Sincronización de Azure AD Connect](how-to-connect-sync-whatis.md) .

Obtenga más información sobre la [Integración de las identidades locales con Azure Active Directory](whatis-hybrid-identity.md).
