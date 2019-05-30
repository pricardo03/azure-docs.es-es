---
title: archivo de inclusión
description: archivo de inclusión con requisitos previos
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 66b5f8e2-e08d-43c8-b460-6204aecda8f7
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.custom: include file
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: maheshu
ms.openlocfilehash: 1fba8cc9ae40cf5539016bbd73de65f557a64136
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66111383"
---
> [!IMPORTANT]
> **Habilite la sincronización de hash de contraseña con Azure AD Domain Services, antes de completar las tareas de este artículo.**
>
> Siga las instrucciones que aparecen a continuación, en función del tipo de usuarios de su directorio de Azure AD. Complete ambos conjuntos de instrucciones si tiene una combinación de cuentas de usuario solo en la nube y sincronizados en su directorio de Azure AD. No es posible que pueda llevar a cabo las siguientes operaciones en caso de que está intentando usar una cuenta de invitado de B2B (ejemplo, su gmail o MSA de un proveedor de identidad distinto que nos permiten) porque no tenemos la contraseña para estos usuarios sincronizados con el dominio administrado ya que estas son las cuentas de invitado en el directorio. La información completa sobre estas cuentas incluidas sus contraseñas sería fuera de Azure AD y que esta información no esté en Azure AD, por tanto, no incluso permanecer sincronizado con el dominio administrado. 
> - [Instrucciones para cuentas de usuario solo en la nube](../articles/active-directory-domain-services/active-directory-ds-getting-started-password-sync.md)
> - [Instrucciones para cuentas de usuario sincronizados desde un directorio local](../articles/active-directory-domain-services/active-directory-ds-getting-started-password-sync-synced-tenant.md)
