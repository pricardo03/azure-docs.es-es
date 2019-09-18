---
title: archivo de inclusión
description: archivo de inclusión
services: active-directory
author: curtand
ms.service: active-directory
ms.topic: include
ms.date: 05/22/2019
ms.author: curtand
ms.custom: include file
ms.openlocfilehash: 421e88374a4ca03311fa77a6568a676aa9ffafa5
ms.sourcegitcommit: d70c74e11fa95f70077620b4613bb35d9bf78484
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2019
ms.locfileid: "70919738"
---
Estas son las restricciones de uso y otros límites de servicio para el servicio Azure Active Directory (Azure AD).

| Categoría | Límites |
| --- | --- |
| Directorios | Un usuario único puede pertenecer a un máximo de 500 directorios de Azure AD como miembro o invitado.<br/>Un usuario único puede crear un máximo de 20 directorios. |
| Dominios | No puede agregar más de 900 nombres de dominio administrados. Si configura todos los dominios para la federación con un entorno local de Active Directory, no puede agregar más de 450 nombres de dominio en cada directorio. |
| Objetos |<ul><li>De forma predeterminada, los usuarios de la edición gratuita de Azure Active Directory pueden crear un máximo de 50 000 objetos en un solo directorio. Si tiene al menos un dominio comprobado, la cuota de servicio de directorio predeterminada de Azure AD se amplía a 300 000 objetos. </li><li>Un usuario que no es administrador puede crear hasta 250 objetos. Tanto los objetos activos como los objetos eliminados que están disponibles para restaurar se contabilizan para esta cuota. Solo están disponibles para restaurar los objetos que se han eliminado hace menos de 30 días. Los objetos eliminados que ya no están disponibles para restaurar se contabilizan para esta cuota en un valor de un cuarto durante 30 días. Quizás deba [asignar un rol de administrador](../articles/active-directory/users-groups-roles/directory-assign-admin-roles.md) a los usuarios que no sean administradores que sea probable que superen repetidamente la cuota al realizar sus tareas habituales.</li></ul> |
| Extensiones de esquema |<ul><li>Las extensiones de tipo String pueden tener un máximo de 256 caracteres. </li><li>Las extensiones de tipo Binary están limitadas a 256 bytes.</li><li>Los valores de extensión 100 (entre *todos* los tipos y *todas* las aplicaciones) son los únicos que se pueden escribir en cualquier objeto único.</li><li>Las entidades User, Group, TenantDetail, Device, Application y ServicePrincipal son las únicas que se pueden ampliar con atributos de valor único de tipo String o de tipo Binary.</li><li>Las extensiones de esquema solo están disponibles en la versión preliminar de la versión 1.21 de Graph API. La aplicación debe tener acceso de escritura para registrar una extensión.</li></ul> |
| APLICACIONES |Un máximo de 100 usuarios pueden ser propietarios de una sola aplicación. |
| Grupos |<ul><li>Un máximo de 100 usuarios pueden ser propietarios de un solo grupo.</li><li>Cualquier número de objetos puede ser miembro de un solo grupo.</li><li>Un usuario puede ser un miembro de cualquier número de grupos.</li><li>El número de miembros de un grupo que pueden realizar la sincronización desde su instancia local de Active Directory a Azure Active Directory mediante Azure AD Connect se limita a 50 000 miembros.</li></ul> |
| Proxy de aplicación | <ul><li>Un máximo de 500 transacciones por segundo por aplicación de proxy de aplicaciones</li><li>Un máximo de 750 transacciones por segundo para el inquilino</li></ul><br/>Una transacción se define como una única solicitud y respuesta http para un único recurso. Cuando se limitan, los clientes recibirán una respuesta 429 (demasiadas solicitudes). |
| Panel de acceso |<ul><li>No hay límite en el número de aplicaciones que se pueden ver en el Panel de acceso por usuario. Esto se aplica a los usuarios con licencias para Azure AD Premium o Enterprise Mobility Suite.</li><li>Cada usuario puede ver un máximo de 10 iconos de aplicación en el Panel de acceso. Este límite se aplica a los usuarios que tienen licencias asignadas para el plan de licencias de Azure AD Free. Algunos ejemplos de iconos de aplicación son Box, Salesforce o Dropbox. Este límite no se aplica a las cuentas de administrador.</li></ul> |
| Informes | Se puede ver o descargar en cualquier informe un máximo de 1.000 filas. Los datos adicionales se truncan. |
| Unidades administrativas | Un objeto puede ser un miembro de como máximo 30 unidades administrativas. |
| Roles y permisos de administrador | <ul><li>No se puede agregar un grupo como [propietario](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context#object-ownership).</li><li>No se puede asignar un grupo a un [rol](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles).</li><li>La capacidad de los usuarios para leer la información del directorio de otros usuarios no puede restringirse fuera del modificador de todo los inquilinos para desactivar el acceso de todos los usuarios no administradores a toda la información del directorio (no recomendado). Mas información sobre los permisos predeterminados [aquí](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context#to-restrict-the-default-permissions-for-member-users).</li><li>Pueden pasar hasta 15 minutos o el cierre o inicio de sesión antes de que surtan efecto las adiciones y revocaciones de los miembros del rol de administrador.</li></ul> |
