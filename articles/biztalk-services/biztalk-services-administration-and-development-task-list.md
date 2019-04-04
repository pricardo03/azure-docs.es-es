---
title: Lista de tareas de administración y desarrollo de BizTalk Services | Microsoft Docs
description: Ayuda de planeación y de trabajo para implementar Azure BizTalk Services.
services: biztalk-services
documentationcenter: ''
author: msftman
manager: erikre
editor: ''
ms.assetid: 0ab70b5b-1a88-4ba5-b329-ec51b785010e
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2016
ms.author: deonhe
ms.openlocfilehash: 6b8e0fea73dc24f7e680482be1f06fba6d702804
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/04/2019
ms.locfileid: "58916247"
---
# <a name="administration-and-development-task-list-in-biztalk-services"></a>Lista de tareas de administración y desarrollo de BizTalk Services

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]
> 
> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

## <a name="getting-started"></a>Introducción
Cuando se trabaja con Microsoft Azure BizTalk Services, hay varios componentes locales y basados en la nube que hay que tener en cuenta. Para empezar, piense en el siguiente flujo de proceso:  

| Paso | Quién es el responsable | Tarea | Vínculos relacionados |
| --- | --- | --- | --- |
| 1. |Administrador |Creación de la suscripción de Microsoft Azure con una cuenta de Microsoft o una cuenta de la organización |[Azure Portal](https://portal.azure.com) |
| 2. |Administrador |Cree o aprovisione un servicio de BizTalk. |[Crear un Servicio de BizTalk](/previous-versions/azure/reference/dn232347(v=azure.100)) |
| 3. |Administrador |Registrarse a usted o registrar la implementación de BizTalk Services de su empresa |[Registrar y actualizar una implementación del servicio de BizTalk en el Portal BizTalk Services](/previous-versions/azure/hh689837(v=azure.100)) |
| 4. |Administrador |Se aplica si la aplicación usa el servicio de adaptador de BizTalk para conectarse a un sistema de línea de negocio (LOB) local o usa un destino de tema o cola.  Cree el espacio de nombres de Azure Service Bus. Dé los valores de este espacio de nombres, del nombre del emisor de Service Bus y de la clave del emisor de Service Bus al desarrollador. |[Uso de Crear o modificar un Namespace de servicio de Bus de servicio](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md) y [valores obtener nombre de emisor y clave del emisor](biztalk-issuer-name-issuer-key.md) |
| 5. |Developer |Instale el SDK y cree el proyecto del Servicio de BizTalk en Visual Studio. |[Instalar SDK de Azure BizTalk Services](/previous-versions/azure/hh689760(v=azure.100)) y [Crear puntos de conexión de mensajería enriquecida en Azure](/previous-versions/azure/hh689766(v=azure.100)) |
| 6. |Developer |Implemente su proyecto del Servicio de BizTalk para su Servicio de BizTalk hospedado en Azure. |[Implementar y actualizar el proyecto de BizTalk Services](/previous-versions/azure/hh689881(v=azure.100)) |
| 7. |Administrador |Se aplica si está usando EDI.  Puede agregar partner y crear acuerdos en el portal de Microsoft Azure BizTalk Services. Al crear un contrato, puede agregar el puente o las transformaciones creadas por el desarrollador a la configuración del acuerdo. |[Configuración de EDI, AS2 y EDIFACT en el Portal de BizTalk Services](/previous-versions/azure/hh689853(v=azure.100)) |
| 8. |Administrador |Con [REST](/previous-versions/azure/reference/dn232347(v=azure.100)), supervise el estado de su instancia de BizTalk Services, incluidas las métricas de rendimiento. |[BizTalk Services: Pestañas panel, Monitor y escala](https://go.microsoft.com/fwlink/p/?LinkID=302281) |
| 9. |Administrador |Mediante el portal de Microsoft Azure BizTalk Services, administre los artefactos usados por BizTalk Services y realice un seguimiento de los mensajes a medida que los archivos puente los procesan. |[Uso del Portal de BizTalk Services](/previous-versions/azure/dn874043(v=azure.100)) |
| 10. |Administrador |Cree un plan de copia de seguridad para realizar una copia de seguridad del Servicio de BizTalk. |[Continuidad del negocio y recuperación ante desastres en BizTalk Services](/previous-versions/azure/dn509557(v=azure.100)) |

## <a name="next-steps"></a>Pasos siguientes
[Tutoriales y ejemplos](/previous-versions/azure/hh689895(v=azure.100))

[Creación del proyecto en Visual Studio](/previous-versions/azure/hh689811(v=azure.100))

[Instalar el SDK de Azure BizTalk Services](/previous-versions/azure/hh689760(v=azure.100))

## <a name="concepts"></a>Conceptos
[Creación del proyecto en Visual Studio](/previous-versions/azure/hh689811(v=azure.100))  
[EDI, AS2 y mensajería de EDIFACT (negocio a negocio)](/previous-versions/azure/hh689898(v=azure.100))  

## <a name="other-resources"></a>Otros recursos
[Agregar origen, destino y puente de extremos de mensajería](/previous-versions/azure/hh689877(v=azure.100))  
[Obtenga información y crear mapas de mensajes y transformaciones](/previous-versions/azure/hh689905(v=azure.100))  
[Uso del servicio de adaptador de BizTalk (BAS)](/previous-versions/azure/hh689889(v=azure.100))  
[Azure BizTalk Services](https://go.microsoft.com/fwlink/p/?LinkID=303664)

