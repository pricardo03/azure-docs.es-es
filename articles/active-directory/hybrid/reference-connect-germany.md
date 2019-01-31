---
title: Azure AD Connect en Microsoft Cloud Germany
description: Azure AD Connect integrará sus directorios locales con Azure Active Directory. Esto le permite proporcionar una identidad común para las aplicaciones de Office 365, Azure y SaaS integradas con Azure AD.
keywords: introducción a Azure AD Connect, información general de Azure AD Connect, qué es Azure AD Connect, instalación de active directory, Alemania, Selva Negra
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 2bcb0caf-5d97-46cb-8c32-bda66cc22dad
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/12/2017
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: 5ccfab71edf9f5fd61191ae8bd4ef5421a055e23
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55164629"
---
# <a name="azure-ad-connect-in-microsoft-cloud-germany---public-preview"></a>Azure AD Connect en Microsoft Cloud Germany- Versión preliminar pública
## <a name="introduction"></a>Introducción
Azure AD Connect proporciona sincronización entre instancias de Active Directory y Azure Active Directory locales.
Actualmente, muchos de los escenarios en [Microsoft Cloud Germany](https://azure.microsoft.com/global-infrastructure/germany/
) debe realizarlos el operador. Al usar Microsoft Cloud Germany, debe tener en cuenta la siguiente información:

* Las direcciones URL siguientes deben estar abiertas en un servidor proxy para que tenga lugar correctamente la sincronización:
  
  * *. microsoftonline.de
  * * .windows.net
  * * Listas de revocación de certificados
* Al iniciar sesión en el directorio de Azure AD tiene que usar una cuenta en el dominio onmicrosoft.de.

 
## <a name="download"></a>Descargar
Puede descargar Azure AD Connect en la hoja Azure AD Connect en el portal.  Utilice las instrucciones siguientes para localizar la hoja Azure AD Connect.

### <a name="the-azure-ad-connect-blade"></a>Hoja Azure AD Connect
Una vez que ha iniciado sesión en Azure Portal:

1. Vaya a Examinar.
2. Seleccione Azure Active Directory.
3. Después, seleccione Azure AD Connect.

Verá estos detalles:

![Hoja Azure AD Connect](./media/reference-connect-germany/germany1.png)

En la tabla siguiente se describen las características mostradas en la hoja.

| Título | DESCRIPCIÓN |
| --- | --- |
| ESTADO DE SINCRONIZACIÓN |Le permite saber si la sincronización está habilitada o no. |
| ÚLTIMA SINCRONIZACIÓN |La última vez que se realizó correctamente una sincronización. |
| DOMINIOS FEDERADOS |Muestra el número de dominios federados configurados. |

## <a name="installation"></a>Instalación
Para instalar Azure AD Connect, puede utilizar [esta](how-to-connect-install-roadmap.md)documentación.

## <a name="advanced-features-and-additional-information"></a>Características avanzadas e información adicional
Para más información sobre la configuración personalizada o configuraciones avanzadas, vaya a [Integración de las identidades locales con Azure Active Directory](whatis-hybrid-identity.md). Esta página proporciona información y vínculos a información adicional.

