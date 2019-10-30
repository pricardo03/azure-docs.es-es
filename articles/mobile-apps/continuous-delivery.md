---
title: Automatización de la implementación y el lanzamiento de las aplicaciones móviles con Visual Studio App Center y servicios de Azure
description: Conozca los servicios de App Center que ayudan a configurar la canalización de entrega continua de sus aplicaciones móviles.
author: elamalani
ms.assetid: 34a8a070-9b3c-4faf-8588-ccff02097224
ms.service: vs-appcenter
ms.topic: article
ms.date: 10/22/2019
ms.author: emalani
ms.openlocfilehash: 3b8305d02345abe98b674a8ed61626eaf6b5528d
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72795120"
---
# <a name="automate-the-deployment-and-release-of-your-mobile-applications-with-continuous-delivery-services"></a>Automatización de la implementación y el lanzamiento de las aplicaciones móviles con servicios de entrega continua

Los desarrolladores escriben código y lo insertan en el repositorio de código; sin embargo, las confirmaciones insertadas en el repositorio no siempre son coherentes. Cuando hay varios desarrolladores que trabajan en el mismo proyecto, surgen problemas con la integración y el equipo se encuentra en situaciones en las que las cosas no funcionan, los errores se acumulan y el desarrollo del proyecto se retrasa. Los desarrolladores tienen que esperar a que todo el código de software se haya compilado y probado para buscar errores. Esto hace que el proceso sea lento y menos iterativo.

Con **Entrega continua**, puede automatizar la implementación y el lanzamiento de las aplicaciones móviles, con independencia de si se van a distribuir a un grupo de evaluadores o los empleados de la empresa (para pruebas beta) o a la tienda de aplicaciones (para producción). Este servicio reduce el riesgo de las implementaciones, fomenta las iteraciones rápidas y le permite publicar nuevos cambios para los clientes de forma continua.

## <a name="distribute-application-binaries-to-beta-testers"></a>Distribución de los archivos binarios de la aplicación a evaluadores beta
La prueba beta de la aplicación móvil es uno de los pasos fundamentales durante el proceso de desarrollo de las aplicaciones. Ayuda a encontrar errores y problemas en la aplicación en una fase temprana y los comentarios mejoran la calidad de la aplicación al prepararla para su uso en producción.

Use los servicios siguientes para habilitar la canalización de entrega continua en las aplicaciones móviles.

### <a name="visual-studio-app-center"></a>Visual Studio App Center
[App Center Distribute](/appcenter/distribution/) es una herramienta para que los desarrolladores publiquen rápidamente compilaciones para los dispositivos de los usuarios finales. Su experiencia de portal de instalación completo convierte a Distribute no solo en una solución eficaz para la distribución de evaluadores de aplicaciones beta, sino también en una práctica alternativa para la distribución mediante las tiendas de aplicaciones públicas. Los desarrolladores pueden automatizar su flujo de trabajo de distribución aún más con App Center Build y publicar integraciones en tiendas de aplicaciones.

**Características principales**
- **Distribución de la aplicación a los evaluadores y usuarios de la versión beta** y garantía de que todos los evaluadores se encuentran en la versión más reciente de la aplicación.
- **Notificación a los evaluadores de nuevas versiones** sin que tengan que volver a pasar de nuevo por el flujo de descarga.
- **Administración de grupos de distribución** para distintas versiones de la aplicación.
- **Distribución a tiendas** 
    - [Apple](/appcenter/distribution/stores/apple)
    - [Google Play](/appcenter/distribution/stores/googleplay)
    - [Intune](/appcenter/distribution/stores/intune)
- **Compatibilidad con plataformas**: iOS, Android, macOS, tvOS, Xamarin, React Native, Unity y Cordova.
- Registro automático de dispositivos iOS en el perfil de aprovisionamiento.

