---
title: Conexión a Azure Alemania mediante Visual Studio | Microsoft Docs
description: Información acerca de la administración de suscripciones en Azure Alemania mediante Visual Studio
services: germany
cloud: na
documentationcenter: na
author: gitralf
manager: rainerst
ms.assetid: na
ms.service: germany
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/13/2017
ms.author: ralfwi
ms.openlocfilehash: 9d7131d7ce6a8fd86ba2f154b8f7ca9db8afc3ee
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67033543"
---
# <a name="connect-to-azure-germany-by-using-visual-studio"></a>Conexión a Azure Alemania mediante Visual Studio
Los desarrolladores usan Visual Studio para administrar fácilmente sus suscripciones de Azure mientras se implementan soluciones. Actualmente, no se puede configurar una conexión a Azure Alemania en la interfaz de usuario de Visual Studio.  

## <a name="visual-studio-2017"></a>Visual Studio 2017
Visual Studio 2017 requiere un archivo de configuración para conectarse a Azure Alemania. Si dicho archivo está en su lugar, Visual Studio se conecta a Azure Alemania, en lugar de a Azure global.

### <a name="create-a-configuration-file-for-azure-germany"></a>Creación de un archivo de configuración para Azure Alemania
Cree un archivo llamado **AadProvider.Configuration.json** con el siguiente contenido:

        {
          "AuthenticationQueryParameters":null,
          "AsmEndPoint":"https://management.microsoftazure.de/",
          "Authority":"https://login.microsoftonline.de/",
          "AzureResourceManagementEndpoint":"https://management.microsoftazure.de/",
          "AzureResourceManagementAudienceEndpoints":["https://management.core.cloudapi.de/"],
          "ClientIdentifier":"872cd9fa-d31f-45e0-9eab-6e460a02d1f1",
          "EnvironmentName":"BlackForest",
          "GraphEndpoint":"https://graph.cloudapi.de",
          "MsaHomeTenantId":"f577cd82-810c-43f9-a1f6-0cc532871050",
          "NativeClientRedirect":"urn:ietf:wg:oauth:2.0:oob",
          "PortalEndpoint":"https://portal.core.cloudapi.de/",
          "ResourceEndpoint":"https://management.microsoftazure.de/",
          "ValidateAuthority":true,
          "VisualStudioOnlineEndpoint":"https://app.vssps.visualstudio.com/",
          "VisualStudioOnlineAudience":"499b84ac-1321-427f-aa17-267ca6975798"
        }

### <a name="update-visual-studio-for-azure-germany"></a>Actualización de Visual Studio para Azure Alemania

1.  Cierre Visual Studio.
2.  Coloque **AadProvider.Configuration.json** en **%localappdata%\\.IdentityService\AadConfigurations**. Cree esta carpeta, en caso de que no la encuentre.
3.  Inicie Visual Studio y empiece a usar su cuenta de Azure Germany.

> [!NOTE]
> Con el archivo de configuración solo se puede acceder a Azure Alemania. Seguirá viendo las suscripciones que configuró anteriormente, pero no funcionan porque Visual Studio está ahora conectado a Azure Alemania, en lugar de a Azure global. Para conectarse a Azure global, quite el archivo.
> 
> 

### <a name="revert-a-visual-studio-connection-to-azure-germany"></a>Reversión de una conexión de Visual Studio a Azure Alemania
Para que Visual Studio pueda conectarse a Azure global, es preciso quitar el archivo de configuración que permite la conexión a Azure Alemania.

1.  Cierre Visual Studio.
2.  Elimine o cambie el nombre de la carpeta **%localappdata%\.IdentityService\AadConfigurations**.
3.  Inicie Visual Studio y empiece a usar su cuenta de Azure global.

> [!NOTE]
> Después de revertir esta configuración, no se puede acceder a las suscripciones de Azure Germany.
> 
>

## <a name="visual-studio-2015"></a>Visual Studio 2015
Visual Studio 2015 requiere un cambio en el Registro para conectarse a Azure Alemania. Después de que se establecer esta clave del Registro, Visual Studio se conecta a Azure Alemania, en lugar de a Azure global.

### <a name="update-visual-studio-for-azure-germany"></a>Actualización de Visual Studio para Azure Alemania
Para que Visual Studio se pueda conectar a Azure Alemania, es preciso actualizar el Registro.

1. Cierre Visual Studio.
2. Cree un archivo de texto llamado **VisualStudioForAzureGermany.reg**.
3. Copie y pegue el siguiente texto en **VisualStudioForAzureGermany.reg**:
   
        Windows Registry Editor Version 5.00
 
        [HKEY_CURRENT_USER\Software\Microsoft\VSCommon\ConnectedUser]
        "AadInstance"="https://login.microsoftonline.de/"
        "adaluri"="https://management.microsoftazure.de"
        "AzureRMEndpoint"="https://management.microsoftazure.de"
        "AzureRMAudienceEndpoint"="https://management.core.cloudapi.de"
        "EnableAzureRMIdentity"="true"
        "GraphUrl"="graph.cloudapi.de"
        "AadApplicationTenant"="f577cd82-810c-43f9-a1f6-0cc532871050"

4. Guarde y luego ejecute el archivo haciendo doble clic en él. Se le pedirá que combine el archivo en el Registro.
5. Inicie Visual Studio y empiece a usar [Cloud Explorer](../vs-azure-tools-resources-managing-with-cloud-explorer.md) con su cuenta de Azure Alemania.

> [!NOTE]
> Cuando esta clave del Registro está establecida, solo se puede acceder a las suscripciones de Azure Alemania. Seguirá viendo las suscripciones que configuró anteriormente, pero no funcionan porque Visual Studio está ahora conectado a Azure Alemania, en lugar de a Azure global. Para conectarse a Azure global, revierta los cambios.
> 
> 

### <a name="revert-a-visual-studio-connection-to-azure-germany"></a>Reversión de una conexión de Visual Studio a Azure Alemania
Para que Visual Studio pueda conectarse a Azure global, es preciso quitar los valores del Registro que permiten la conexión a Azure Alemania.

1. Cierre Visual Studio.
2. Cree un archivo de texto llamado **VisualStudioForAzureGermany_Remove.reg**.
3. Copie y pegue el siguiente texto en **VisualStudioForAzureGermany_Remove.reg**:
   
        Windows Registry Editor Version 5.00
   
        [HKEY_CURRENT_USER\Software\Microsoft\VSCommon\ConnectedUser]
        "AadInstance"=-
        "adaluri"=-
        "AzureRMEndpoint"=-
        "AzureRMAudienceEndpoint"=-
        "EnableAzureRMIdentity"=-
        "GraphUrl"=-
        
4. Guarde y luego ejecute el archivo haciendo doble clic en él. Se le pedirá que combine el archivo en el Registro.
5. Inicie Visual Studio.

> [!NOTE]
> Cuando se haya revertido esta clave del Registro, aparecerán las suscripciones de Azure Alemania , pero no podrá acceder a ellas. Quítelas de forma segura.
> 
> 

## <a name="next-steps"></a>Pasos siguientes
Para más información acerca de cómo conectarse a Azure Alemania, consulte los siguientes recursos:

* [Conexión a Azure Alemania mediante PowerShell](./germany-get-started-connect-with-ps.md)
* [Conexión a Azure Alemania mediante la CLI de Azure](./germany-get-started-connect-with-cli.md)
* [Conexión a Azure Alemania mediante Azure Portal](./germany-get-started-connect-with-portal.md)





