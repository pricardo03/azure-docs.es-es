---
title: Creación visual en Azure Data Factory | Microsoft Docs
description: Información acerca de cómo utilizar la creación visual de Azure Data Factory
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/09/2019
author: sharonlo101
ms.author: shlo
ms.reviewer: ''
manager: craigg
ms.openlocfilehash: b32e4abebdfb93312c60a25ca8b483f071e5e73c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65507816"
---
# <a name="visual-authoring-in-azure-data-factory"></a>Creación visual de Azure Data Factory
La experiencia de la interfaz de usuario (UX) de Azure Data Factory le permite crear e implementar visualmente recursos para la factoría de datos sin tener que escribir código. Puede arrastrar y colocar las actividades en un lienzo de canalización, realizar ejecuciones de prueba, depurar de forma iterativa e implementar y supervisar ejecuciones de canalizaciones. Hay dos enfoques a la hora de utilizar la experiencia de la interfaz de usuario para llevar a cabo la creación visual:

- Crear directamente con el servicio Data Factory.
- Crear con la integración de Git de Azure Repos para la colaboración, el control de código fuente y el control de versiones.

## <a name="author-directly-with-the-data-factory-service"></a>Creación directa con el servicio Data Factory
La creación visual con el servicio Data Factory difiere de la creación visual con la integración de Git de dos maneras:

- El servicio Data Factory no incluye un repositorio que permita almacenar las entidades JSON para los cambios.
- El servicio Data Factory no está optimizado para la colaboración ni el control de versiones.

![Configuración del servicio Data Factory](media/author-visually/configure-data-factory.png)

Cuando usa el **lienzo de creación** de UX para crear directamente con el servicio Data Factory, solo está disponible el modo **Publicar todo**. Los cambios que realice se publicarán directamente en el servicio Data Factory.

![Modo Publicar](media/author-visually/data-factory-publish.png)

