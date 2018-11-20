---
title: Creación de una plantilla base de Terraform en Azure mediante Yeoman
description: Obtenga información sobre cómo crear una plantilla base de Terraform en Azure mediante Yeoman.
services: terraform
ms.service: terraform
keywords: terraform, devops, máquina virtual, azure, yeoman
author: v-mavick
manager: jeconnoc
ms.author: v-mavick
ms.topic: tutorial
ms.date: 11/08/2018
ms.openlocfilehash: 9ef27166e84192dec81fd8f8da508785342ffefc
ms.sourcegitcommit: 02ce0fc22a71796f08a9aa20c76e2fa40eb2f10a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2018
ms.locfileid: "51288023"
---
# <a name="create-a-terraform-base-template-in-azure-using-yeoman"></a>Creación de una plantilla base de Terraform en Azure mediante Yeoman

[Terraform](https://docs.microsoft.com/azure/terraform/
) proporciona una forma de crear fácilmente infraestructura en Azure. [Yeoman](http://yeoman.io/) facilita enormemente el trabajo del desarrollador de módulos consistente en la creación de módulos de Terraform al mismo tiempo que proporciona un marco de *procedimientos recomendados* excelente.

En este artículo, aprenderá a usar el generador de módulos de Yeoman para crear una plantilla de Terraform básica. A continuación, aprenderá a probar la nueva plantilla de Terraform mediante dos métodos diferentes:

- Ejecutar el módulo de Terraform mediante un archivo de Docker que se crea en este artículo.
- Ejecutar el módulo de Terraform de forma nativa en Azure Cloud Shell.

## <a name="prerequisites"></a>Requisitos previos

- **Suscripción de Azure**: si no tiene una suscripción a Azure, cree una [cuenta gratuita de Azure](https://azure.microsoft.com/free/) antes de empezar.
- **Visual Studio Code**: usaremos [Visual Studio Code](https://www.bing.com/search?q=visual+studio+code+download&form=EDGSPH&mkt=en-us&httpsmsn=1&refig=dffc817cbc4f4cb4b132a8e702cc19a3&sp=3&ghc=1&qs=LS&pq=visual+studio+code&sk=LS1&sc=8-18&cvid=dffc817cbc4f4cb4b132a8e702cc19a3&cc=US&setlang=en-US) para examinar los archivos creados por el generador de Yeoman. Sin embargo, puede usar el editor de código que elija.
- **Terraform**: necesitará una instalación de [Terraform](https://docs.microsoft.com/azure/virtual-machines/linux/terraform-install-configure ) para ejecutar el módulo creado por Yeoman.
- **Docker**: usaremos [Docker](https://www.docker.com/get-started) para ejecutar el módulo creado por el generador de Yeoman (si lo prefiere, puede usar Ruby en lugar de Docker para ejecutar el módulo de ejemplo).
- **Lenguaje de programación Go**: necesitará una instalación de [Go](https://golang.org/), ya que los casos de prueba generados por Yeoman se escriben en Go.

>[!NOTE]
>La mayoría de los procedimientos de este tutorial implican entradas de línea de comandos. Los pasos descritos aquí se aplican a todos los sistemas operativos y herramientas de línea de comandos. En nuestros ejemplos, hemos elegido usar PowerShell para el entorno local y Bash de Git para el entorno de Cloud Shell.

## <a name="prepare-your-environment"></a>Preparación del entorno

### <a name="install-nodejs"></a>Instalación de Node.js

Para usar Terraform en Cloud Shell, deberá [instalar Node.js](https://nodejs.org/en/download/) 6.0 o posterior.

>[!NOTE]
>Para comprobar si se ha instalado Node.js, abra una ventana de terminal y escriba `node --version`.

### <a name="install-yeoman"></a>Instalación de Yeoman

Desde un símbolo del sistema, escriba `npm install -g yo`

![Instalación de Yeoman](media/terraform-vscode-module-generator/ymg-npm-install-yo.png)

### <a name="install-the-yeoman-template-for-terraform-module"></a>Instalar la plantilla de Yeoman para el módulo de Terraform

Desde un símbolo del sistema, escriba `npm install -g generator-az-terra-module`.

![Instalar generator-az-terra-module](media/terraform-vscode-module-generator/ymg-pm-install-generator-module.png)

>[!NOTE]
>Para comprobar si se ha instalado Yeoman, desde una ventana de terminal, escriba `yo --version`.

### <a name="create-an-empty-folder-to-hold-the-yeoman-generated-module"></a>Crear una carpeta vacía para incluir el módulo generado por Yeoman

La plantilla de Yeoman genera archivos en el **directorio actual**. Por este motivo, debe crear un directorio.

>[!Note]
>Es necesario poner este directorio vacío en $GOPATH/src. Encontrará instrucciones [aquí](https://github.com/golang/go/wiki/SettingGOPATH) para realizar esto.

Desde un símbolo del sistema:

1. Vaya al directorio principal que desea que incluya el directorio nuevo y vacío que estamos a punto de crear.
1. Escriba `mkdir <new-directory-name>`.

    >[!NOTE]
    >Reemplace <new-directory-name> por el nombre de su nuevo directorio. En este ejemplo, asignamos al nuevo directorio el nombre `GeneratorDocSample`.

    ![mkdir](media/terraform-vscode-module-generator/ymg-mkdir-GeneratorDocSample.png)

1. Vaya al nuevo directorio escribiendo `cd <new directory's name>` y, a continuación, presionando **entrar**.

    ![Vaya a su nuevo directorio](media/terraform-vscode-module-generator/ymg-cd-GeneratorDocSample.png)

    >[!NOTE]
    >Para asegurarse de que este directorio está vacío, escriba `ls`. No debería incluirse ningún archivo en el resultado que se obtiene de este comando.

## <a name="create-a-base-module-template"></a>Crear una plantilla de módulo básica

Desde un símbolo del sistema:

1. Escriba `yo az-terra-module`.

1. Siga las instrucciones en pantalla para proporcionar la siguiente información:

    - *Nombre de proyecto del módulo de Terraform*

        ![Nombre de proyecto](media/terraform-vscode-module-generator/ymg-project-name.png)       

        >[!NOTE]
        >En este ejemplo, escribimos `doc-sample-module`.

    - *¿Desea incluir el archivo de imagen de Docker?*

        ![¿Incluir el archivo de imagen de Docker?](media/terraform-vscode-module-generator/ymg-include-docker-image-file.png) 

        >[!NOTE]
        >Escriba `y`. Si selecciona **n**, el código de módulo generado solo admitirá la ejecución en modo nativo.

3. Escriba `ls` para ver los archivos resultantes que se crean.

    ![Incluir los archivos creados](media/terraform-vscode-module-generator/ymg-ls-GeneratorDocSample-files.png)

## <a name="review-the-generated-module-code"></a>Revisar el código de módulo generado

1. Iniciar Visual Studio Code

1. En la barra de menús, seleccione **Archivo > Abrir carpeta**, además de la carpeta que creó.

    ![Visual Studio Code](media/terraform-vscode-module-generator/ymg-open-in-vscode.png)

Echemos un vistazo a algunos de los archivos que creó el generador de módulos de Yeoman.

>[!Note]
>En este artículo usaremos los archivos main.tf, variables.tf y outputs.tf creados por el generador de módulos de Yeoman. Sin embargo, al crear sus propios módulos, editaría estos archivos para adaptarse a la funcionalidad de su módulo de Terraform. Para una explicación más detallada de estos archivos y su uso, consulte [Terratest in Terraform Modules](https://mseng.visualstudio.com/VSJava/_git/Terraform?path=%2FTerratest%20Introduction.md&version=GBmaster) (Terratest en los módulos de Terraform).

### <a name="maintf"></a>main.tf

Defina un módulo llamado *random-shuffle*. La entrada es una *string_list*. El resultado es el recuento de las permutaciones.

### <a name="variablestf"></a>variables.tf

Define las variables de entrada y salida usadas por el módulo.

### <a name="outputstf"></a>outputs.tf

Define lo que genera el módulo. Aquí, es el valor devuelto por **random_shuffle**, que es un módulo de Terraform integrado.

### <a name="rakefile"></a>Rakefile

Define los pasos de compilación. Estos pasos incluyen:

- **compilación**: valida el formato del archivo main.tf.
- **unidad**: el esqueleto del módulo generado no incluye el código para una prueba unitaria. Si desea especificar un escenario de prueba unitaria, agregaría ese código aquí.
- **e2e**: ejecuta una prueba de un extremo a otro del módulo.

### <a name="test"></a>test

- Los casos de prueba están escritos en Go.
- Todos los códigos de la prueba son pruebas de un extremo a otro.
- Las pruebas de un extremo a otro intentan usar Terraform para aprovisionar todos los elementos definidos en el **accesorio** y, a continuación, comparan el resultado del código **template_output.go** con los valores esperados predefinidos.
- **Gopkg.lock** y **Gopkg.toml**: definen sus dependencias. 

## <a name="test-your-new-terraform-module-using-a-docker-file"></a>Prueba del nuevo módulo de Terraform mediante un archivo de Docker

>[!NOTE]
>En nuestro ejemplo, ejecutamos el módulo como módulo local, sin tocar Azure realmente.

### <a name="confirm-docker-is-installed-and-running"></a>Confirmar que Docker está instalado y en ejecución

Desde un símbolo del sistema, escriba `docker version`.

![Versión de Docker](media/terraform-vscode-module-generator/ymg-docker-version.png)

El resultado que se obtiene confirma que Docker está instalado.

Para confirmar que Docker se está ejecutando realmente, escriba `docker info`.

![Información de Docker](media/terraform-vscode-module-generator/ymg-docker-info.png)

### <a name="set-up-a-docker-container"></a>Configurar un contenedor de Docker

1. Desde un símbolo del sistema, escriba

    `docker build --build-arg BUILD_ARM_SUBSCRIPTION_ID= --build-arg BUILD_ARM_CLIENT_ID= --build-arg BUILD_ARM_CLIENT_SECRET= --build-arg BUILD_ARM_TENANT_ID= -t terra-mod-example .`.

    Se mostrará el mensaje **Successfully built** (La compilación se realizó correctamente).

    ![Successfully built (La compilación se realizó correctamente)](media/terraform-vscode-module-generator/ymg-successfully-built.png)

1. En el símbolo del sistema, escriba `docker image ls`.

    Verá su módulo recién creado, *terra-mod-example*, incluido.

    ![Resultados del repositorio](media/terraform-vscode-module-generator/ymg-repository-results.png)

    >[!NOTE]
    >El nombre del módulo, *terra-mod-example*, se especificó en el comando que escribió en el paso 1, más arriba.

1. Escriba `docker run -it terra-mod-example /bin/sh`.

    Ahora se ejecuta en Docker y puede incluir el archivo escribiendo `ls`.

    ![Incluir el archivo de Docker](media/terraform-vscode-module-generator/ymg-list-docker-file.png)

### <a name="build-the-module"></a>Compilación del módulo

1. Escriba `bundle install`.

    Espere a que aparezca el mensaje **Bundle complete** (Se completó la agrupación) y, a continuación, continúe con el siguiente paso.

1. Escriba `rake build`.

    ![Amontonar la compilación](media/terraform-vscode-module-generator/ymg-rake-build.png)

### <a name="run-the-end-to-end-test"></a>Ejecución de la prueba de un extremo a otro

1. Escriba `rake e2e`.

1. Poco después, aparecerá el mensaje **PASS** (Sin errores).

    ![PASS (Sin errores)](media/terraform-vscode-module-generator/ymg-pass.png)

1. Escriba `exit` para finalizar la prueba de un extremo a otro y salir del entorno de Docker.

## <a name="use-yeoman-generator-to-create-and-test-a-module-in-cloud-shell"></a>Uso del generador Yeoman para crear y probar un módulo en Cloud Shell

En la sección anterior ha aprendido cómo probar un módulo de Terraform mediante un archivo de Docker. En esta sección, usará el generador de Yeoman para crear y probar un módulo en Cloud Shell.

El uso de Cloud Shell en lugar de usar un archivo de Docker simplifica en gran medida el proceso. Con Cloud Shell:

- No es necesario instalar Node.js
- No es necesario instalar Yeoman
- No es necesario instalar Terraform

Todos estos elementos ya están instalados en Cloud Shell.

### <a name="start-a-cloud-shell-session"></a>Inicio de una sesión de Cloud Shell

1. Inicie una sesión de Azure Cloud Shell mediante [Azure Portal](https:/portal.azure.com/), [shell.azure.com](https://shell.azure.com) o la [aplicación móvil de Azure](https://azure.microsoft.com/features/azure-portal/mobile-app/).

1. Se abre la página de **bienvenida a Azure Cloud Shell**. Seleccione **Bash (Linux)**. (No se admite Power Shell).

    ![Bienvenido a Azure Cloud Shell](media/terraform-vscode-module-generator/ymg-welcome-to-azure-cloud-shell.png)

    >[!NOTE]
    >En este ejemplo, se seleccionó Bash (Linux).

1. Si no ha configurado una cuenta de almacenamiento de Azure, aparecerá la pantalla siguiente. Seleccione **Crear almacenamiento**.

    ![No tiene ningún almacenamiento montado](media/terraform-vscode-module-generator/ymg-you-have-no-storage-mounted.png)

1. Azure Cloud Shell se inicia en el shell que seleccionó anteriormente y muestra la información de la unidad en la nube que acaba de crear para usted.

    ![Se ha creado su unidad en la nube.](media/terraform-vscode-module-generator/ymg-your-cloud-drive-has-been-created-in.png)

### <a name="prepare-a-folder-to-hold-your-terraform-module"></a>Preparación de una carpeta para almacenar el módulo de Terraform

1. En este momento, Cloud Shell ya habrá configurado GOPATH en las variables de entorno. Para ver la ruta de acceso, escriba `go env`.

1. Cree la carpeta $GOPATH, si aún no existe: escriba `mkdir ~/go`.

1. Cree una carpeta dentro de la carpeta $GOPATH: escriba `mkdir ~/go/src`. Esta carpeta se utilizará para almacenar y organizar las distintas carpetas del proyecto que pueda crear, como la carpeta <nombre-de-módulo> que se creará en el paso siguiente.

1. Cree una carpeta para almacenar el módulo de Terraform: escriba `mkdir ~/go/src/<your-module-name>`.

    >[!NOTE]
    >En este ejemplo, elegimos `my-module-name` para el nombre de carpeta.

1. Vaya a la carpeta del módulo: escriba `cd ~/go/src/<your-module-name>`

### <a name="create-and-test-your-terraform-module"></a>Creación y prueba del módulo de Terraform

1. Escriba `yo az-terra-module` y siga las instrucciones del asistente.

    >[!NOTE]
    >Cuando se le pregunte si desea crear los archivos de Docker, puede escribir `N`.

1. Escriba `bundle install` para instalar las dependencias.

    Espere a que aparezca el mensaje **Bundle complete** (Se completó la agrupación) y, a continuación, continúe con el siguiente paso.

1. Escriba `rake build` para compilar el módulo.

    ![Amontonar la compilación](media/terraform-vscode-module-generator/ymg-rake-build.png)

1. Escriba `rake e2e` para ejecutar la prueba de un extremo a otro.

1. Poco después, aparecerá el mensaje **PASS** (Sin errores).

    ![PASS (Sin errores)](media/terraform-vscode-module-generator/ymg-pass.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Instale y use la extensión de Visual Studio Code para Azure Terraform.](https://docs.microsoft.com/azure/terraform/terraform-vscode-extension)
