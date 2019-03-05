---
title: ¿Qué es Azure App Configuration? | Microsoft Docs
description: Introducción al servicio Azure App Configuration.
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 02/24/2019
ms.author: yegu
ms.openlocfilehash: 11dd91039bb352e86800982d0a294f82622a56fe
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2019
ms.locfileid: "56885025"
---
# <a name="what-is-azure-app-configuration"></a>¿Qué es Azure App Configuration?

Azure App Configuration proporciona un servicio para administrar la configuración de la aplicación de forma centralizada. Los programas actuales, especialmente los que se ejecutan en una nube, tienen por lo general muchos componentes que están distribuidos por naturaleza. La propagación de valores de configuración entre estos componentes puede conducir a errores difíciles de solucionar durante la implementación de una aplicación. App Configuration le permite almacenar toda la configuración de la aplicación y proteger sus accesos en un solo lugar.

## <a name="why-use-app-configuration"></a>Razones para usar App Configuration

Las aplicaciones basadas en la nube a menudo se ejecutan en varias máquinas virtuales o contenedores de varias regiones y emplean varios servicios externos. Crear una aplicación distribuida de este tipo que sea sólida y escalable constituye un auténtico reto. Han surgido varias metodologías de programación para ayudar a los desarrolladores a abordar el aumento en la complejidad de la compilación de estas aplicaciones. Por ejemplo, la aplicación 12-factor detalla muchos patrones arquitectónicos bien comprobados y procedimientos recomendados para su uso con aplicaciones en la nube. Una de las recomendaciones principales de esta guía es separar la configuración del código. Esto significa que la configuración de una aplicación se debería mantener de forma externa a su archivo ejecutable y leerse desde su entorno de ejecución o desde un origen externo.

Aunque todas las aplicaciones pueden usarlo, los siguientes son buenos ejemplos de tipos de aplicación que deberían usar App Configuration:

* Microservicios basados en AKS, Service Fabric u otras aplicaciones en contenedores implementadas en una o varias ubicaciones geográficas.
* Aplicaciones sin servidor, incluida Azure Functions u otras aplicaciones de proceso sin estado controladas por eventos.
* Canalización de implementación continua.

App Configuration ofrece las siguientes ventajas:

* Un servicio totalmente administrado que puede configurar en cuestión de minutos.
* Representaciones y asignaciones de claves flexibles.
* Etiquetado con etiquetas.
* Reproducción de la configuración en un momento dado.
* Comparación de dos conjuntos de configuraciones en dimensiones definidas de forma personalizada.
* Seguridad mejorada mediante las identidades administradas de Azure.
* Cifrado de datos completo, en reposo o en tránsito.
* Integración nativa con plataformas conocidas.

## <a name="how-to-use-app-configuration"></a>Uso de App Configuration

La manera más fácil de agregar un almacén de App Configuration a la aplicación es mediante una biblioteca cliente que proporciona Microsoft. A continuación, se muestran los mejores métodos disponibles según el lenguaje y la plataforma de programación:

| Lenguaje y plataforma de programación | Conexión |
|---|---|
| **.NET Core** y **ASP.NET Core** | Proveedor de configuraciones de App Configuration para .NET Core. |
| **.NET** y **ASP.NET** | Generador de configuraciones de App Configuration para .NET. |
| **Java Spring** | Cliente de configuración de App Configuration para Spring Cloud. |
| Otros | API REST de App Configuration. |

## <a name="next-steps"></a>Pasos siguientes

* [Inicio rápido: Creación de una aplicación web de ASP.NET](quickstart-aspnet-core-app.md)  
