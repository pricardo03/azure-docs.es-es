---
title: Ejecutar Micro Focus Enterprise Server 4.0 en un contenedor de Docker en Azure Virtual Machines
description: Rehospedar las cargas de trabajo de mainframes de IBM z/OS mediante la ejecución de Micro Focus Enterprise Server en un contenedor de Docker en Azure Virtual Machines.
services: virtual-machines-linux
documentationcenter: ''
author: njray
ms.author: sread
ms.date: 04/02/2019
ms.topic: article
ms.service: multiple
ms.openlocfilehash: 30d99c3f4767eb50361f7074c0d508fcf309faca
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "58896601"
---
# <a name="run-micro-focus-enterprise-server-40-in-a-docker-container-on-azure"></a>Ejecute Micro Focus Enterprise Server 4.0 en un contenedor de Docker en Azure

Puede ejecutar Micro foco Enterprise Server 4.0 en un contenedor de Docker en Azure. Este tutorial le enseña cómo hacerlo. Utiliza la demostración de acctdemo CICS (Customer Information Control System) de Windows para servidores de empresa.

Docker agrega portabilidad y aislamiento para las aplicaciones. Por ejemplo, puede exportar una imagen de Docker desde una máquina virtual de Windows para ejecutarse en otro, o desde un repositorio para un servidor de Windows con Docker. La imagen de Docker se ejecuta en la nueva ubicación con la misma configuración, sin tener que instalar el servidor de empresa. Es parte de la imagen. Consideraciones de licencias se siguen aplican.

En este tutorial se instala el **Windows 2016 Datacenter con Containers** máquina virtual (VM) de Azure Marketplace. Esta máquina virtual incluye **Docker 18.09.0**. Los pasos siguientes muestran cómo implementar el contenedor, ejecútelo y, a continuación, conectarse a ella con un emulador 3270.

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, revise estos requisitos previos:

- Una suscripción de Azure. Si no tiene una, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

