---
title: Conectar datos de Firewall de Windows a Azure Sentinel, versión preliminar | Microsoft Docs
description: Aprenda a conectar datos de Firewall de Windows a Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 0e41f896-8521-49b8-a244-71c78d469bc3
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/17/2019
ms.author: rkarlin
ms.openlocfilehash: a863910ee338da5655e9f3b5610b0a8049b8b2a9
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2019
ms.locfileid: "67620772"
---
# <a name="connect-windows-firewall"></a>Conexión del firewall de Windows

> [!IMPORTANT]
> Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

El conector de Firewall de Windows le permite conectarse fácilmente a los registros de los Firewall de Windows, si están conectados al área de trabajo Azure Sentinel. Esta conexión le permite ver paneles, crear alertas personalizadas y mejorar la investigación, de modo que dispondrá de más información sobre la red de la organización y de mejores capacidades de seguridad. La solución recopila eventos de firewall de Windows de las máquinas de Windows en las que está instalado un agente de Log Analytics. 


> [!NOTE]
> Los datos se almacenarán en la ubicación geográfica del área de trabajo en la que se ejecute Azure Sentinel.

## <a name="enable-the-connector"></a>Habilitar el conector 

1. En el portal de Azure Sentinel, seleccione **Data connectors** (Conectores de datos) y, después, haga clic en el icono de **Firewall de Windows**. 
1.  Si las máquinas Windows están en Azure:
    1. Haga clic en **Install agent on Azure Windows virtual machine** (Instalar el agente en la máquina virtual Windows de Azure).
    1. En la lista **Máquinas virtuales**, seleccione la máquina Windows que quiera transmitir a Azure Sentinel. Asegúrese de que se trata de una máquina virtual Windows.
    1. En la ventana de la máquina virtual que se abre, haga clic en **Connect**(Conectar).  
    1. Haga clic en **Habilitar** en la ventana **Windows firewall connector** (Conector de Firewall de Windows). 

2. Si la máquina Windows no es una máquina virtual de Azure:
    1. Haga clic en **Install agent on non-Azure machines** (Instalar el agente en máquinas que no son de Azure).
    1. En la ventana **Direct agent** (Agente directo), seleccione **Download Windows agent (64 bit)** (Descargar agente de Windows de [64 bits]) o **Download Windows agent (32 bit)** (Descargar agente de Windows de [32 bits]).
    1. Instale el agente en su máquina Windows. Copie los valores de **Workspace ID** (Identificador de área de trabajo), **Primary key** (Clave principal) y **Secondary key** (Clave secundaria) y úselos cuando se le solicite durante la instalación.

4. Seleccione los tipos de datos que quiera transmitir.
5. Haga clic en **Install solution** (Instalar solución).
6. Para usar el esquema correspondiente en Log Analytics para encontrar Firewall de Windows, busque **SecurityEvent**.

## <a name="validate-connectivity"></a>Validar conectividad

Hasta que los registros empiecen a aparecer en Log Analytics, pueden transcurrir más de 20 minutos. 



## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar Firewall de Windows a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats.md).

