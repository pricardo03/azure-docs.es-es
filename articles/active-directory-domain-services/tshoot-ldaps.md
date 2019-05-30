---
title: Solución de problemas de LDAP seguro (LDAPS) en Azure AD Domain Services | Microsoft Docs
description: Solución de problemas de LDAP seguro (LDAPS) para un dominio administrado con Azure AD Domain Services
services: active-directory-ds
documentationcenter: ''
author: MikeStephens-MS
manager: daveba
editor: curtand
ms.assetid: 445c60da-e115-447b-841d-96739975bdf6
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: mstephen
ms.openlocfilehash: c9732545e1759ea23d62c0a56379e3868ed40a0b
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66246635"
---
# <a name="troubleshoot-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Solución de problemas de LDAP seguro (LDAPS) para un dominio administrado con Azure AD Domain Services

## <a name="connection-issues"></a>Problemas de conexión
Si tiene problemas para conectarse al dominio administrado mediante LDAP seguro:

* La cadena del emisor del certificado LDAP seguro debe ser de confianza en el cliente. Puede optar por agregar la entidad de certificación raíz en el almacén de certificados raíz de confianza en el cliente para establecer la confianza.
* Compruebe que el cliente LDAP (por ejemplo, ldp.exe) se conecta al punto de conexión de LDAP seguro con un nombre DNS, no con la dirección IP.
* Compruebe el nombre DNS al que se conecta el cliente LDAP. Debe resolver a la dirección IP pública para LDAP seguro en el dominio administrado.
* Compruebe que el certificado LDAP seguro para el dominio administrado tiene el nombre DNS en el atributo Sujeto o Nombres alternativos del sujeto.
* La configuración de NSG para la red virtual debe permitir el tráfico al puerto 636 desde Internet. Este paso se aplica solo si tiene habilitado el acceso mediante LDAP seguro a través de Internet.


## <a name="need-help"></a>¿Necesita ayuda?
Si sigue teniendo problemas para conectarse al dominio administrado mediante LDAP seguro, [póngase en contacto con el equipo del producto](contact-us.md) para obtener ayuda. Incluya la siguiente información para que podamos diagnosticar el problema mejor:
* Una captura de pantalla de ldp.exe cundo establece la conexión y cuando genera un error.
* El identificador de inquilino de Azure AD y el nombre de dominio DNS del dominio administrado.
* El nombre de usuario exacto con el que está intentando enlazar.


## <a name="related-content"></a>Contenido relacionado
* [Introducción a Azure AD Domain Services](create-instance.md)
* [Administrar un dominio de Azure AD Domain Services](manage-domain.md)
* [LDAP query basics](https://technet.microsoft.com/library/aa996205.aspx) (Conceptos básicos de las consultas LDAP)
* [Administrar directivas de grupo para Azure AD Domain Services](manage-group-policy.md)
* [Grupos de seguridad de red](../virtual-network/security-overview.md)
* [Creación de un grupo de seguridad de red](../virtual-network/tutorial-filter-network-traffic.md)
