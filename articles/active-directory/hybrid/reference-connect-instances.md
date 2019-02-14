---
title: 'Azure AD Connect: Instancias de servicio de sincronización | Microsoft Docs'
description: Esta página documenta las consideraciones especiales para instancias de Azure AD.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: f340ea11-8ff5-4ae6-b09d-e939c76355a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 06/18/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: ca481d50efb99d6e36c66388192e9f27cd66bf45
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56217197"
---
# <a name="azure-ad-connect-special-considerations-for-instances"></a>Azure AD Connect: Consideraciones especiales para instancias
Azure AD Connect se utiliza habitualmente con la instancia mundial de Azure AD y Office 365. Pero también hay otras instancias y estas tienen requisitos diferentes para las direcciones URL y otras consideraciones especiales.

## <a name="microsoft-cloud-germany"></a>Microsoft Cloud Germany
[Microsoft Cloud Germany](https://www.microsoft.de/cloud-deutschland) es una nube soberana operada por un administrador de datos alemán.

| Direcciones URL para abrir en el servidor proxy |
| --- |
| \*.microsoftonline.de |
| \*.windows.net |
| +Listas de revocación de certificados |

Al iniciar sesión en el inquilino de Azure AD tiene que usar una cuenta en el dominio onmicrosoft.de.

Características que actualmente no están presentes en Microsoft Cloud Germany:

* La **escritura diferida de contraseñas** está disponible en versión preliminar con Azure AD Connect versión 1.1.570.0 y posteriores.
* Otros servicios de Azure AD Premium no están disponibles.

## <a name="microsoft-azure-government-cloud"></a>Microsoft Azure Government Cloud
[Microsoft Azure Government Cloud](https://azure.microsoft.com/features/gov/) es una nube para el gobierno de Estados Unidos.

Esta nube ha sido compatible con versiones anteriores de DirSync. A partir de la compilación 1.1.180 de Azure AD Connect la próxima generación de la nube es compatible. Esta generación utiliza solo puntos de conexión basados en EE. UU. y tiene una lista de direcciones URL diferentes para abrir en el servidor proxy.

| Direcciones URL para abrir en el servidor proxy |
| --- |
| \*.microsoftonline.com |
| \*.microsoftonline.us |
| \*. windows.net (se requiere para la detección automática del inquilino de gobierno de Azure AD) |
| \*.gov.us.microsoftonline.com |
| +Listas de revocación de certificados |

> [!NOTE]
> A partir de la versión 1.1.647.0 de AAD Connect, no hace falta establecer el valor de AzureInstance en el registro, siempre que *. windows.net esté abierto en los servidores proxy.

Características que actualmente no están presentes en Microsoft Azure Government Cloud:

* La **escritura diferida de contraseñas** está disponible en versión preliminar con Azure AD Connect versión 1.1.570.0 y posteriores.
* Otros servicios de Azure AD Premium no están disponibles.

## <a name="next-steps"></a>Pasos siguientes
Obtenga más información sobre la [Integración de las identidades locales con Azure Active Directory](whatis-hybrid-identity.md).
