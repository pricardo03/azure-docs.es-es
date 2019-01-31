---
title: 'Azure Active Directory Domain Services: Creación de una cuenta de servicio administrada de grupos | Microsoft Docs'
description: Administración de dominios administrados con Azure Active Directory Domain Services
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: e6faeddd-ef9e-4e23-84d6-c9b3f7d16567
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/26/2018
ms.author: ergreenl
ms.openlocfilehash: ef21383db52d45d996f2c6c1d6900ebba2349635
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55203086"
---
# <a name="create-a-group-managed-service-account-gmsa-on-an-azure-ad-domain-services-managed-domain"></a>Cree una cuenta de servicio administrada de grupo (gMSA) en un dominio administrado de Azure AD Domain Services
En este artículo se muestra cómo crear cuentas de servicio administradas en un dominio administrado de Azure AD Domain Services.

## <a name="managed-service-accounts"></a>Cuentas de servicio administradas
Una cuenta de servicio administrada independiente (sMSA) es una cuenta de dominio administrada cuya contraseña se administra automáticamente. Simplifica la administración de los nombre de entidad de seguridad de servicio (SPN), y permite delegar la administración a otros administradores. Este tipo de cuenta de servicio administrada (MSA) se incorporó por primera vez en Windows Server 2008 R2 y Windows 7.

La cuenta de servicio administrada de grupo (gMSA) proporciona las mismas ventajas a los numerosos servidores del dominio. Para poder funcionar, todas las instancias de un servicio hospedado en una granja de servidores deben usar la misma entidad de servicio para los protocolos de autenticación mutua. Cuando se usa una gMSA como entidad de servicio, el sistema operativo Windows administra la contraseña de la cuenta en lugar de recurrir al administrador.

**Más información:**
- [Información general sobre las cuentas de servicio administradas de grupo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)
- [Introducción a las cuentas de servicio administradas de grupo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/getting-started-with-group-managed-service-accounts)


## <a name="using-service-accounts-in-azure-ad-domain-services"></a>Uso de cuentas de servicio en Azure AD Domain Services
Los dominios administrados de Azure AD Domain Services están bloqueados y los administra Microsoft. Hay un par de consideraciones importantes a la hora de usar cuentas de servicio con Azure AD Domain Services.

### <a name="create-service-accounts-within-custom-organizational-units-ou-on-the-managed-domain"></a>Creación de cuentas de servicio dentro de unidades organizativas personalizadas en el dominio administrado
No se puede crear una cuenta de servicio en las unidades organizativas integradas "Usuarios AADDC" ni "Equipos AADDC". [Cree la unidad organizativa personalizada](active-directory-ds-admin-guide-create-ou.md) en el dominio administrado y, después, cree las cuentas de servicio dentro de esa unidad organizativa personalizada.

### <a name="the-key-distribution-services-kds-root-key-is-already-pre-created"></a>La clave raíz de servicios de distribución de claves (KDS) ya se ha creado previamente
La clave raíz de servicios de distribución de claves (KDS) se ha creado previamente en un dominio administrado de Azure AD Domain Services. No es necesario crear una clave raíz de KDS y no tiene privilegios para hacerlo. Tampoco puede ver la clave raíz de KDS en el dominio administrado.

## <a name="sample---create-a-gmsa-using-powershell"></a>Ejemplo: crear una gMSA con PowerShell
En el ejemplo siguiente se muestra cómo crear una unidad organizativa personalizada con PowerShell. A continuación, puede crear una gMSA dentro de esa unidad organizativa con el parámetro ```-Path``` para especificar la unidad organizativa.

```powershell
# Create a new custom OU on the managed domain
New-ADOrganizationalUnit -Name "MyNewOU" -Path "DC=CONTOSO100,DC=COM"

# Create a service account 'WebFarmSvc' within the custom OU.
New-ADServiceAccount -Name WebFarmSvc  `
-DNSHostName ` WebFarmSvc.contoso100.com  `
-Path "OU=MYNEWOU,DC=CONTOSO100,DC=com"  `
-KerberosEncryptionType AES128, AES256  ` -ManagedPasswordIntervalInDays 30  `
-ServicePrincipalNames http/WebFarmSvc.contoso100.com/contoso100.com, `
http/WebFarmSvc.contoso100.com/contoso100,  `
http/WebFarmSvc/contoso100.com, http/WebFarmSvc/contoso100  `
-PrincipalsAllowedToRetrieveManagedPassword CONTOSO-SERVER$
```

**Documentación de cmdlets de PowerShell:**
- [Cmdlet New-ADOrganizationalUnit](https://docs.microsoft.com/powershell/module/addsadministration/new-adorganizationalunit)
- [Cmdlet New-ADServiceAccount](https://docs.microsoft.com/powershell/module/addsadministration/New-ADServiceAccount)


## <a name="next-steps"></a>Pasos siguientes
- [Creación de una unidad organizativa personalizada en un dominio administrado](active-directory-ds-admin-guide-create-ou.md)
- [Información general sobre las cuentas de servicio administradas de grupo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)
- [Introducción a las cuentas de servicio administradas de grupo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/getting-started-with-group-managed-service-accounts)
