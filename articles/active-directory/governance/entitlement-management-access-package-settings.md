---
title: 'Compartición de un vínculo para solicitar un paquete de acceso en la administración de derechos de Azure AD: Azure Active Directory'
description: Obtenga información sobre cómo compartir un vínculo para solicitar un paquete de acceso en la administración de derechos de Azure Active Directory.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 10/15/2019
ms.author: ajburnle
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 94d32bf26def6832f51b4e09c697bca885162dc2
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2019
ms.locfileid: "73174407"
---
# <a name="share-link-to-request-an-access-package-in-azure-ad-entitlement-management"></a>Compartición de un vínculo para solicitar un paquete de acceso en la administración de derechos de Azure AD

La mayoría de los usuarios del directorio pueden iniciar sesión en el portal Mi acceso y ver automáticamente una lista de paquetes de acceso que pueden solicitar. Pero para los usuarios de socios comerciales externos que aún no están en el directorio, deberá enviarles un vínculo que pueden usar para solicitar un paquete de acceso. 

Siempre que el catálogo para el paquete de acceso esté [habilitado para los usuarios externos](entitlement-management-catalog-create.md) y tenga una [directiva para el directorio del usuario externo](entitlement-management-access-package-request-policy.md), este puede utilizar el vínculo del portal Mi acceso para solicitar el paquete de acceso.

## <a name="share-link-to-request-an-access-package"></a>Compartición de un vínculo para solicitar un paquete de acceso

**Rol necesario:** Administrador global, administrador de usuarios, propietario del catálogo o administrador de paquetes de acceso.

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobernanza de identidades**.

1. En el menú de la izquierda, haga clic en **Paquetes de acceso** y luego abra el paquete de acceso.

1. En la página Información general, copie el **vínculo del portal Mi acceso**.

    ![Información general del paquete acceso: vínculo del portal Mi acceso](./media/entitlement-management-shared/my-access-portal-link.png)

    Es importante que copie todo el vínculo al portal Mi acceso cuando lo envíe a un partner comercial interno. De este modo, se garantiza que el partner obtendrá acceso al portal del directorio para realizar su solicitud. El vínculo comienza con `myaccess`, incluye una sugerencia de directorio y termina con un identificador de paquete de acceso.

    `https://myaccess.microsoft.com/@<directory_hint>#/access-packages/<access_package_id>`

1. Envíe por correo electrónico o envíe el vínculo a su socio comercial externo. Este puede compartir el vínculo con sus usuarios para solicitar el paquete de acceso.

## <a name="next-steps"></a>Pasos siguientes

- [Solicitud de acceso a un paquete de acceso](entitlement-management-request-access.md)
- [Aprobación o denegación de solicitudes de acceso](entitlement-management-request-approve.md)