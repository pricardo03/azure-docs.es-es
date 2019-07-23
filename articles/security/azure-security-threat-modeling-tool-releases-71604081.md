---
title: 'Versiones de Threat Modeling Tool: Microsoft Threat Modeling Tool en Azure | Microsoft Docs'
description: Documentación de las notas de la versión de la herramienta de modelado de amenazas
services: security
documentationcenter: na
author: jegeib
manager: jegeib
editor: jegeib
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2019
ms.author: jegeib
ms.openlocfilehash: 6ad6eca20827e1683237b2c668eba71acedfaade
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "64917619"
---
# <a name="threat-modeling-tool-update-release-71604081---492019"></a>Threat Modeling Tool, versión actualizada 7.1.60408.1: 9 de abril de 2019

El 9 de abril de 2019 se lanzó la versión 7.1.60408.1 de Microsoft Threat Modeling Tool (TMT), que incluye los cambios siguientes:

- Nuevas galerías de símbolos para Azure Key Vault y Azure Traffic Manager
- El número de versión de TMT ahora se muestra en la pantalla principal
- Se actualizaron los vínculos de soporte técnico
- Corrección de errores

## <a name="feature-changes"></a>Cambios de características

### <a name="new-stencils-for-azure-key-vault-and-azure-traffic-manager"></a>Nuevas galerías de símbolos para Azure Key Vault y Azure Traffic Manager

![Galerías de símbolos de Azure Key Vault](./media/azure-security-threat-modeling-tool-releases-71604081/tmt_keyvault_trafficmanager.PNG)

Se agregaron nuevas galerías de símbolos y amenazas para Azure Key Vault y Azure Traffic Manager en el conjunto de galerías de símbolos de Azure. Cuando abre los modelos basados en el conjunto de galerías de símbolos de Azure, se pedirá a los usuarios que actualicen la plantilla asociada con el modelo. También es puede iniciar manualmente la actualización de un modelo basada en el conjunto de galerías de símbolos de Azure mediante el comando "Aplicar plantilla" del menú "Archivo" y la reaplicación del archivo Azure Cloud Services.tb7 más reciente.

### <a name="tmt-version-number-is-now-shown-on-the-home-screen"></a>El número de versión de TMT ahora se muestra en la pantalla principal

La versión de cliente de Threat Modeling Tool ahora se muestra en la pantalla principal de la aplicación para facilitar su acceso.

![Galerías de símbolos de Azure Key Vault](./media/azure-security-threat-modeling-tool-releases-71604081/tmt_version.PNG)

### <a name="support-links-have-been-updated"></a>Se actualizaron los vínculos de soporte técnico

Se han actualizado todos los vínculos de soporte técnico dentro de la herramienta para dirigir a los usuarios a [tmtextsupport@microsoft.com](mailto:tmtextsupport@microsoft.com) en lugar de a un foro de MSDN.

## <a name="system-requirements"></a>Requisitos del sistema

- Sistemas operativos compatibles
  - [Actualización de aniversario de Microsoft Windows 10](https://blogs.windows.com/windowsexperience/2016/08/02/how-to-get-the-windows-10-anniversary-update/#HTkoK5Zdv0g2F2Zq.97) o posterior
- Versión de .NET necesaria
  - [.Net 4.7.1](https://go.microsoft.com/fwlink/?LinkId=863262) o posterior
- Requisitos adicionales
  - Se necesita una conexión a Internet para recibir actualizaciones de la herramienta, así como plantillas.

## <a name="documentation-and-feedback"></a>Documentación y comentarios

- La documentación de Threat Modeling Tool se encuentra en [docs.microsoft.com](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool) e incluye información [sobre el uso de la herramienta](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool-getting-started).

## <a name="next-steps"></a>Pasos siguientes

Descargue la versión más reciente de [Microsoft Threat Modeling Tool](https://aka.ms/threatmodelingtool).
