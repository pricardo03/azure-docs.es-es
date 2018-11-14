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
ms.openlocfilehash: a2d9ece119003c341f49ee03d735d5636b179a32
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2018
ms.locfileid: "51259894"
---
# <a name="enable-multitenant-applications-with-azure-digital-twins"></a>Habilitar aplicaciones multiinquilino con Azure Digital Twins

Los desarrolladores que usan Azure Digital Twins normalmente desean crear aplicaciones multiinquilino. Un *aplicación multiinquilino* es una instancia única de aprovisionamiento que admite varios clientes. Cada cliente tiene sus propios privilegios y datos independientes.

En este documento se describe cómo crear una aplicación multiinquilino de Azure Digital Twins que admita varios inquilinos y clientes de Azure Active Directory (Azure AD).

## <a name="scenario-summary"></a>Resumen del escenario

En este escenario, considere al Desarrollador D y al Cliente C:

- El Desarrollador D tiene una suscripción de Azure con un inquilino de Azure AD.
- El Desarrollador D implementa una instancia de Azure Digital Twins en su suscripción de Azure.
- Los usuarios que pertenecen al inquilino de Azure AD del Desarrollador D pueden obtener tokens en el servicio de Azure Digital Twins, ya que Azure AD ha creado una entidad de servicio en el inquilino de Azure AD del Desarrollador D.
- El Desarrollador D ahora crea una aplicación móvil que se integra directamente con la API de administración de Azure Digital Twins.
- El Desarrollador D permite al Cliente C usar la aplicación móvil.
- El Cliente C debe estar autorizado para usar la API de administración de Azure Digital Twins dentro de la aplicación del Desarrollador D.

  > [!IMPORTANT]
  > - Cuando el Cliente C inicie sesión en la aplicación del Desarrollador D, la aplicación no podrá adquirir tokens para que los usuarios del Cliente C puedan comunicarse con la API de administración.
  > - Azure AD producirá un error que indica que Azure Digital Twins no se reconoce en el directorio del Cliente C.

## <a name="solution"></a>Solución

Para resolver el escenario anterior, debe realizar las siguientes acciones a fin de crear una entidad de servicio de Azure Digital Twins dentro del inquilino de Azure AD del Cliente C:

- Si el Cliente C aún no tiene una suscripción de Azure con un inquilino de Azure AD:

  - El administrador del inquilino de Azure AD del Cliente C debe adquirir una [suscripción de Azure de pago por uso](https://azure.microsoft.com/offers/ms-azr-0003p/).
  - El administrador del inquilino de Azure AD del Cliente C, a continuación, debe [vincular el inquilino con la nueva suscripción](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).

- En [Azure Portal](https://portal.azure.com), el administrador del inquilino de Azure AD del Cliente C debe realizar los pasos siguientes:

  1. Abra la página de **Suscripciones**.
  1. Seleccione la suscripción correspondiente al inquilino de Azure AD que se usará en la aplicación del Desarrollador D.
  1. Seleccione **Proveedores de recursos**.
  1. Busque **Microsoft.IoTSpaces**.
  1. Seleccione **Registrar**.
  
## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre cómo usar funciones definidas por el usuario con Azure Digital Twins, lea [funciones definidas por el usuario en Azure Digital Twins](how-to-user-defined-functions.md).

Para obtener información sobre cómo usar el control de acceso basado en roles para proteger aún más la aplicación con las asignaciones de roles, consulte [control de acceso basado en rol de Azure Digital Twins](security-create-manage-role-assignments.md).
