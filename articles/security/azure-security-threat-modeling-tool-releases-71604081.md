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
ms.openlocfilehash: 502c1e8a422eb9e74586e5a6820d5b12ec4ae2a4
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2019
ms.locfileid: "59501909"
---
# <a name="threat-modeling-tool-update-release-71604081---492019"></a>Versión de actualización de Threat Modeling Tool 7.1.60408.1 - 4/9/2019

Versión 7.1.60408.1 de la herramienta de modelado de amenazas de Microsoft (TMT) se lanzó en 9 de abril de 2019 y contiene los siguientes cambios:

- Galerías de símbolos nuevos para el almacén de claves de Azure y Azure Traffic Manager
- Número de versión TMT ahora se muestra en la pantalla principal
- Se han actualizado los vínculos de soporte técnico
- Corrección de errores

## <a name="feature-changes"></a>Cambios de características

### <a name="new-stencils-for-azure-key-vault-and-azure-traffic-manager"></a>Galerías de símbolos nuevos para el almacén de claves de Azure y Azure Traffic Manager

![Galería de símbolos de almacén de claves de Azure](./media/azure-security-threat-modeling-tool-releases-71604081/tmt_keyvault_trafficmanager.PNG)

Se agregaron nuevas galerías de símbolos y amenazas para Azure Key Vault y Azure Traffic Manager para el conjunto de Azure de la Galería de símbolos. Al abrir los modelos basados en el conjunto de Azure de la Galería de símbolos, se solicitará a los usuarios actualizar la plantilla asociada con el modelo. Actualizar un modelo basado en el conjunto de Azure de la Galería de símbolos puede también iniciar manualmente utilizando el comando "Aplicar plantilla" en el menú "Archivo" y vuelva a aplicar el archivo Services.tb7 en la nube de Azure más reciente.

### <a name="tmt-version-number-is-now-shown-on-the-home-screen"></a>Número de versión TMT ahora se muestra en la pantalla principal

La versión de cliente de Threat Modeling Tool no se muestra en la pantalla principal de la aplicación de facilidad de acceso.

![Galería de símbolos de almacén de claves de Azure](./media/azure-security-threat-modeling-tool-releases-71604081/tmt_version.PNG)

### <a name="support-links-have-been-updated"></a>Se han actualizado los vínculos de soporte técnico

Se han actualizado todos los vínculos de soporte técnico dentro de la herramienta para dirigir a los usuarios a [ tmtextsupport@microsoft.com ](mailto:tmtextsupport@microsoft.com) en lugar de un foro de MSDN.

## <a name="system-requirements"></a>Requisitos del sistema

- Sistemas operativos compatibles
  - [Actualización de aniversario de Microsoft Windows 10](https://blogs.windows.com/windowsexperience/2016/08/02/how-to-get-the-windows-10-anniversary-update/#HTkoK5Zdv0g2F2Zq.97) o posterior
- Versión de .NET necesaria
  - [.Net 4.7.1](http://go.microsoft.com/fwlink/?LinkId=863262) o posterior
- Requisitos adicionales
  - Se necesita una conexión a Internet para recibir actualizaciones de la herramienta, así como plantillas.

## <a name="documentation-and-feedback"></a>Documentación y comentarios

- La documentación de Threat Modeling Tool se encuentra en [docs.microsoft.com](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool) e incluye información [sobre el uso de la herramienta](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool-getting-started).

## <a name="next-steps"></a>Pasos siguientes

Descargue la versión más reciente de [Microsoft Threat Modeling Tool](https://aka.ms/threatmodelingtool).