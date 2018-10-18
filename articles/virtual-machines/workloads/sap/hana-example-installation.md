---
title: Procedimiento para instalar HANA en SAP HANA en Azure (instancias grandes) | Microsoft Docs
description: Procedimiento para instalar HANA en SAP HANA en Azure (instancias grandes).
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
ms.openlocfilehash: 76a7ce99799b85d81aa6e127ebe1e57e2df3e59a
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44167652"
---
# <a name="example-of-an-sap-hana-installation-on-hana-large-instances"></a>Ejemplo de una instalación de SAP HANA en instancias grandes de HANA

En esta sección se muestra cómo instalar SAP HANA en una unidad de instancia grande de HANA. El estado de inicio que tenemos tiene el siguiente aspecto:

- Ha proporcionado a Microsoft todos los datos para implementar una instancia grande de SAP HANA.
- Ha recibido la instancia grande de SAP HANA de Microsoft.
- Ha creado una red virtual de Azure que está conectada a la red local.
- Ha conectado el circuito ExpressRotue para instancias grandes de HANA a la misma red virtual de Azure.
- Ha instalado una máquina virtual de Azure que se usa como host administrativo seguro para instancias grandes de HANA.
- Se ha asegurado de que puede conectarse desde el host administrativo seguro a la unidad de instancia grande de HANA y viceversa.
- Ha comprobado si están instalados todos los paquetes y revisiones necesarios.
- Ha leído las notas de SAP y la documentación relativa a la instalación de HANA en el sistema operativo que usa y se ha asegurado de que la versión de HANA seleccionada es compatible con la versión del sistema operativo.

En las secuencias siguientes se muestra la descarga de los paquetes de instalación de HANA a la máquina virtual de host administrativo seguro, que en este caso se ejecuta en un sistema operativo de Windows, la copia de los paquetes a la unidad de instancia grande de HANA y la secuencia de la instalación.

## <a name="download-of-the-sap-hana-installation-bits"></a>Descarga de los elementos de instalación de SAP HANA
Puesto que las unidades de instancia grande de HANA no tienen conectividad directa a Internet, no puede descargar directamente los paquetes de instalación desde SAP en la máquina virtual de la instancia grande de HANA. Para solucionar la falta de conectividad directa a Internet, se necesita el host administrativo seguro. Descargue los paquetes a la máquina virtual de host administrativo seguro.

Para descargar los paquetes de instalación de HANA, necesitará un usuario S de SAP u otro usuario, lo que le permite obtener acceso a la tienda de SAP. Después de iniciar sesión, recorra esta secuencia de pantallas:

