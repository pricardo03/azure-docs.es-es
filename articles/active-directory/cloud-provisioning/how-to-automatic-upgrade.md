---
title: 'Agente de aprovisionamiento en la nube de Azure AD Connect: Actualización automática | Microsoft Docs'
description: En este tema se describe la característica de actualización automática integrada en el agente de aprovisionamiento en la nube de Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/02/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 193804064fbf6d1abb2ce06df1e923ec709ef6de
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74795519"
---
# <a name="azure-ad-connect-cloud-provisioning-agent-automatic-upgrade"></a>Agente de aprovisionamiento en la nube de Azure AD Connect: Actualización automática

Tener la seguridad de que la instalación del agente de aprovisionamiento en la nube de Azure AD Connect está siempre actualizada nunca ha sido más fácil con la característica de **actualización automática**. Esta característica se habilitada de forma predeterminada y no se puede deshabilitar.

El agente se instala aquí:  **"Archivos de programa\Azure AD Connect Provisioning Agent\AADConnectProvisioningAgent.exe"**

Para comprobar su versión, haga clic con el botón derecho en el archivo ejecutable y seleccione Propiedades; a continuación, seleccione los detalles.

![Versión del archivo del agente](media/how-to-automatic-upgrade/agent1.png)

El actualizador del agente se instala aquí:  **"Archivos de programa\Azure AD Connect Provisioning Agent Updater\AzureADConnectAgentUpdater.exe"**

Para comprobar su versión, haga clic con el botón derecho en el archivo ejecutable y seleccione Propiedades; a continuación, seleccione los detalles.

![Versión del actualizador del agente](media/how-to-automatic-upgrade/agent2.png)

## <a name="uninstalling-the-agent"></a>Desinstalación del agente
Para quitar el agente, vaya a **Desinstalar o cambiar un programa** y desinstale lo siguiente:

- Agent Updater de Microsoft Azure AD Connect
- Microsoft Azure AD Connect Provisioning Agent
- Microsoft Azure AD Connect Provisioning Agent Package

![Eliminación del agente](media/how-to-automatic-upgrade/agent3.png)

## <a name="next-steps"></a>Pasos siguientes 

- [¿Qué es el aprovisionamiento?](what-is-provisioning.md)
- [¿Qué es el aprovisionamiento en la nube de Azure AD Connect?](what-is-cloud-provisioning.md)

