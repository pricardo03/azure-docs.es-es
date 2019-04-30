---
title: Conecte los datos de Symantec ICDX a la versión preliminar de Azure Sentinel | Microsoft Docs
description: Obtenga información sobre cómo conectarse a datos Symantec ICDX Centinela de Azure.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: d068223f-395e-46d6-bb94-7ca1afd3503c
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: 1a6843fb1668307aa442011233999c648901d404
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60714555"
---
# <a name="connect-your-symantec-icdx-appliance"></a>Conecte el dispositivo de Symantec ICDX 

> [!IMPORTANT]
> Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Symantec ICDX conector le permite conectar fácilmente todos los registros de solución de seguridad de Symantec con su Centinela de Azure, para ver los paneles, crear alertas personalizadas y mejorará la investigación. Esto le ofrece más información sobre la red de su organización y mejora las capacidades de seguridad. Integración entre Symantec ICDX y Azure Sentinel hace uso de API de REST.


> [!NOTE]
> Datos se almacenarán en la ubicación geográfica del área de trabajo en el que se ejecuta Azure Sentinel.

## <a name="configure-and-connect-symantec-icdx"></a>Configurar y conectar Symantec ICDX 

Symantec ICDX puede integrar y exportar registros directamente a Azure Sentinel.

1. Abra la consola de administración ICDX.
2. En el menú de navegación izquierdo, seleccione **configuración** y, a continuación, el **reenviadores** ficha.
3. En la fila de Microsoft Azure Log Analytics, haga clic en **más**, seguido de **editar**. 
4. En el **reenviador de Log Analytics de Microsoft Azure** ventana, configure lo siguiente:
    - Deje el nombre de registro personalizado como el valor predeterminado, SymantecICDX.
    - Copie el identificador de área de trabajo y péguelo en el **identificador de cliente** campo. Copia el **clave principal** y péguelo en el campo de clave compartida. Puede copiar estos valores desde el portal de Azure Sentinel seleccionando **conectores de datos** y, a continuación, **Symantec ICDX**.
6. Para usar el esquema correspondiente en Log Analytics para los eventos de Symantec ICDX, busque **SymantecICDX_CL**.


## <a name="validate-connectivity"></a>Validar conectividad

Puede tardar más de 20 minutos hasta que los registros se empiecen a aparecer en Log Analytics. 



## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido cómo conectar Symantec ICDX a Centinela de Azure. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Obtenga información sobre cómo [Obtenga visibilidad sobre sus datos y las posibles amenazas](quickstart-get-visibility.md).
- Introducción a [detección de amenazas con Azure Sentinel](tutorial-detect-threats.md).