**Referencias**
- [Registro en App Center](https://appcenter.ms/signup?utm_source=Mobile%20Development%20Docs&utm_medium=Azure&utm_campaign=New%20azure%20docs)
- [Introducción a App Center Distribute](/appcenter/build/)

### <a name="azure-pipelines"></a>Azure Pipelines

[Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/), un completo servicio de integración continua (CI) y entrega continua (CD) que funciona con el proveedor de Git de su elección y que se puede implementar en la mayoría de los principales servicios en la nube, incluidos los servicios de Azure. Puede empezar con el código de GitHub, GitHub Enterprise Server, GitLab, Bitbucket Cloud o Azure Repos. A continuación, puedes automatizar la compilación, las pruebas y la implementación del código en Microsoft Azure, Google Cloud Platform o Amazon Web Services.

**Características principales**
- Experiencia simplificada basada en tareas para configurar un servidor de CI para **aplicaciones móviles nativas (Android, iOS y Windows) y multiplataforma (Xamarin, Cordova y React Native)** .
- **Cualquier lenguaje, plataforma y nube**: compile, pruebe e implemente aplicaciones de Node.js, Python, Java, PHP, Ruby, Go, C/C++, C#, Android e iOS. Trabaje en paralelo en Linux, macOS y Windows. Implemente en proveedores en la nube, como Azure, AWS y GCP. Distribuya aplicaciones móviles a través de canales beta y tiendas de aplicaciones.
- **Compatibilidad nativa con contenedores**: cree contenedores con facilidad e insértelos en cualquier registro. Implemente contenedores en hosts independientes o en Kubernetes.
- **Flujos de trabajo y características avanzados**: encadenamiento sencillo de compilaciones y compilaciones en varias fases. Compatibilidad con YAML, integración de pruebas, validación de versiones, informes, etc.
- **Extensible**: use diversas tareas de compilación, prueba e implementación creadas por la comunidad (cientos de extensiones de Slack a SonarCloud). Incluso se pueden implementar soluciones de otros sistemas de CI, como Jenkins. Los webhooks y las API REST ayudan a la integración.
- **Compilaciones gratuitas hospedadas en la nube** para repositorios públicos y privados.
- **Admite la implementación en otros proveedores de nube**, como AWS, GCP, etc.

**Referencias**
- [Guía de introducción a Azure Pipelines](/azure/devops/pipelines/get-started/pipelines-get-started?view=azure-devops)
- [Introducción a Azure DevOps](https://app.vsaex.visualstudio.com/signup/) 
  
## <a name="distribute-your-application-directly-to-app-stores"></a>Distribución directa de la aplicación a las tiendas de aplicaciones
Una vez que la aplicación está lista para su uso en producción y quiere que se utilice públicamente, debe enviarse a las tiendas de aplicaciones donde los clientes puedan descargarla. Hay varias maneras de distribuir la aplicación directamente a las tiendas de aplicaciones. 

### <a name="visual-studio-app-center"></a>Visual Studio App Center
El servicio [App Center Distribute](/appcenter/distribution/stores/) le permite publicar las aplicaciones móviles directamente en las tiendas de aplicaciones. Una vez que la aplicación esté lista para que la descarguen los usuarios finales, puede publicar los archivos binarios de la aplicación directamente desde el portal de App Center.  

Puede distribuirla directamente a:
- [App Store de Apple](/appcenter/distribution/stores/apple)
- [Google Play Store](/appcenter/distribution/stores/googleplay)
- [Microsoft Intune](/appcenter/distribution/stores/intune)
    
### <a name="apple-app-store"></a>App Store de Apple
La aplicación App Store, de cuyo desarrollo y mantenimiento se encarga Apple, permite a los usuarios examinar y descargar aplicaciones desarrolladas para dispositivos iOS, MacOS, WatchOS y tvOS. Los desarrolladores deben enviar sus aplicaciones iOS a Apple Store para su uso público.

### <a name="google-play"></a>Google Play

Google Play es la tienda de aplicaciones oficial para el sistema operativo Android y permite a los usuarios examinar y descargar aplicaciones desarrolladas para dispositivos Android y publicadas a través de Google.

### <a name="intune"></a>Intune

[Microsoft Intune](/intune/app-management) es un servicio basado en la nube perteneciente al espacio de administración de movilidad empresarial (EMM) que ayuda a que los recursos seas productivos, al tiempo que mantiene protegidos los datos corporativos. Con Intune, puede: 
- Administrar los dispositivos móviles y los equipos que usan los recursos para acceder a los datos de la empresa
- Administrar las aplicaciones móviles que usan los recursos
- Proteger la información de la empresa mediante el control del modo en que los recursos acceden a ella y la comparten
- Garantizar que los dispositivos y las aplicaciones son compatibles con los requisitos de seguridad de la empresa
    
## <a name="deploy-updates-directly-to-users-devices"></a>Implementar actualizaciones directamente en los dispositivos de los usuarios.

### <a name="codepush"></a>CodePush
El servicio [CodePush](/appcenter/distribution/codepush/) de App Center permite a los desarrolladores de Apache Cordova y React Native implementar actualizaciones de las aplicaciones móviles directamente en los dispositivos de los usuarios. Funciona como repositorio central en el que los desarrolladores pueden publicar determinadas actualizaciones (por ejemplo, cambios en JS, HTML, CSS e imágenes). A continuación, las aplicaciones pueden consultar las actualizaciones desde el repositorio mediante los SDK de cliente proporcionados. De esta forma, dispone de un modelo de interacción más determinista y directo con los usuarios finales, al mismo tiempo que se solucionan errores o se agregan pequeñas características sin necesidad de volver a compilar un archivo binario o redistribuirlo a través de una tienda de aplicaciones pública.

**Características principales**
- Permite a los desarrolladores de Apache Cordova y React Native implementar actualizaciones de las aplicaciones móviles directamente en los dispositivos de los usuarios sin publicarlas en una tienda.
- Resulta útil para corregir errores o agregar o quitar pequeñas características que no requieren recompilar archivos y redistribuirlos a través de las tiendas respectivas.

**Referencias**
- [Registro en App Center](https://appcenter.ms/signup?utm_source=Mobile%20Development%20Docs&utm_medium=Azure&utm_campaign=New%20azure%20docs)
- [Introducción a Code Push de App Center](/appcenter/distribution/codepush/)
- [CLI de CodePush](/appcenter/distribution/codepush/cli)