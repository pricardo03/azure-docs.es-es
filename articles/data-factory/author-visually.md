---
title: Creación visual en Azure Data Factory | Microsoft Docs
description: Información acerca de cómo utilizar la creación visual de Azure Data Factory
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/7/2018
ms.author: shlo
ms.openlocfilehash: 7a0d72ac67b329cb6d25be955205a2bbcef38e81
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2018
ms.locfileid: "51281700"
---
# <a name="visual-authoring-in-azure-data-factory"></a>Creación visual de Azure Data Factory
La experiencia de la interfaz de usuario (UX) de Azure Data Factory le permite crear e implementar visualmente recursos para la factoría de datos sin tener que escribir código. Puede arrastrar y colocar las actividades en un lienzo de canalización, realizar ejecuciones de prueba, depurar de forma iterativa e implementar y supervisar ejecuciones de canalizaciones. Hay dos enfoques a la hora de utilizar la experiencia de la interfaz de usuario para llevar a cabo la creación visual:

- Crear directamente con el servicio Data Factory.
- Crear con la integración de Git de Azure Repos para la colaboración, el control de código fuente y el control de versiones.

## <a name="author-directly-with-the-data-factory-service"></a>Creación directa con el servicio Data Factory
La creación visual con el servicio Data Factory difiere de la creación visual con la integración de Git de dos maneras:

- El servicio Data Factory no incluye un repositorio que permita almacenar las entidades JSON para los cambios.
- El servicio Data Factory no está optimizado para la colaboración ni el control de versiones.

![Configuración del servicio Data Factory ](media/author-visually/configure-data-factory.png)

Cuando usa el **lienzo de creación** de UX para crear directamente con el servicio Data Factory, solo está disponible el modo **Publicar todo**. Los cambios que realice se publicarán directamente en el servicio Data Factory.

![Modo Publicar](media/author-visually/data-factory-publish.png)

