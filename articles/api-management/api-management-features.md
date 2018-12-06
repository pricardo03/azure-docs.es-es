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
ms.openlocfilehash: a00328608c582dcd28dbc78b5b56829f9d1ab500
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "52585492"
---
# <a name="feature-based-comparison-of-the-azure-api-management-tiers"></a>Comparación de características de los planes de tarifa de Azure API Management

Cada [plan de tarifa](https://aka.ms/apimpricing) de API Management ofrece un conjunto diferente de características y [capacidad](api-management-capacity.md) por unidad. En la tabla siguiente se resumen las características clave disponibles en cada uno de los planes. Algunas características podrían funcionar de manera diferente o tener funcionalidades diferentes según el plan. En tales casos, se indican las diferencias en los artículos de documentación que describen estas características individuales.

| Característica                                                                                      | Consumo <sup>VERSIÓN PRELIMINAR</sup> | Developer      | Básica          | Estándar       | Premium        |
| -------------------------------------------------------------------------------------------- | ----------------------------- | -------------- | -------------- | -------------- | -------------- |
| Integración de Azure AD<sup>1</sup>                                                             | Sin                             | Sí            | Sin              | SÍ            | SÍ            |
| Compatibilidad con redes virtuales (VNet)                                                               | Sin                             | Sí            | No             | No             | SÍ            |
| Implementación en varias regiones                                                                      | Sin                             | No             | No             | No             | SÍ            |
| Múltiples nombres de dominio personalizados                                                                 | Sin                             | No             | No             | No             | SÍ            |
| Portal para desarrolladores<sup>2</sup>                                                                 | Sin                             | SÍ            | Sí            | Sí            | SÍ            |
| Memoria caché integrada                                                                               | Sin                             | SÍ            | Sí            | Sí            | SÍ            |
| Análisis integrado                                                                           | Sin                             | SÍ            | Sí            | Sí            | SÍ            |
| [Configuración de SSL](api-management-howto-manage-protocols-ciphers.md)                             | Sin                             | SÍ            | Sí            | Sí            | SÍ            |
| [Caché externa](https://aka.ms/apimbyoc)                                                    | SÍ                           | No<sup>3</sup> | No<sup>3</sup> | No<sup>3</sup> | No<sup>3</sup> |
| [Autenticación de certificados de clientes](api-management-howto-mutual-certificates-for-clients.md) | No<sup>4</sup>                | SÍ            | Sí            | Sí            | SÍ            |
| [Copia de seguridad y restauración](api-management-howto-disaster-recovery-backup-restore.md)               | Sin                             | SÍ            | Sí            | Sí            | SÍ            |
| [Administración a través de Git](api-management-configuration-repository-git.md)                        | Sin                             | SÍ            | Sí            | Sí            | SÍ            |
| API de administración directa                                                                        | Sin                             | SÍ            | Sí            | Sí            | SÍ            |
| Métricas y registros de Azure Monitor                                                               | No<sup>5</sup>                | SÍ            | Sí            | Sí            | SÍ            |

<sup>1</sup> Habilita el uso de Azure AD (y Azure AD B2C) como proveedor de identidades para el inicio de sesión del usuario en el portal para desarrolladores.<br/>
<sup>2</sup> Incluye funciones relacionadas, por ejemplo, plantillas y notificaciones de usuarios, grupos, problemas, aplicaciones y correo electrónico.<br/>
<sup>3</sup> Próximamente, se admitirá la memoria caché externa para este nivel.<br/>
<sup>4</sup> La autenticación de certificados de cliente se agregará al plan de consumo antes de alcanzar la fase de disponibilidad general.<br/>
<sup>5</sup> Se agregará compatibilidad total con Azure Monitor en el plan de consumo.
