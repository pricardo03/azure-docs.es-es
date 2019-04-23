---
title: 'Azure AD Connect: Solución de problemas de delimitador de origen durante la instalación | Microsoft Docs'
description: En este tema se proporciona los pasos para solucionar problemas relacionados con el delimitador de origen durante la instalación.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 04/19/2019
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: fac33a01afc2efc1ab06c4783c11f7a089bb6208
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "60014754"
---
# <a name="troubleshooting-source-anchor-issues-during-installation"></a>Solución de problemas de delimitador de origen durante la instalación
En este artículo se explica los diferentes problemas que pueden producirse durante la instalación y ofrece formas de resolver estos problemas relacionados con el delimitador de origen.

## <a name="invalid-source-anchor-in-azure-active-directory"></a>Delimitador de origen no válido en Azure Active Directory

### <a name="custom-installation"></a>Instalación personalizada

Durante la instalación personalizada, Azure AD Connect lee la directiva de delimitador de origen de Azure Active Directory. Si la directiva no existe en Azure Active Directory, Azure AD Connect aplica la misma directiva a menos que se haya reemplazado por el cliente. El asistente indica qué atributo se ha leído. Además, el Asistente advierte si al intentar invalidar la directiva de delimitador de origen.

Durante esta operación de lectura, es posible que se esperaba la directiva de delimitador de origen en Azure Active Directory. En este caso, Azure AD Connect no sabe qué el delimitador de origen para usar y necesita la invalidación de forma manual.</br>
![unexpected](media/tshoot-connect-source-anchor/source1.png)

Para resolver este problema, puede invalidar manualmente el delimitador de origen seleccionando un atributo determinado. Continuar con esta opción solo si está seguro de qué atributo que seleccione. Si no está seguro, póngase en contacto con [soporte técnico de Microsoft](https://support.microsoft.com/contactus/) para obtener instrucciones. Si cambia la directiva de delimitador de origen, puede interrumpir la asociación entre los usuarios locales y sus recursos de Azure asociados.</br>
![unexpected](media/tshoot-connect-source-anchor/source2.png)

### <a name="express-installation"></a>Instalación rápida
Durante la instalación rápida, Azure AD Connect lee la directiva de delimitador de origen de Azure Active Directory. Si la directiva no existe en Azure Active Directory, Azure AD Connect aplica la misma directiva. No hay ninguna opción para realizar la invalidación de forma manual.

Durante esta operación de lectura, es posible que se esperaba la directiva de delimitador de origen en Azure Active Directory. En este caso, Azure AD Connect no sabe cuál debe ser el delimitador de origen.</br>
![unexpected](media/tshoot-connect-source-anchor/source3.png)

Para resolver este problema, deberá volver a instalar con el modo personalizado e invalidar manualmente el delimitador de origen seleccionando un atributo determinado. Continuar con esta opción solo si está seguro de qué atributo que seleccione. Si no está seguro, póngase en contacto con [soporte técnico de Microsoft](https://support.microsoft.com/contactus/) para obtener instrucciones. Si cambia la directiva de delimitador de origen, puede interrumpir la asociación entre los usuarios locales y sus recursos de Azure asociados.

### <a name="invalid-source-anchor-in-sync-engine"></a>Delimitador de origen no válido en el motor de sincronización
Durante la instalación, es posible Azure AD Connect intenta configurar el motor de sincronización mediante un delimitador de origen no válido. Esta operación es muy probable que un problema del producto y se producirá un error en la instalación de Azure AD Connect. Póngase en contacto con [soporte técnico de Microsoft](https://support.microsoft.com/contactus/) si se ejecuta en este problema.</br>
![unexpected](media/tshoot-connect-source-anchor/source4.png)


## <a name="next-steps"></a>Pasos siguientes
Obtenga más información sobre la [Integración de las identidades locales con Azure Active Directory](whatis-hybrid-identity.md).