Vaya a [SAP Service Marketplace](https://support.sap.com/en/index.html) (Tienda del servicio SAP) > haga clic en Descargar Software > Instalaciones y actualización > Por índice alfabético > En H: SAP HANA Platform Edition > SAP HANA Platform Edition 2.0 > Instalación > descargue los archivos siguientes.

![Descargar la instalación de HANA](./media/hana-installation/image16_download_hana.PNG)

En el caso de ejemplo, se descargan los paquetes de instalación de SAP HANA 2.0. En la máquina virtual de host administrativo seguro de Azure, expanda los archivos autoextraíbles en el directorio tal y como se muestra a continuación.

![Extraer la instalación de HANA](./media/hana-installation/image17_extract_hana.PNG)

Una vez extraídos los archivos, copie el directorio creado por la extracción, 51052030 en el caso anterior, a la unidad de instancia grande de HANA en el volumen /hana/shared en un directorio que haya creado.

> [!Important]
> No copie los paquetes de instalación en la raíz o el LUN de inicio dado que el espacio es limitado y también debe usarse por otros procesos.


## <a name="install-sap-hana-on-the-hana-large-instance-unit"></a>Instalar SAP HANA en la unidad de instancia grande de HANA
Para poder instalar SAP HANA, debe iniciar sesión como raíz del usuario. Solo la raíz tiene los permisos suficientes para instalar SAP HANA.
Lo primero que debe hacer es establecer permisos en el directorio que copió en /hana/shared. Los permisos deben establecerse de esta forma

```
chmod –R 744 <Installation bits folder>
```

Si quiere instalar SAP HANA mediante el programa de instalación gráfico, es necesario instalar el paquete gtk2 en las instancias grandes de HANA. Compruebe si está instalado con el comando

```
rpm –qa | grep gtk2
```

En los pasos siguientes se describe la instalación de SAP HANA con la interfaz gráfica de usuario. Como paso siguiente, vaya al directorio de instalación y navegue al subdirectorio HDB_LCM_LINUX_X86_64. Iniciar

```
./hdblcmgui 
```
desde ese directorio. Ahora se le guiará por una secuencia de pantallas donde debe proporcionar los datos de la instalación. En el caso del ejemplo, se instalan los componentes de servidor de base de datos y cliente de SAP HANA. Por tanto, nuestra selección es "SAP HANA Database" (Base de datos de SAP HANA) como se muestra a continuación

![Seleccionar HANA en la instalación](./media/hana-installation/image18_hana_selection.PNG)

En la siguiente pantalla, elija la opción "Install New System" (Instalar nuevo sistema).

![Seleccionar la nueva instalación de HANA](./media/hana-installation/image19_select_new.PNG)

Después de este paso, debe seleccionar entre varios componentes adicionales que también se pueden instalar en el servidor de base de datos de SAP HANA.

![Seleccionar componentes adicionales de HANA](./media/hana-installation/image20_select_components.PNG)

En esta documentación, elegimos el cliente de SAP HANA y SAP HANA Studio. También se instala una instancia de escalado vertical. Por tanto, en la siguiente pantalla, debe elegir "Single-Host System" (Sistema de un único host). 

![Seleccionar la instalación de escala vertical](./media/hana-installation/image21_single_host.PNG)

En la siguiente pantalla, debe proporcionar algunos datos.

![Proporcionar el SID de SAP HANA](./media/hana-installation/image22_provide_sid.PNG)

> [!Important]
> Como Id. de sistema de HANA (SID), debe proporcionar el mismo SID que proporcionó a Microsoft cuando solicitó la implementación de la instancia grande de HANA. Elegir un SID distinto hace que se produzca un error en la instalación debido a problemas de permisos de acceso en los diferentes volúmenes.

Como directorio de instalación use el directorio /hana/shared. En el paso siguiente, debe proporcionar las ubicaciones para los archivos de datos y de registro de HANA.


![Proporcionar la ubicación del registro de HANA](./media/hana-installation/image23_provide_log.PNG)

> [!Note]
> Como archivos de datos y de registro debe definir los volúmenes que ya se incluían con los puntos de montaje que contienen al SID que eligió en la selección de la pantalla anterior a esta. Si el SID no coincide con el que escribió en la pantalla anterior, vuelva atrás y ajuste el SID en el valor de los puntos de montaje.

En el paso siguiente, revise el nombre de host y corríjalo si es necesario. 

![Revisar el nombre de host](./media/hana-installation/image24_review_host_name.PNG)

En el paso siguiente, también debe recuperar los datos que proporcionó a Microsoft cuando solicitó la implementación de la instancia grande de HANA. 

![Proporcionar el UID y GID del usuario del sistema](./media/hana-installation/image25_provide_guid.PNG)

> [!Important]
> Debe proporcionar el mismo Id. de usuario del sistema y el mismo Id. de grupo de usuarios que proporcionó a Microsoft cuando solicitó la implementación de la unidad. Si no proporciona los mismos identificadores, se produce un error en la instalación de SAP HANA en la unidad de instancia grande de HANA.

En las dos pantallas siguientes, que no se muestran en esta documentación, tiene que proporcionar la contraseña para el usuario SYSTEM de la base de datos de SAP HANA y la contraseña para el usuario sapadm, que se usa para el agente de host de SAP que se instala como parte de la instancia de base de datos de SAP HANA.

Después de definir la contraseña, aparece una pantalla de confirmación. Compruebe todos los datos que se muestran y continúe con la instalación. Llegará a una pantalla de progreso en la que se documenta el progreso de la instalación, como la siguiente:

![Comprobar el progreso de la instalación](./media/hana-installation/image27_show_progress.PNG)

Cuando finalice la instalación, debería ver una imagen como la siguiente:

![La instalación ha finalizado](./media/hana-installation/image28_install_finished.PNG)

En este momento, la instancia de SAP HANA debe estar en ejecución y lista para su uso. Debe poder conectarse a ella desde SAP HANA Studio. Asegúrese también de buscar y aplicar las últimas revisiones de SAP HANA.


**Pasos siguientes**

- Consulte [Alta disponibilidad y recuperación ante desastres de SAP HANA en Azure (instancias grandes)](hana-overview-high-availability-disaster-recovery.md).

