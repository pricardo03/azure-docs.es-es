---
title: Implementación de la máquina virtual de Azure con Chef | Microsoft Docs
description: Obtenga información acerca de cómo usar chef para realizar la implementación y configuración de máquinas virtuales automatizada en Microsoft Azure
services: virtual-machines-windows
documentationcenter: ''
author: diegoviso
manager: gwallace
tags: azure-service-management,azure-resource-manager
editor: ''
ms.assetid: 0b82ca70-89ed-496d-bb49-c04ae59b4523
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 07/09/2019
ms.author: diviso
ms.openlocfilehash: 74b92c277b1d6eaa0984e55a70459bad59c2bf84
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2019
ms.locfileid: "67719279"
---
# <a name="automating-azure-virtual-machine-deployment-with-chef"></a>Automatización de la implementación de la máquina virtual de Azure con Chef

Chef es una fantástica herramienta para ofrecer automatización y las configuraciones de estado que desee.

Con la versión de API de nube más reciente, Chef proporciona una perfecta integración con Azure, lo que ofrece la posibilidad de aprovisionar e implementar los estados de configuración mediante un único comando.

En este artículo, se configura un entorno de Chef para aprovisionar máquinas virtuales de Azure y se le guía por la creación de una directiva o "libro de recetas", y su posterior implementación en una máquina virtual de Azure.

