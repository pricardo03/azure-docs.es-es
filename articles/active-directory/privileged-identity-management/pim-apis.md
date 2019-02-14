---
title: Instancias de Microsoft Graph API para Azure AD Privileged Identity Management (PIM) (versión preliminar) | Microsoft Docs
description: Proporciona información acerca del uso de instancias de Microsoft Graph API para Azure Active Directory Privileged Identity Management (PIM) (versión preliminar).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: pim
ms.topic: overview
ms.date: 11/13/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 97b548d199dd98a0f8c788c8c50ba618f721f4ab
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56183945"
---
# <a name="microsoft-graph-apis-for-pim-preview"></a>Instancias de Microsoft Graph API para PIM (versión preliminar)

La mayoría de las tareas que se pueden realizar en Azure AD Privileged Identity Management (PIM) desde Azure Portal también puede realizar mediante instancias de [Microsoft Graph API](https://developer.microsoft.com/graph/docs/concepts/overview). En este artículo se describen varios conceptos importantes que se deben tener en cuenta cuando se usan instancias de Microsoft Graph API para PIM. Para más información acerca de las instancias de Microsoft Graph API, consulte la [referencia de las API de Azure AD Privileged Identity Management](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/privilegedidentitymanagement_root).

> [!IMPORTANT]
> Las API de la versión /beta de Microsoft Graph están en versión preliminar y sujetas a cambios. No se admite su uso en aplicaciones de producción.

## <a name="required-permissions"></a>Permisos necesarios

Para llamar a las instancias de Microsoft Graph API para PIM, debe tener **uno o varios** de los siguientes permisos:

- `Directory.AccessAsUser.All`
- `Directory.Read.All`
- `Directory.ReadWrite.All`
- `PrivilegedAccess.ReadWrite.AzureAD`

### <a name="set-permissions"></a>Establecer permisos

Para que las aplicaciones llamen a la instancias de Microsoft Graph API para PIM, deben tener los permisos necesarios. La forma más fácil de especificar los permisos necesarios es usar el [marco de consentimiento de Azure AD](../develop/consent-framework.md).

### <a name="set-permissions-in-graph-explorer"></a>Establecimiento de permisos en el Probador de Graph

Si utiliza el Probador de Graph para probar las llamadas, puede especificar los permisos en dicha herramienta.

1. Inicie sesión en [Probador de Graph](https://developer.microsoft.com/graph/graph-explorer) como administrador global.

1. Haga clic en **modificar permisos**.

    ![Probador de Graph: modificar permisos](./media/pim-apis/graph-explorer.png)

1. Agregue marcas de verificación junto a los permisos que desea incluir. `PrivilegedAccess.ReadWrite.AzureAD` aún no está disponible en el Probador de Graph.

    ![Probador de Graph: modificar permisos](./media/pim-apis/graph-explorer-modify-permissions.png)

1. Haga clic en **Modificar permisos** para aplicar los cambios de permisos.

## <a name="next-steps"></a>Pasos siguientes

- [Referencia de las API de Azure AD Privileged Identity Management](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/privilegedidentitymanagement_root)
