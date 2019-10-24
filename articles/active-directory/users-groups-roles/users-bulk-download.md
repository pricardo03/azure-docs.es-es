---
title: Descarga de una lista de usuarios (versión preliminar) en el portal de Azure Active Directory | Microsoft Docs
description: Descargue los registros de usuarios de forma masiva en el Centro de administración de Azure en Azure Active Directory.
services: active-directory
author: curtand
ms.author: curtand
manager: mtillman
ms.date: 07/15/2019
ms.topic: conceptual
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
ms.custom: it-pro
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: fd0829afca05058892d3a0ceeb50c9955d792dc3
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2019
ms.locfileid: "72517064"
---
# <a name="download-a-list-of-users-preview-in-azure-active-directory-portal"></a>Descarga de una lista de usuarios (versión preliminar) en el portal de Azure Active Directory

Azure Active Directory (Azure AD) admite operaciones (creación) de importación masiva de usuarios.

## <a name="required-permissions"></a>Permisos necesarios

Para descargar la lista de usuarios del Centro de administración de Azure AD, debe haber iniciado sesión con un usuario asignado a uno o más roles de administrador de nivel de organización en Azure AD. Los invitadores de usuarios invitados y los desarrolladores de aplicaciones no se consideran roles de administrador.

## <a name="to-download-a-list-of-users"></a>Para descargar una lista de usuarios

1. [Inicie sesión en la organización de Azure AD](https://aad.portal.azure.com) con una cuenta de administrador de usuarios de la organización.
1. En Azure AD, seleccione **Usuarios** > **Descargar usuarios**.
1. En la página **Descargar usuarios**, seleccione **Iniciar** para recibir un archivo .csv que enumera las propiedades de perfil del usuario. Si hay errores, puede descargar y ver el archivo de resultados en la página de resultados de la operación masiva. El archivo contiene el motivo de cada error.

   ![Seleccione dónde quiere la lista de usuarios que quiere descargar.](./media/users-bulk-download/bulk-download.png)

   El archivo de descarga contendrá la lista filtrada de usuarios.

   Se incluirán los atributos de usuario siguientes: 

   - userPrincipalName
   - DisplayName
   - surname
   - mail
   - givenName
   - objectId
   - userType
   - jobTitle
   - department
   - manager
   - accountEnabled
   - usageLocation
   - streetAddress
   - state
   - country
   - physicalDeliveryOfficeName
   - city
   - postalCode
   - telephoneNumber
   - mobile
   - authenticationPhoneNumber
   - authenticationAlternativePhoneNumber
   - authenticationEmail
   - alternateEmailAddress
   - ageGroup
   - consentProvidedForMinor
   - legalAgeGroupClassification

## <a name="check-status"></a>Comprobar estado

Puede ver el estado de las solicitudes masivas pendientes en la página de **resultados de la operación masiva (versión preliminar)** .

   ![Comprobación del estado de carga en la página de resultados de la operación masiva](./media/users-bulk-download/bulk-center.png)

## <a name="bulk-download-service-limits"></a>Límites del servicio de descarga de forma masiva

La ejecución de cada actividad en bloque para crear una lista de usuarios puede durar hasta una hora. Esto permite la creación y descarga de una lista de al menos 500 000 usuarios.

## <a name="next-steps"></a>Pasos siguientes

- [Adición masiva de usuarios](users-bulk-add.md)
- [Eliminación masiva de usuarios](users-bulk-delete.md)
- [Restauración de usuarios masiva](users-bulk-restore.md)
