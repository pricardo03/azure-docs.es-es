---
title: Comparación de características de los planes de tarifa de Azure API Management | Microsoft Docs
description: En este artículo se comparan los planes de tarifa de API Management según las características que ofrecen.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/26/2018
ms.author: apimpm
ms.openlocfilehash: eae36aa6e60e3da03c59952a1d9e035e6a773d2d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60531074"
---
# <a name="feature-based-comparison-of-the-azure-api-management-tiers"></a>Comparación de características de los planes de tarifa de Azure API Management

Cada [plan de tarifa](https://aka.ms/apimpricing) de API Management ofrece un conjunto diferente de características y [capacidad](api-management-capacity.md) por unidad. En la tabla siguiente se resumen las características clave disponibles en cada uno de los planes. Algunas características podrían funcionar de manera diferente o tener funcionalidades diferentes según el plan. En tales casos, se indican las diferencias en los artículos de documentación que describen estas características individuales.

| Característica                                                                                      | Consumo <sup>VERSIÓN PRELIMINAR</sup> | Developer      | Básica          | Estándar       | Premium        |
| -------------------------------------------------------------------------------------------- | ----------------------------- | -------------- | -------------- | -------------- | -------------- |
| Integración de Azure AD<sup>1</sup>                                                             | Sin                             | Sí            | Sin              | Sí            | Sí            |
| Compatibilidad con redes virtuales (VNet)                                                               | Sin                             | Sí            | No             | No             | Sí            |
| Implementación en varias regiones                                                                      | Sin                             | No             | No             | No             | Sí            |
| Múltiples nombres de dominio personalizados                                                                 | Sin                             | No             | No             | No             | Sí            |
| Portal para desarrolladores<sup>2</sup>                                                                 | Sin                             | Sí            | Sí            | Sí            | Sí            |
| Memoria caché integrada                                                                               | Sin                             | Sí            | Sí            | Sí            | Sí            |
| Análisis integrado                                                                           | Sin                             | Sí            | Sí            | Sí            | Sí            |
| [Configuración de SSL](api-management-howto-manage-protocols-ciphers.md)                             | Sin                             | Sí            | Sí            | Sí            | Sí            |
| [Caché externa](https://aka.ms/apimbyoc)                                                    | Sí                           | No<sup>3</sup> | No<sup>3</sup> | No<sup>3</sup> | No<sup>3</sup> |
| [Autenticación de certificados de clientes](api-management-howto-mutual-certificates-for-clients.md) | No<sup>4</sup>                | Sí            | Sí            | Sí            | Sí            |
| [Copia de seguridad y restauración](api-management-howto-disaster-recovery-backup-restore.md)               | Sin                             | Sí            | Sí            | Sí            | Sí            |
| [Administración a través de Git](api-management-configuration-repository-git.md)                        | Sin                             | Sí            | Sí            | Sí            | Sí            |
| API de administración directa                                                                        | Sin                             | Sí            | Sí            | Sí            | Sí            |
| Métricas y registros de Azure Monitor                                                               | No<sup>5</sup>                | Sí            | Sí            | Sí            | Sí            |

<sup>1</sup> Habilita el uso de Azure AD (y Azure AD B2C) como proveedor de identidades para el inicio de sesión del usuario en el portal para desarrolladores.<br/>
<sup>2</sup> Incluye funciones relacionadas, por ejemplo, plantillas y notificaciones de usuarios, grupos, problemas, aplicaciones y correo electrónico.<br/>
<sup>3</sup> Próximamente, se admitirá la memoria caché externa para este nivel.<br/>
<sup>4</sup> La autenticación de certificados de cliente se agregará al plan de consumo antes de alcanzar la fase de disponibilidad general.<br/>
<sup>5</sup> Se agregará compatibilidad total con Azure Monitor en el plan de consumo.
