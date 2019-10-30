---
title: Creación del back-end de una aplicación móvil sin servidor con Azure Functions y otros servicios
description: Obtenga información sobre Compute Services para crear el back-end de una aplicación móvil sin servidor sólida.
author: elamalani
manager: elamalani
ms.service: vs-appcenter
ms.assetid: 444f0959-aa7f-472c-a6c7-9eecea3a34b9
ms.topic: conceptual
ms.date: 10/22/2019
ms.author: emalani
ms.openlocfilehash: b8378be675b86ada10b8442c40a54b635ee115a9
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72795200"
---
# <a name="build-mobile-backend-components-with-compute-services"></a>Cree componentes de back-end móvil con Compute Services
Cada aplicación móvil necesita un back-end responsable del almacenamiento de datos, la lógica de negocios y la seguridad. La administración de la infraestructura de hospedaje y ejecución de código de back-end requiere el tamaño, el aprovisionamiento y el escalado de un grupo de servidores, la administración de actualizaciones del sistema operativo y el hardware implicado, la aplicación de revisiones de seguridad y la supervisión de todos estos componentes de la infraestructura en cuanto a rendimiento. disponibilidad y tolerancia a errores. Es aquí cuando la arquitectura sin servidor resulta útil, ya que los desarrolladores no tienen servidores que administrar, ningún sistema operativo ni actualizaciones de software o hardware relacionadas que administrar. Ahorra mucho tiempo y costos en desarrolladores, lo que significa un tiempo de comercialización más rápido y una energía concentrada en la compilación de aplicaciones.

## <a name="benefits-of-compute"></a>Ventajas de Compute
- Abstracción de servidores: no hay que preocuparse por el hospedaje, la aplicación de revisiones ni la seguridad, lo que permite a los desarrolladores centrarse simplemente en el código.
- El escalado instantáneo y eficaz garantiza que los recursos se aprovisionan automáticamente o a petición a cualquier escala que se necesite.
- Alta disponibilidad y tolerancia a errores.
- La microfacturación garantiza que solo se le cobrará cuando el código se ejecute realmente.
- Escriba el código que se ejecuta en la nube en el idioma de su elección.

Use los servicios siguientes para habilitar las funcionalidades de proceso sin servidor en sus aplicaciones móviles.

