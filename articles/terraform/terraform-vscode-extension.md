---
title: Extensión de Azure Terraform VS Code | Microsoft Docs
description: En este artículo, obtendrá información sobre cómo instalar y usar la extensión de Terraform en Visual Studio Code.
keywords: terraform, devops, máquina virtual, azure
author: v-mavick
ms.author: v-mavick
ms.date: 08/14/2018
ms.topic: article
ms.prod: vs-code
ms.openlocfilehash: 0c88879faae100372055479ad4edb8c36d8f557d
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2018
ms.locfileid: "40191153"
---
# <a name="azure-terraform-vs-code-extension"></a>Extensión de Azure Terraform VS Code

La extensión de Microsoft Azure Terraform Visual Studio Code (VS Code) está diseñada para aumentar la productividad de los desarrolladores al crear, probar y usar Terraform con Azure. La extensión proporciona compatibilidad con los comandos de Terraform, la visualización de gráficos de recursos y la integración de CloudShell en VS Code.

## <a name="what-you-do"></a>Qué debe hacer

- Instale el ejecutable de HashiCorp Terraform de código abierto en el equipo.
- Instale la extensión de Terraform VS Code para Azure en la instalación local de VS Code.

## <a name="what-you-learn"></a>Conocimientos que adquirirá

En este tutorial, aprenderá:

- Cómo Terraform puede automatizar y simplificar el aprovisionamiento de servicios de Azure.
- Cómo instalar y usar la extensión de Microsoft Terraform VS Code para servicios de Azure.
- Cómo usar VS Code para escribir, planear y ejecutar los planes de Terraform.

## <a name="what-you-need"></a>Lo que necesita

