---
title: Servicios de identidad y seguridad de Azure Alemania | Microsoft Docs
description: Proporciona una comparación de los servicios de seguridad e identidad de Azure Alemania
services: germany
cloud: na
documentationcenter: na
author: gitralf
manager: rainerst
ms.assetid: na
ms.service: germany
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/13/2017
ms.author: ralfwi
ms.openlocfilehash: ee98e074b8bf21abe198506899b99be4e2908f14
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67033493"
---
# <a name="azure-germany-security-and-identity-services"></a>Servicios de identidad y seguridad de Azure Alemania
## <a name="key-vault"></a>Key Vault
Para más información acerca del servicio Azure Key Vault y cómo usarlo, consulte la [documentación global de Azure Key Vault](../key-vault/index.yml).

Key Vault está disponible en Azure Alemania con carácter general. Como en Azure global, no hay ninguna extensión, por lo que solo está disponible mediante PowerShell y la CLI.

## <a name="azure-active-directory"></a>Azure Active Directory
Azure Active Directory ofrece funcionalidades de identidad y acceso para los sistemas de información que se ejecutan en Microsoft Azure. Mediante el uso de servicios de directorio, grupos de seguridad y una directiva de grupo, puede ayudar a controlar las directivas de acceso y seguridad de las máquinas que usan Azure Active Directory. Puede usar las cuentas y los grupos de seguridad, junto con el control de acceso basado en roles (RBAC), para ayudar a administrar el acceso a los sistemas de información. 

Azure Active Directory está disponible en Azure Alemania con carácter general.

### <a name="variations"></a>Variaciones

* Azure Active Directory en Azure Alemania es completamente independiente de Azure Active Directory en Azure global. 
* Los clientes no pueden usar una cuenta Microsoft para iniciar sesión en Azure Alemania.
* El sufijo de inicio de sesión de Azure Alemania es *onmicrosoft.de* (no *onmicrosoft.com*, como en Azure global).
* Los clientes necesitan una suscripción independiente para trabajar en Azure Alemania.
* Los clientes de Azure Alemania no pueden acceder a recursos que requieran una suscripción o una identidad en Azure global.
* Los clientes de Azure global no pueden acceder a recursos que requieran una suscripción o una identidad en Azure Alemania.
* Los dominios adicionales se pueden agregar y comprobar solo en uno de los entornos de nube.
 
> [!NOTE]
> La asignación de derechos a los usuarios de otros inquilinos con *ambos inquilinos dentro de Azure Alemania* aún no está disponible.


## <a name="next-steps"></a>Pasos siguientes
Para información complementaria y actualizaciones, suscríbase al [blog de Azure Alemania](https://blogs.msdn.microsoft.com/azuregermany/).




