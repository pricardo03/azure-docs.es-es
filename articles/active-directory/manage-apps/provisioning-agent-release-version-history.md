---
title: 'Agente de aprovisionamiento de Azure AD Connect: Historial de lanzamiento de versiones | Microsoft Docs'
description: En este artículo se enumeran todas las versiones del agente de aprovisionamiento de Azure AD Connect y se describen las nuevas características y los problemas corregidos.
services: active-directory
documentationcenter: ''
author: cmmdesai
manager: daveba
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/05/2019
ms.subservice: app-mgmt
ms.author: chmutali
ms.collection: M365-identity-device-management
ms.openlocfilehash: de6ef5dedb0779536cfbc1e6a3440e748ff78524
ms.sourcegitcommit: 65131f6188a02efe1704d92f0fd473b21c760d08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70861991"
---
# <a name="azure-ad-connect-provisioning-agent-version-release-history"></a>Agente de aprovisionamiento de Azure AD Connect: Historial de lanzamiento de versiones
En este artículo se enumeran las versiones y características del agente de aprovisionamiento de Azure AD Connect que se han publicado. El equipo de Azure AD actualiza periódicamente el agente de aprovisionamiento con nuevas características y funciones. Los agentes de aprovisionamiento se actualizan de forma automática cuando se publica una nueva versión. 

Se recomienda habilitar la actualización automática de los agentes para estar seguro de que cuenta con las características y correcciones de errores más recientes. Microsoft proporciona soporte técnico directo para la versión más reciente del agente y una versión anterior.

## <a name="11670"></a>1.1.67.0

### <a name="release-status"></a>Estado de la versión

9 de septiembre de 2019: publicada para actualización automática

### <a name="new-features-and-improvements"></a>Nuevas características y mejoras

* Capacidad de configurar el seguimiento y el registro adicionales para depurar problemas del agente de aprovisionamiento
* Capacidad de capturar solo los atributos de AD configurados en la asignación para mejorar el rendimiento de la sincronización

### <a name="fixed-issues"></a>Problemas corregidos

* Se corrigió un error en el que el agente dejaba de responder si había problemas de conexión con AD.
* Se corrigió un error que causaba problemas cuando se leían datos binarios de Active Directory.
* Se corrigió un error por el que el agente no podía renovar la confianza con el servicio de identidad híbrida en la nube.

## <a name="11300"></a>1.1.30.0

### <a name="release-status"></a>Estado de la versión

23 de enero de 2019: publicado para descarga.

### <a name="new-features-and-improvements"></a>Nuevas características y mejoras

* Se ha renovado la arquitectura del conector y el agente de aprovisionamiento para mejorar el rendimiento, la estabilidad y la confiabilidad. 
* Se ha simplificado la configuración del agente de aprovisionamiento simplificado mediante el Asistente para la instalación basada en la interfaz de usuario 
* Se ha agregado la compatibilidad para las actualizaciones automáticas del agente.

