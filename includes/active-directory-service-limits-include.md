---
title: archivo de inclusión
description: archivo de inclusión
services: active-directory
author: curtand
ms.service: active-directory
ms.topic: include
ms.date: 02/19/2019
ms.author: curtand
ms.custom: include file
ms.openlocfilehash: 619dd7f3b01e2e7ce71e945fce77aa73cb87f264
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/20/2019
ms.locfileid: "56443309"
---
Estas son las restricciones de uso y otros límites de servicio para el servicio Azure Active Directory (Azure AD).

| Categoría | Límites |
| --- | --- |
| Directorios | Un usuario único puede pertenecer a un máximo de 500 directorios de Azure AD como miembro o invitado.<br/>Un usuario único puede crear un máximo de 20 directorios. |
| Dominios | No puede agregar más de 900 nombres de dominio administrados. Si va a configurar todos los dominios para la federación con un entorno local de Active Directory, no puede agregar más de 450 nombres de dominio en cada directorio. |
| Objetos |<ul><li>Se puede crear un máximo de 500 000 objetos en un solo directorio por los usuarios de la edición gratuita de Azure Active Directory.</li><li>Un usuario que no es administrador puede crear hasta 250 objetos.</li></ul> |
| Extensiones de esquema |<ul><li>Las extensiones de tipo cadena pueden tener 256 caracteres como máximo. </li><li>Las extensiones de tipo Binary están limitadas a 256 bytes.</li><li>Los valores de extensión 100 (en todos los tipos y todas las aplicaciones) se pueden escribir en cualquier objeto único.</li><li>Solo las entidades "User", "Group", "TenantDetail", "Device", "Application" y "ServicePrincipal" se pueden extender con atributos de valor único de tipo "String" o tipo "Binary".</li><li>Las extensiones de esquema solo están disponibles en la vista previa de la versión 1.21 de Graph API. La aplicación debe tener acceso de escritura para registrar una extensión.</li></ul> |
| APLICACIONES |Un máximo de 100 usuarios pueden ser propietarios de una sola aplicación. |
| Grupos |<ul><li>Un máximo de 100 usuarios pueden ser propietarios de un solo grupo.</li><li>Cualquier número de objetos puede ser miembro de un solo grupo.</li><li>Un usuario puede ser un miembro de cualquier número de grupos.</li><li>El número de miembros de un grupo que pueden realizar la sincronización desde su instancia de Active Directory local a Azure Active Directory mediante Azure AD Connect se limita a 50 000 miembros.</li></ul> |
| Panel de acceso |<ul><li>No existe ningún límite en el número de aplicaciones que se ven en el Panel de acceso por cada usuario final, en el caso de las licencias asignadas a usuarios de Azure AD Premium o Enterprise Mobility Suite.</li><li>En el Panel de acceso pueden verse hasta 10 iconos de aplicación (ejemplos: Box, Salesforce o Dropbox) para cada usuario final en el caso de usuarios con licencias asignadas de las ediciones gratuita o Azure AD Basic de Azure Active Directory. Este límite no se aplica a las cuentas de administrador.</li></ul> |
| Informes | Se puede ver o descargar en cualquier informe un máximo de 1.000 filas. Los datos adicionales se truncan. |
| Unidades administrativas | Un objeto puede ser un miembro de como máximo 30 unidades administrativas. |
| Roles y permisos de administrador | <li>No se puede agregar un grupo como propietario<li>No se puede asignar un grupo a un rol<li>No se pueden cambiar los permisos de usuario predeterminados, a excepción del cambio de inquilino (configuración de usuario en Azure AD) |
