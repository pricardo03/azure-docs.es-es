---
title: Terraform de Azure frente al generador de módulos de código
description: Aprenda a usar Yeoman para crear una plantilla básica de Terraform.
services: terraform
ms.service: terraform
keywords: terraform, devops, máquina virtual, azure, yeoman
author: v-mavick
manager: jeconnoc
ms.author: v-mavick
ms.topic: tutorial
ms.date: 09/12/2018
ms.openlocfilehash: 513b123c44cf2cd37cf81a91e0d2da9599eb1fcd
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47396354"
---
# <a name="create-a-terraform-base-template-using-yeoman"></a>Creación de una plantilla base de Terraform mediante Yeoman

[Terraform](https://docs.microsoft.com/azure/terraform/
) proporciona una forma de crear fácilmente infraestructura en Azure. [Yeoman](http://yeoman.io/) facilita enormemente el trabajo del desarrollador de módulos consistente en la creación de módulos de Terraform al mismo tiempo que proporciona un marco de *procedimientos recomendados* excelente.

En este artículo, aprenderá a usar el generador de módulos de Yeoman para crear una plantilla de Terraform básica.

## <a name="prerequisites"></a>Requisitos previos

- Un equipo con Windows 10, Linux o macOS 10.10 o versiones posteriores.
- **Suscripción de Azure**: si no tiene una suscripción a Azure, cree una [cuenta gratuita de Azure](https://azure.microsoft.com/free/) antes de empezar.
- **Visual Studio Code**: usaremos [Visual Studio Code](https://www.bing.com/search?q=visual+studio+code+download&form=EDGSPH&mkt=en-us&httpsmsn=1&refig=dffc817cbc4f4cb4b132a8e702cc19a3&sp=3&ghc=1&qs=LS&pq=visual+studio+code&sk=LS1&sc=8-18&cvid=dffc817cbc4f4cb4b132a8e702cc19a3&cc=US&setlang=en-US) para examinar los archivos creados por el generador de Yeoman. Sin embargo, puede usar el editor de código que elija.
- **Terraform**: necesitará una instalación de [Terraform](https://docs.microsoft.com/azure/virtual-machines/linux/terraform-install-configure ) para ejecutar el módulo creado por Yeoman.
- **Docker**: usaremos [Docker](https://www.docker.com/get-started) para ejecutar el módulo creado por el generador de Yeoman (si lo prefiere, puede usar Ruby en lugar de Docker para ejecutar el módulo de ejemplo).
- **Lenguaje de programación Go**: necesitará una instalación de [Go](https://golang.org/), ya que los casos de prueba generados por Yeoman se escriben en Go.

>[!NOTE]
>La mayoría de los procedimientos de este tutorial implican entradas de línea de comandos. Los pasos descritos aquí se aplican a todos los sistemas operativos y herramientas de línea de comandos. En nuestros ejemplos hemos elegido usar PowerShell. Sin embargo, puede usar cualquiera de estas alternativas: GIT Bash, símbolos del sistema de Windows o comandos de línea de comandos de macOS o Linux.

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
        >Escriba `y`. Si selecciona **n**, el código de módulo generado admitirá la ejecución en modo nativo solamente.

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

## <a name="test-the-module-using-docker"></a>Probar el módulo con Docker

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

1. Escriba `bundle install`.

    Espere a que aparezca el mensaje **Bundle complete** (Se completó la agrupación) y, a continuación, continúe con el siguiente paso.

1. Escriba `rake build`.

    ![Amontonar la compilación](media/terraform-vscode-module-generator/ymg-rake-build.png)

### <a name="perform-the-end-to-end-test"></a>Realizar la prueba de un extremo a otro

1. Escriba `rake e2e`.

1. Poco después, aparecerá el mensaje **PASS** (Sin errores).

    ![PASS (Sin errores)](media/terraform-vscode-module-generator/ymg-pass.png)

1. Escriba `exit` para completar la prueba de un extremo a otro.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Instale y use la extensión de Visual Studio Code para Azure Terraform.](https://docs.microsoft.com/azure/terraform/terraform-vscode-extension)
