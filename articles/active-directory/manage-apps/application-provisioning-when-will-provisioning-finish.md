---
title: Aprovisionamiento de usuarios a una aplicación de la Galería de Azure AD que tarda horas o más | Microsoft Docs
description: Cómo averiguar por qué el aprovisionamiento de la aplicación puede estar tardando más de lo previsto
services: active-directory
documentationcenter: ''
author: barbkess
manager: daveba
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 28e688edffe79990b76b92b17636484eab353aa3
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55157965"
---
# <a name="user-provisioning-to-an-azure-ad-gallery-application-is-taking-hours-or-more"></a>Aprovisionamiento de usuarios a una aplicación de la Galería de Azure AD que tarda horas o más

Cuando se habilita por primera vez el aprovisionamiento automático de una aplicación, la sincronización inicial puede tardar desde 20 minutos hasta varias horas, según el tamaño del directorio de Azure AD y el número de usuarios en el ámbito de aprovisionamiento. 

Las sincronizaciones posteriores a la inicial serán más rápidas, ya que el servicio de aprovisionamiento almacena marcas de agua que representan el estado de ambos sistemas tras la sincronización inicial, lo cual mejora el rendimiento de las posteriores.

## <a name="how-to-improve-provisioning-performance"></a>Cómo mejorar el rendimiento del aprovisionamiento

Si la sincronización inicial está tardando más de unas horas, puede hacer algo para mejorar el rendimiento:

-   **Filtros de ámbito del usuario.** Los filtros de ámbito le permiten ajustar los datos que el servicio de aprovisionamiento de Azure AD extrae mediante el filtrado de usuarios basado en valores de atributo determinados. Para más información sobre los filtros de ámbito, consulte [Aprovisionamiento de aplicaciones basado en atributos con filtros de ámbito](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters).

## <a name="next-steps"></a>Pasos siguientes
[Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](user-provisioning.md)

