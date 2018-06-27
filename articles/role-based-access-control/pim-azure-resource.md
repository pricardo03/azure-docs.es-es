---
title: Administración del acceso a los recursos de Azure con Privileged Identity Management (PIM)
description: Obtenga información sobre cómo administrar el acceso a los recursos de Azure con Privileged Identity Management (PIM) y el control de acceso basado en rol (RBAC).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: skwan
ms.assetid: ba06b8dd-4a74-4bda-87c7-8a8583e6fd14
ms.service: role-based-access-control
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/30/2018
ms.author: rolyon
ms.reviewer: skwan
ms.openlocfilehash: 838c889f2dc099b4a4c5d84521871c64eb989163
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36293757"
---
# <a name="manage-access-to-azure-resources-with-privileged-identity-management"></a>Administración del acceso a recursos de Azure con Privileged Identity Management

Para proteger las cuentas con privilegios de ciberataques malintencionados, puede usar Azure Active Directory Privileged Identity Management (PIM) para reducir el tiempo de exposición de los privilegios y aumentar la visibilidad de su uso a través de alertas e informes. PIM lo hace limitando a los usuarios a que solo acepten sus privilegios "just-in-time" (JIT) o mediante la asignación de privilegios por un período de tiempo más corto, tras el cual se revocan automáticamente. 

Ya puede usar PIM con control de acceso basado en roles (RBAC) de Azure para administrar, controlar y supervisar el acceso a recursos de Azure. PIM puede administrar la pertenencia de roles integrados y personalizados para ayudarle a: 

- Habilitar el acceso "just-in-time" y a petición a recursos de Azure
- Hacer expirar automáticamente el acceso a los recursos para usuarios y grupos asignados
- Asignar acceso temporal a recursos de Azure para tareas rápidas u horarios de guardia
- Reciba alertas cuando se asigne acceso a los recursos para nuevos usuarios o grupos, así como cuando activen asignaciones aptas

Para más información, consulte [Información general de control de acceso basado en rol de Azure PIM](../active-directory/privileged-identity-management/azure-pim-resource-rbac.md).