---
title: archivo de inclusión
description: archivo de inclusión
services: active-directory
author: curtand
ms.service: active-directory
ms.topic: include
ms.date: 02/21/2019
ms.author: curtand
ms.custom: include file
ms.openlocfilehash: 38f2dd301ddc2a5f8d28322856b2011bd2034c30
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57554665"
---
Estas son las restricciones de uso y otros límites de servicio para el servicio Azure Active Directory (Azure AD).

| Categoría | Límites |
| --- | --- |
| Directorios | Un usuario único puede pertenecer a un máximo de 500 directorios de Azure AD como miembro o invitado.<br/>Un usuario único puede crear un máximo de 20 directorios. |
| Dominios | No puede agregar más de 900 nombres de dominio administrados. Si configura todos los dominios para federación con Active Directory local, puede agregar no más de 450 nombres de dominio en cada directorio. |
| Objetos |<ul><li>Se puede crear un máximo de 500 000 objetos en un solo directorio por los usuarios de la edición gratuita de Azure Active Directory.</li><li>Un usuario que no es administrador puede crear hasta 250 objetos. Objetos activos y los objetos eliminados que están disponibles para restaurar esta cuota se contabilizan. Solo los objetos eliminados que se eliminaron hace menos de 30 días están disponibles para restaurar. Los objetos eliminados que ya no están disponibles para restaurar se contabilizan esta cuota en un valor de un cuarto durante 30 días. Quizás [asignar un rol de administrador](../articles/active-directory/users-groups-roles/directory-assign-admin-roles.md) a los usuarios sin derechos administrativos que es probables que repetidamente sobrepasar la cuota en el transcurso de sus tareas habituales.</li></ul> |
| Extensiones de esquema |<ul><li>Las extensiones de tipo de cadena pueden tener un máximo de 256 caracteres. </li><li>Las extensiones de tipo binario están limitadas a 256 bytes.</li><li>Solo 100 valores de extensión, en *todas* tipos y *todas* aplicaciones, pueden escribirse en cualquier objeto único.</li><li>Solo las entidades de usuario, grupo, TenantDetail, dispositivo, Application y ServicePrincipal se pueden extender con atributos de valor único de tipo binario o de tipo de cadena.</li><li>Extensiones de esquema solo están disponibles en la vista previa versión 1.21 de Graph API. La aplicación debe tener acceso de escritura para registrar una extensión.</li></ul> |
| APLICACIONES |Un máximo de 100 usuarios pueden ser propietarios de una sola aplicación. |
| Grupos |<ul><li>Un máximo de 100 usuarios pueden ser propietarios de un solo grupo.</li><li>Cualquier número de objetos puede ser miembro de un solo grupo.</li><li>Un usuario puede ser un miembro de cualquier número de grupos.</li><li>El número de miembros de un grupo que se puede sincronizar desde Active Directory local a Azure Active Directory con Azure AD Connect se limita a 50.000 miembros.</li></ul> |
| Panel de acceso |<ul><li>No hay ningún límite al número de aplicaciones que pueden verse en el Panel de acceso por usuario. Esto se aplica a los usuarios con licencias asignadas de Azure AD Premium o Enterprise Mobility Suite.</li><li>Un máximo de 10 iconos de aplicación puede verse en el Panel de acceso para cada usuario. Este límite se aplica a los usuarios que tienen asignados licencias gratis o ediciones de Azure AD Basic de Azure Active Directory. Algunos ejemplos de iconos de aplicación son cuadro, Salesforce o Dropbox. Este límite no se aplica a las cuentas de administrador.</li></ul> |
| Informes | Se puede ver o descargar en cualquier informe un máximo de 1.000 filas. Los datos adicionales se truncan. |
| Unidades administrativas | Un objeto puede ser un miembro de como máximo 30 unidades administrativas. |
| Roles y permisos de administrador | <li>No se puede agregar un grupo como propietario.<li>No se puede asignar un grupo a un rol.<li>No se puede cambiar los permisos, excepto los modificadores de inquilino, que es la configuración de usuario de Azure AD de usuario de forma predeterminada. |
