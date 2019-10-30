---
title: Hospedaje del código fuente de la aplicación móvil en la nube con GitHub y Azure DevOps
description: Conozca los servicios de Microsoft para hospedar el código de la aplicación móvil en la nube.
author: elamalani
ms.assetid: 12a8a079-9b3c-4faf-2222-ccff02097224
ms.service: vs-appcenter
ms.topic: article
ms.date: 10/22/2019
ms.author: emalani
ms.openlocfilehash: f0af2ee5e62bbdbfb5d18cffc3e3ed62edec81f3
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72795128"
---
# <a name="cloud-hosted-source-code-management-services"></a>Servicios de administración del código fuente hospedados en la nube
Si tiene equipos de desarrollo con varios miembros que trabajan con el mismo código base, debe encontrar un lugar adecuado para hospedar el código. Almacenar los datos en la nube y tener un repositorio centralizado permite que todos los usuarios carguen, editen y administren los archivos de código, interactúen con otros desarrolladores en los proyectos y proporcionen la posibilidad de un acceso rápido al código sin importar dónde se encuentre, siempre y cuando tenga una conexión a Internet.

El hospedaje en la nube es mucho más sencillo que las opciones locales, ya que requiere menos configuración de hardware y permite a las organizaciones realizar el proceso de implementación de una manera más ágil.

## <a name="benefits-of-hosting-source-code-in-the-cloud"></a>Ventajas de hospedar el código fuente en la nube
- **Almacenamiento en la nube centralizado**, para ver y administrar los datos desde cualquier lugar.
- Permite una **mejor colaboración y un código más limpio** dentro de los equipos para mantener el seguimiento del código y administrar los proyectos a fin de garantizar la entrega continua de software excelente.
- **Participativo** y **facilidad para contribuir** a los proyectos.
- **Ciclo de versiones más rápido** dado que el equipo puede trabajar más rápido y **contribuir fácilmente** a los proyectos.
- **Reducción de los costos** sin tener que preocuparse por mantener su propio hardware, servidores, VPN, etc.

Use los siguientes servicios para hospedar los datos de la aplicación en la nube.

## <a name="github"></a>GitHub
[GitHub](https://github.com/) es un servicio de hospedaje de repositorio de código abierto que hospeda proyectos de código fuente en varios lenguajes de programación y realiza un seguimiento de los distintos cambios realizados en cada iteración.

**Características principales**
- **Hospedaje del código** con todo el código en un solo lugar. Tanto sin son privados, públicos o de código abierto, todos los repositorios están equipados con herramientas para ayudarle a hospedar el código, controlar sus versiones y publicarlo.
- La **revisión completa del código** y las herramientas de revisión integradas convierten la revisión del código en una parte esencial de cualquier proceso en equipo.
    - Proteja las ramas, proponga cambios y solicite revisiones.
    - Vea la diferencia, realice comentarios en contexto y reciba información clara.
- Coordine antes los procesos, permanezca alineado y sea más productivo con las **herramientas de administración de proyectos** de GitHub.
    - Obtenga una visión global del proyecto.
    - **Paneles de tareas** justo al lado del código en GitHub.
    - **Tarjetas de arrastrar y colocar** para permitirle asignar problemas o solicitudes de incorporación de cambios a los miembros del equipo.
    - **Hitos** para organizar y realizar un seguimiento del progreso.
    - **Notas** para capturar ideas que podrían ser útiles pero que no pertenecen a un problema o una solicitud de incorporación de cambios determinados.
- Descubra y elija fácilmente las herramientas adecuadas para mejorar la comunicación y la automatización del trabajo en **[Marketplace de GitHub](https://github.com/marketplace)** (el lugar de compra de las aplicaciones).
- **Administración de equipos** para administrar y desarrollar equipos fácilmente, **Roles de usuario** para organizar los equipos y los permisos de acceso, herramientas de **Hilos de discusión** para mantener las conversaciones sobre el tema y centradas en el equipo y **Directrices de la comunidad** para configurar rápidamente una cuenta para los nuevos miembros del equipo.
- **Examine** y **puntúe con estrellas** los proyectos más populares para seguirlos.
- **Red de asociados** para aprender de otras personas del sector.

**Referencias**
- [GitHub](https://github.com/)
- [Guías de GitHub](https://guides.github.com/)
- [Foro de la comunidad de GitHub](https://github.community/)
- [Marketplace de GitHub](https://github.com/marketplace)

## <a name="azure-devops"></a>Azure DevOps
[Azure DevOps](https://azure.microsoft.com/services/devops/) admite [Azure Repos](https://azure.microsoft.com/services/devops/repos/) y el [control de versiones de Team Foundation (TFVC)](https://docs.microsoft.com/azure/devops/repos/tfvc/index?view=azure-devops) como opciones de control del código fuente. Dispone de repositorios privados gratuitos ilimitados con revisiones de código en colaboración, administración avanzada de archivos, búsqueda de código y directivas de rama para garantizar la alta calidad del código. Azure Repos es muy adecuado para proyectos pequeños, así como para grandes organizaciones que necesitan compatibilidad nativa con AAD y directivas avanzadas.
    
**Características principales**
- **Repositorio ilimitado de código fuente de Git hospedado en la nube** para su repositorios públicos y privados.
    - Compatibilidad con cualquier cliente Git.
    - Integración con webhooks y API.
- **Ponga en marcha la próxima compilación** con una solicitud de incorporación de cambios en Repos.
    - Colabore para compilar mejor el código mediante hilos de discusiones y la integración continua de cada cambio.
    - Configure la integración y la entrega continuas (CI/CD) para desencadenar automáticamente compilaciones, pruebas e implementaciones cada vez que se complete una solicitud de incorporación de cambios mediante Azure Pipelines o sus propias herramientas.
    - Proteja la calidad del código con directivas de rama.
- **Control de versiones centralizado con el control de versiones de Team Foundation**, incluida la revisión del código.
- **Conéctese al código** con Xcode, Eclipse, IntelliJ, Android Studio, Visual Studio, Visual Studio Code, etc.
- **Búsqueda semántica eficaz de código**.
- **Listo para la empresa** gracias a la integración nativa con Azure Active Directory (AD), lo que simplifica el proceso de administrar el acceso a los repositorios de código.
- **Garantice la calidad del código** con directivas de rama, como el número mínimo de revisiones de código, la necesidad de compilaciones correctas y la aplicación de estrategias de combinación de Git.
- **Conecte su entorno de desarrollo favorito** para acceder a los repositorios y administrar el trabajo.

**Referencias**
- [Introducción a Azure Repos](https://azure.microsoft.com/services/devops/repos/) 
- [Documentación de Azure Repos](/azure/devops/repos/?view=azure-devops)