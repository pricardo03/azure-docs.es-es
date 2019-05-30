---
title: Conecte los datos de Symantec ICDX a la versión preliminar de Azure Sentinel | Microsoft Docs
description: Obtenga información sobre cómo conectarse a datos Symantec ICDX Centinela de Azure.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: d068223f-395e-46d6-bb94-7ca1afd3503c
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2019
ms.author: rkarlin
ms.openlocfilehash: 3b21371d6321b208b19ca8b2524308736c3ceca9
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66244338"
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

1. Abra la consola de administración ICDX para agregar reenviadores de Microsoft Azure Sentinel (Log Analytics).
2. En la barra de navegación ICDx, haga clic en **configuración**. 
3. En la parte superior de la **configuración** pantalla, haga clic en **reenviadores**.
4. En **reenviadores**, al lado de Microsoft Azure Sentinel (Log Analytics), haga clic en **agregar**. 
4. En el **Microsoft Azure Sentinel (Log Analytics)** ventana, haga clic en **mostrar avanzadas**. 
5. En la parte superior de la forma expandida en la ventana de Microsoft Azure Sentinel (Log Analytics), haga lo siguiente:
    -   **Nombre**: Escriba un nombre para el reenviador con no más de 30 caracteres. Elija un nombre único y descriptivo. Este nombre aparece en la lista de reenviadores de los **configuración** pantalla y en los paneles en el **panel** pantalla. Por ejemplo: Microsoft Azure Log Analytics East. Este campo es obligatorio.
    -   **Descripción**: Escriba una descripción para el reenviador. Esta descripción aparecerá también en la lista de reenviadores en el **configuración** pantalla. Se incluyen detalles como el tipo de evento que se reenvían y el grupo que necesita para inspeccionar los datos.
    -   **Tipo de inicio**: Seleccione el método de inicio de la configuración de reenviador. Las opciones son manual y automática.<br>El valor predeterminado es automático. 
6. En **eventos**, realice lo siguiente: 
    - **Origen**: Seleccione uno o varios archivos desde el que se va a reenviar eventos. Puede seleccionar archivos de selector activo (incluido el archivo común), huérfanos archivos recopilador (es decir, los archivos de los selectores que ha eliminado), archivos de receptor ICDx o el archivo del sistema. <br>El valor predeterminado es archivo comunes.
      > [!NOTE]
      > Los archivos de receptor de ICDx se muestran por separado, por su nombre. 
 
    - **Filtro**: Agregue un filtro que especifica el subconjunto de eventos que reenviar. Realice una de las operaciones siguientes:
        - Para seleccionar una condición de filtro, haga clic en un tipo, atributo, operador y valor. 
        - En el campo de filtro, revise la condición de filtro. Puede editar directamente en el campo o eliminarla según sea necesario.
        - Haga clic en y u o agregar a la condición de filtro.
        - También puede hacer clic en las consultas guardadas para aplicar una consulta guardada.
    - **Incluye atributos**: Escriba la lista delimitada por comas de atributos que se va a incluir en los datos reenviados. Los atributos incluidos tienen prioridad sobre los atributos excluidos.
    - **Atributos excluidos**: Escriba la lista delimitada por comas de los atributos que se excluirán de los datos reenviados.
    - **Tamaño del lote**: Seleccione el número de eventos que deben enviarse por lote. Las opciones son 10, 50, 100, 500 y 1000.<br>El valor predeterminado es 100. 
    - **Límite de frecuencia**: Seleccione la velocidad a la que se reenvían eventos, expresado como eventos por segundo. Las opciones son ilimitados, 500, 1000, 10000, 5000. <br> El valor predeterminado es 5000. 
7. En **Azure destino**, realice lo siguiente: 
    - **Id. de área de trabajo**: Pegue el identificador del área de trabajo a continuación. Este campo es obligatorio.
    - **Clave principal**: Pegue la clave principal de los siguientes. Este campo es obligatorio.
    - **Nombre de registro personalizado**: Escriba el nombre de registro personalizado en el área de trabajo de Log Analytics portal de Microsoft Azure a la que se van a reenviar eventos. El valor predeterminado es SymantecICDx. Este campo es obligatorio.
8. Haga clic en *guardar* para finalizar la configuración de reenviador. 
9. Para iniciar el reenviador, en **opciones**, haga clic en **más** y, a continuación, **iniciar**.
10. Para usar el esquema correspondiente en Log Analytics para los eventos de Symantec ICDX, busque **SymantecICDX_CL**.


## <a name="validate-connectivity"></a>Validar conectividad

Puede tardar más de 20 minutos hasta que los registros se empiecen a aparecer en Log Analytics. 



## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido cómo conectar Symantec ICDX a Centinela de Azure. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Obtenga información sobre cómo [Obtenga visibilidad sobre sus datos y las posibles amenazas](quickstart-get-visibility.md).
- Introducción a [detección de amenazas con Azure Sentinel](tutorial-detect-threats.md).

