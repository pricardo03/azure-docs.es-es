---
title: Establecer la seguridad de una aplicación a los desarrolladores controlan distribución (ADCD) en IBM zD & T v1 | Microsoft Docs
description: Ejecutar un entorno de entorno de prueba (zD & T) y IBM Z Development en Azure Virtual Machines (VM).
services: virtual-machines-linux
documentationcenter: ''
author: njray
manager: edprice
editor: edprice
tags: ''
keywords: ''
ms.openlocfilehash: c6fcb345b49ce6354a24408ebe163fb928990252
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64925686"
---
# <a name="set-up-an-application-developers-controlled-distribution-adcd-in-ibm-zdt-v1"></a>Establecer la seguridad de una aplicación a los desarrolladores controlan distribución (ADCD) en IBM zD & T v1

Puede ejecutar un entorno de entorno de prueba (zD & T) y IBM Z Development en Azure Virtual Machines (VM). Este entorno emula la arquitectura de IBM Z Series. Puede hospedar una variedad de sistemas operativos de serie Z o instalaciones (también denominadas paquetes o las instancias de la Z), que están disponibles a través de paquetes personalizados llama a las distribuciones de controlado a los desarrolladores de IBM aplicación (ADCDs).

Este artículo muestra cómo configurar una instancia ADCD en un zD & entorno T en Azure. ADCDs crear implementaciones de sistema operativo Z Series completas para entornos de desarrollo y pruebas que se ejecutan en zD & T.

