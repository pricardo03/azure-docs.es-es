---
title: Conecte los datos DNS en la versión preliminar de Azure Sentinel | Microsoft Docs
description: Aprenda a conectar datos DNS en Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 77af84f9-47bc-418e-8ce2-4414d7b58c0c
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: e34db2bdc78eb846cf4885b1ef083fd3b21e21b5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60445573"
---
# <a name="connect-your-domain-name-server"></a>Conectar el servidor de nombres de dominio

> [!IMPORTANT]
> Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Puede conectar cualquier servidor de nombres de dominio (DNS) que se ejecuta en Windows Azure Sentinel. Esto se hace mediante la instalación de un agente en el equipo DNS. Los registros de uso de DNS, puede obtener información relacionada con las operaciones en la infraestructura DNS de su organización, el rendimiento y seguridad al recopilar, analizar, y correlacionar analíticos y los registros de auditoría y otros datos relacionados de los servidores DNS.

Cuando se habilita la conexión de registro DNS, puede:
- Identificar a los clientes que intentan resolver nombres de dominio malintencionados
- Identificar los registros de recursos obsoletos
- Identificar los nombres de dominio consultados con más frecuencia y los clientes DNS participativos
- Ver la carga de solicitudes en los servidores DNS
- Ver errores de registro DNS dinámicos

## <a name="how-it-works"></a>Cómo funciona

Conexión de DNS se consigue instalando un agente en el equipo DNS. El agente extrae eventos de DNS y los pasa a Log Analytics.

## <a name="connect-your-dns-appliance"></a>Conecte el dispositivo de DNS

1. En el portal de Azure Sentinel, seleccione **conectores de datos** y elija el **DNS** icono.
1. Si las máquinas DNS se encuentran en Azure:
    1. Haga clic en **descargar e instalar el agente para las máquinas virtuales de Windows**.
    1. En el **máquinas virtuales** lista, seleccione la máquina DNS que desea transmitir a Azure Sentinel. Asegúrese de que se trata de una máquina virtual de Windows.
    1. En la ventana que se abre para esa máquina virtual, haga clic en **Connect**.  
    1. Haga clic en **habilitar** en el **conector DNS** ventana. 

2. Si la máquina DNS no es una máquina virtual de Azure:
    1. Haga clic en **descargar e instalar el agente para las máquinas de Windows que no son de Azure**.
    1. En el **agente directo** ventana, seleccione **agente Windows de descargar (64 bits)** o **agente Windows de descargar (32 bits)**.
    1. Instale al agente en el equipo DNS. Copia el **Id. de área de trabajo**, **clave principal**, y **clave secundaria** y usarlas cuando se le solicite durante la instalación.

3. Para usar el esquema correspondiente en Log Analytics para los registros DNS, busque **DnsEvents**.

## <a name="validate"></a>Validación 

En Log Analytics, busque el esquema **DnsEvents** y asegúrese de que hay eventos.

## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido cómo conectar dispositivos locales DNS con Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Obtenga información sobre cómo [Obtenga visibilidad sobre sus datos y las posibles amenazas](quickstart-get-visibility.md).
- Introducción a [detección de amenazas con Azure Sentinel](tutorial-detect-threats.md).
