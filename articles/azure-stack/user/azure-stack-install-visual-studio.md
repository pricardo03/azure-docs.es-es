---
title: Instalar Visual Studio y conectarse a Azure Stack | Microsoft Docs
description: Aprenda los pasos necesarios para instalar Visual Studio y conectarse a Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.custom: vs-azure
ms.workload: azure-vs
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/04/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: 274240aab54f27f36734516026e9feebf64ae4b5
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55248119"
---
# <a name="install-visual-studio-and-connect-to-azure-stack"></a>Instalar Visual Studio y conectarse a Azure Stack

*Se aplica a: Sistemas integrados de Azure Stack y Kit de desarrollo de Azure Stack*

Puede usar Visual Studio para escribir e implementar [plantillas](azure-stack-arm-templates.md) de Azure Resource Manager en Azure Stack. Los pasos de este artículo le guiarán por la instalación de Visual Studio en [Azure Stack](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), o en un equipo externo si planea realizar la instalación de Azure Stack a través de la [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn).

## <a name="install-visual-studio"></a>Instalación de Visual Studio

1. Descargue y ejecute el [Instalador de plataforma web](https://www.microsoft.com/web/downloads/platform.aspx).  

2. Abra el **Instalador de plataforma web de Microsoft**.

3. Busque **Visual Studio Community 2015 con el SDK de Microsoft Azure - 2.9.6**. Haga clic en **Agregar** y en **Instalar**.

4. Desinstale la versión de **Microsoft Azure PowerShell** que está instalada como parte de Azure SDK.

    ![Captura de pantalla de los pasos de la instalación de WebPI](./media/azure-stack-install-visual-studio/image1.png) 

5. [Install PowerShell for Azure Stack](azure-stack-powershell-install.md) (Instalación de PowerShell para Azure Stack)

6. Una vez finalizada la instalación, reinicie el sistema operativo.

## <a name="connect-to-azure-stack-with-azure-ad"></a>Conexión a Azure Stack con Azure AD

1. Inicie Visual Studio.

2. En el menú **Ver**, seleccione **Cloud Explorer**.

3. En el nuevo panel, seleccione **Agregar cuenta** e inicie sesión con sus credenciales de Azure Active Directory (Azure AD).  

    ![Captura de pantalla de Cloud Explorer después de iniciar sesión en Azure Stack y conectarse](./media/azure-stack-install-visual-studio/image2.png)

Después de iniciar sesión, puede [implementar plantillas](azure-stack-deploy-template-visual-studio.md) o examinar los tipos y los grupos de recursos disponibles para crear sus propias plantillas.  

## <a name="connect-to-azure-stack-with-ad-fs"></a>Conexión a Azure Stack con AD FS

1. Inicie Visual Studio.

2. En **Herramientas**, seleccione **Opciones**.

3. Expanda **Entorno** en el **Panel de navegación** y seleccione **Cuentas**.

4. Seleccione **Agregar** y especifique el punto de conexión de usuario de Azure Resource Manager.  
  Para el Kit de desarrollo de Azure Stack, la dirección URL es `https://management.local.azurestack/external`.  
  Para los sistemas integrados de Azure Stack, la dirección URL es `https://management.[Region}.[External FQDN]`.

    ![X](./media/azure-stack-install-visual-studio/image5.png)

5. Seleccione **Agregar**.  

    Visual Studio llama a Azure Resource Manager y descubre los puntos de conexión, incluido el punto de conexión de autenticación para Azure Directory Federated Services (AD FS).

    ![Captura de pantalla de Cloud Explorer después de iniciar sesión en Azure Stack y conectarse](./media/azure-stack-install-visual-studio/image6.png)

6. Seleccione **Cloud Explorer** en el menú **Ver**.

1. Seleccione **Agregar cuenta** e inicie sesión con sus credenciales de AD FS.  

    ![Cloud Explorer](./media/azure-stack-install-visual-studio/image7.png)

    Cloud Explorer consulta las suscripciones disponibles. Puede seleccionar una suscripción disponible para administrar.

    ![Cloud Explorer](./media/azure-stack-install-visual-studio/image8.png)

8. Examinar los recursos existentes, grupos de recursos o implementar plantillas.

## <a name="next-steps"></a>Pasos siguientes

 - Obtenga más información sobre la [coexistencia](https://msdn.microsoft.com/library/ms246609.aspx) de Visual Studio con otras versiones de Visual Studio.
 - [Desarrollo de plantillas para Azure Stack](azure-stack-develop-templates.md).