---
title: Instalación de IBM zD & entorno de desarrollo y pruebas T en Azure | Microsoft Docs
description: Implementar IBM Z Development y entorno de prueba (zD & T) en la infraestructura de máquina Virtual (VM) de Azure como servicio (IaaS).
services: virtual-machines-linux
documentationcenter: ''
author: njray
manager: edprice
editor: edprice
tags: ''
keywords: ''
ms.openlocfilehash: 2f5520213e7d8792c89f5445d470987323173dc3
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "58894532"
---
# <a name="install-ibm-zdt-devtest-environment-on-azure"></a>Instalación de IBM zD & entorno de desarrollo y pruebas T en Azure

Para crear un entorno de desarrollo y pruebas para las cargas de trabajo de mainframes en IBM Z Systems, puede implementar IBM Z Development y entorno de prueba (zD & T) en la infraestructura de máquina Virtual (VM) de Azure como servicio (IaaS).

Con zD & T, que puede aprovechar el ahorro de costos de la x86 plataforma para sus entornos de desarrollo y pruebas menos críticos y, a continuación, las actualizaciones de vuelta a un entorno de producción del sistema de Z de inserción. Para obtener más información, consulte el [las instrucciones de instalación de IBM ZD & T](https://www-01.ibm.com/support/docview.wss?uid=swg24044565#INSTALL).

Azure y Azure Stack admiten las siguientes versiones:

- zD & T Personal Edition
- zD & T Parallel Sysplex
- zD&T Enterprise Edition

Todas las ediciones de zD & T ejecutarán solo en x86 sistemas Linux, no a Windows Server. Enterprise Edition es compatible con Red Hat Enterprise Linux (RHEL) o Ubuntu/Debian. Las imágenes de RHEL y máquina virtual Debian están disponibles para Azure.

Este artículo muestra cómo configurar zD & T Enterprise Edition en Azure, por lo que puede usar el servidor de web de Enterprise Edition de T & zD para la creación y administración de entornos. Instalar zD & T, no instala ningún entorno. Deben crearse por separado como paquetes de instalación. Por ejemplo, la aplicación a los desarrolladores controlan las distribuciones (ADCD) son las imágenes de volumen de entornos de prueba. Se encuentran en las imágenes de zip en la distribución de contenido multimedia disponible de IBM. Vea cómo [configurar un entorno ADCD en Azure](demo.md).

Para obtener más información, consulte el [zD & información general de T](https://www.ibm.com/support/knowledgecenter/en/SSTQBD_12.0.0/com.ibm.zdt.overview.gs.doc/topics/c_product_overview.html) en el centro de información de IBM.

Este artículo muestra cómo configurar desarrollo Z y entorno de prueba (zD & T) Enterprise Edition en Azure. A continuación, puede usar el servidor de web de Enterprise Edition de T & zD para crear y administrar entornos basados en Z en Azure.

## <a name="prerequisites"></a>Requisitos previos

> [!NOTE]
> IBM permite zD & T Enterprise Edition para instalarse en entornos de desarrollo y pruebas solo:*no* entornos de producción.

- Una suscripción de Azure. Si no tiene una, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

- Necesita acceso a los medios, que solo está disponibles para los socios y clientes de IBM. Para obtener más información, póngase en contacto con su representante de IBM o ver la información de contacto en el [zD & T](https://www.ibm.com/us-en/marketplace/z-systems-development-test-environment) sitio Web.

- Un [el servidor de licencias](https://www.ibm.com/support/knowledgecenter/en/SSTQBD_12.0.0/com.ibm.zsys.rdt.tools.user.guide.doc/topics/zdt_ee.html). Esto es necesario para tener acceso a los entornos. La manera de crear depende de cómo se licencia del software de IBM:

     - **El servidor de licencias basada en hardware** requiere un dispositivo de hardware USB que contenga los Tokens de racionales es necesario para tener acceso a todas las partes del software. Esto debe obtener de IBM.

     - **El servidor de licencias basada en software** , deberá configurar un servidor centralizado para la administración de las claves de licencia. Este método es preferible y requiere que configure las recibe de IBM en el servidor de administración de claves.

## <a name="create-the-base-image-and-connect"></a>Crear la imagen base y conectar

1. En Azure portal, [crear una máquina virtual](/azure/virtual-machines/linux/quick-create-portal) con la configuración del sistema operativo que desee. Este artículo supone que una máquina virtual de B4ms (con 4 vCPU y 16 GB de memoria) ejecutan Ubuntu 16.04.

2. Una vez creada la máquina virtual, abra los puertos de entrada 22 para SSH, 21 para FTP y 9443 para el servidor web.

3. Obtener las credenciales SSH que se muestra en el **Introducción** hoja de la máquina virtual a través de la **Connect** botón. Seleccione el **SSH** pestaña y copie el comando de inicio de sesión SSH en el Portapapeles.

4. Inicie sesión en un [shell de Bash](/azure/cloud-shell/quickstart) desde su equipo local y pegue el comando. Estará en el formulario **ssh\<Id. de usuario\>\@\<dirección IP\>**. Cuando se le solicite las credenciales, escriba para establecer una conexión con el directorio de inicio.

## <a name="copy-the-installation-file-to-the-server"></a>Copie el archivo de instalación en el servidor

El archivo de instalación para el servidor web es **ZDT\_instalar\_EE\_V12.0.0.1.tgz**. Se incluye en los medios proporcionados por IBM. Debe cargar este archivo a la VM de Ubuntu.

1. Desde la línea de comandos, escriba el siguiente comando para asegurarse de que todo está actualizado en la imagen recién creada:

    ```
    sudo apt-get update
    ```

2. Cree el directorio de instalación para:

    ```
    mkdir ZDT
    ```

3. Copie el archivo desde el equipo local a la máquina virtual:

    ```
    scp ZDT_Install_EE_V12.0.0.1.tgz  your_userid@<IP Address /ZDT>   =>
    ```
    
> [!NOTE]
> Este comando copia el archivo de instalación en el directorio ZDT en el directorio de inicio, que varía dependiendo de si el cliente ejecuta Windows o Linux.

## <a name="install-the-enterprise-edition"></a>Instalar la edición Enterprise

1. Vaya al directorio ZDT y descomprimir el ZDT\_instalar\_EE\_archivo V12.0.0.1.tgz mediante los siguientes comandos:

    ```
    cd ZDT
    chmod 755 ZDT\_Install\_EE\_V12.0.0.0.tgz
    ```

2. Ejecute el programa de instalación:

    ```
    ./ZDT_Install_EE_V12.0.0.0.x86_64
    ```

3. Seleccione **1** para instalar el servidor de empresa.

4. Presione **ENTRAR** y lea detenidamente los contratos de licencia. Al final de la licencia, escriba **Sí** para continuar.

5. Cuando se le pedirá que cambie la contraseña para el usuario recién creado, **ibmsys1**, use el comando **sudo passwd ibmsys1** y escriba la nueva contraseña.

6. Para comprobar si la instalación fue correcta escriba

    ```
    dpkg -l | grep zdtapp
    ```

7. Compruebe que la salida contiene la cadena **zdtapp 12.0.0.0**, lo que indica que la gasolina de paquete se ha instalado correctamente

### <a name="starting-enterprise-edition"></a>A partir de Enterprise Edition

Tenga en cuenta que cuando se inicia el servidor web, se ejecuta en el identificador de usuario zD & T que se creó durante el proceso de instalación.

1. Para iniciar el servidor web, use el Id. de usuario raíz para ejecutar el comando siguiente:

    ```
    sudo /opt/ibm/zDT/bin/startServer
    ```

2. Copie el script, que es similar a la salida de la dirección URL:

    ```
    https://<your IP address or domain name>:9443/ZDTMC/login.htm
    ```

3. Pegue la dirección URL en un explorador web para abrir el componente de administración para la instalación de T & zD.

## <a name="next-steps"></a>Pasos siguientes

[Establecer la seguridad de una aplicación a los desarrolladores controlan distribución (ADCD) en IBM zD & T v1](./demo.md)
