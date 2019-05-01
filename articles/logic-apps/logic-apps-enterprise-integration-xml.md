---
title: 'Mensajes XML para la integración empresarial B2B: Azure Logic Apps | Microsoft Docs'
description: Procesar, validar, transformar y enriquecer mensajes XML para soluciones B2B en Azure Logic Apps con Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: 47672dc4-1caa-44e5-b8cb-68ec3a76b7dc
ms.date: 02/27/2017
ms.openlocfilehash: a75ac9773072423c13eef85ecad29c632c13d024
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60996588"
---
# <a name="xml-messages-and-flat-files-in-azure-logic-apps-with-enterprise-integration-pack"></a>Mensajes XML y archivos planos en Azure Logic Apps con Enterprise Integration Pack

Con las aplicaciones lógicas, tiene la capacidad de procesar los mensajes XML que envía y recibe. Esta característica se incluye con Enterprise Integration Pack. Para los usuarios con experiencia en BizTalk Server, Enterprise Integration Pack ofrece funcionalidades similares para transformar y validar los mensajes, trabajar con archivos planos e incluso usar XPath para enriquecer o extraer propiedades específicas de un mensaje. 

Para aquellos usuarios que no conocen nada de este espacio, estas características expanden la forma de procesar los mensajes dentro del flujo de trabajo. Por ejemplo, si se encuentra en un escenario de negocio a negocio y trabaja con esquemas XML específicos, puede usar Enterprise Integration Pack para mejorar cómo su empresa procesa estos mensajes. 

Enterprise Integration Pack incluye: 

* [Validación XML](logic-apps-enterprise-integration-xml-validation.md "Información sobre la validación de mensajes XML"): Permite validar un mensaje XML entrante o saliente en un esquema concreto.
* [Transformación XML](../logic-apps/logic-apps-enterprise-integration-transform.md "Información sobre las asignaciones y transformaciones de mensajes XML"): Permite convertir o personalizar un mensaje XML basándose en sus requisitos o en los de un asociado.
* [Codificación y descodificación de archivos sin formato](logic-apps-enterprise-integration-flatfile.md "Información sobre la codificación y descodificación de archivos sin formato"): permiten codificar o descodificar un archivo sin formato. Por ejemplo, SAP acepta y envía archivos IDOC en formato de archivo plano. Muchas plataformas de integración crean mensajes XML, incluida Logic Apps. Por lo tanto, puede crear una aplicación lógica que use el codificador de archivo plano para "convertir" archivos XML en archivos planos. 
* [XPath](https://msdn.microsoft.com/library/mt643789.aspx): permite enriquecer un mensaje y extraer propiedades específicas de él. Las propiedades extraídas se pueden utilizar después para redirigir el mensaje a un destino o un punto de conexión intermediario.

## <a name="try-it-out"></a>Prueba
[Implementar una aplicación lógica totalmente operativa](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-veter-pipeline) (ejemplo de GitHub) mediante el uso de las características de XML en Azure Logic Apps.

## <a name="learn-more"></a>Más información
[Más información sobre Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md "Información sobre Enterprise Integration Pack")
