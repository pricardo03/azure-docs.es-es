---
title: Habilitar aplicaciones multiinquilino con Azure Digital Twins | Microsoft Docs
description: Descripción de cómo registrar los inquilinos de Azure Active Directory de sus clientes con Azure Digital Twins
author: mavoge
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/08/2018
ms.author: mavoge
ms.openlocfilehash: e465ab95b670268f8ef31472259783fce8f24b36
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "49324018"
---
# <a name="how-to-enable-multitenant-applications-with-azure-digital-twins"></a>Habilitar aplicaciones multiinquilino con Azure Digital Twins

Los desarrolladores que usan la plataforma de Azure Digital Twins probablemente querrán crear aplicaciones para varios inquilinos. Una *aplicación multiinquilino* es una instancia única de aprovisionado que se usa para admitir varios clientes, cada uno con sus propios datos y privilegios independientes.

En este documento se describe cómo crear una aplicación multiinquilino de Azure Digital Twins que admita varios inquilinos y clientes de Azure Active Directory (AD).

## <a name="scenario-summary"></a>Resumen del escenario

En este escenario, considere al Desarrollador D y al Cliente C:

- El Desarrollador D tiene una suscripción de Azure con un inquilino de Azure AD.
- El Desarrollador D ha implementado una instancia de Digital Twins en su suscripción de Azure.
- Los usuarios que pertenecen al inquilino de Azure AD del Desarrollador D pueden obtener tokens en el servicio de Digital Twins, ya que Azure AD ha creado una entidad de servicio en el inquilino de Azure AD del Desarrollador D.
- El Desarrollador D ahora crea una aplicación móvil que se integra directamente con la API de administración de Digital Twins.
- El Desarrollador D, a continuación, permite al Cliente C usar la aplicación móvil.
- Ahora, al Cliente C deberá estar autorizado para usar la API de administración de Digital Twins dentro de la aplicación del Desarrollador D.

  > [!IMPORTANT]
  > - Cuando el Cliente C inicie sesión en la aplicación del Desarrollador D, la aplicación no podrá adquirir tokens para que los usuarios del Cliente C puedan comunicarse con la API de administración.
  > - Azure AD, a continuación, producirá un error que indica que Digital Twins no se reconoce en el directorio del Cliente C.

## <a name="solution"></a>Solución

Para resolver el escenario anterior, debe realizar las siguientes acciones a fin de crear una entidad de servicio de Digital Twins dentro del inquilino de Azure AD del Cliente C:

- If el Cliente C aún no tiene una suscripción de Azure con el inquilino de Azure AD:

  - El administrador del inquilino de Azure AD perteneciente al Cliente C deberá adquirir una [suscripción de Azure de pago por uso](https://azure.microsoft.com/offers/ms-azr-0003p/).
  - El administrador del inquilino de Azure AD perteneciente al Cliente C, a continuación, tendrá que [vincular el inquilino con la nueva suscripción](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).

- Después, desde [Azure Portal](https://portal.azure.com), el administrador del inquilino de Azure AD perteneciente al Cliente C debe realizar lo siguiente:
  1. Abra la página de **Suscripciones**.
  1. Seleccione la suscripción correspondiente al inquilino de Azure AD que se usará en la aplicación del Desarrollador D.
  1. Seleccione **Proveedores de recursos**.
  1. Busque **Microsoft.IoTSpaces**.
  1. Haga clic en **Registrar**.
  
## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre el uso de funciones definidas por el usuario con Azure Digital Twins, lea [funciones definidas por el usuario en Azure Digital Twins](how-to-user-defined-functions.md).

Para obtener información sobre cómo usar el control de acceso basado en roles para proteger aún más la aplicación con las asignaciones de roles, consulte [control de acceso basado en rol de Digital Twins](security-create-manage-role-assignments.md).
