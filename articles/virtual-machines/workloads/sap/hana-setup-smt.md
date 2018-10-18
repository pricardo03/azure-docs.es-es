---
title: Configuración de un servidor SMT para SAP HANA en Azure (instancias grandes) | Microsoft Docs
description: Configuración de un servidor SMT para SAP HANA en Azure (instancia grande).
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f387c1afe88f2bba476309b2e2e01942d2b7ae5b
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "45982632"
---
# <a name="setting-up-smt-server-for-suse-linux"></a>Configuración de servidor SMT para SUSE Linux
Las instancias grandes de SAP HANA no tienen conectividad directa a Internet. Por tanto, no es un proceso sencillo registrar una de estas unidades con el proveedor de sistema operativo y descargar y aplicar las revisiones. Si usa SUSE Linux, una solución podría ser configurar un servidor SMT en una máquina virtual de Azure. Al mismo tiempo, la máquina virtual de Azure debe estar hospedada en una red virtual de Azure, que está conectada a la instancia grande de HANA. Con este tipo de servidor SMT, la unidad de instancia grande de HANA podría registrar y descargar las revisiones. 

SUSE proporciona una guía más extensa en [Subscription Management Tool for SLES 12 SP2](https://www.suse.com/documentation/sles-12/pdfdoc/book_smt/book_smt.pdf) (Herramienta de administración de suscripciones para SLES 12 SP2). 

Como condición previa para la instalación de un servidor SMT que realice la tarea para la instancia grande de HANA, necesitará:

- Una red virtual de Azure conectada al circuito ER de instancias grandes de HANA.
- Una cuenta de SUSE que esté asociada a una organización. La organización necesitaría tener alguna suscripción válida de SUSE.

## <a name="installation-of-smt-server-on-azure-vm"></a>Instalación del servidor SMT en la máquina virtual de Azure

En este paso, se instala al servidor SMT en una máquina virtual de Azure. La primera medida consiste en iniciar sesión en [SUSE Customer Center](https://scc.suse.com/) (Centro de servicios al cliente de SUSE).

Después de iniciar sesión, vaya a Organización--> Credenciales de organización. En esa sección debería encontrar las credenciales necesarias para configurar el servidor SMT.

El tercer paso consiste en instalar una máquina virtual de SUSE Linux en la red virtual de Azure. Para implementar la máquina virtual, tome una imagen de SLES 12 SP2 de la galería de Azure (seleccione la imagen SUSE BYOS). En el proceso de implementación, no defina un nombre DNS y no use direcciones IP estáticas, tal como se muestra en esta captura de pantalla

![implementación de VM para servidor SMT](./media/hana-installation/image3_vm_deployment.png)

La máquina virtual implementada era una máquina virtual más pequeña y tenía la dirección IP interna 10.34.1.4 en la red virtual de Azure. El nombre de la máquina virtual era smtserver. Después de la instalación, se comprobó la conectividad con las unidades de instancia grande de HANA. En función de cómo haya organizado la resolución de nombres, es posible que tenga que configurar la resolución de las unidades de instancia grande de HANA en etc/hosts en la máquina virtual de Azure. Agregue un disco adicional a la máquina virtual que se va a usar para contener las revisiones. El propio disco de arranque podría ser demasiado pequeño. En el caso del ejemplo, el disco se montó en /srv/www/htdocs, como se muestra en la captura de pantalla siguiente. Un disco de 100 GB debería ser suficiente.

![implementación de VM para servidor SMT](./media/hana-installation/image4_additional_disk_on_smtserver.PNG)

Inicie sesión en las unidades de instancia grande de HANA, mantenga /etc/hosts y compruebe si puede tener acceso a la máquina virtual de Azure en la que supuestamente se ejecuta el servidor SMT a través de la red.

Después de realizar correctamente esta comprobación, debe iniciar sesión en la máquina virtual de Azure que debería ejecutar el servidor SMT. Si usa putty para iniciar sesión en la máquina virtual, debe ejecutar esta secuencia de comandos en la ventana de Bash:

```
cd ~
echo "export NCURSES_NO_UTF8_ACS=1" >> .bashrc
```

Después de ejecutar estos comandos, reinicie Bash para activar la configuración. Después, inicie YAST.

Conecte su máquina virtual (smtserver) al sitio de SUSE.

```
smtserver:~ # SUSEConnect -r <registration code> -e s<email address> --url https://scc.suse.com
Registered SLES_SAP 12.2 x86_64
To server: https://scc.suse.com
Using E-Mail: email address
Successfully registered system.
```

Una vez que la máquina virtual esté conectada al sitio de SUSE, instale los paquetes de smt. Use el siguiente comando putty para instalar los paquetes smt.

```
smtserver:~ # zypper in smt
Refreshing service 'SUSE_Linux_Enterprise_Server_for_SAP_Applications_12_SP2_x86_64'.
Loading repository data...
Reading installed packages...
Resolving package dependencies...
```


También puede usar la herramienta YAST para instalar los paquetes smt. En YAST, vaya a Mantenimiento de software y busque smt. Seleccione smt, que cambia automáticamente a yast2-smt, como se muestra a continuación

![SMT en yast](./media/hana-installation/image5_smt_in_yast.PNG)


Acepte la selección para la instalación en el servidor SMT. Una vez instalado, vaya a la configuración del servidor SMT y escriba las credenciales de la organización que recuperó anteriormente desde el Centro de servicios al cliente de SUSE. Escriba también el nombre de host de la máquina virtual de Azure y la dirección URL del servidor SMT. En esta demostración, era https://smtserver, como se muestra en los siguientes gráficos.

![Configuración del servidor SMT](./media/hana-installation/image6_configuration_of_smtserver1.png)

Como paso siguiente, debe comprobar si funciona la conexión al Centro de servicios al cliente de SUSE. Como se ve en los gráficos siguientes, en el caso del ejemplo, ha funcionado.

![Probar la conexión al Centro de servicios al cliente de SUSE](./media/hana-installation/image7_test_connect.png)

Una vez se inicia el programa de instalación de SMT, debe proporcionar una contraseña de base de datos. Como es una instalación nueva, debe definir esa contraseña como se muestra en el gráfico siguiente.

![Definir la contraseña de la base de datos](./media/hana-installation/image8_define_db_passwd.PNG)

La interacción siguiente tiene lugar cuando se crea un certificado. Recorra el cuadro de diálogo como se muestra a continuación y continúe con el siguiente paso.

![Crear certificado para el servidor SMT](./media/hana-installation/image9_certificate_creation.PNG)

Es posible que se tarden varios minutos en el paso de "Ejecutar comprobación de sincronización" al final de la configuración. Después de la instalación y configuración del servidor SMT, debería encontrar el repositorio de directorio bajo el punto de montaje /srv/www/htdocs/ junto con varios subdirectorios. 

Reinicie el servidor SMT y sus servicios relacionados con estos comandos.

```
rcsmt restart
systemctl restart smt.service
systemctl restart apache2
```

## <a name="download-of-packages-onto-smt-server"></a>Descarga de paquetes al servidor SMT

Después de reiniciar todos los servicios, seleccione los paquetes adecuados en la administración de SMT con Yast. La selección de paquetes depende de la imagen del sistema operativo del servidor de instancias grandes de HANA y no de la versión de SLES de la máquina virtual en la que se ejecuta el servidor SMT. A continuación se muestra un ejemplo de la pantalla de selección.

![Seleccionar paquetes](./media/hana-installation/image10_select_packages.PNG)

Una vez haya terminado con la selección de paquetes, debe iniciar la copia inicial de los paquetes seleccionados en el servidor SMT que configuró. Esta copia se desencadena en el shell mediante el comando smt-mirror como se muestra a continuación


![Descargar los paquetes al servidor SMT](./media/hana-installation/image11_download_packages.PNG)

Como puede ver arriba, los paquetes se deben copiar en los directorios creados bajo el punto de montaje /srv/www/htdocs. Este proceso puede tardar unos minutos. Según el número de paquetes que seleccione, se puede tardar una hora o más.
Cuando finalice este proceso, debe pasar a la configuración del cliente SMT. 

## <a name="set-up-the-smt-client-on-hana-large-instance-units"></a>Configurar el cliente SMT en unidades de instancia grande de HANA

En este caso, los clientes son las unidades de instancia grande de HANA. El programa de instalación del servidor SMT copió el script clientSetup4SMT.sh en la máquina virtual de Azure. Copie ese script en la unidad de instancia grande de HANA que quiere conectar a su servidor SMT. Inicie el script con la opción -h y asígnele como parámetro el nombre del servidor SMT. En este ejemplo es smtserver.

![Configurar el cliente SMT](./media/hana-installation/image12_configure_client.PNG)

Es posible que haya un escenario en el que la carga del certificado desde el servidor por el cliente se realice correctamente, pero como se muestra a continuación no se pudo realizar el registro.

![Se produce un error en el registro de cliente](./media/hana-installation/image13_registration_failed.PNG)

Si se produce un error en el registro, lea este [documento de soporte de SUSE](https://www.suse.com/de-de/support/kb/doc/?id=7006024) y ejecute los pasos que describe.

> [!IMPORTANT] 
> Como nombre del servidor debe proporcionar el nombre de la máquina virtual, en este caso smtserver, sin el nombre de dominio completo. Con el nombre de la máquina virtual es suficiente. 

Después de ejecutar estos pasos, debe ejecutar el comando siguiente en la unidad de instancia grande de HANA.

```
SUSEConnect –cleanup
```

> [!Note] 
> En nuestras pruebas siempre se tuvieron que esperar unos minutos después de ese paso. La ejecución inmediata de clientSetup4SMT.sh, después de las medidas correctoras que se describen en el artículo de SUSE, finalizó con mensajes que indicaban que el certificado todavía no era válido. Para configurar correctamente el cliente normalmente se tenía que esperar 5-10 minutos y ejecutar clientSetup4SMT.sh.

Si se produce el problema que tuvo que corregir según los pasos descritos en el artículo de SUSE, debe reiniciar clientSetup4SMT.sh en la unidad de instancia grande de HANA de nuevo. Ahora debería finalizar correctamente como se muestra a continuación.

![Registro correcto del cliente](./media/hana-installation/image14_finish_client_config.PNG)

Con este paso, ha configurado el cliente SMT de la unidad de instancia grande de HANA para conectarse con el servidor SMT instalado en la máquina virtual de Azure. Ahora puede ejecutar "zypper up" o "zypper in" para instalar revisiones del sistema operativo en instancias grandes de HANA o instalar paquetes adicionales. Se entiende que solo puede obtener las revisiones que descargó antes en el servidor SMT.

**Pasos siguientes**
- Consulte sobre la [Instalación de HANA en HLI](hana-example-installation.md).











