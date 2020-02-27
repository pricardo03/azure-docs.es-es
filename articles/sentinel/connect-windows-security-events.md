---
title: Conectar datos de eventos de seguridad de Windows a Azure Sentinel| Microsoft Docs
description: Aprenda a conectar datos de eventos de seguridad de Windows a Azure Sentinel.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: d51d2e09-a073-41c8-b396-91d60b057e6a
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: yelevin
ms.openlocfilehash: 70190eeb3adec239d1e52f51afcd173497d08e6a
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/25/2020
ms.locfileid: "77588015"
---
# <a name="connect-windows-security-events"></a>Conexión de eventos de seguridad de Windows 



Puede transmitir todos los eventos de seguridad de los servidores de Windows conectados al área de trabajo Azure Sentinel. Esta conexión le permite ver paneles, crear alertas personalizadas y mejorar la investigación, de modo que dispondrá de más información sobre la red de la organización y de mejores capacidades de seguridad.  Puede seleccionar qué eventos transmitir:

- **Todos los eventos**: todos los eventos de seguridad de Windows y AppLocker.
- **Común**: conjunto estándar de eventos con fines de auditoría. En este conjunto se proporciona una pista de auditoría de usuario completa. Por ejemplo, este conjunto contiene los eventos de inicio de sesión y cierre de sesión de usuario (identificador de evento 4634). Se incluyen acciones de auditoría como cambios en los grupos de seguridad, operaciones de Kerberos en los controladores de dominio de clave y otros eventos que recomiendan las organizaciones del sector.

Los eventos que tienen un volumen muy bajo se han incluido en el conjunto Común, ya que la motivación principal para elegirlo respecto de todos los eventos pasa por reducir el volumen, y no por filtrar eventos específicos.
- **Mínimo**: conjunto reducido de eventos que pueden señalar posibles amenazas. Si habilita esta opción, no podrá tener una pista de auditoría completa.  Este conjunto abarca solamente los eventos que podrían indicar una brecha correcta y eventos importantes que tienen un volumen muy bajo. Por ejemplo, este conjunto contiene un inicio de sesión de usuario correcto y uno erróneo (identificadores de evento 4624 y 4625), pero no contiene información del cierre de sesión, que es importante para la auditoría pero no lo es para la detección y tiene un volumen relativamente alto. La mayor parte del volumen de datos de este conjunto son los eventos de inicio de sesión y el evento de creación de proceso (identificador de evento 4688).
- **Ninguno**: no se transmite ningún evento de seguridad o de AppLocker.

> [!NOTE]
> 
> - Los datos se almacenarán en la ubicación geográfica del área de trabajo en la que Azure Sentinel se ejecute.
> - Si Azure Security Center y Azure Sentinel se ejecutan en la misma área de trabajo, el conector de eventos de seguridad solo se podrá conectar desde Azure Security Center o Azure Sentinel. Para administrar estos eventos desde Azure Sentinel, recomendamos que lo desconecte de Azure Security Center y lo conecte solo a Azure Sentinel.


En la lista siguiente se muestra un desglose completo de los identificadores de evento de seguridad y de AppLocker para cada conjunto:

| Capa de datos | Indicadores de eventos recopilados |
| --- | --- |
| mínimo | 1102,4624,4625,4657,4663,4688,4700,4702,4719,4720,4722,4723,4724,4727,4728,4732,4735,4737,4739,4740,4754,4755, |
| | 4756,4767,4799,4825,4946,4948,4956,5024,5033,8001,8002,8003,8004,8005,8006,8007,8222 |
| Comunes | 1,299,300,324,340,403,404,410,411,412,413,431,500,501,1100,1102,1107,1108,4608,4610,4611,4614,4622, |
| |  4624,4625,4634,4647,4648,4649,4657,4661,4662,4663,4665,4666,4667,4688,4670,4672,4673,4674,4675,4689,4697, |
| | 4700,4702,4704,4705,4716,4717,4718,4719,4720,4722,4723,4724,4725,4726,4727,4728,4729,4733,4732,4735,4737, |
| | 4738,4739,4740,4742,4744,4745,4746,4750,4751,4752,4754,4755,4756,4757,4760,4761,4762,4764,4767,4768,4771, |
| | 4774,4778,4779,4781,4793,4797,4798,4799,4800,4801,4802,4803,4825,4826,4870,4886,4887,4888,4893,4898,4902, |
| | 4904,4905,4907,4931,4932,4933,4946,4948,4956,4985,5024,5033,5059,5136,5137,5140,5145,5632,6144,6145,6272, |
| | 6273,6278,6416,6423,6424,8001,8002,8003,8004,8005,8006,8007,8222,26401,30004 |

## <a name="set-up-the-windows-security-events-connector"></a>Configurar el conector de eventos de seguridad de Windows

Para integrar totalmente los eventos de seguridad de Windows con Azure Sentinel:

1. En el portal de Azure Sentinel, seleccione **Data connectors** (Conectores de datos) y, después, haga clic en el icono de **eventos de seguridad**. 
1. Seleccione los tipos de datos que quiera transmitir.
1. Haga clic en **Update**(Actualizar).
6. Para usar el esquema correspondiente en Log Analytics para encontrar eventos de seguridad de Windows, busque **SecurityEvent**.

## <a name="validate-connectivity"></a>Validar conectividad

Hasta que los registros empiecen a aparecer en Log Analytics, pueden transcurrir unos 20 minutos. 



## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar eventos de seguridad de Windows a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).