## <a name="chef-basics"></a>Conceptos básicos de Chef
Antes de comenzar, [revise los conceptos básicos de Chef](https://www.chef.io/chef).

El diagrama siguiente muestra la arquitectura Chef de alto nivel.

![][2]

La arquitectura de Chef tiene tres componentes principales: el servidor Chef, el cliente Chef (nodo) y la estación de trabajo Chef.

El servidor Chef es el punto de administración y hay dos opciones para el servidor Chef: una solución hospedada o una solución local.

El cliente Chef (nodo) es el agente que se encuentra en los servidores que está administrando.

La estación de trabajo Chef es el nombre tanto de la estación de trabajo de administración en la que se crean directivas y se ejecutan comandos de administración como del paquete de software de las herramientas de Chef.

Por lo general, verá _su estación de trabajo_ como la ubicación en la que puede realizar acciones y la _estación de trabajo Chef_ como el paquete de software.
Por ejemplo, el comando de knife se descargará como parte de la _estación de trabajo Chef_, pero los comandos de knife que administran la infraestructura se ejecutan desde _su estación de trabajo_.

Chef también usa los conceptos de "libros de recetas" y "recetas", que son realmente las directivas que definimos y aplicamos a los servidores.

## <a name="preparing-your-workstation"></a>Preparación de la estación de trabajo

En primer lugar, prepare la estación de trabajo mediante la creación de un directorio que almacene archivos de configuración de Chef y libros de recetas.

Cree un directorio denominado C:\Chef.

Descargue e instale la última versión de la [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) en su estación de trabajo.

## <a name="configure-azure-service-principal"></a>Configuración de una entidad de servicio de Azure

Explicándolo de forma sencilla, una entidad de servicio de Azure es una cuenta de servicio.   Usaremos una entidad de servicio para ayudarnos a crear recursos de Azure desde nuestra estación de trabajo Chef.  Para crear la entidad de servicio correspondiente con los permisos necesarios, tenemos que ejecutar los siguientes comandos en PowerShell:
 
```powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<yourSubscriptionName>"
$myApplication = New-AzureRmADApplication -DisplayName "automation-app" -HomePage "https://chef-automation-test.com" -IdentifierUris "https://chef-automation-test.com" -Password "#1234p$wdchef19"
New-AzureRmADServicePrincipal -ApplicationId $myApplication.ApplicationId
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $myApplication.ApplicationId
```

Apunte los valores de SubscriptionID, TenantID, ClientID y el secreto de cliente (la contraseña que ha establecido antes), los necesitará más adelante. 

## <a name="setup-chef-server"></a>Configuración del servidor Chef

En esta guía se da por supuesto que se registrará para Hosted Chef.

Si no usa un servidor Chef, puede:

* Registrarse para obtener [Hosted Chef](https://manage.chef.io/signup), que es la forma más rápida de empezar a usar Chef.
* Instalar un servidor Chef independiente en el equipo con Linux siguiendo las [instrucciones de instalación](https://docs.chef.io/install_server.html) de [Chef Docs](https://docs.chef.io/).

### <a name="creating-a-hosted-chef-account"></a>Creación de una cuenta de Hosted Chef

Para disfrutar de una cuenta de Hosted Chef, regístrese [aquí](https://manage.chef.io/signup).

Durante el proceso de registro, se le pedirá que cree una organización.

![][3]

Cuando se cree la organización, descargue el starter kit.

![][4]

> [!NOTE]
> Si recibe un mensaje que le advierte que se restablecerán las claves, está bien continuar como si no tuviéramos ninguna infraestructura existente configurada todavía.
>

El archivo zip de Starter Kit contiene los archivos de configuración y la clave de usuario de la organización que están en el directorio `.chef`.

`organization-validator.pem` debe descargarse por separado, porque es una clave privada y las claves privadas no se deben almacenar en el servidor Chef. En [Chef Manage](https://manage.chef.io/), vaya a la sección de administración y seleccione "Reset Validation Key" (Restablecer clave de validación); se le proporcionará un archivo que se descarga por separado. Guarde el archivo en c:\chef.

### <a name="configuring-your-chef-workstation"></a>Configuración de la estación de trabajo Chef

Extraiga el contenido del archivo chef-starter.zip en c:\chef.

Copie todos los archivos de chef-starter\chef-repo\.chef en el directorio c:\chef.

Copie el archivo `organization-validator.pem` en c:\chef, si se guarda en c:\Downloads

El directorio debería tener ahora un aspecto similar al siguiente ejemplo.

```powershell
    Directory: C:\Users\username\chef

Mode           LastWriteTime    Length Name
----           -------------    ------ ----
d-----    12/6/2018   6:41 PM           .chef
d-----    12/6/2018   5:40 PM           chef-repo
d-----    12/6/2018   5:38 PM           cookbooks
d-----    12/6/2018   5:38 PM           roles
-a----    12/6/2018   5:38 PM       495 .gitignore
-a----    12/6/2018   5:37 PM      1678 azuredocs-validator.pem
-a----    12/6/2018   5:38 PM      1674 user.pem
-a----    12/6/2018   5:53 PM       414 knife.rb
-a----    12/6/2018   5:38 PM      2341 README.md
```

Ahora debería tener cinco archivos y cuatro directorios (incluido el directorio chef-repo vacío) en la raíz de c:\chef.

### <a name="edit-kniferb"></a>Edición de knife.rb

Los archivos PEM contienen sus claves privadas de organización y administración para la comunicación y el archivo knife.rb contiene la configuración de knife. Tendremos que editar el archivo knife.rb.

Abra el archivo knife.rb en el editor que prefiera. El archivo sin modificar debe ser similar a este:

```rb
current_dir = File.dirname(__FILE__)
log_level           :info
log_location        STDOUT
node_name           "mynode"
client_key          "#{current_dir}/user.pem"
chef_server_url     "https://api.chef.io/organizations/myorg"
cookbook_path       ["#{current_dir}/cookbooks"]
```

Agregue la siguiente información a knife.rb:

validation_client_name "validador-de-la-organización"

validation_key           "#{directorio_actual}/myorg.pem"

knife[:azure_tenant_id] =         "0000000-1111-aaaa-bbbb-222222222222"

knife[:azure_subscription_id] =   "11111111-bbbbb-cccc-1111-222222222222"

knife[:azure_client_id] =         "11111111-bbbbb-cccc-1111-2222222222222"

knife[:azure_client_secret] =     "#1234p$wdchef19"


Estas líneas asegurarán que Knife haga referencia al directorio de cookbooks en c:\chef\cookbooks y que también use la entidad de servicio de Azure que ha creado durante las operaciones de Azure.

Ahora el archivo knife.rb debería ser ahora similar al del siguiente ejemplo:

![][14]

<!--- Giant problem with this section: Chef 12 uses a config.rb instead of knife.rb
// However, the starter kit hasn't been updated
// So, I don't think this will even work on the modern Chef -->

<!--- update image [6] knife.rb -->

```rb
current_dir = File.dirname(__FILE__)
log_level                :info
log_location             STDOUT
node_name                "myorg"
client_key               "#{current_dir}/myorg.pem"
validation_client_name   "myorg-validator"
validation_key           "#{current_dir}/myorg-validator.pem"
chef_server_url          "https://api.chef.io/organizations/myorg"
cookbook_path            ["#{current_dir}/../cookbooks"]
knife[:azure_tenant_id] = "0000000-1111-aaaa-bbbb-222222222222"
knife[:azure_subscription_id] = "11111111-bbbbb-cccc-1111-222222222222"
knife[:azure_client_id] = "11111111-bbbbb-cccc-1111-2222222222222"
knife[:azure_client_secret] = "#1234p$wdchef19"
```

## <a name="install-chef-workstation"></a>Instalación de la estación de trabajo Chef

A continuación, [descargue e instale](https://downloads.chef.io/chef-workstation/) la estación de trabajo Chef.
Instale la estación de trabajo Chef en la ubicación predeterminada. Esta instalación puede tardar unos minutos.

En el escritorio, verá "CW PowerShell", que es un entorno que se carga con la herramienta que se necesita para interactuar con los productos de Chef. CW PowerShell permite usar nuevos comandos ad hoc, como `chef-run`, así como comandos tradicionales de la CLI de Chef, como `chef`. Consulte la versión instalada de la estación de trabajo Chef y de las herramientas de Chef con `chef -v`. También puede consultar la versión de la estación de trabajo. Para ello, debe seleccionar "About Chef Workstation" (Acerca de la estación de trabajo Chef) en la aplicación de la estación de trabajo Chef.

`chef --version` debe devolver algo similar a esto:

```
Chef Workstation: 0.4.2
  chef-run: 0.3.0
  chef-client: 15.0.300
  delivery-cli: 0.0.52 (9d07501a3b347cc687c902319d23dc32dd5fa621)
  berks: 7.0.8
  test-kitchen: 2.2.5
  inspec: 4.3.2
```

> [!NOTE]
> El orden de la ruta de acceso es importante. Si las rutas de acceso de opscode no están en el orden correcto, tendrá problemas.
>

Reinicie la estación de trabajo antes de continuar.

### <a name="install-knife-azure"></a>Instalación de Knife Azure

En este tutorial se da por supuesto que usa Azure Resource Manager para interactuar con su máquina virtual.

Instale la extensión de Knife Azure. De esta se proporciona a Knife el "complemento de Azure".

Ejecute el siguiente comando.

    chef gem install knife-azure ––pre

> [!NOTE]
> El argumento –pre garantiza que está recibiendo la versión de RC más reciente del complemento Knife Azure que ofrece acceso al conjunto más reciente de API.
>
>

Es probable que también se instalen varias dependencias al mismo tiempo.

![][8]

Para garantizar que todo esté configurado correctamente, ejecute el siguiente comando.

    knife azurerm server list

Si todo está configurado correctamente, verá una lista de imágenes de Azure disponibles en desplazamiento.

¡Enhorabuena! La estación de trabajo está configurada.

## <a name="creating-a-cookbook"></a>Crear una guía

Chef usa las guías para definir un conjunto de comandos que desea ejecutar en el cliente administrado. La creación de un libro de recetas es un proceso sencillo, solo haya que usar el comando **chef generate cookbook** para generar la plantilla del libro de recetas. Dicho libro de recetas es para un servidor web que implementa IIS automáticamente.

En el directorio C:\Chef, ejecute el siguiente comando.

    chef generate cookbook webserver

Este comando genera un conjunto de archivos en el directorio C:\Chef\cookbooks\webserver. A continuación, defina el conjunto de comandos que el cliente Chef ejecutara en la máquina virtual administrada.

Los comandos se almacenan en el archivo default.rb. En dicho archivo, defina un conjunto de comandos que instala IIS, inicia IIS y copia un archivo de plantilla en la carpeta wwwroot.

Modifique el archivo C:\chef\cookbooks\webserver\recipes\default.rb y agregue las siguientes líneas.

    powershell_script 'Install IIS' do
         action :run
         code 'add-windowsfeature Web-Server'
    end

    service 'w3svc' do
         action [ :enable, :start ]
    end

    template 'c:\inetpub\wwwroot\Default.htm' do
         source 'Default.htm.erb'
         rights :read, 'Everyone'
    end

Cuando haya terminado, guarde el archivo.

## <a name="creating-a-template"></a>Crear una plantilla
En este paso, generará un archivo de plantilla que se va a utilizar como la página default.html.

Ejecute el siguiente comando para generar la plantilla:

    chef generate template webserver Default.htm

Vaya al archivo `C:\chef\cookbooks\webserver\templates\default\Default.htm.erb`. Edite el archivo agregando el código HTML simple "Hello World" y guarde el archivo.

## <a name="upload-the-cookbook-to-the-chef-server"></a>Cargar la guía en el servidor Chef
En este paso se realiza una copia de la guía que se ha creado en el equipo local y se cargamos en el servidor hospedado de Chef. Una vez que se carga, la guía aparece en la pestaña **Policy** (Directiva).

    knife cookbook upload webserver

![][9]

## <a name="deploy-a-virtual-machine-with-knife-azure"></a>Implementar una máquina virtual con Knife Azure
Implemente una máquina virtual de Azure y aplique la guía "Webserver", que instalará el servicio web de IIS y la página web predeterminada.

Para ello, use el comando **knife azurerm server create**.

A continuación aparece un ejemplo del comando.

    knife azurerm server create `
    --azure-resource-group-name rg-chefdeployment `
    --azure-storage-account store `
    --azure-vm-name chefvm `
    --azure-vm-size 'Standard_DS2_v2' `
    --azure-service-location 'westus' `
    --azure-image-reference-offer 'WindowsServer' `
    --azure-image-reference-publisher 'MicrosoftWindowsServer' `
    --azure-image-reference-sku '2016-Datacenter' `
    --azure-image-reference-version 'latest' `
    -x myuser -P myPassword123 `
    --tcp-endpoints '80,3389' `
    --chef-daemon-interval 1 `
    -r "recipe[webserver]"


En el ejemplo anterior se creará una máquina virtual Standard_DS2_v2 con Windows Server 2016 instalado en la región Oeste de EE. UU. Sustituir sus variables concretas y ejecutar.

> [!NOTE]
> Mediante la línea de comandos, también estoy automatizando mis reglas de filtro de red de punto de conexión mediante el parámetro –tcp-endpoints. He abierto los puertos 80 y 3389 para proporcionar acceso a la página web y la sesión de RDP.
>
>

Cuando haya ejecutado el comando, vaya a Azure Portal y verá que el equipo comienza a realizar el aprovisionamiento.

![][15]

A continuación aparecerá el símbolo del sistema:

![][16]

Una vez completada la implementación, la dirección IP pública de la nueva máquina virtual se mostrará al finalizar la implementación; puede copiarla y pegarla en un explorador web y ver el sitio web que ha implementado. Al implementar la máquina virtual, hemos abierto el puerto 80, por lo que debería estar disponible externamente.   

![][11]

En este ejemplo se utiliza código HTML creativo.

También puede ver el estado del nodo en [Chef Manage](https://manage.chef.io/). 

![][17]

No olvide que también puede conectarse mediante una sesión RDP desde Azure Portal a través del puerto 3389.

Gracias. Empiece hoy su viaje de su infraestructura como código con Azure.

<!--Image references-->
[2]: media/chef-automation/2.png
[3]: media/chef-automation/3.png
[4]: media/chef-automation/4.png
[5]: media/chef-automation/5.png
[6]: media/chef-automation/6.png
[7]: media/chef-automation/7.png
[8]: media/chef-automation/8.png
[9]: media/chef-automation/9.png
[10]: media/chef-automation/10.png
[11]: media/chef-automation/11.png
[13]: media/chef-automation/13.png
[14]: media/chef-automation/14.png
[15]: media/chef-automation/15.png
[16]: media/chef-automation/16.png
[17]: media/chef-automation/17.png


<!--Link references-->
