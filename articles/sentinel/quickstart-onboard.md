---
title: Incorporar en versión preliminar de Azure Centinela | Microsoft Docs
description: Obtenga información sobre cómo recopilar datos en Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: d5750b3e-bfbd-4fa0-b888-ebfab7d9c9ae
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/12/2019
ms.author: rkarlin
ms.openlocfilehash: 30f6d08594bdf2e5d78bc8c7881a135d6e57f397
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57852236"
---
# <a name="on-board-azure-sentinel-preview"></a>Vista previa de centinela integrada de Azure

> [!IMPORTANT]
> Sentinel Azure está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

En este tutorial rápido, aprenderá cómo incorporar Azure Sentinel. 

Para incorporar Centinela de Azure, primero debe conectarse a los orígenes de datos. Sentinel Azure incluye una serie de conectores para las soluciones de Microsoft, disponibles fuera de la casilla y que proporcionaba además integración en tiempo real, incluidas las soluciones de protección contra amenazas de Microsoft, Microsoft 365 orígenes, incluidos Office 365, Azure AD, ATP de Azure, y Microsoft Cloud App Security y mucho más. Además, hay conectores integrados para el amplio ecosistema de seguridad para soluciones ajenas a Microsoft. También puede usar el formato de evento común, Syslog o API de REST para conectar los orígenes de datos con Azure Sentinel.  

Después de conectar los orígenes de datos, elija entre una galería de paneles creados por expertos que insights según los datos de la superficie. Estos paneles se pueden personalizar fácilmente a sus necesidades.


## <a name="global-prerequisites"></a>Requisitos previos globales

- Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

- Área de trabajo de análisis de registros. Obtenga información sobre cómo [crear un área de trabajo de Log Analytics](../log-analytics/log-analytics-quick-create-workspace.md)

- Permisos de colaborador en el inquilino para habilitar Azure Sentinel

- Global del inquilino o permisos de administrador de seguridad
 

## Habilitar Azure Centinela <a name="enable"></a>

1. Vaya al portal de Azure.
2. Asegúrese de que la suscripción en la que se crea el Centinela de Azure, está seleccionado. 
3. Búsqueda de Centinela de Azure. 
   ![search](./media/quickstart-onboard/search-product.png)

1. Haga clic en **+Agregar**.
1. Seleccione el área de trabajo que desea usar o crear uno nuevo. Puede ejecutar Azure Sentinel en más de un área de trabajo, pero los datos se aíslan en un área de trabajo.

   ![búsqueda](./media/quickstart-onboard/choose-workspace.png)

   >[!NOTE] 
   > - **Ubicación del área de trabajo** es importante entender que todos los datos que se transmite a Azure Sentinel se almacenan en la ubicación geográfica del área de trabajo seleccionado.  
   > - Áreas de trabajo predeterminadas creadas por Azure Security Center no aparecerá en la lista. no se puede instalar Azure Sentinel en ellos.
   > - Puede ejecutar Azure centinela en áreas de trabajo que se implementan en cualquiera de las siguientes regiones:  Sudeste de Australia, Canadá Central, India Central, este de Estados Unidos, EUAP de este de EE.UU. 2 (controladas), este de Japón, sudeste asiático, Reino Unido sur, Europa occidental, oeste de Estados Unidos 2.

6. Haga clic en **agregar Azure centinela**.
  

## <a name="connect-data-sources"></a>Conexión con orígenes de datos

Sentinel Azure crea la conexión a aplicaciones y servicios al conectar con el servicio y reenviar los eventos y registros para Azure Sentinel. Para equipos y máquinas virtuales, puede instalar al agente de Azure Sentinel que recopila los registros y las reenvía a Centinela de Azure. Para los Firewalls y servidores proxy, Sentinel Azure utiliza un servidor Linux Syslog. El agente está instalado en él y de que el agente recopila el registro de archivos y las reenvía a Centinela de Azure. 
 
1. Haga clic en **la recopilación de datos**.
2. Hay un icono para cada origen de datos que puede conectarse.<br>
Por ejemplo, haga clic en **Azure Active Directory**. Si se conecta a este origen de datos, transmitir todos los registros de Azure AD en Azure Sentinel. Puede seleccionar qué tipo de registros que desea para obtener - registros de inicio de sesión o los registros de auditoría. <br>
En la parte inferior, Azure Sentinel proporciona recomendaciones para los paneles, debe instalar para cada conector por lo que puede inmediatamente obtener interesante insights a través de los datos. <br> Siga las instrucciones de instalación o [consulte la Guía de conexión relevante](connect-data-sources.md) para obtener más información. Para obtener información acerca de los conectores de datos, vea [servicios de Microsoft Connect](connect-data-sources.md).

Después de los datos que están conectados los orígenes, los datos inicia en Azure Centinela de transmisión por secuencias y está listos para que pueda empezar a trabajar con. Puede ver los registros en el [paneles integrados](quickstart-get-visibility.md) y comience a crear consultas en Log Analytics para [investigar los datos](tutorial-investigate-cases.md).




## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido acerca de cómo conectarse a orígenes de datos a Azure Sentinel. Para obtener más información acerca de Centinela de Azure, consulte los artículos siguientes:
- Obtenga información sobre cómo [Obtenga visibilidad sobre sus datos y las posibles amenazas](quickstart-get-visibility.md).
- Introducción a [detección de amenazas con Azure Sentinel](tutorial-detect-threats.md).
- Datos procedentes de Stream [aparatos de formato de Error común](connect-common-event-format.md) en Centinela de Azure.
