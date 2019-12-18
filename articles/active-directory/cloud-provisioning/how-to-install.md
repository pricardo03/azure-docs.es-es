---
title: Instalación del agente de aprovisionamiento en la nube de Azure AD Connect
description: En este tema se describe paso a paso cómo instalar el agente de aprovisionamiento.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/02/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 11e3b2a113d46ff3d8799927f56fa66601c94ed5
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2019
ms.locfileid: "74846239"
---
# <a name="install-azure-ad-connect-cloud-provisioning-agent"></a>Instalación del agente de aprovisionamiento en la nube de Azure AD Connect
Este documento le guiará en el proceso de instalación del agente de aprovisionamiento de Azure AD Connect y en el modo de configurarlo inicialmente en Azure Portal.

>[!IMPORTANT]
>En las siguientes instrucciones de instalación se supone que se han cumplido todos los [requisitos previos](how-to-prerequisites.md).

La instalación y configuración del aprovisionamiento de Azure AD Connect se realiza en los pasos siguientes:
    
- [Instalación del agente](#install-the-agent)
- [Comprobación de la instalación del agente](#verify-agent-installation)


## <a name="install-the-agent"></a>Instalación del agente

1. Inicie sesión en el servidor que va a usar con permisos de administrador de empresa.
2. Vaya a Azure Portal y seleccione Azure Active Directory a la izquierda.
3. Haga clic en **Administrar el aprovisionamiento (versión preliminar)** y seleccione **Revisar todos los agentes**.
3. Descargue el agente de aprovisionamiento de Azure AD Connect en Azure Portal.
![Pantalla principal](media/how-to-install/install9.png)</br>
3. Ejecución del aprovisionamiento de Azure AD Connect (AADConnectProvisioningAgent.Installer)
3. En la pantalla de presentación, **acepte** los términos de la licencia y haga clic en **Install** (Instalar).</br>
![Pantalla principal](media/how-to-install/install1.png)</br>

4. Una vez que finalice esta operación, se iniciará el asistente para configuración.  Inicie sesión con su cuenta de administrador global de Azure AD.
5. En la pantalla **Connect Active Directory** (Conectar Active Directory), haga clic en **Add directory** (Agregar directorio) e inicie sesión con su cuenta de administrador de Active Directory.  Esta operación permitirá agregar su directorio local.  Haga clic en **Next**.</br>
![Pantalla principal](media/how-to-install/install3.png)</br>

6. En la pantalla **Configuración completa**, haga clic en **Confirmar**.  Esta operación registrará el agente y lo reiniciará.</br>
![Pantalla principal](media/how-to-install/install4.png)</br>

7. Una vez que se complete esta operación, debería aparecer un aviso **Your was successfully verified** (Se ha comprobado correctamente).  Puede hacer clic en **Salir**.</br>
![Pantalla principal](media/how-to-install/install5.png)</br>
8. Si la pantalla de presentación inicial no desaparece, haga clic en **Cerrar**.


## <a name="verify-agent-installation"></a>Comprobación de la instalación del agente
La comprobación del agente se produce en Azure Portal y en el servidor local que ejecuta el agente.

### <a name="azure-portal-agent-verification"></a>Comprobación del agente en Azure Portal
Para comprobar que Azure ve el agente, siga estos pasos:

1. Inicie sesión en el Portal de Azure.
2. A la izquierda, seleccione **Azure Active Directory**, haga clic en **Azure AD Connect** y, en el centro, seleccione **Administración del aprovisionamiento (versión preliminar)** .</br>
![Azure Portal](media/how-to-install/install6.png)</br>

3.  En la pantalla **Aprovisionamiento de Azure AD (versión preliminar)** , haga clic en **Revisar todos los agentes**.
![Aprovisionamiento de Azure AD](media/how-to-install/install7.png)</br>
 
4. En la pantalla **On-premises provisioning agents** (Agentes de aprovisionamiento locales) verá los agentes que ha instalado.  Compruebe que el agente en cuestión está ahí y que se ha marcado como **Activo**.
![Agentes de aprovisionamiento](media/how-to-install/verify1.png)</br>

### <a name="verify-the-port"></a>Comprobación del puerto
Para comprobar que Azure está escuchando en el puerto 443 y que el agente puede comunicarse con él, puede utilizar lo siguiente:

https://aadap-portcheck.connectorporttest.msappproxy.net/ 

En esta prueba se comprobará que los agentes pueden comunicarse con Azure a través del puerto 443.  Abra un explorador y vaya a la dirección URL anterior desde el servidor en el que está instalado el agente.
![Servicios](media/how-to-install/verify2.png)

### <a name="on-the-local-server"></a>En el servidor local
Para comprobar que el agente se ejecuta, siga estos pasos:

1.  Inicie sesión en el servidor con una cuenta de administrador.
2.  Abra **Servicios**. Para ello, vaya a él o a Inicio/Ejecutar/Services.msc.
3.  En **Servicios**, asegúrese de que tanto el **Actualizador del Agente de Microsoft Azure AD Connect** como el **Agente de aprovisionamiento de Microsoft Azure AD Connect** están ahí y que su estado es **En ejecución**.
![Servicios](media/how-to-troubleshoot/troubleshoot1.png)

>[!IMPORTANT]
>El agente se ha instalado, pero debe configurarse y habilitarse antes de que empiece a sincronizar a los usuarios.  Para configurar un agente nuevo, consulte [Configuración del nuevo agente de aprovisionamiento de Azure AD Connect](how-to-configure.md).



## <a name="next-steps"></a>Pasos siguientes 

- [¿Qué es el aprovisionamiento?](what-is-provisioning.md)
- [¿Qué es el aprovisionamiento en la nube de Azure AD Connect?](what-is-cloud-provisioning.md)
 
