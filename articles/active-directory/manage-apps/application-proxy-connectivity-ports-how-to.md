---
title: Apertura de los puertos de firewall necesarios para una aplicación de proxy de aplicación | Microsoft Docs
description: Averigüe qué puertos se deben abrir para que el proxy de aplicación de Azure AD funcione correctamente
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: celested
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: efc8c0f0581878d41eb62cbfbd88aec730ecf789
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56175267"
---
# <a name="how-to-open-the-firewall-ports-required-for-an-application-proxy-application"></a>Apertura de los puertos de firewall necesarios para una aplicación de proxy de aplicación

Para ver una lista completa de los puertos necesarios y la función de cada uno, consulte la sección Requisitos previos de la [documentación del proxy de aplicación](application-proxy-add-on-premises-application.md). Tenga en cuenta que el proxy de aplicación solo usa puertos de salida.

También puede comprobar si tiene todos los puertos necesarios abiertos abriendo la [herramienta de prueba de puertos de conector](https://aadap-portcheck.connectorporttest.msappproxy.net/) desde la red local. Cuanto mayor sea el número de marcas de verificación verdes, mayor será la resistencia. 

## <a name="app-proxy-regions"></a>Regiones de proxy de aplicación

Se está trabajando en una forma de hacerle saber cuál de estas regiones debe estar verde para su caso. Por ahora, asegúrese de que todas ellas lo estén. También se necesita Centro de EE. UU., independientemente de en qué región se encuentre.

Para asegurarse de que la herramienta ofrezca los resultados correctos, debe:

-   Abrir la herramienta en un explorador desde el servidor donde esté instalado el conector.

-   Asegurarse de que los servidores proxy o firewalls aplicables a su conector también estén aplicados a esta página. Para hacerlo en Internet Explorer, vaya a **Configuración** -&gt; **Opciones de Internet** -&gt; **Conexiones** -&gt; **Configuración de LAN**. En esta página, verá el campo "Usar un servidor proxy para la LAN". Active esta casilla y escriba la dirección del proxy en el campo "Dirección".

## <a name="next-steps"></a>Pasos siguientes
[Descripción de los conectores del Proxy de aplicación de Azure AD](application-proxy-connectors.md)