Al igual que zD & T, ADCDs solo están disponibles para los socios y clientes de IBM y son exclusivamente para fines de prueba y desarrollo. No son que se usará para entornos de producción. Están disponibles para su descarga a través de numerosos paquetes de instalación de IBM [Passport ventaja](https://www.ibm.com/support/knowledgecenter/en/SSTQBD_12.0.0/com.ibm.zsys.rdt.guide.adcd.doc/topics/installation_ps.html) o [IBM PartnerWorld](https://www-356.ibm.com/partnerworld/wps/servlet/ContentHandler/isv_com_sys_zos_adcd).

## <a name="prerequisites"></a>Requisitos previos

- Una suscripción de Azure. Si no tiene una, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

- El [zD & entorno T] [ ibm-install-z] configurado anteriormente en Azure. En este artículo se da por supuesto que está usando la misma imagen de máquina virtual de Ubuntu 16.04 que creó anteriormente.

- Acceso a los medios ADCD a través de IBM PartnerWorld o las ventajas de Passport.

- Un [el servidor de licencias](https://www.ibm.com/support/knowledgecenter/en/SSTQBD_12.0.0/com.ibm.zsys.rdt.tools.user.guide.doc/topics/zdt_ee.html). Esto es necesario para ejecutar IBM zD & T. La manera de crear depende de cómo se licencia del software de IBM:

  - **El servidor de licencias basada en hardware** requiere un dispositivo de hardware USB que contenga los Tokens de racionales es necesario para tener acceso a todas las partes del software. Esto debe obtener de IBM.

  - **El servidor de licencias basada en software** , deberá configurar un servidor centralizado para la administración de las claves de licencia. Este método es preferible y requiere que configure las recibe de IBM en el servidor de administración de claves.

## <a name="download-the-installation-packages-from-passport-advantage"></a>Descargar los paquetes de instalación de las ventajas de Passport

Se requiere acceso a los medios ADCD. Los pasos siguientes se supone un clientes de IBM y puede usar la ventaja de Passport. Pueden usar los asociados de IBM [IBM PartnerWorld](https://www-356.ibm.com/partnerworld/wps/servlet/ContentHandler/isv_com_sys_zos_adcd).

> [!NOTE]
> En este artículo se da por supuesto que se usa un equipo de Windows para acceder a Azure portal y descargar los medios de IBM. Si usa un escritorio de Mac o Ubuntu, los comandos y el proceso para obtener la media de IBM pueden diferir ligeramente.

1. Inicie sesión en [Passport ventaja](https://www.ibm.com/software/howtobuy/passportadvantage/paocustomer).

2. Seleccione **descargas de Software** y **acceso a medios**.

3. Seleccione **número de oferta y el contrato del programa**y haga clic en **continuar**.

4. Escriba la descripción de la parte o el número de pieza y haga clic en **buscador**.

5. Si lo desea, haga clic en la lista por orden alfabético para mostrar y ver del producto por su nombre.

6. Seleccione **todos los sistemas operativos** en el **campo del sistema operativo**, y **todos los idiomas** en el **campo idiomas**. A continuación, haga clic en **vaya**.

7. Haga clic en **seleccionar archivos individuales** para expandir la lista y mostrar la multimedia individuales para descargar.

8. Compruebe que los paquetes que desea descargar, seleccione **descargar**y, a continuación, descargue los archivos en el directorio que desee.

## <a name="upload-the-adcd-packages"></a>Cargar los paquetes ADCD

Ahora que tiene los paquetes, debe cargarlos en la máquina virtual en Azure.

1. En el portal de Azure, puede iniciar un **ssh** sesión con la VM de Ubuntu que creó. Vaya a la máquina virtual, seleccione el **Introducción** hoja y, a continuación, seleccione **Connect**.

2. Seleccione el **SSH** ficha y, a continuación, copie el comando "ssh" en el Portapapeles.

3. Inicie sesión en la máquina virtual mediante las credenciales y la [cliente SSH](/azure/virtual-machines/linux/use-remote-desktop) de elección. Esta demostración utiliza las extensiones de Linux para Windows 10, que agrega un shell de bash en el símbolo del sistema de Windows. PuTTY funciona igual de bien.

4. Cuando inicia sesión, cree un directorio para cargar los paquetes de IBM. Tenga en cuenta Linux distingue mayúsculas de minúsculas. Por ejemplo, esta demostración supone que los paquetes se cargan en:

        /home/MyUserID/ZDT/adcd/nov2017/volumes

5. Cargue los archivos mediante un cliente SSH como[WinSCP](https://winscp.net/eng/index.php). Dado que SCP es una parte de SSH, se usa el puerto 22, que es lo que usa SSH. Si el equipo local no es Windows, puede escribir el [comando scp](http://man7.org/linux/man-pages/man1/scp.1.html) en la sesión SSH.

6. Iniciar la carga en el directorio de la máquina virtual de Azure que creó, que se convierte en el almacenamiento de imágenes para zD & T.

    > [!NOTE]
    > Asegúrese de que **ADCDTOOLS. XML** se incluye en la carga de la **principal/MyUserID/ZDT/adcd/nov2017** directory. Lo necesitará más adelante.

7. Espere a que los archivos para cargar, lo que pueden tardar algún tiempo dependiendo de la conexión a Azure.

8. Cuando las cargas se completan, navegue hasta el directorio de volúmenes y descomprimir todos los **gz** volúmenes:

    ```
        gunzip \*.gz
    ```
    
![Que muestra el Explorador de archivos descomprimidos gz volúmenes](media/01-gunzip.png)

## <a name="configure-the-image-storage"></a>Configurar el almacenamiento de imágenes

El siguiente paso es configurar zD & T para usar los paquetes cargados. El proceso de almacenamiento de la imagen dentro de zD & T permite montar y utilizar las imágenes. Puede usar SSH o FTP.

1. Iniciar el **zDTServer**. Para ello, debe ser en el nivel raíz. Escriba los dos comandos siguientes en orden:
    ```
        sudo su -
        /opt/ibm/zDT/bin/startServer
    ```
2. Tenga en cuenta la salida de la dirección URL mediante el comando y use esta dirección URL para acceder al servidor web. Es similar:
     > https://(Your VM name or IP Address):9443/ZDTMC/index.HTML
     >
     > Recuerde que el acceso web usa el puerto 9443. Úselo para iniciar sesión en el servidor web. Es el identificador de usuario para ZD & T **zdtadmin** y la contraseña es **contraseña**.

    ![IBM zD & pantalla de bienvenida de T Enterprise Edition](media/02-welcome.png)

3. En el **inicio rápido** página, en **configurar**, seleccione **almacenamiento de imágenes**.

     ![Pantalla de inicio rápido de T Enterprise Edition y IBM zD](media/03-quickstart.png)

4. En el **configurar el almacenamiento de imagen** página, seleccione **SSH File Transfer Protocol**.

5. Para **nombre de Host**, tipo **Localhost** y escriba la ruta del directorio donde se cargan las imágenes. Por ejemplo, /home/MyUserID/ZDT/adcd/nov2017/volumes.

6. Escriba el **Id. de usuario** y **contraseña** para la máquina virtual. No use el Id. de usuario de T & ZD y la contraseña.

7. Probar la conexión para asegurarse de que tiene acceso y, a continuación, seleccione **guardar** para guardar la configuración.

## <a name="configure-the-target-environments"></a>Configurar los entornos de destino

El siguiente paso es configurar el zD & entorno de destino T. Este entorno emulado hospedado es donde ejecutan sus imágenes.

1. En el **inicio rápido** página, en **configurar**, seleccione **entornos de destino**.

2. En el **configurar entornos de destino** página, seleccione **Add Target**.

3. Seleccione **Linux**. IBM admite dos tipos de entornos, Linux y Cloud(OpenStack), pero esta demostración se ejecuta en Linux.

4. En el **agregar entorno de destino** página, para **nombre de Host**, escriba **localhost**. Mantener **puerto SSH** establecido en **22**.

5. En el **etiqueta de entorno de destino** cuadro, escriba una etiqueta como **MyCICS.**

     ![Agregar pantalla del entorno de destino](media/04-add-target.png)

## <a name="configure-adcd-and-deploy"></a>Configurar ADCD e implementar

Después de completar los pasos de configuración anterior, debe configurar zD & T para utilizar el entorno de paquetes y de destino. De nuevo, use el proceso de almacenamiento de la imagen en zD & T, que le permite montar y utilizar las imágenes. Puede usar SSH o FTP.

1. En el **inicio rápido** página, en **configurar**, seleccione **ADCD**. Aparece un conjunto de instrucciones, que le indica los pasos que deben realizarse antes de que se puede montar un paquete ADCD. Esto explica por qué hemos llamado el directorio de destino, tal como hicimos anteriormente.

2. Suponiendo que todas las imágenes se cargaron en los directorios correctos, haga clic en el **imagen desde ADCD** vínculo que se muestra en la esquina inferior derecha (se muestra en el paso 7 de captura de pantalla siguiente).

     ![IBM zD & T Enterprise Edition: pantalla Configurar ADCD](media/05-adcd.png)

## <a name="create-the-image"></a>Crear la imagen

Cuando se completa, el paso de configuración anterior el **crear una imagen con componentes ADCD** aparece la página.

1. Seleccione el volumen (noviembre de 2017 en este caso) para mostrar los paquetes que están en ese volumen.

2. Para esta demostración, seleccione **Customer Information Control System (CICS) - 5.3**.

3. En el **nombre de la imagen** , escriba un nombre para la imagen como **MyCICS imagen**.

4. Seleccione el **crear imagen** situado en la esquina inferior derecha.

     ![IBM zD & T Enterprise Edition: crear una imagen mediante la pantalla de componentes ADCD](media/06-adcd.png)

5. En la ventana que aparece, que le indica que la imagen se ha implementado correctamente, elija **implementar imágenes**.

6. En el **implementar una imagen en un entorno de destino** , seleccione la imagen que ha creado en la página anterior (**MyCICS imagen**) y el entorno de destino que creó anteriormente (**MyCICS**).

7. En la siguiente pantalla, proporcione sus credenciales para la máquina virtual (es decir, no la ztadmin credencial).

8. En el panel Propiedades, escriba el número de **procesadores Central (CPs)**, la cantidad de **(GB) de memoria del sistema**y el **directorio de implementación** para la imagen en ejecución. Puesto que esta es una demostración, mantener un tamaño reducido.

9. Asegúrese de que la casilla está activada para **automáticamente problema IPL comando para z/OS después de implementar**.

     ![Pantalla de propiedades](media/07-properties.png)

10. Seleccione **completa**.

11. Seleccione **implemente la imagen** desde el **implementar una imagen en un entorno de destino** página.

La imagen ahora puede implementar y está lista para montar un emulador de terminal 3270.

> [!NOTE]
> Si recibe un error que indica que no tiene suficiente espacio en disco, tenga en cuenta que la región requiere 151 Gb.

Felicidades. Ahora se ejecuta un entorno de gran sistema IBM en Azure.

## <a name="learn-more"></a>Más información

- [Migración de mainframes: mitos y los hechos](https://docs.microsoft.com/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/myths-and-facts)
- [IBM DB2 pureScale en Azure](https://docs.microsoft.com/azure/virtual-machines/linux/ibm-db2-purescale-azure)
- [Solución de problemas](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/)
- [Desmitificación del sistema central para la migración a Azure](https://azure.microsoft.com/resources/demystifying-mainframe-to-azure-migration/)

<!-- INTERNAL LINKS -->
[microfocus-get-started]: /microfocus/get-started.md
[microfocus-setup]: /microfocus/set-up-micro-focus-on-azure.md
[microfocus-demo]: /microfocus/demo.md
[ibm-get-started]: /ibm/get-started.md
[ibm-install-z]: install-ibm-z-environment.md
[ibm-demo]: /ibm/demo.md
