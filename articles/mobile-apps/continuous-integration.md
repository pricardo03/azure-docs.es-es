---
title: Automatización del ciclo de vida de las aplicaciones con Visual Studio App Center y los servicios de Azure
description: Conozca servicios como App Center, que ayuda a configurar la compilación y la integración continuas para sus aplicaciones móviles.
author: elamalani
ms.assetid: 34a8a070-9b3c-4faf-8588-ccff02097224
ms.service: vs-appcenter
ms.topic: article
ms.date: 10/22/2019
ms.author: emalani
ms.openlocfilehash: 07e8e0edc4638c6c0bf9acc205175aa731c0f8bc
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72795104"
---
# <a name="automate-the-lifecycle-of-your-apps-with-continuous-build-and-integration"></a>Automatización del ciclo de vida de las aplicaciones con la compilación y la integración continuas

Los desarrolladores escriben código y lo insertan en el repositorio de código; sin embargo, las confirmaciones insertadas en el repositorio no siempre son coherentes. Cuando hay varios desarrolladores que trabajan en el mismo proyecto, surgen problemas con la integración y el equipo se encuentra en situaciones en las que las cosas no funcionan, los errores se acumulan y el desarrollo del proyecto se retrasa. Los desarrolladores tienen que esperar a que todo el código de software se haya compilado y probado para buscar errores. Esto hace que el proceso sea lento y menos iterativo. 

Con la **compilación y la integración continuas**, los desarrolladores pueden simplificar la compilación y la prueba de su código no solo al confirman sus cambios en el repositorio de código fuente, sino también al poner las pruebas y comprobaciones en el entorno de compilación, de modo que siempre están ejecutando pruebas con el código. Todos los cambios realizados en el código fuente se compilan continuamente cada vez que se realiza una confirmación en el repositorio. Con cada inserción en el repositorio, el servidor de CI valida y ejecuta cualquier prueba que el desarrollador haya creado. Si no se superan las pruebas, el código se devuelve para seguir modificándolo. De esta forma, el desarrollador no tiene que interrumpir las compilaciones que se crean y no tiene que ejecutar todas las pruebas de forma local en su equipo, lo que a su vez aumenta su productividad. 

## <a name="key-benefits"></a>Ventajas principales
- Compilación, prueba e implementación **automatizadas** de las canalizaciones.
- **Detección de errores y solución de problemas** de forma temprana para garantizar velocidades de lanzamiento más rápidas.
- **Confirmación del código** con mayor frecuencia y compilación de aplicaciones más rápida.
- **Flexibilidad** para cambiar el código rápidamente sin ningún problema.
- **Tiempo de comercialización más rápido** que garantiza que solo el código de buena calidad recorre todo el camino.
- **Cambios pequeños en el código** que permiten integrar pequeños fragmentos de código al mismo tiempo.
- **Mayor transparencia y responsabilidad de los equipos**, lo que permite obtener **comentarios continuos** no solo de sus clientes, sino también del equipo.

Use los servicios siguientes para habilitar la canalización de integración continua en las aplicaciones móviles.

## <a name="visual-studio-app-center"></a>Visual Studio App Center
El servicio [App Center Build](/appcenter/build/) ayuda a compilar aplicaciones nativas y multiplataforma con las que trabaja el equipo, mediante una infraestructura de nube segura. Puede conectar fácilmente el repositorio de App Center y comenzar a compilar la aplicación en la nube en cada confirmación sin tener que preocuparse de configurar los servidores de compilación localmente, de configuraciones complicadas y de código que se compila en la máquina de un compañero pero no en la suya.

Gracias a la potencia agregada de los servicios de App Center, puede automatizar aún más el flujo de trabajo. Publique automáticamente compilaciones para evaluadores y tiendas de aplicaciones públicas con App Center Distribute, o bien ejecute pruebas automatizadas de IU en miles de configuraciones reales de sistemas operativos y dispositivos en la nube con App Center Test.

**Características principales**
- **Configure la integración continua** en cuestión de minutos y compile aplicaciones con mayor frecuencia y rapidez.
- Realice la integración con **GitHub, BitBucket, Azure DevOps y GitLab**.
- **Compilaciones rápidas y seguras** en máquinas administradas hospedadas en la nube.
- **Permita que se prueben sus compilaciones** y compruebe si la aplicación se compila en dispositivos iOS y Android del mundo real.
- **Compatibilidad nativa y multiplataforma**: iOS, Android, macOS, Windows, Xamarin y React Native.
- **Personalice las compilaciones** y agregue scripts posteriores a la clonación y previos y posteriores a la compilación.

**Referencias**
- [Registro en App Center](https://appcenter.ms/signup?utm_source=Mobile%20Development%20Docs&utm_medium=Azure&utm_campaign=New%20azure%20docs)
- [Introducción a App Center Build](/appcenter/build/)

## <a name="azure-pipelines"></a>Azure Pipelines
 [Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/), un servicio de Azure DevOps, es un completo servicio de integración continua (CI) y entrega continua (CD) que funciona con el proveedor de Git de su elección y que se puede implementar en la mayoría de los principales servicios en la nube, Azure incluido. Puede empezar con el código de GitHub, GitHub Enterprise Server, GitLab, Bitbucket Cloud o Azure Repos. A continuación, puedes automatizar la compilación, las pruebas y la implementación del código en Microsoft Azure, Google Cloud Platform o Amazon Web Services.

**Características principales**
- Experiencia simplificada basada en tareas para configurar un servidor de CI tanto para aplicaciones móviles nativas (Android, iOS y Windows) como multiplataforma (Xamarin, Cordova y Recta Native), además de tecnologías de servidor de Microsoft (Node.js, Java) y que no es de Microsoft.
- **Cualquier lenguaje, plataforma y nube**: compile, pruebe e implemente aplicaciones de Node.js, Python, Java, PHP, Ruby, Go, C/C++, C#, Android e iOS. Trabaje en paralelo en Linux, macOS y Windows. Implemente en proveedores en la nube, como Azure, AWS y GCP. Distribuya aplicaciones móviles a través de canales beta y tiendas de aplicaciones.
- **Compatibilidad nativa con contenedores**: cree contenedores con facilidad e insértelos en cualquier registro. Implemente contenedores en hosts independientes o en Kubernetes.
- **Flujos de trabajo avanzados**: encadenamiento sencillo de compilaciones y compilaciones en varias fases. Compatibilidad con YAML, integración de pruebas, validación de versiones, informes, etc.
- **Extensible**: use diversas tareas de compilación, prueba e implementación creadas por la comunidad (cientos de extensiones de Slack a SonarCloud). Incluso puede implementar desde otros sistemas de CI, como Jenkins, y usar webhooks y API REST para la integración.
- **Compilaciones gratuitas hospedadas en la nube** para repositorios públicos y privados.
- **Admite la implementación en otros proveedores de nube** como Amazon Web Services, Google Cloud Platform, etc.

**Referencias**
- [Guía de introducción a Azure Pipelines](/azure/devops/pipelines/get-started/pipelines-get-started?view=azure-devops)
- [Introducción a Azure DevOps](https://app.vsaex.visualstudio.com/signup/) 
- [Guías de inicio rápido](/azure/devops/pipelines/create-first-pipeline?view=azure-devops&tabs=tfs-2018-2)

Para elegir el servicio adecuado para las compilaciones de aplicaciones, siga el artículo en el que se comparan [App Center Build y Azure Pipelines](/appcenter/build/choose-between-services).
