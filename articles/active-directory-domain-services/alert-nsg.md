---
title: 'Azure Active Directory Domain Services: Solución de problemas relacionados con la configuración del grupo de seguridad de red | Microsoft Docs'
description: Solución de problemas de configuración de NSG para Azure AD Domain Services
services: active-directory-ds
documentationcenter: ''
author: MikeStephens-MS
manager: ''
editor: ''
ms.assetid: 95f970a7-5867-4108-a87e-471fa0910b8c
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2019
ms.author: mstephen
ms.openlocfilehash: 743f83fd25ff897492fda7103d3db1f4b961714d
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66246575"
---
# <a name="troubleshoot-invalid-networking-configuration-for-your-managed-domain"></a>Solución de problemas de configuración de red no válida para el dominio administrado
Este artículo le ayuda a solucionar problemas y resolver errores de configuración relacionados con la red que generan el mensaje de alerta siguiente:

## <a name="alert-aadds104-network-error"></a>Alerta AADDS104: Error de red
**Mensaje de alerta:** *Microsoft no puede tener acceso a los controladores de dominio de este dominio administrado. Esto puede ocurrir si un grupo de seguridad de red (NSG) configurado en la red virtual bloquea el acceso al dominio administrado. Otro motivo posible es que hay una ruta definida por el usuario que bloquea el tráfico entrante desde Internet.*

Las configuraciones de NSG no válidas son la causa más común de los errores de red en Azure AD Domain Services. El grupo de seguridad de red (NSG) configurado para la red virtual debe permitir el acceso a [puertos específicos](network-considerations.md#ports-required-for-azure-ad-domain-services). Si estos puertos están bloqueados, Microsoft no puede supervisar ni actualizar el dominio administrado. Además, se afecta la sincronización entre el directorio de Azure AD y el dominio administrado. Al crear el NSG, mantenga abiertos estos puertos para evitar la interrupción del servicio.

### <a name="checking-your-nsg-for-compliance"></a>Comprobación del cumplimiento del grupo de seguridad de red

1. Vaya a la página [Grupos de seguridad de red](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FNetworkSecurityGroups) en Azure Portal.
2. En la tabla, elija el NSG asociado con la subred en que está habilitado el dominio administrado.
3. En **Configuración** en el panel de la izquierda, haga clic en **Reglas de seguridad de entrada**.
4. Revise las reglas vigentes e identifique las que bloquean el acceso a [estos puertos](network-considerations.md#ports-required-for-azure-ad-domain-services)
5. Edite el grupo de seguridad de red para garantizar su cumplimiento mediante la eliminación de la regla, la incorporación de otra o la creación de un grupo de seguridad de red totalmente nuevo. Los pasos para [agregar una regla](#add-a-rule-to-a-network-security-group-using-the-azure-portal) o crear un grupo de seguridad de red conforme se encuentran a continuación.

## <a name="sample-nsg"></a>NSG de ejemplo
En la tabla siguiente se muestra un NSG de ejemplo que mantendría protegido el dominio administrado al permitir que Microsoft supervise, administre y actualice la información.

![NSG de ejemplo](./media/active-directory-domain-services-alerts/default-nsg.png)

>[!NOTE]
> Azure AD Domain Services requiere acceso saliente sin restricciones desde la red virtual. No se recomienda crear ninguna regla de NSG adicional que restrinja el acceso saliente para la red virtual.

## <a name="add-a-rule-to-a-network-security-group-using-the-azure-portal"></a>Incorporación de una regla en un grupo de seguridad de red con Azure Portal
Si no desea usar PowerShell, puede agregar reglas únicas de manera manual en grupos de seguridad de red con Azure Portal. Para crear reglas en el grupo de seguridad de red, complete estos pasos:

1. Vaya a la página [Grupos de seguridad de red](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FNetworkSecurityGroups) en Azure Portal.
2. En la tabla, elija el NSG asociado con la subred en que está habilitado el dominio administrado.
3. En **Configuración** del panel de la izquierda, haga clic en **Reglas de seguridad de entrada** o en **Reglas de seguridad de salida**.
4. Para crear la regla, haga clic en **Agregar** y rellene la información. Haga clic en **OK**.
5. Para comprobar que se creó la regla, ubíquela en la tabla de reglas.


## <a name="need-help"></a>¿Necesita ayuda?
Póngase en contacto con el equipo de productos de Active Directory Domain Services para [compartir comentarios u obtener asistencia](contact-us.md).
