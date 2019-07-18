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
ms.openlocfilehash: a57f8e44d19432f82abe4fa5e7bafce900db3394
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448011"
---
# <a name="feature-based-comparison-of-the-azure-api-management-tiers"></a>Comparación de características de los planes de tarifa de Azure API Management

Cada [plan de tarifa](https://aka.ms/apimpricing) de API Management ofrece un conjunto diferente de características y [capacidad](api-management-capacity.md) por unidad. En la tabla siguiente se resumen las características clave disponibles en cada uno de los planes. Algunas características podrían funcionar de manera diferente o tener funcionalidades diferentes según el plan. En tales casos, se indican las diferencias en los artículos de documentación que describen estas características individuales.

| Característica                                                                                      | Consumo | Developer      | Básica          | Estándar       | Premium        |
| -------------------------------------------------------------------------------------------- | ----------------------------- | -------------- | -------------- | -------------- | -------------- |
| Integración de Azure AD<sup>1</sup>                                                             | Sin                            | Sí            | Sin             | Sí            | Sí            |
| Compatibilidad con redes virtuales (VNet)                                                               | Sin                            | Sí            | No             | No             | Sí            |
| Implementación en varias regiones                                                                      | Sin                            | No             | No             | No             | Sí            |
| Múltiples nombres de dominio personalizados                                                                 | Sin                            | No             | No             | No             | Sí            |
| Portal para desarrolladores<sup>2</sup>                                                                 | Sin                            | Sí            | Sí            | Sí            | Sí            |
| Memoria caché integrada                                                                               | Sin                            | Sí            | Sí            | Sí            | Sí            |
| Análisis integrado                                                                           | Sin                            | Sí            | Sí            | Sí            | Sí            |
| [Configuración de SSL](api-management-howto-manage-protocols-ciphers.md)                             | Sí                            | Sí            | Sí            | Sí            | Sí            |
| [Caché externa](https://aka.ms/apimbyoc)                                                    | Sí                           | Sí            | Sí            | Sí            | Sí            |
| [Autenticación de certificados de clientes](api-management-howto-mutual-certificates-for-clients.md) | Sí                | Sí            | Sí            | Sí            | Sí            |
| [Copia de seguridad y restauración](api-management-howto-disaster-recovery-backup-restore.md)               | Sin                            | Sí            | Sí            | Sí            | Sí            |
| [Administración a través de Git](api-management-configuration-repository-git.md)                        | Sin                            | Sí            | Sí            | Sí            | Sí            |
| API de administración directa                                                                        | Sin                            | Sí            | Sí            | Sí            | Sí            |
| Métricas y registros de Azure Monitor                                                               | Sin                | Sí            | Sí            | Sí            | Sí            |
| IP estática                                                               | Sin                | Sí            | Sí            | Sí            | Sí            |

<sup>1</sup> Habilita el uso de Azure AD (y Azure AD B2C) como proveedor de identidades para el inicio de sesión del usuario en el portal para desarrolladores.<br/>
<sup>2</sup> Incluye funciones relacionadas, por ejemplo, plantillas y notificaciones de usuarios, grupos, problemas, aplicaciones y correo electrónico.<br/>