- El software de Micro Focus y una licencia válida (o licencia de prueba). Si es un cliente existente de Micro Focus, póngase en contacto con su representante de Micro Focus. En caso contrario, [solicite una evaluación](https://www.microfocus.com/products/enterprise-suite/enterprise-server/trial/).

     > [!NOTE]
     > Los archivos de demostración de Docker se incluye con Enterprise Server 4.0. Este tutorial usa ent\_server\_dockerfiles\_4.0\_windows.zip. Acceder a él desde el mismo lugar que ha accedido el archivo de instalación del servidor de empresa o vaya a *Micro Focus* para empezar a trabajar.

- La documentación de [Enterprise Server y Enterprise Developer](https://www.microfocus.com/documentation/enterprise-developer/#").

## <a name="create-a-vm"></a>Crear una VM

1. Proteger el medio desde el ent\_server\_dockerfiles\_4.0\_windows.zip archivo. Proteger el archivo de licencia ES-Docker-Prod-XXXXXXXX.mflic (necesario para compilar las imágenes de Docker).

2. Cree la máquina virtual. Para ello, abra Azure portal, seleccione **crear un recurso** desde la esquina superior izquierda y filtre por *server windows*. En los resultados, seleccione **Windows Server 2016 Datacenter – con contenedores**.

     ![Resultados de búsqueda de Azure portal](media/01-portal.png)

3. Para configurar las propiedades de la máquina virtual, elija Detalles de la instancia:

    1. Elija el tamaño de la máquina virtual. Para este tutorial, considere el uso de un **DS2 estándar\_v2** máquina virtual con 2 vCPU y 7 GB de memoria.

    2. Seleccione el **región** y **grupo de recursos** que le gustaría implementar en.

    3. Para **las opciones de disponibilidad**, use la configuración predeterminada.

    4. Para **Username**, escriba la cuenta de administrador que desea utilizar y la contraseña.

    5. Asegúrese de que **puerto RDP 3389** está abierto. Solo este puerto debe estar expuestos públicamente, por lo que puede iniciar sesión en la máquina virtual. A continuación, acepte todos los valores predeterminados y haga clic en **revisión + crear**.

     ![Crear un panel de máquina virtual](media/container-02.png)

4. Espere a que finalice (un par de minutos) de la implementación. Un mensaje indica que se ha creado la máquina virtual.

5. Haga clic en **ir al recurso** para ir a la **Introducción** hoja para la máquina virtual.

6. A la derecha, haga clic en el **Connect** botón. El **conectar a máquina virtual** opciones que aparecen en la derecha.

7. Haga clic en el **Descargar archivo RDP** botón para descargar el archivo RDP que le permite adjuntar a la máquina virtual.

8. Después de que ha terminado de descargar el archivo, abra y escriba el nombre de usuario y contraseña que creó para la máquina virtual.

     > [!NOTE]
     > No use sus credenciales corporativas para iniciar sesión. (El cliente RDP supone que desea usarlas. No.)

9.  Seleccione **más opciones**, a continuación, seleccione las credenciales de la máquina virtual.

En este momento, la máquina virtual está ejecutándose y conectado a través de RDP. Ha iniciado en y listo para el paso siguiente.

## <a name="create-a-sandbox-directory-and-upload-the-zip-file"></a>Cree un directorio de recinto de seguridad y cargue el archivo zip

1.  Cree un directorio en la máquina virtual donde se puede cargar los archivos de licencia y demostración. Por ejemplo, **C:\\Sandbox**.

2.  Cargar **ent\_server\_dockerfiles\_4.0\_windows.zip** y **ES-Docker-Prod-XXXXXXXX.mflic** archivo al directorio que creó.

3.  Extraiga el contenido del archivo zip para el **ent\_server\_dockerfiles\_4.0\_windows** directorio creado por el proceso de extracción. Este directorio incluye un archivo Léame (como .html y el archivo .txt) y dos de los subdirectorios, **EnterpriseServer** y **ejemplos**.

4.  Copia **ES-Docker-Prod-XXXXXXXX.mflic** a la unidad C:\\Sandbox\\ent\_server\_dockerfiles\_4.0\_windows\\ EnterpriseServer y C:\\Sandbox\\ent\_server\_dockerfiles\_4.0\_windows\\ejemplos\\directorios CICS.

> [!NOTE]
> Asegúrese de que copiar el archivo de licencia para ambos directorios. Son necesarios para el paso de compilación de Docker para asegurarse de que las imágenes tengan la licencia correcta.

## <a name="check-docker-version-and-create-base-image"></a>Comprobar la versión de Docker y crear la imagen base

> [!IMPORTANT]
> Creación de la imagen de Docker correspondiente es un proceso de dos pasos. En primer lugar, cree la imagen base Enterprise Server 4.0. A continuación, cree otra imagen para el x64 plataforma. Aunque puede crear un x86 (32 bits) de la imagen, use la imagen de 64 bits.

1. Abra el símbolo del sistema.

2. Compruebe que Docker está instalado. En el símbolo del sistema, escriba:

    ```
        docker version
    ```

     Por ejemplo, la versión era 18.09.0 cuando se escribía esto.

3. Para cambiar el directorio, escriba **\Sandbox\ent_server_dockerfiles_4.0_windows\EnterpriseServer cd**.

4. Tipo **bld.bat IacceptEULA** para comenzar el proceso de compilación para la imagen inicial de base. Espere unos minutos para este proceso en ejecución. En los resultados, tenga en cuenta las dos imágenes que se han creado: uno para x64 y otro para x86:

     ![Ventana de comandos que muestra las imágenes](media/container-04.png)

5. Para crear la imagen final para la demostración de CICS, cambie al directorio CICS escribiendo **cd\\Sandbox\\ent\_server\_dockerfiles\_4.0\_windows\\ Ejemplos\\CICS**.

6. Para crear la imagen, escriba **bld.bat x64**. Espere unos minutos para ejecutar el proceso y el mensaje que indica que se creó la imagen.

7. Tipo **imágenes de docker** para mostrar una lista de todas las imágenes de Docker instaladas en la máquina virtual. Asegúrese de que **microfocus, es-acctdemo** es uno de ellos.

     ![Ventana de comandos que muestra la imagen es acctdemo](media/container-05.png)

## <a name="run-the-image"></a>Ejecución de la imagen 

1.  Para iniciar Enterprise Server 4.0 y la aplicación acctdemo, en el símbolo del sistema, escriba:

    ```
         docker run -p 16002:86/tcp -p 16002:86/udp -p 9040-9050:9040-9050 -p 9000-9010:9000-9010 -ti --network="nat" --rm microfocus/es-acctdemo:win_4.0_x64
    ```

2.  Instalar un emulador de terminal 3270 como [x3270](http://x3270.bgp.nu/) y usarlo para conectar a través del puerto 9040, a la imagen que se está ejecutando.

3.  Para obtener la dirección IP del contenedor acctdemo, por lo que Docker puede actuar como un servidor DHCP para los contenedores que administra:

    1.  Obtiene el identificador de contenedor en ejecución. Tipo **Docker ps** en el símbolo del sistema y anote el identificador (**22a0fe3159d0** en este ejemplo). Guárdelo para el paso siguiente.

    2.  Para obtener la dirección IP para el contenedor acctdemo, use el identificador del contenedor en el paso anterior como sigue:

    ```
       docker inspect <containerID> --format="{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}"
    ```

       Por ejemplo: 

    ```   
        docker inspect 22a0fe3159d0 --format="{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}"
    ```
4. Tenga en cuenta la dirección IP de la imagen acctdemo. Por ejemplo, la dirección en la siguiente salida es 172.19.202.52.

     ![Ventana de comandos que muestra la dirección IP](media/container-06.png)

5. Monte la imagen mediante el emulador. Configurar el emulador para usar la dirección de la imagen acctdemo y puerto 9040. En este caso, tiene **172.19.202.52:9040**. Suyo será similar. El **inicio de sesión para CICS** abre la pantalla.

    ![Inicio de sesión a la pantalla CICS](media/container-07.png)

6. Inicie sesión la región de CICS escribiendo **SYSAD** para el **USERID** y **SYSAD** para el **contraseña**.

7. Desactive la pantalla, con el mapa de teclas del emulador. Para x3270, seleccione el **mapa de teclas** opción de menú.

8. Para iniciar la aplicación acctdemo, escriba **ACCT**. Se muestra la pantalla inicial de la aplicación.

     ![Pantalla de cuenta de demostración](media/container-08.png)

9. Experimente con los tipos de cuenta de presentación. Por ejemplo, escriba **d.** para la solicitud y **11111** para el **cuenta**. Para probar otros números de cuenta son 22222 33333 y así sucesivamente.

     ![Pantalla de cuenta de demostración](media/container-09.png)

10. Para mostrar la consola de administración de servidor de empresa, vaya a la línea de comandos y escriba **iniciar http:172.19.202.52:86**

     ![Consola de administración de Enterprise Server](media/container-010.png)

Eso es todo. Ahora se está ejecutando y administración de una aplicación CICS en un contenedor de Docker.

## <a name="next-steps"></a>Pasos siguientes

- [Instalación de Micro Focus Enterprise Server 4.0 y desarrollador empresarial 4.0 en Azure](./set-up-micro-focus-azure.md)
- [Migrar la aplicación del sistema central](/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/application-strategies)
