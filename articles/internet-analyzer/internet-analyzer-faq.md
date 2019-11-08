---
title: Preguntas frecuentes sobre Internet Analyzer | Microsoft Docs
description: Las preguntas más frecuentes sobre Azure Internet Analyzer.
services: internet-analyzer
author: diego-perez-botero
ms.service: internet-analyzer
ms.topic: guide
ms.date: 10/16/2019
ms.author: mebeatty
ms.openlocfilehash: 61cb6003549530f381b9cbbed74c1cb62c91431c
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510305"
---
# <a name="azure-internet-analyzer-faq-preview"></a>Preguntas frecuentes sobre Azure Internet Analyzer (versión preliminar)

Estas son las preguntas más frecuentes sobre Azure Internet Analyzer: si tiene alguna otra pregunta, vaya al [foro de comentarios](https://aka.ms/internetAnalyzerFeedbackForum) y publique su pregunta. Si una pregunta es frecuente, se agrega a este artículo para que se pueda encontrar de forma rápida y sencilla.

## <a name="how-do-i-participate-in-the-preview"></a>¿Cómo participo en la versión preliminar?

La versión preliminar está disponible solo para algunos clientes. Si está interesado en participar en la versión preliminar, siga los pasos que se describen a continuación:

1. Inicie sesión en el [Azure Portal](https://ms.portal.azure.com).
2. Vaya a la página de **Suscripciones**.
3. Haga clic en la suscripción de Azure con la que planea usar Internet Analyzer.
4. Vaya a la configuración de **Proveedores de recursos** en la suscripción.
5. Busque **Microsoft.Network** y haga clic en el botón **Registrar** (o en el botón **Volver a registrar**).
![solicitud de acceso](./media/ia-faq/request-preview-access.png)

6. [Solicite la aprobación](https://aka.ms/internetAnalyzerContact) al proporcionarnos su dirección de correo electrónico y el identificador de suscripción de Azure que usó para solicitar acceso.
7. Una vez aprobada la solicitud, recibirá una confirmación por correo electrónico y podrá crear, actualizar o modificar los recursos de Internet Analyzer desde la suscripción de Azure recién permitida.

## <a name="do-i-need-to-embed-the-client-to-run-a-test"></a>¿Es necesario instalar el cliente para ejecutar una prueba?

Sí, el cliente de Internet Analyzer debe estar instalado en su aplicación para recopilar métricas específicas de los usuarios. [Vea cómo instalar el cliente.](internet-analyzer-embed-client.md) 

## <a name="do-i-get-billed-for-participating-in-the-preview"></a>¿Me cobrarán por participar en la versión preliminar?
No, el uso de la versión preliminar de Azure Internet Analyzer es gratuito. Durante la versión preliminar no hay ningún contrato de nivel de servicio.

## <a name="what-scenarios-is-internet-analyzer-designed-to-address"></a>¿Para qué escenarios está diseñado Internet Analyzer?

Internet Analyzer está diseñado para proporcionar información sobre el rendimiento de la red en función de la población de usuarios. Para ayudar a tomar las mejores decisiones de rendimiento para los usuarios, Internet Analyzer compara el rendimiento de dos puntos de conexión de Internet con su propia población de usuarios. Aunque Internet Analyzer puede responder a una multitud de preguntas, algunas de las más comunes son:

* ¿Cuál es el impacto en el rendimiento de la migración a la nube? 
    * *Prueba sugerida: Personalizado (la infraestructura local actual) frente a Azure (cualquier punto de conexión preconfigurado)*
* ¿Cuál es el valor de poner mis datos en el perímetro en vez de en un centro de datos? 
    *  *Prueba sugerida: Azure frente a Azure Front Door, Azure frente a Azure CDN de Microsoft*
* ¿Cuál es la ventaja de rendimiento de Azure Front Door?
    *  *Prueba sugerida: Personalizado/Azure/CDN frente a Azure Front Door*
* ¿Cuál es la ventaja de rendimiento de Azure CDN de Microsoft? 
    *  *Prueba sugerida: Personalizado/Azure/AFD frente a Azure CDN de Microsoft*
* ¿Cómo funciona Azure CDN de Microsoft? 
    *  *Prueba sugerida: Personalizado (otro punto de conexión de CDN) frente a Azure CDN de Microsoft*
* ¿Cuál es la mejor nube para su población de usuarios finales en cada región? 
    *  *Prueba sugerida: Personalizado (otro servicio en la nube) frente a Azure (cualquier punto de conexión preconfigurado)*

## <a name="which-tests-can-i-run-in-preview"></a>¿Qué pruebas se pueden ejecutar en la versión preliminar?

Cada prueba que cree en Internet Analyzer consta de dos puntos de conexión: el punto de conexión A y el punto de conexión B. Cualquiera de las siguientes combinaciones se puede ejecutar como prueba:  
* Dos puntos de conexión preconfigurados,
* Un punto de conexión personalizado y uno preconfigurado, o bien
* Dos [puntos de conexión personalizados](internet-analyzer-custom-endpoint.md) (un punto de conexión personalizado debe residir en Azure).

Los siguientes puntos de conexión preconfigurados están disponibles durante la versión preliminar:
* **Regiones de Azure**
    * Sur de Brasil
    * India Central
    * Centro de EE. UU.
    * Asia oriental
    * East US
    * Oeste de Japón
    * Europa del Norte
    * Norte de Sudáfrica
    * Sudeste asiático
    * Norte de Emiratos Árabes Unidos
    * Oeste de Reino Unido  
    * Europa occidental
    * Oeste de EE. UU.
    * Oeste de EE. UU. 2
* **Varias combinaciones de regiones de Azure**
    * Este de EE. UU., Sur de Brasil
    * Este de EE. UU., Asia Oriental
    * Oeste de Europa, Sur de Brasil
    * Oeste de Europa, Sudeste Asiático
    * Oeste de Europa, Norte de Emiratos Árabes Unidos
    * Oeste de EE. UU., Este de EE. UU.
    * Oeste de EE. UU., Oeste de Europa
    * Oeste de EE. UU., Norte de Emiratos Árabes Unidos
    * Oeste de Europa, Norte de Emiratos Árabes Unidos, Sudeste Asiático
    * Oeste de EE. UU., Oeste de Europa, Asia Oriental
    * Oeste de EE. UU., Norte de Europa, Sudeste Asiático, Norte de Emiratos Árabes Unidos, Norte de Sudáfrica 
* **Azure + Azure Front Door**: implementado en una o varias de las combinaciones de regiones de Azure mencionadas anteriormente
* **Azure + Azure CDN de Microsoft**: implementado en una o varias de las combinaciones de regiones de Azure mencionadas anteriormente
* **Azure + Azure Traffic Manager**: implementado en cualquier combinación de varias de las regiones de Azure mencionadas anteriormente

## <a name="how-is-internet-analyzer-different-from-other-monitoring-services-provided-by-azure"></a>¿En qué se diferencia Internet Analyzer de otros servicios de supervisión proporcionados por Azure?

Internet Analyzer le ayuda a comprender el rendimiento de los usuarios finales y a tomar decisiones para mejorar su rendimiento. Aunque otras herramientas de supervisión de Azure proporcionan información detallada sobre los servicios de Azure, Internet Analyzer se centra en medir el rendimiento de Internet de un extremo a otro para los usuarios.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, consulte la [Información general de Internet Analyzer](internet-analyzer-overview.md).