## <a name="author-with-azure-repos-git-integration"></a>Creación con la integración de Git de Azure Repos
La creación visual con la integración de Git de Azure Repos admite el control del código fuente y la colaboración para trabajar en canalizaciones de factoría de datos. Puede asociar una factoría de datos con un repositorio de organizaciones de Git de Azure Repos para el control del código fuente, la colaboración, el control de versiones, etc. Una sola organización de Git de Azure Repos puede tener varios repositorios, pero un repositorio de Git de Azure Repos solo puede asociarse a una factoría de datos. Si no tiene un repositorio o una organización de Azure Repos, siga [estas instrucciones](https://docs.microsoft.com/azure/devops/organizations/accounts/create-organization-msa-or-work-student) para crear los recursos.

> [!NOTE]
> Puede almacenar archivos de datos y script en un repositorio de Git de Azure Repos. Sin embargo, tiene que cargar manualmente los archivos en Azure Storage. Una canalización de Data Factory no carga automáticamente archivos de script ni datos en un repositorio de Git de Azure Repos en Azure Storage.

### <a name="configure-an-azure-repos-git-repository-with-azure-data-factory"></a>Configuración de un repositorio de Git de Azure Repos con Azure Data Factory
Los usuarios pueden configurar un repositorio de Git de Azure Repos con una factoría de datos mediante dos métodos.

#### <a name="method1"></a> Método de configuración 1 (repositorio Git de Azure Repos): Página de introducción

En Azure Data Factory, vaya a la página **Comencemos**. Seleccione **Configuración del repositorio de código**:

![Configurar un repositorio de código de Azure Repos](media/author-visually/configure-repo.png)

Aparece el panel **Configuración del repositorio**:

![Configuración del repositorio de código](media/author-visually/repo-settings.png)

El panel muestra la siguiente configuración del repositorio de código de Azure Repos:

| Configuración | DESCRIPCIÓN | Valor |
|:--- |:--- |:--- |
| **Tipo de repositorio** | Tipo de repositorio de código de Azure Repos.<br/> | Git de Azure Repos |
| **Azure Active Directory** | El nombre de su inquilino de Azure AD. | `<your tenant name>` |
| **Organización de Azure Repos** | Nombre de la organización de Azure Repos. Puede buscar el nombre de la organización de Azure Repos en `https://{organization name}.visualstudio.com`. Puede [iniciar sesión en la organización de Azure Repos](https://www.visualstudio.com/team-services/git/) para acceder a su perfil de Visual Studio y ver sus proyectos y repositorios. | `<your organization name>` |
| **ProjectName** | Nombre de proyecto de Azure Repos. Puede buscar el nombre de proyecto de Azure Repos en `https://{organization name}.visualstudio.com/{project name}`. | `<your Azure Repos project name>` |
| **RepositoryName** | Nombre del repositorio de código de Azure Repos. Los proyectos de Azure Repos contienen repositorios de Git para administrar el código fuente a medida que crece el proyecto. Puede crear un repositorio nuevo o usar uno existente en el proyecto. | `<your Azure Repos code repository name>` |
| **Rama de colaboración** | Rama de colaboración de Azure Repos que se usa para la publicación. De forma predeterminada, es `master`. Cámbiela en caso de que desee publicar recursos de otra rama. | `<your collaboration branch name>` |
| **Carpeta raíz** | Carpeta raíz de la rama de colaboración de Azure Repos. | `<your root folder name>` |
| **Import existing Data Factory resources to repository** (Importar recursos existentes de Data Factory en el repositorio). | Especifica si se deben importar los recursos de la factoría de datos existente del **lienzo de creación** de UX en un repositorio Git de Azure Repos. Active la casilla para importar los recursos de la factoría de datos en el repositorio Git asociado en formato JSON. Esta acción exporta cada recurso individualmente (es decir, los servicios vinculados y los conjuntos de datos se exportan a archivos JSON independientes). Cuando esta casilla no está activada, no se importan los recursos existentes. | Activada (valor predeterminado) |

#### <a name="configuration-method-2-azure-repos-git-repo-ux-authoring-canvas"></a>Método de configuración 2 (repositorio Git de Azure Repos): lienzo de creación de la experiencia de usuario
En el **lienzo de creación** de la experiencia de interfaz de usuario de Azure Data Factory, busque su factoría de datos. Seleccione el menú desplegable **Factoría de datos** y, a continuación, seleccione **Configurar repositorio de código**.

Aparece un panel de configuración. Para obtener más detalles acerca de las opciones de configuración, consulte las descripciones en <a href="#method1">Método de configuración 1</a>.

![Configuración del repositorio de código para la creación con UX](media/author-visually/configure-repo-2.png)

### <a name="use-a-different-azure-active-directory-tenant"></a>Uso de un inquilino de Azure Active Directory distinto

Puede crear un repositorio Git de Azure Repos en un inquilino de Azure Active Directory distinto. Para especificar otro inquilino de Azure AD, deberá tener permisos de administrador para la suscripción de Azure que utilice.

### <a name="use-your-personal-microsoft-account"></a>Uso de la cuenta de Microsoft personal

Para usar una cuenta de Microsoft personal para la integración de Git, puede vincular el repositorio de Azure personal al entorno de Active Directory de su organización.

1. Agregue la cuenta de Microsoft personal al entorno de Active Directory de su organización como invitado. Para más información, vea [Incorporación de usuarios de colaboración B2B de Azure Active Directory en Azure Portal](../active-directory/b2b/add-users-administrator.md).

2. Inicie sesión en Azure Portal con la cuenta de Microsoft personal. A continuación, cambie al entorno de Active Directory de su organización.

3. Vaya a la sección de Azure DevOps, donde verá ahora el repositorio personal. Seleccione el repositorio y establezca la conexión con Active Directory.

Después de estos pasos de configuración, su repositorio personal está disponible al configurar la integración de Git en la interfaz de usuario de Data Factory.

Para más información sobre cómo conectar Azure Repos al entorno de Active Directory de su organización, vea [Connect your Azure DevOps organization to Azure Active Directory](/azure/devops/organizations/accounts/connect-organization-to-azure-ad) (Conexión de la organización de Azure DevOps a Azure Active Directory).

### <a name="switch-to-a-different-git-repo"></a>Cambie a otro repositorio de Git diferente

Para cambiar a un repositorio de Git diferente, busque el icono en la esquina superior derecha de la página de información general de Data Factory, como se muestra en la siguiente captura de pantalla. Si no ve el icono, borre la caché del explorador local. Seleccione el icono para quitar la asociación con el repositorio actual.

Después de quitar la asociación con el repositorio actual, puede configurar la configuración de Git para que use un repositorio diferente. Después, puede importar los recursos de Data Factory existentes en el nuevo repositorio.

![Elimine la asociación con el repositorio de Git actual.](media/author-visually/remove-repo.png)

### <a name="use-version-control"></a>Uso del control de versiones
Los sistemas de control de versiones, conocidos también como _de control de código fuente_, permiten a los desarrolladores colaborar en el código y llevar a cabo el seguimiento de los cambios realizados en la base de código. El control del código fuente es una herramienta esencial para proyectos de varios desarrolladores.

Tan pronto como cada repositorio Git de Azure Repos se asocia a una factoría de datos, tiene una rama de colaboración. (`master` es la rama de colaboración predeterminada). Para crear ramas de características, los usuarios también pueden hacer clic en **+ Nueva rama** y desarrollar en las ramas de características.

![Cambio del código sincronizando o publicando](media/author-visually/sync-publish.png)

Cuando haya terminado con el desarrollo de la característica en la rama de características, haga clic en **Crear solicitud de incorporación de cambios**. Esto le llevará al Git de Azure Repos, donde podrá generar la solicitud de incorporación de cambios, revisar el código y combinar los cambios en su rama de colaboración. (`master` es el valor predeterminado). Solo tiene permitido publicar en el servicio Data Factory de la rama de colaboración. 

![Creación de una nueva solicitud de incorporación de cambios](media/author-visually/create-pull-request.png)

### <a name="configure-publishing-settings"></a>Configurar los valores de publicación

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

### <a name="publish-code-changes"></a>Cambios de código publicando
Después de haber combinado los cambios en la rama de colaboración (`master` es el valor predeterminado), seleccione **Publicar** para publicar manualmente los cambios de código en la rama principal para el servicio Data Factory.

![Publicación de cambios en el servicio Data Factory](media/author-visually/publish-changes.png)

> [!IMPORTANT]
> La rama principal no es representativa de lo que se implementa en el servicio Data Factory. La rama principal se *debe* publicar manualmente en el servicio Data Factory.

### <a name="advantages-of-git-integration"></a>Ventajas de la integración de Git

-   **Control de código fuente**. A medida que las cargas de trabajo de la factoría de datos se vuelven cruciales, le convendría integrar la factoría con Git para aprovechar varias ventajas de control de código fuente como las siguientes:
    -   Capacidad para realizar el seguimiento y la auditoría de los cambios.
    -   Capacidad para revertir los cambios que presentaron errores.
-   **Operaciones de guardado parcial**. Al realizar muchos cambios en la factoría, se dará cuenta de que, en el modo real normal, no puede guardar los cambios como borrador, porque no está preparado o porque no quiere perder los cambios en caso de que su equipo se bloquee. Con la integración de Git, puede continuar guardando los cambios de forma incremental y publicarlos en la factoría solo cuando esté listo. Git actúa como un espacio de almacenamiento provisional para el trabajo, hasta que haya probado los cambios y estén a su gusto.
-   **Colaboración y control**. Si varios miembros del equipo participan en la misma factoría, puede que le convenga permitir que sus compañeros de equipo colaboren entre sí mediante un proceso de revisión de código. También puede configurar la factoría de manera que no todos sus colaboradores tengan permiso para implementar en ella. Es posible que los miembros del equipo solo puedan hacer cambios a través de Git, pero solo algunas personas del equipo puedan "Publicar" los cambios en la factoría.
-   **Muestra de las diferencias**. En el modo de Git, puede ver una clara diferencia de la carga útil que está a punto de publicar en la factoría. Esta diferencia muestra todos los recursos y entidades que se han modificado, agregado y eliminado desde la última vez que se publicó en la factoría. Según esta diferencia, puede continuar con la publicación o volver y comprobar los cambios, así como volver más tarde.
-   **Mejor proceso de CI/CD**. Si utiliza el modo de Git, puede configurar la canalización de versión para que se desencadene automáticamente en cuanto haya algún cambio en la factoría en desarrollo. También podrá personalizar las propiedades de la factoría que estén disponibles como parámetros en la plantilla de Resource Manager. Puede ser útil mantener solo el conjunto requerido de propiedades como parámetros, y tener todo lo demás codificado.
-   **Mejor rendimiento**. Una factoría media carga 10 veces más rápido en el modo de Git que en el modo real normal, porque los recursos se descargan a través de Git.

### <a name="best-practices-for-git-integration"></a>Procedimientos recomendados para la integración de Git

-   **Permisos**. Normalmente no conviene que todos los miembros del equipo tengan permisos para actualizar la factoría.
    -   Todos los miembros del equipo deberían tener permisos de lectura en la factoría de datos.
    -   Solo un conjunto seleccionado de usuarios debería poder publicar en la factoría y, para ello, tienen que formar parte del rol "Colaborador de Data Factory" en la factoría.
    -   Uno de los procedimientos recomendados del control de código fuente consiste también en no permitir inserciones directas en el repositorio en la rama de colaboración. Este requisito evita los errores, ya que cada inserción en el repositorio pasa por un proceso de solicitud de incorporación de cambios.
-   **Cambio de modos**.
    -    Cuando esté en modo de Git, no se recomienda cambiar una y otra vez al modo normal, principalmente porque cualquier cambio que se haga en el modo normal no se verá al volver a Git. Procure realizar los cambios en el mismo modo de Git y publicarlos luego a través de la interfaz de usuario.
    -   De forma similar, no utilice tampoco ningún cmdlet de Powershell en la factoría de datos, ya que se consigue el mismo efecto aplicando directamente los cambios especificados a la factoría real.
-   **Uso de contraseñas de Azure Key Vault**.
    -   Se recomienda muy especialmente usar AzureKeyVault para almacenar las cadenas de conexión o las contraseñas en servicios vinculados de DataFactory.
    -   No almacenamos ninguna información secreta de este tipo en Git (por motivos de seguridad), por lo que los cambios efectuados en los servicios vinculados se publican inmediatamente en la factoría real. Esta publicación inmediata no siempre es conveniente, ya que es posible que los cambios no se hayan probado, lo que frustra el propósito de Git.
    -   Por consiguiente, los cambios en todos estos secretos deben recuperarse de servicios vinculados que utilizan Azure Key Vault.
    -   Otra de las ventajas de utilizar Key Vault es que facilita el proceso de CI/CD al no tener que especificar estos secretos durante la implementación de la plantilla de Resource Manager.

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

**Método de configuración 1 (repositorio público): página de introducción**

En Azure Data Factory, vaya a la página  **Empecemos** . Seleccione  **Configuración del repositorio de código**:

![Página de introducción de Data Factory](media/author-visually/github-integration-image1.png)

Aparece el panel  **Configuración del repositorio** :

![Configuración del repositorio de GitHub](media/author-visually/github-integration-image2.png)

El panel muestra la siguiente configuración del repositorio de código de Azure Repos:

| **Configuración**                                              | **Descripción**                                                                                                                                                                                                                                                                                                                                                                                                                   | **Valor**          |
|----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------|
| **Tipo de repositorio**                                      | Tipo de repositorio de código de Azure Repos.                                                                                                                                                                                                                                                                                                                                                                                             | GitHub             |
| **Cuenta de GitHub**                                       | El nombre de la cuenta de GitHub. Este nombre se puede encontrar en https:\//github.com/{nombre de cuenta}/{nombre de repositorio}. Para ir a esta página se le pide que introduzca credenciales de OAuth de GitHub en la cuenta de GitHub.                                                                                                                                                                                                                                               |                    |
| **RepositoryName**                                       | El nombre del repositorio de código de GitHub. Las cuentas de GitHub contienen repositorios de Git para administrar el código fuente. Puede crear un repositorio o usar uno existente que ya se encuentre en su cuenta.                                                                                                                                                                                                                              |                    |
| **Rama de colaboración**                                 | La rama de colaboración de GitHub que se usa para la publicación. De forma predeterminada, es master. Cámbiela en caso de que desee publicar recursos de otra rama.                                                                                                                                                                                                                                                               |                    |
| **Carpeta raíz**                                          | La carpeta raíz de la rama de colaboración de GitHub.                                                                                                                                                                                                                                                                                                                                                                             |                    |
| **Import existing Data Factory resources to repository** (Importar recursos existentes de Data Factory en el repositorio). | Especifica si se deben importar los recursos de Data Factory existentes del  **lienzo de creación**  de UX en un repositorio de GitHub. Active la casilla para importar los recursos de la factoría de datos en el repositorio Git asociado en formato JSON. Esta acción exporta cada recurso individualmente (es decir, los servicios vinculados y los conjuntos de datos se exportan a archivos JSON independientes). Cuando esta casilla no está activada, no se importan los recursos existentes. | Activada (valor predeterminado) |
| **Rama donde importar recursos**                       | Especifica en qué rama se importan los recursos de la factoría de datos (canalizaciones, conjuntos de datos, servicios vinculados etc.). Puede importar recursos en una de las siguientes ramas: a. Colaboración b. Crear nuevo c. Usar existente                                                                                                                                                                                                     |                    |

#### <a name="configuration-method-2-public-repo-ux-authoring-canvas"></a>Método de configuración 2 (repositorio público): lienzo de creación de la experiencia de usuario

En el  **lienzo de creación** de la experiencia de usuario de Azure Data Factory, busque su factoría de datos. Seleccione el menú desplegable  **Data Factory**  y, a continuación, seleccione  **Configurar repositorio de código**.

Aparece un panel de configuración. Para más detalles sobre las opciones de configuración, consulte las descripciones en el anterior  *Método de configuración 1*.

### <a name="configure-a-github-enterprise-repository-with-azure-data-factory"></a>Configuración de un repositorio de GitHub Enterprise con Azure Data Factory

Los usuarios pueden configurar un repositorio GitHub Enterprise con una factoría de datos mediante dos métodos.

#### <a name="configuration-method-1-enterprise-repo-lets-get-started-page"></a>Método de configuración 1 (repositorio de Enterprise): Página de introducción

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
| **Cuenta de GitHub**                                       | El nombre de la cuenta de GitHub. Este nombre se puede encontrar en https:\//github.com/{nombre de cuenta}/{nombre de repositorio}. Para ir a esta página se le pide que introduzca credenciales de OAuth de GitHub en la cuenta de GitHub.                                                                                                                                                                                                                                               |                    |
| **RepositoryName**                                       | El nombre del repositorio de código de GitHub. Las cuentas de GitHub contienen repositorios de Git para administrar el código fuente. Puede crear un repositorio o usar uno existente que ya se encuentre en su cuenta.                                                                                                                                                                                                                              |                    |
| **Rama de colaboración**                                 | La rama de colaboración de GitHub que se usa para la publicación. De forma predeterminada, es master. Cámbiela en caso de que desee publicar recursos de otra rama.                                                                                                                                                                                                                                                               |                    |
| **Carpeta raíz**                                          | La carpeta raíz de la rama de colaboración de GitHub.                                                                                                                                                                                                                                                                                                                                                                             |                    |
| **Import existing Data Factory resources to repository** (Importar recursos existentes de Data Factory en el repositorio). | Especifica si se deben importar los recursos de Data Factory existentes del  **lienzo de creación**  de UX en un repositorio de GitHub. Active la casilla para importar los recursos de la factoría de datos en el repositorio Git asociado en formato JSON. Esta acción exporta cada recurso individualmente (es decir, los servicios vinculados y los conjuntos de datos se exportan a archivos JSON independientes). Cuando esta casilla no está activada, no se importan los recursos existentes. | Activada (valor predeterminado) |
| **Rama donde importar recursos**                       | Especifica en qué rama se importan los recursos de la factoría de datos (canalizaciones, conjuntos de datos, servicios vinculados etc.). Puede importar recursos en una de las siguientes ramas: a. Colaboración b. Crear nuevo c. Usar existente                                                                                                                                                                                                     |                    |

#### <a name="configuration-method-2-enterprise-repo-ux-authoring-canvas"></a>Método de configuración 2 (repositorio de Enterprise): lienzo de creación de la experiencia de usuario

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