## <a name="azure-functions"></a>Azure Functions
[Azure Functions](https://azure.microsoft.com/services/functions/) es una experiencia de proceso controlado por eventos que permite ejecutar código, escrito en el lenguaje de programación que elija, sin necesidad de preocuparse por servidores. Los desarrolladores no tienen que administrar la aplicación o la infraestructura para ejecutarla. Las funciones se escalan a petición y se paga solo por el tiempo en que se ejecuta el código. Azure Functions es una excelente manera de implementar una API para una aplicación móvil porque es muy fácil de implementar y mantener, y es accesible a través de HTTP.

**Principales características**
- **Orientado a eventos y escalable** donde los desarrolladores pueden usar **desencadenadores y enlaces** para definir cuándo se invoca una función y a qué datos se conecta.
- **Traiga sus propias dependencias** : Funciones de Azure admite NuGet y NPM, para que pueda usar sus bibliotecas favoritas.
- La **seguridad integrada** le permite proteger funciones desencadenadas por HTTP con proveedores de OAuth tales como Azure Active Directory, Facebook, Google, Twitter y cuenta Microsoft.
- **Integración simplificada** con distintas ofertas de [Servicios de Azure](/azure/azure-functions/functions-overview#integrations) y de software como servicio (SaaS).
- El **desarrollo flexible** le permite programar las funciones directamente en el portal o configurar la integración continua e implementar el código mediante GitHub, Azure DevOps Services y otras herramientas de desarrollo compatibles.
- El runtime de Functions es de **código abierto** y está disponible en [GitHub](https://github.com/azure/azure-webjobs-sdk-script).
-  **Experiencia de desarrollo mejorada** donde los desarrolladores pueden programar, probar y depurar localmente con su editor preferido o una interfaz web fácil de usar con supervisión con herramientas integradas y funcionalidad integrada de DevOps.
- **Diversos lenguajes de programación y opciones de hospedaje**: desarrolle con C#, Node.js, Java, Javascript o Python.
- **Modelo de precios de pago por uso** : pague solo el tiempo que haya empleado ejecutando el código.

**Referencias**
- [Azure Portal](https://portal.azure.com)
- [Documentación](/azure/azure-functions/)
- [Guía para desarrolladores de Azure Functions](/azure/azure-functions/functions-reference)
- [Guías de inicio rápido](/azure/azure-functions/functions-create-first-function-vs-code)
- [Muestras](/samples/browse/?products=azure-functions&languages=csharp)

## <a name="app-service"></a>App Service
[Azure App Service](https://azure.microsoft.com/services/app-service/) le permite compilar y hospedar aplicaciones web e interfaces API RESTful en el lenguaje de programación que prefiera sin tener que administrar la infraestructura. Ofrece escalado automático y alta disponibilidad, es compatible con Windows y Linux y permite implementaciones automatizadas desde GitHub, Azure DevOps o cualquier repositorio Git.

**Principales características**
- Compatibilidad de **varios lenguajes y marcos** con ASP.NET, ASP.NET Core, Java, Ruby, Node.js, PHP o Python. También puede ejecutar PowerShell y otros scripts o ejecutables como servicios en segundo plano.
- **Optimización con DevOps**: configure la integración y la implementación continuas con Azure DevOps, GitHub, BitBucket, Docker Hub o Azure Container Registry. Administre las aplicaciones de App Service mediante Azure PowerShell o la interfaz de la línea de comandos (CLI) multiplataforma.
- **Escala global con alta disponibilidad** para escalar vertical u horizontalmente de forma manual o automática.
- **Conexiones a plataformas SaaS y a datos locales** para elegir entre más de 50 conectores para sistemas empresariales (como SAP), servicios SaaS (como Salesforce) y servicios de Internet (como Facebook). Acceda a los datos locales mediante Conexiones híbridas e instancias de Azure Virtual Network.
- **Seguridad y cumplimiento**: Azure App Service cumple con ISO, SOC y PCI. Autentique a los usuarios con Azure Active Directory o con inicio de sesión social (Google, Facebook, Twitter y Microsoft). Cree restricciones de direcciones IP y administre las identidades de servicio.
- **Plantillas de aplicación**: para elegir entre una amplia lista de plantillas de aplicación en Azure Marketplace, como WordPress, Joomla y Drupal.
- **Integración con Visual Studio** con herramientas dedicadas en Visual Studio que permiten optimizar las tareas de creación, implementación y depuración.

**Referencias**
- [Azure Portal](https://portal.azure.com/)
- [Documentación](/azure/app-service/)
- [Guías de inicio rápido](/azure/app-service/app-service-web-get-started-dotnet)
- [Muestras](/azure/app-service/samples-cli)
- [Tutoriales](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)

## <a name="azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS)
[Azure Kubernetes Service (AKS)](https://azure.microsoft.com/services/kubernetes-service/) permite administrar el entorno hospedado de Kubernetes, lo que hace que sea fácil y rápido implementar y administrar aplicaciones en contenedores sin necesidad de tener conocimientos de orquestación de contenedores. También permite eliminar la carga de las operaciones en curso y las de mantenimiento mediante el aprovisionamiento, actualización y escalado de los recursos a petición, sin tener que desconectar las aplicaciones. 

**Principales características**
- **Migre fácilmente las aplicaciones existentes** a Containers y ejecútelas dentro de AKS.
- **Simplifique la implementación y administración** de aplicaciones basadas en microservicios.
- **Proteja DevOps para AKS** para lograr un equilibrio entre velocidad y seguridad y ofrecer código más rápido a escala.
- **Escale fácilmente mediante AKS y ACI** para aprovisionar pods dentro de ACI que se inician en segundos.
- **Implementación y administración de dispositivos de IoT** a petición.
- **Entrenamiento de modelos de Machine Learning** con el uso de herramientas tales como TensorFlow y KubeFlow.

**Referencias**
- [Azure Portal](https://portal.azure.com/)
- [Documentación](/azure/aks/)
- [Guías de inicio rápido](/azure/aks/kubernetes-walkthrough-portal)
- [Tutoriales](/azure/aks/tutorial-kubernetes-prepare-app)

## <a name="azure-container-instances"></a>Azure Container Instances
[Azure Container Instances (ACI)](https://azure.microsoft.com/services/container-instances/) es una excelente solución para cualquier escenario que pueda funcionar en contenedores aislados, incluidas las aplicaciones simples, la automatización de tareas y los trabajos de compilación. Desarrolle aplicaciones rápidamente sin necesidad de administrar máquinas virtuales.

**Principales características**
- **Tiempos de inicio rápido** ya que ACI puede iniciar contenedores en Azure en segundos sin que sea necesario aprovisionar y administrar máquinas virtuales.
- **Conectividad IP pública y nombre DNS personalizado**.
- **Seguridad de nivel de hipervisor** que garantiza que la aplicación está tan aislada en un contenedor como lo estaría en una máquina virtual.
- **Tamaños personalizados** para un uso óptimo al permitir especificaciones exactas de los núcleos y la memoria de la CPU. El usuario paga según lo que necesita y se le factura por segundo, para que pueda optimizar con precisión los gastos según sus necesidades reales.
- **Almacenamiento persistente** para recuperar y conservar el estado, ACI ofrece el montaje directo de recursos compartidos de Azure Files.
- **Contenedores de Linux y Windows** programados con la misma API.

**Referencias**
- [Azure Portal](https://portal.azure.com/)
- [Documentación](/azure/container-instances/)
- [Guías de inicio rápido](/azure/container-instances/container-instances-quickstart-portal)
- [Muestras](https://azure.microsoft.com/resources/samples/?sort=0&term=aci)
- [Tutoriales](/azure/container-instances/container-instances-tutorial-prepare-app)