## <a name="author-with-azure-repos-git-integration"></a>Creación con la integración de Git de Azure Repos
La creación visual con la integración de Git de Azure Repos admite el control del código fuente y la colaboración para trabajar en canalizaciones de factoría de datos. Puede asociar una factoría de datos con un repositorio de organizaciones de Git de Azure Repos para el control del código fuente, la colaboración, el control de versiones, etc. Una sola organización de Git de Azure Repos puede tener varios repositorios, pero un repositorio de Git de Azure Repos solo puede asociarse a una factoría de datos. Si no tiene un repositorio o una organización de Azure Repos, siga [estas instrucciones](https://docs.microsoft.com/azure/devops/organizations/accounts/create-organization-msa-or-work-student) para crear los recursos.

> [!NOTE]
> Puede almacenar archivos de datos y script en un repositorio de Git de Azure Repos. Sin embargo, tiene que cargar manualmente los archivos en Azure Storage. Una canalización de Data Factory no carga automáticamente archivos de script ni datos en un repositorio de Git de Azure Repos en Azure Storage.

### <a name="configure-an-azure-repos-git-repository-with-azure-data-factory"></a>Configuración de un repositorio de Git de Azure Repos con Azure Data Factory
Los usuarios pueden configurar un repositorio de Git de Azure Repos con una factoría de datos mediante dos métodos.

#### <a name="method1"></a> Método de configuración 1 (repositorio de Git de Azure Repos): página de introducción

En Azure Data Factory, vaya a la página **Comencemos**. Seleccione **Configuración del repositorio de código**:

![Configurar un repositorio de código de Azure Repos](media/author-visually/configure-repo.png)

Aparece el panel **Configuración del repositorio**:

![Configuración del repositorio de código](media/author-visually/repo-settings.png)

El panel muestra la siguiente configuración del repositorio de código de Azure Repos:

| Configuración | DESCRIPCIÓN | Valor |
|:--- |:--- |:--- |
| **Tipo de repositorio** | Tipo de repositorio de código de Azure Repos.<br/>**Nota**: GitHub actualmente no se admite. | Git de Azure Repos |
| **Azure Active Directory** | El nombre de su inquilino de Azure AD. | <your tenant name> |
| **Organización de Azure Repos** | Nombre de la organización de Azure Repos. Puede buscar el nombre de la organización de Azure Repos en `https://{organization name}.visualstudio.com`. Puede [iniciar sesión en la organización de Azure Repos](https://www.visualstudio.com/team-services/git/) para acceder a su perfil de Visual Studio y ver sus proyectos y repositorios. | <your organization name> |
| **ProjectName** | Nombre de proyecto de Azure Repos. Puede buscar el nombre de proyecto de Azure Repos en `https://{organization name}.visualstudio.com/{project name}`. | <your Azure Repos project name> |
| **RepositoryName** | Nombre del repositorio de código de Azure Repos. Los proyectos de Azure Repos contienen repositorios de Git para administrar el código fuente a medida que crece el proyecto. Puede crear un repositorio nuevo o usar uno existente en el proyecto. | <your Azure Repos code repository name> |
| **Rama de colaboración** | Rama de colaboración de Azure Repos que se usa para la publicación. De forma predeterminada, es `master`. Cámbiela en caso de que desee publicar recursos de otra rama. | <your collaboration branch name> |
| **Carpeta raíz** | Carpeta raíz de la rama de colaboración de Azure Repos. | <your root folder name> |
| **Import existing Data Factory resources to repository** (Importar recursos existentes de Data Factory en el repositorio). | Especifica si se deben importar los recursos de la factoría de datos existente del **lienzo de creación** de UX en un repositorio Git de Azure Repos. Active la casilla para importar los recursos de la factoría de datos en el repositorio Git asociado en formato JSON. Esta acción exporta cada recurso individualmente (es decir, los servicios vinculados y los conjuntos de datos se exportan a archivos JSON independientes). Cuando esta casilla no está activada, no se importan los recursos existentes. | Activada (valor predeterminado) |

#### <a name="configuration-method-2-azure-repos-git-repo-ux-authoring-canvas"></a>Método de configuración 2 (repositorio Git de Azure Repos): lienzo de creación de UX
En el **lienzo de creación** de la experiencia de interfaz de usuario de Azure Data Factory, busque su factoría de datos. Seleccione el menú desplegable **Factoría de datos** y, a continuación, seleccione **Configurar repositorio de código**.

Aparece un panel de configuración. Para obtener más detalles acerca de las opciones de configuración, consulte las descripciones en <a href="#method1">Método de configuración 1</a>.

![Configuración del repositorio de código para la creación con UX](media/author-visually/configure-repo-2.png)

## <a name="use-a-different-azure-active-directory-tenant"></a>Uso de un inquilino de Azure Active Directory distinto

Puede crear un repositorio Git de Azure Repos en un inquilino de Azure Active Directory distinto. Para especificar otro inquilino de Azure AD, deberá tener permisos de administrador para la suscripción de Azure que utilice.

## <a name="use-your-personal-microsoft-account"></a>Uso de la cuenta de Microsoft personal

Para usar una cuenta de Microsoft personal para la integración de Git, puede vincular el repositorio de Azure personal al entorno de Active Directory de su organización.

1. Agregue la cuenta de Microsoft personal al entorno de Active Directory de su organización como invitado. Para más información, vea [Incorporación de usuarios de colaboración B2B de Azure Active Directory en Azure Portal](../active-directory/b2b/add-users-administrator.md).

2. Inicie sesión en Azure Portal con la cuenta de Microsoft personal. A continuación, cambie al entorno de Active Directory de su organización.

3. Vaya a la sección de Azure DevOps, donde verá ahora el repositorio personal. Seleccione el repositorio y establezca la conexión con Active Directory.

Después de estos pasos de configuración, su repositorio personal está disponible al configurar la integración de Git en la interfaz de usuario de Data Factory.

Para más información sobre cómo conectar Azure Repos al entorno de Active Directory de su organización, vea [Connect your Azure DevOps organization to Azure Active Directory](/azure/devops/organizations/accounts/connect-organization-to-azure-ad) (Conexión de la organización de Azure DevOps a Azure Active Directory).

## <a name="switch-to-a-different-git-repo"></a>Cambie a otro repositorio de Git diferente

Para cambiar a un repositorio de Git diferente, busque el icono en la esquina superior derecha de la página de información general de Data Factory, como se muestra en la siguiente captura de pantalla. Si no ve el icono, borre la caché del explorador local. Seleccione el icono para quitar la asociación con el repositorio actual.

Después de quitar la asociación con el repositorio actual, puede configurar la configuración de Git para que use un repositorio diferente. Después, puede importar los recursos de Data Factory existentes en el nuevo repositorio.

![Elimine la asociación con el repositorio de Git actual.](media/author-visually/remove-repo.png)

## <a name="use-version-control"></a>Uso del control de versiones
Los sistemas de control de versiones, conocidos también como _de control de código fuente_, permiten a los desarrolladores colaborar en el código y llevar a cabo el seguimiento de los cambios realizados en la base de código. El control del código fuente es una herramienta esencial para proyectos de varios desarrolladores.

Tan pronto como cada repositorio Git de Azure Repos se asocia a una factoría de datos, tiene una rama de colaboración. (`master` es la rama de colaboración predeterminada). Para crear ramas de características, los usuarios también pueden hacer clic en **+ Nueva rama** y desarrollar en las ramas de características.

![Cambio del código sincronizando o publicando](media/author-visually/sync-publish.png)

Cuando haya terminado con el desarrollo de la característica en la rama de características, haga clic en **Crear solicitud de incorporación de cambios**. Esto le llevará al Git de Azure Repos, donde podrá generar la solicitud de incorporación de cambios, revisar el código y combinar los cambios en su rama de colaboración. (`master` es el valor predeterminado). Solo tiene permitido publicar en el servicio Data Factory de la rama de colaboración. 

![Creación de una nueva solicitud de incorporación de cambios](media/author-visually/create-pull-request.png)

## <a name="configure-publishing-settings"></a>Configurar los valores de publicación

Para configurar la rama de publicación, es decir, la rama donde se guardan las plantillas de Resource Manager, agregue un archivo `publish_config.json` a la carpeta raíz de la rama de colaboración. Data Factory lee este archivo, busca el campo `publishBranch` y crea una nueva rama (si aún no existe) con el valor proporcionado. Después guarda todas las plantillas de Resource Manager en la ubicación especificada. Por ejemplo: 

```json
{
    "publishBranch": "factory/adf_publish"
}
```

Al publicar desde el modo de Git, puede confirmar que Data Factory usa la rama de publicación que espera, como se muestra en esta captura de pantalla:

![Confirmar la rama de publicación correcta](media/author-visually/configure-publish-branch.png)

Al especificar una nueva rama de publicación, Data Factory no elimina la rama de publicación anterior. Si quiere quitar la rama de publicación anterior, tiene que hacerlo manualmente.

Data Factory solo lee el archivo `publish_config.json` cuando se carga la factoría. Si ya tiene la factoría cargada en el portal, actualice el explorador para que los cambios surtan efecto.

## <a name="publish-code-changes"></a>Cambios de código publicando
Después de haber combinado los cambios en la rama de colaboración (`master` es el valor predeterminado), seleccione **Publicar** para publicar manualmente los cambios de código en la rama principal para el servicio Data Factory.

![Publicación de cambios en el servicio Data Factory](media/author-visually/publish-changes.png)

> [!IMPORTANT]
> La rama principal no es representativa de lo que se implementa en el servicio Data Factory. La rama principal se *debe* publicar manualmente en el servicio Data Factory.

## <a name="author-with-github-integration"></a>Creación con la integración de GitHub

La creación visual con la integración de GitHub admite el control del código fuente y la colaboración para trabajar en canalizaciones de factoría de datos. Puede asociar una factoría de datos con un repositorio de cuentas de GitHub para el control del código fuente, la colaboración y el control de versiones. Una sola cuenta de GitHub puede tener varios repositorios, pero un repositorio de GitHub solo puede asociarse a una factoría de datos. Si no tiene un repositorio o una cuenta de GitHub, siga  [estas instrucciones](https://github.com/join) para crear los recursos.

La integración de GitHub con Data Factory admite tanto GitHub público (es decir, [https://github.com](https://github.com)) como GitHub Enterprise. Puede usar los repositorios de GitHub públicos y privados con Data Factory siempre que tenga permisos de lectura y escritura para el repositorio en GitHub.

Para configurar un repositorio de GitHub, deberá tener permisos de administrador para la suscripción de Azure que utilice.

Si desea ver una introducción y una demostración de esta característica, vea el siguiente vídeo de nueve minutos de duración:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Azure-Data-Factory-visual-tools-now-integrated-with-GitHub/player]

### <a name="limitations"></a>Limitaciones

- Puede almacenar archivos de datos y script en un repositorio de GitHub. Sin embargo, tiene que cargar manualmente los archivos en Azure Storage. Una canalización de Data Factory no carga automáticamente archivos de script o datos en un repositorio de GitHub en Azure Storage.

- GitHub Enterprise con una versión anterior a 2.14.0 no funciona en el explorador Microsoft Edge.

- La integración de GitHub con las herramientas de creación visual de Data Factory solo funciona con la versión de Data Factory disponible de manera general.

### <a name="configure-a-public-github-repository-with-azure-data-factory"></a>Configuración de un repositorio de GitHub público con Azure Data Factory

Los usuarios pueden configurar un repositorio GitHub con una factoría de datos mediante dos métodos.

**Método 1 de configuración (repositorio público): página de introducción**

En Azure Data Factory, vaya a la página  **Empecemos** . Seleccione  **Configuración del repositorio de código**:

![Página de introducción de Data Factory](media/author-visually/github-integration-image1.png)

Aparece el panel  **Configuración del repositorio** :

![Configuración del repositorio de GitHub](media/author-visually/github-integration-image2.png)

El panel muestra la siguiente configuración del repositorio de código de Azure Repos:

| **Configuración**                                              | **Descripción**                                                                                                                                                                                                                                                                                                                                                                                                                   | **Valor**          |
|----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------|
| **Tipo de repositorio**                                      | Tipo de repositorio de código de Azure Repos.                                                                                                                                                                                                                                                                                                                                                                                             | GitHub             |
| **Cuenta de GitHub**                                       | El nombre de la cuenta de GitHub. Este nombre se puede encontrar en https://github.com/{account nombre}/{nombre del repositorio}. Para ir a esta página se le pide que introduzca credenciales de OAuth de GitHub en la cuenta de GitHub.                                                                                                                                                                                                                                               |                    |
| **RepositoryName**                                       | El nombre del repositorio de código de GitHub. Las cuentas de GitHub contienen repositorios de Git para administrar el código fuente. Puede crear un repositorio o usar uno existente que ya se encuentre en su cuenta.                                                                                                                                                                                                                              |                    |
| **Rama de colaboración**                                 | La rama de colaboración de GitHub que se usa para la publicación. De forma predeterminada, es master. Cámbiela en caso de que desee publicar recursos de otra rama.                                                                                                                                                                                                                                                               |                    |
| **Carpeta raíz**                                          | La carpeta raíz de la rama de colaboración de GitHub.                                                                                                                                                                                                                                                                                                                                                                             |                    |
| **Import existing Data Factory resources to repository** (Importar recursos existentes de Data Factory en el repositorio). | Especifica si se deben importar los recursos de Data Factory existentes del  **lienzo de creación**  de UX en un repositorio de GitHub. Active la casilla para importar los recursos de la factoría de datos en el repositorio Git asociado en formato JSON. Esta acción exporta cada recurso individualmente (es decir, los servicios vinculados y los conjuntos de datos se exportan a archivos JSON independientes). Cuando esta casilla no está activada, no se importan los recursos existentes. | Activada (valor predeterminado) |
| **Rama donde importar recursos**                       | Especifica en qué rama se importan los recursos de la factoría de datos (canalizaciones, conjuntos de datos, servicios vinculados etc.). Puede importar recursos en una de las siguientes ramas: a. Colaboración b. Crear nuevo c. Usar existente                                                                                                                                                                                                     |                    |

#### <a name="configuration-method-2-public-repo-ux-authoring-canvas"></a>Método de configuración 2 (repositorio público): lienzo de creación de UX

En el  **lienzo de creación** de la experiencia de usuario de Azure Data Factory, busque su factoría de datos. Seleccione el menú desplegable  **Data Factory**  y, a continuación, seleccione  **Configurar repositorio de código**.

Aparece un panel de configuración. Para más detalles sobre las opciones de configuración, consulte las descripciones en el anterior  *Método de configuración 1*.

### <a name="configure-a-github-enterprise-repository-with-azure-data-factory"></a>Configuración de un repositorio de GitHub Enterprise con Azure Data Factory

Los usuarios pueden configurar un repositorio GitHub Enterprise con una factoría de datos mediante dos métodos.

 #### <a name="configuration-method-1-enterprise-repo-lets-get-started-page"></a>Método 1 de configuración (repositorio Enterprise): página de introducción

En Azure Data Factory, vaya a la página  **Empecemos** . Seleccione  **Configuración del repositorio de código**:

![Página de introducción de Data Factory](media/author-visually/github-integration-image1.png)

Aparece el panel  **Configuración del repositorio** :

![Configuración del repositorio de GitHub](media/author-visually/github-integration-image3.png)

El panel muestra la siguiente configuración del repositorio de código de Azure Repos:

| **Configuración**                                              | **Descripción**                                                                                                                                                                                                                                                                                                                                                                                                                   | **Valor**          |
|----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------|
| **Tipo de repositorio**                                      | Tipo de repositorio de código de Azure Repos.                                                                                                                                                                                                                                                                                                                                                                                             | GitHub             |
| **Use GitHub Enterprise** (Usar GitHub Enterprise)                                | Casilla para seleccionar GitHub Enterprise                                                                                                                                                                                                                                                                                                                                                                                              |                    |
| **GitHub Enterprise URL** (URL de GitHub Enterprise)                                | Dirección URL raíz de GitHub Enterprise. Por ejemplo: https://github.mydomain.com                                                                                                                                                                                                                                                                                                                                                          |                    |
| **Cuenta de GitHub**                                       | El nombre de la cuenta de GitHub. Este nombre se puede encontrar en https://github.com/{account nombre}/{nombre del repositorio}. Para ir a esta página se le pide que introduzca credenciales de OAuth de GitHub en la cuenta de GitHub.                                                                                                                                                                                                                                               |                    |
| **RepositoryName**                                       | El nombre del repositorio de código de GitHub. Las cuentas de GitHub contienen repositorios de Git para administrar el código fuente. Puede crear un repositorio o usar uno existente que ya se encuentre en su cuenta.                                                                                                                                                                                                                              |                    |
| **Rama de colaboración**                                 | La rama de colaboración de GitHub que se usa para la publicación. De forma predeterminada, es master. Cámbiela en caso de que desee publicar recursos de otra rama.                                                                                                                                                                                                                                                               |                    |
| **Carpeta raíz**                                          | La carpeta raíz de la rama de colaboración de GitHub.                                                                                                                                                                                                                                                                                                                                                                             |                    |
| **Import existing Data Factory resources to repository** (Importar recursos existentes de Data Factory en el repositorio). | Especifica si se deben importar los recursos de Data Factory existentes del  **lienzo de creación**  de UX en un repositorio de GitHub. Active la casilla para importar los recursos de la factoría de datos en el repositorio Git asociado en formato JSON. Esta acción exporta cada recurso individualmente (es decir, los servicios vinculados y los conjuntos de datos se exportan a archivos JSON independientes). Cuando esta casilla no está activada, no se importan los recursos existentes. | Activada (valor predeterminado) |
| **Rama donde importar recursos**                       | Especifica en qué rama se importan los recursos de la factoría de datos (canalizaciones, conjuntos de datos, servicios vinculados etc.). Puede importar recursos en una de las siguientes ramas: a. Colaboración b. Crear nuevo c. Usar existente                                                                                                                                                                                                     |                    |

#### <a name="configuration-method-2-enterprise-repo-ux-authoring-canvas"></a>Método de configuración 2 (repositorio de Enterprise): lienzo de creación de UX

En el  **lienzo de creación** de la experiencia de usuario de Azure Data Factory, busque su factoría de datos. Seleccione el menú desplegable  **Data Factory**  y, a continuación, seleccione  **Configurar repositorio de código**.

Aparece un panel de configuración. Para más detalles sobre las opciones de configuración, consulte las descripciones en el anterior  *Método de configuración 1*.

## <a name="use-the-expression-language"></a>Uso del lenguaje de expresión
Puede especificar expresiones para los valores de propiedad mediante el lenguaje de expresión que admite Azure Data Factory.

Para especificar las expresiones para los valores de propiedad, seleccione **Agregar contenido dinámico**:

![Uso del lenguaje de expresión](media/author-visually/dynamic-content-1.png)

## <a name="use-functions-and-parameters"></a>Uso de funciones y parámetros

Puede usar funciones o especificar parámetros para canalizaciones y conjuntos de datos en el **compilador de expresiones** de Data Factory:

Para más información sobre las expresiones admitidas, consulte [Expresiones y funciones de Azure Data Factory](control-flow-expression-language-functions.md).

![Incorporación de contenido dinámico](media/author-visually/dynamic-content-2.png)

## <a name="provide-feedback"></a>Envío de comentarios
Seleccione **Comentarios** para comentar sobre las características o para notificar a Microsoft sobre los problemas con la herramienta:

![Comentarios](media/author-visually/provide-feedback.png)

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre la supervisión y la administración de canalizaciones, consulte el artículo [Supervisión y administración de canalizaciones mediante programación](monitor-programmatically.md).