- Un equipo con Windows 10, Linux o macOS 10.10 o versiones posteriores.
- [Visual Studio Code](https://www.bing.com/search?q=visual+studio+code+download&form=EDGSPH&mkt=en-us&httpsmsn=1&refig=dffc817cbc4f4cb4b132a8e702cc19a3&sp=3&ghc=1&qs=LS&pq=visual+studio+code&sk=LS1&sc=8-18&cvid=dffc817cbc4f4cb4b132a8e702cc19a3&cc=US&setlang=en-US)
- Una suscripción de Azure activa. [Active una cuenta de Microsoft Azure de prueba de 30 días gratis](https://azure.microsoft.com/free/).
- Una instalación de la herramienta de código abierto [Terraform](https://www.terraform.io/) en el equipo local.
  
## <a name="prepare-your-dev-environment"></a>Preparación del entorno de desarrollo

### <a name="install-git"></a>Instalar Git

Para completar los ejercicios del artículo, deberá [instalar Git](https://git-scm.com/).

### <a name="install-hashicorp-terraform"></a>Instalar HashiCorp Terraform

Siga las instrucciones de la página web [Install Terraform](https://www.terraform.io/intro/getting-started/install.html) (Instalar Terraform) de HashiCorp, en las que se incluye:

- Descarga de Terraform
- Instalación de Terraform
- Comprobación de que Terraform está instalado correctamente

>[!Tip]
>Asegúrese de seguir las instrucciones sobre la configuración de la variable del sistema PATH.

### <a name="install-nodejs"></a>Instalación de Node.js

Para usar Terraform en Cloud Shell, deberá [instalar Node.js](https://nodejs.org/) 6.0 o posterior.

>[!NOTE]
>Para comprobar si se ha instalado Node.js, abra una ventana de terminal y escriba: `node -v`

### <a name="install-graphviz"></a>Instalar GraphViz

Para usar la función de visualización de Terraform, deberá [instalar GraphViz](http://graphviz.org/).

>[!NOTE]
>Para comprobar si se ha instalado GraphViz, abra una ventana de terminal y escriba: `dot -V`

### <a name="install-the-azure-terraform-vs-code-extension"></a>Instalar la extensión de Azure Terraform VS Code:

1. Inicie VS Code.
2. Haga clic en el icono **Extensiones**.

    ![Botón Extensiones](media/terraform-vscode-extension/tf-vscode-extensions-button.png)

3. Use el cuadro de texto **Search Extensions in Marketplace** (Buscar extensiones en Marketplace) para buscar la extensión de Azure Terraform:

    ![Buscar extensiones de VS Code en Marketplace](media/terraform-vscode-extension/tf-search-extensions.png)

4. Haga clic en **Instalar**.

    >[!NOTE]
    >Al hacer clic en **Instalar** para la extensión de Azure Terraform, VS Code instalará automáticamente la extensión de la cuenta de Azure. La cuenta de Azure es un archivo de dependencia para la extensión de Azure Terraform, que se usa para realizar las autenticaciones de suscripción a Azure y extensiones de código relacionado con Azure.

#### <a name="verify-the-terraform-extension-is-installed-in-visual-studio-code"></a>Compruebe que está instalada la extensión de Terraform en Visual Studio Code

1. Haga clic en el icono Extensiones.
2. Escriba `@installed` en el cuadro de texto de búsqueda.

    ![Extensiones instaladas](media/terraform-vscode-extension/tf-installed-extensions.png)

La extensión de Azure Terraform aparecerá en la lista de extensiones instaladas.

![Extensiones instaladas de Terraform](media/terraform-vscode-extension/tf-installed-terraform-extension-button.png)

Ahora puede ejecutar todos los comandos admitidos de Terraform en su entorno de Cloud Shell desde dentro de VS Code.

## <a name="exercise-1-basic-terraform-commands-walk-through"></a>Ejercicio 1: Tutorial de comandos básicos de Terraform

En este ejercicio, se crea y ejecuta un archivo de configuración básica de Terraform que aprovisiona un nuevo grupo de recursos de Azure.

### <a name="prepare-a-test-plan-file"></a>Preparar un archivo de plan de pruebas

1. En VS Code, seleccione **Archivo > Nuevo archivo** en la barra de menús.
2. Navegue hasta [azurerm_resource_group](https://www.terraform.io/docs/providers/azurerm/r/resource_group.html#) y copie el código en el bloque de código **Ejemplo de uso**:
3. Pegue el código copiado en el nuevo archivo que creó en VS Code.

    ![Pegar el código de ejemplo de uso](media/terraform-vscode-extension/tf-paste-example-usage-code.png)

    >[!NOTE]
    >Puede cambiar el valor de **nombre** del grupo de recursos, pero debe ser único para su suscripción a Azure.

4. En la barra de menús, seleccione **Archivo > Guardar como**.
5. En el cuadro de diálogo **Guardar como**, navegue hasta una ubicación de su elección y, a continuación, haga clic en **Nueva carpeta**. (Cambie el nombre de la nueva carpeta por otro que sea más descriptivo que *Nueva carpeta*).

    >[!NOTE]
    >En este ejemplo, la carpeta se denomina TERRAFORM-TEST-PLAN.

6. Asegúrese de que la nueva carpeta este resaltada (seleccionada) y haga clic en **Abrir**.
7. En el cuadro de diálogo **Guardar como**, cambie el nombre predeterminado del archivo por *main.tf*.

    ![Guardar como main.tf](media/terraform-vscode-extension/tf-save-as-main.png)

8. Haga clic en **Save**(Guardar).
- En la barra de menús, seleccione **Archivo > Abrir carpeta**. Navegue hasta la nueva carpeta que creó y selecciónela.

### <a name="run-terraform-init-command"></a>Ejecutar el comando *init* de Terraform

1. Inicie Visual Studio Code.
2. En la barra de menús de VS Code, seleccione **Archivo > Abrir carpeta...** y busque y seleccione su archivo *main.tf*.

    ![Archivo main.tf](media/terraform-vscode-extension/tf-main-tf.png)

3. En la barra de menús, seleccione **Vista > Paleta de comandos... > Azure Terraform: Init**.
4. Transcurridos unos instantes, cuando se le pida *Do you want to open Cloud Shell?* (¿Quiere abrir Cloud Shell?), Haga clic en **OK**.

    ![¿Quiere abrir Cloud Shell?](media/terraform-vscode-extension/tf-do-you-want-to-open-cloud-shell.png)

5. La primera vez que inicie Cloud Shell desde una carpeta nueva, se le pedirá que configure la aplicación web. Haga clic en **Abrir**.

    ![Primer inicio de Cloud Shell](media/terraform-vscode-extension/tf-first-launch-of-cloud-shell.png)

6. Se abre la página de bienvenida a Azure Cloud Shell. Seleccione Bash o PowerShell.

    ![Bienvenido a Azure Cloud Shell](media/terraform-vscode-extension/tf-welcome-to-azure-cloud-shell.png)

    >[!NOTE]
    >En este ejemplo, se seleccionó Bash (Linux).

7. Si no ha configurado una cuenta de almacenamiento de Azure, aparecerá la pantalla siguiente. Haga clic en **Create storage** (Crear almacenamiento).

    ![No tiene ningún almacenamiento montado](media/terraform-vscode-extension/tf-you-have-no-storage-mounted.png)

1. Azure Cloud Shell se inicia en el shell que seleccionó anteriormente y muestra la información de la unidad en la nube que acaba de crear para usted.

    ![Se ha creado su unidad en la nube.](media/terraform-vscode-extension/tf-your-cloud-drive-has-been-created-in.png)

9. Ahora puede salir de Cloud Shell.
10. En la barra de menús, seleccione **Vista** > **Paleta de comandos** > **Azure Terraform: init**.

    ![Terraform se ha inicializado correctamente.](media/terraform-vscode-extension/tf-terraform-has-been-successfully-initialized.png)

### <a name="visualize-the-plan"></a>Visualizar el plan

Anteriormente en este tutorial, ha instalado GraphViz. Terraform puede usar GraphViz para generar una representación visual de una configuración o plan de ejecución. La extensión de Azure Terraform VS Code implementa esta característica a través del comando *visualize*.

- En la **Barra de menús**, seleccione **Vista > Paleta de comandos > Azure Terraform: Vizualize**.

    ![Visualizar el plan](media/terraform-vscode-extension/tf-graph.png)

### <a name="run-terraform-plan-command"></a>Ejecutar el comando *plan* de Terraform

El comando *plan* de Terraform se usa para comprobar si el plan de ejecución de un conjunto de cambios hará lo que está previsto.

>[!NOTE]
>El comando *plan* de Terraform no realiza cambios en los recursos reales de Azure. Para realizar realmente los cambios incluidos en el plan, se usa el comando *apply* de Terraform.

- En la barra de menús, seleccione **Vista** > **Paleta de comandos** > **Azure Terraform: plan**.

    ![Plan de Terraform](media/terraform-vscode-extension/tf-terraform-plan.png)

### <a name="run-terraform-apply-command"></a>Ejecutar el comando *apply* de Terraform

Cuando esté satisfecho con los resultados del comando *plan* de Terraform, puede ejecutar el comando *apply*.

1. En la barra de menús, seleccione **Vista** > **Paleta de comandos** > **Azure Terraform: apply**.

    ![Terraform apply](media/terraform-vscode-extension/tf-terraform-apply.png)

2. Escriba *yes*.

    ![Terraform apply yes](media/terraform-vscode-extension/tf-terraform-apply-yes.png)

### <a name="verify-your-terraform-plan-was-executed"></a>Comprobar que se ejecutó el plan de Terraform

Para ver si el nuevo grupo de recursos de Azure se creó correctamente:

1. Abra Azure Portal.
2. Seleccione **Grupos de recursos** en el panel de navegación de la izquierda.

    ![Comprobar el nuevo recurso](media/terraform-vscode-extension/tf-verify-resource-group-created.png)

El nuevo grupo de recursos debe aparecer en la columna **NOMBRE**.

>[!NOTE]
>Puede dejar abierta la ventana de Azure Portal por ahora; la usaremos en el paso siguiente.

### <a name="run-terraform-destroy-command"></a>Ejecutar el comando *destroy* de Terraform

1. En la barra de menús, seleccione **Vista** > **Paleta de comandos** > **Azure Terraform: destroy**.

    ![Terraform destroy](media/terraform-vscode-extension/tf-terraform-destroy.png)

2. Escriba *yes*.

    ![Terraform destroy yes](media/terraform-vscode-extension/tf-terraform-destroy-yes.png)

### <a name="verify-your-resource-group-was-destroyed"></a>Comprobar que se ha destruido el grupo de recursos

Para confirmar que Terraform ha destruido correctamente el nuevo grupo de recursos:

1. Haga clic en **Actualizar** en la página *Grupos de recursos* de Azure Portal.
2. Ya no se mostrará el grupo de recursos.

    ![Comprobar que se ha destruido el grupo de recursos](media/terraform-vscode-extension/tf-refresh-resource-groups-button.png)

## <a name="exercise-2-terraform-compute-module"></a>Ejercicio 2: Módulo *compute* de Terraform

En este ejercicio, obtendrá información sobre cómo cargar el módulo *compute*  de Terraform en el entorno de VS Code.

### <a name="clone-the-terraform-azurerm-compute-module"></a>Clonar el módulo terraform-azurerm-compute

1. Use [este vínculo](https://github.com/Azure/terraform-azurerm-compute) para obtener acceso al módulo Terraform Azure Rm Compute en GitHub.
2. Haga clic en **Clone or download** (Clonar o descargar).

    ![Clonar o descargar](media/terraform-vscode-extension/tf-clone-with-https.png)

    >[!NOTE]
    >En este ejemplo, la carpeta se denomina *terraform-azurerm-compute*.

### <a name="open-the-folder-in-vs-code"></a>Abra la carpeta en VS Code

1. Inicie Visual Studio Code.
2. En la **Barra de menús**, seleccione **Archivo > Abrir carpeta** y navegue hasta la carpeta que creó en el paso anterior y selecciónela.

    ![Carpeta terraform-azurerm-compute](media/terraform-vscode-extension/tf-terraform-azurerm-compute-folder.png)

### <a name="initialize-terraform"></a>Inicialización de Terraform

Antes de que pueda empezar a usar los comandos de Terraform desde dentro de VS Code, descargue los complementos para dos proveedores de Azure: aleatorio y azurerm.

1. En el panel Terminal del IDE de VS Code, escriba: `terraform init`

    ![Comando init de Terraform](media/terraform-vscode-extension/tf-terraform-init-command.png)

2. Escriba `az login` y siga las instrucciones que aparecen en pantalla.

### <a name="module-test-lint"></a>Prueba del módulo: *lint*

1. En la **Barra de menús**, seleccione **Vista > Paleta de comandos > Azure Terraform: Execute Test**.
2. En la lista de opciones de tipo de prueba, seleccione **lint**.

    ![Seleccionar el tipo de prueba](media/terraform-vscode-extension/tf-select-type-of-test-lint.png)

3. Cuando se le pida *Do you want to open CloudShell?* (¿Quiere abrir CloudShell?), haga clic en **Aceptar** y siga las instrucciones de la pantalla.

    ![¿Quiere abrir CloudShell?](media/terraform-vscode-extension/tf-do-you-want-to-open-cloudshell-small.png)

>[!NOTE]
>Cuando se ejecuta la prueba **lint** o **end to end**, Azure usa un servicio de contenedor para aprovisionar un equipo de prueba para llevar a cabo la prueba real. Por este motivo, los resultados de la prueba normalmente pueden tardar varios minutos en devolverse.

Transcurridos unos instantes, verá una lista en el panel Terminal similar a este ejemplo:

![Resultados de la prueba Lint](media/terraform-vscode-extension/tf-lint-test-results.png)

### <a name="module-test-end-to-end"></a>Prueba del módulo: *end-to-end*

1. En la **Barra de menús**, seleccione **Vista > Paleta de comandos > Azure Terraform: Execute Test**.
2. En la lista de opciones de tipo de prueba, seleccione **end to end**.

    ![Seleccionar el tipo de prueba](media/terraform-vscode-extension/tf-select-type-of-test-end-to-end.png)

3. Si se le pide *Do you want to open CloudShell?* (¿Quiere abrir CloudShell?), haga clic en **Aceptar** y siga las instrucciones de la pantalla.

    ![¿Quiere abrir CloudShell?](media/terraform-vscode-extension/tf-do-you-want-to-open-cloudshell-small.png)

>[!NOTE]
>Cuando se ejecuta la prueba **lint** o **end to end**, Azure usa un servicio de contenedor para aprovisionar un equipo de prueba para llevar a cabo la prueba real. Por este motivo, los resultados de la prueba normalmente pueden tardar varios minutos en devolverse.

Transcurridos unos instantes, verá una lista en el panel Terminal similar a este ejemplo:

![Resultados de la prueba end-to-end](media/terraform-vscode-extension/tf-end-to-end-test-results.png)

## <a name="next-steps"></a>Pasos siguientes

Ha visto algunas de las formas en que Terraform puede simplificar el aprovisionamiento de servicios de Azure desde Visual Studio Code. Ahora, es posible que desee revisar algunos de estos recursos:
- El [registro de módulos de Terraform](https://registry.terraform.io/) enumera todos los módulos de Terraform disponibles para Azure y otros proveedores compatibles.

Para cada uno de estos módulos, se proporciona la siguiente información:

- Una descripción de las funciones generales del módulo y sus características
- Un ejemplo de uso
- Configuraciones de prueba, que le enseñan a compilar, ejecutar y probar cada módulo en el equipo de desarrollo local
- Un Dockerfile para que pueda compilar y ejecutar localmente un entorno de desarrollo de módulo.
