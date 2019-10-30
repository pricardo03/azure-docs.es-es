---
title: Escalabilidad horizontal de SAP HANA con nodo en espera en máquinas virtuales de Azure con Azure NetApp Files en SUSE Linux Enterprise Server | Microsoft Docs
description: Guía de alta disponibilidad de SAP NetWeaver en SUSE Linux Enterprise Server con Azure NetApp Files para las aplicaciones de SAP
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: rdeltcheva
manager: juergent
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 5e514964-c907-4324-b659-16dd825f6f87
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/30/2019
ms.author: radeltch
ms.openlocfilehash: 11b0746c3e9e137775b2466af776b4cd9ba1e5df
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72791673"
---
# <a name="sap-hana-scale-out-with-standby-node-on-azure-vms-with-azure-netapp-files-on-suse-linux-enterprise-server"></a>Escalabilidad horizontal de SAP HANA con nodo en espera en máquinas virtuales de Azure con Azure NetApp Files en SUSE Linux Enterprise Server 

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

[anf-azure-doc]:https://docs.microsoft.com/azure/azure-netapp-files/
[anf-avail-matrix]:https://azure.microsoft.com/global-infrastructure/services/?products=netapp&regions=all 
[anf-register]:https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-register
[anf-sap-applications-azure]:https://www.netapp.com/us/media/tr-4746.pdf

[2205917]: https://launchpad.support.sap.com/#/notes/2205917
[1944799]: https://launchpad.support.sap.com/#/notes/1944799
[1928533]: https://launchpad.support.sap.com/#/notes/1928533
[2015553]: https://launchpad.support.sap.com/#/notes/2015553
[2178632]: https://launchpad.support.sap.com/#/notes/2178632
[2191498]: https://launchpad.support.sap.com/#/notes/2191498
[2243692]: https://launchpad.support.sap.com/#/notes/2243692
[1984787]: https://launchpad.support.sap.com/#/notes/1984787
[1999351]: https://launchpad.support.sap.com/#/notes/1999351
[1410736]:https://launchpad.support.sap.com/#/notes/1410736
[1900823]: https://launchpad.support.sap.com/#/notes/1900823

[sap-swcenter]:https://support.sap.com/en/my-support/software-downloads.html

[suse-ha-guide]:https://www.suse.com/products/sles-for-sap/resource-library/sap-best-practices/
[suse-drbd-guide]:https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha_techguides/book_sleha_techguides.html
[suse-ha-12sp3-relnotes]:https://www.suse.com/releasenotes/x86_64/SLE-HA/12-SP3/

[sap-hana-ha]:sap-hana-high-availability.md
[nfs-ha]:high-availability-guide-suse-nfs.md


En este artículo se describe cómo implementar un sistema HANA de alta disponibilidad en una configuración de escalado horizontal con nodos en espera en máquinas virtuales de Azure con [Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-introduction/) para los volúmenes de almacenamiento compartido.  
En las configuraciones de ejemplo, como en los comandos de instalación, la instancia de HANA es **03** y el identificador del sistema de HANA es **HN1**. Los ejemplos se basan en HANA 2.0 SP4 y SUSE Linux Enterprise Server para SAP 12 SP4. 

Lea primero las notas y los documentos de SAP siguientes:

* [Documentación sobre Azure NetApp Files][anf-azure-doc] 
* Nota de SAP [1928533], que incluye:  
  * La lista de tamaños de máquina virtual de Azure que se admiten para la implementación del software de SAP
  * Información importante sobre la capacidad para los tamaños de máquina virtual de Azure
  * Software de SAP admitido y combinaciones de sistema operativo y base de datos
  * Versión del kernel de SAP requerida para Windows y Linux en Microsoft Azure
* La nota de SAP [2015553] enumera los requisitos previos para las implementaciones de software de SAP admitidas por SAP en Azure.
* La nota de SAP [2205917] contiene configuraciones recomendadas del sistema operativo para SUSE Linux Enterprise Server para SAP Applications
* La nota de SAP [1944799] contiene guías de SAP para SUSE Linux Enterprise Server para SAP Applications.
* La nota de SAP [2178632] contiene información detallada sobre todas las métricas de supervisión notificadas para SAP en Azure.
* La nota de SAP [2191498] incluye la versión de SAP Host Agent necesaria para Linux en Azure.
* La nota de SAP [2243692] incluye información acerca de las licencias de SAP en Linux en Azure.
* La nota de SAP [1984787] incluye información general sobre SUSE Linux Enterprise Server 12.
* La nota de SAP [1999351] contiene más soluciones de problemas de la extensión de supervisión mejorada de Azure para SAP.
* La nota de SAP [1900823] incluye información sobre los requisitos de almacenamiento de SAP HANA.
* La [WIKI de la comunidad SAP](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) contiene todas las notas de SAP que se necesitan para Linux.
* [Planeación e implementación de Azure Virtual Machines para SAP en Linux][planning-guide]
* [Implementación de Azure Virtual Machines para SAP en Linux][deployment-guide]
* [Implementación de DBMS de Azure Virtual Machines para SAP en Linux][dbms-guide]
* [Guías de procedimientos recomendados de SUSE SAP HA][suse-ha-guide] Las guías contienen toda la información necesaria para la configuración de Netweaver HA y la replicación del sistema de SAP HANA en el entorno local. Use estas guías como base de referencia general. Proporcionan información mucho más detallada.
* [Notas de la versión de la Extensión 12 SP3 de alta disponibilidad para SUSE][suse-ha-12sp3-relnotes]
* [Aplicaciones de NetApp SAP en Microsoft Azure que usan Azure NetApp Files][anf-sap-applications-azure]
* [SAP HANA en sistemas NetApp con NFS](https://www.netapp.com/us/media/tr-4435.pdf). La guía de configuración contiene información sobre cómo configurar SAP HANA mediante NFS proporcionado por Azure NetApp Files.


## <a name="overview"></a>Información general

Uno de los métodos para lograr la alta disponibilidad de HANA es la conmutación por error automática del host. Para configurar la conmutación por error automática del host, se agregan una o varias máquinas virtuales al sistema HANA y se configuran como nodos en espera. Cuando se produce un error en el nodo activo, un nodo en espera toma automáticamente el control. En la configuración presentada con las máquinas virtuales de Azure, esto lo realiza [NFS en Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-introduction/).  

El nodo en espera debe tener acceso a todos los volúmenes de la base de datos. Los volúmenes HANA deben montarse como volúmenes NFSv4. El mecanismo de bloqueo mejorado basado en la concesión de archivos en el protocolo NFSv4 se usa para el vallado `I/O`. 

> [!IMPORTANT]
> Es obligatorio implementar los volúmenes de datos y de registro de HANA como volúmenes NFSv4.1 y montarlos; para ello, se utiliza el protocolo NFSv4.1 para que la configuración sea compatible. La configuración de conmutación por error automática del host de HANA con nodo en espera no se admite con NFSv3.

![Información general sobre la alta disponibilidad de SAP NetWeaver](./media/high-availability-guide-suse-anf/sap-hana-scale-out-standby-netapp-files-suse.png)

Siguiendo las recomendaciones de red de SAP HANA, se han creado tres subredes dentro de una red virtual de Azure: para la comunicación con el sistema de almacenamiento, para la comunicación interna entre nodos de HANA y para la comunicación del cliente. Los volúmenes de Azure NetApp están en una subred independiente y [delegan a Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-delegate-subnet).  

En esta configuración de ejemplo, las subredes son:  

  - `storage` 10.23.2.0/24  
  - `hana` 10.23.3.0/24  
  - `client` 10.23.0.0/24  
  - `anf` 10.23.1.0/26  

## <a name="setting-up-the-azure-netapp-files-infrastructure"></a>Configuración de la infraestructura de Azure NetApp Files 

Antes de continuar con la configuración de la infraestructura de Azure NetApp Files, familiarícese con la [documentación correspondiente][anf-azure-doc]. Azure NetApp Files está disponible en varias [regiones de Azure](https://azure.microsoft.com/global-infrastructure/services/?products=netapp). Compruebe si la región de Azure seleccionada ofrece Azure NetApp Files.  

El siguiente vínculo muestra la disponibilidad de Azure NetApp Files por región de Azure: [Disponibilidad de Azure NetApp Files por región de Azure][anf-avail-matrix].  
Antes de implementar Azure NetApp Files, solicite la incorporación a este siguiendo las [instrucciones para registrarse en Azure NetApp Files][anf-register]. 

### <a name="deploy-azure-netapp-files-resources"></a>Implementación de recursos de Azure NetApp Files  

En los pasos siguientes se supone que ya ha implementado la [red virtual de Azure](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview). Los recursos de Azure NetApp Files y las máquinas virtuales en las que esos recursos se montarán se deben implementar en la misma red virtual de Azure o en redes virtuales emparejadas.  

1. Si aún no lo ha hecho, solicite la [incorporación a Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-register).  

2. Cree la cuenta de NetApp en la región de Azure seleccionada mediante las [instrucciones para crear la cuenta de NetApp](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-create-netapp-account).  

3. Configure el grupo de capacidad de Azure NetApp Files con las [instrucciones correspondientes](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-set-up-capacity-pool).  
La arquitectura de HANA que se presenta en este artículo utiliza un único grupo de capacidad de Azure NetApp Files, el nivel de servicio Ultra. Se recomienda el [nivel de servicio](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-service-levels) Ultra o Premium de Azure NetApp Files para cargas de trabajo de HANA en Azure.  

4. Delegue una subred en Azure NetApp Files como se describe en las [instrucciones correspondientes](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-delegate-subnet).  

5. Implemente los volúmenes de Azure NetApp Files mediante las [instrucciones correspondientes](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-create-volumes).  Asegúrese de seleccionar la versión **NFSv4.1** al implementar los volúmenes. Actualmente, el acceso a NFSv4.1 requiere una lista de permitidos adicional. Implemente los volúmenes en la [subred](https://docs.microsoft.com/rest/api/virtualnetwork/subnets) de Azure NetApp Files designada. Tenga en cuenta que los recursos de Azure NetApp Files y las máquinas virtuales de Azure deben estar en la misma red virtual de Azure o en redes virtuales emparejadas. Por ejemplo <b>HN1</b>-data-mnt00001, <b>HN1</b>-log-mnt00001, etc. son los nombres de los volúmenes y nfs://10.23.1.5/<b>HN1</b>-data-mnt00001, nfs://10.23.1.4/<b>HN1</b>-log-mnt00001, etc. son las rutas de acceso para los volúmenes de Azure NetApp Files.  

   1. volumen <b>HN1</b>-data-mnt00001 (nfs://10.23.1.5/<b>HN1</b>-data-mnt00001)
   2. volumen <b>HN1</b>-data-mnt00002 (nfs://10.23.1.6/<b>HN1</b>-data-mnt00002)
   3. volumen <b>HN1</b>-log-mnt00001 (nfs://10.23.1.4/<b>HN1</b>-log-mnt00001)
   4. volumen <b>HN1</b>-log-mnt00002 (nfs://10.23.1.6/<b>HN1</b>-log-mnt00002)
   5. volumen <b>HN1</b>-shared (nfs://10.23.1.4/<b>HN1</b>-shared)
   
   En este ejemplo, usamos instancias de Azure NetApp Files independientes para cada volumen de datos y registro de HANA. Para una configuración optimizada para costo en sistemas más pequeños o no productivos, es posible colocar todos los montajes de datos y todos los montajes de registros en un único volumen.  

### <a name="important-considerations"></a>Consideraciones importantes

A la hora de considerar Azure NetApp Files para SAP Netweaver en la arquitectura de alta disponibilidad de SUSE, tenga en cuenta los siguientes aspectos importantes:

- El grupo de capacidad mínimo es de 4 TiB.  
- El tamaño del volumen mínimo es de 100 GiB.
- Azure NetApp Files y todas las máquinas virtuales en las que los volúmenes de Azure NetApp Files se montarán, se deben implementar en la misma red virtual de Azure o en [redes virtuales emparejadas](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) de la misma región.  
- La red virtual seleccionada debe tener una subred delegada en Azure NetApp Files.
- El rendimiento de un volumen de Azure NetApp es una función de la cuota del volumen y del nivel de servicio, como se documenta en [Nivel de servicio para Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-service-levels). Al ajustar el tamaño de los volúmenes de Azure NetApp de HANA, asegúrese de que el rendimiento resultante cumple los requisitos del sistema HANA.  
- Azure NetApp Files ofrece la [directiva de exportación](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-configure-export-policy): puede controlar los clientes permitidos, el tipo de acceso (lectura y escritura, solo lectura, etc.). 
- La característica Azure NetApp Files no depende aún de la zona. En la actualidad, la característica Azure NetApp Files no se implementa en todas las zonas de disponibilidad de una región de Azure. Tenga en cuenta las posibles implicaciones de latencia en algunas regiones de Azure.  
- Es importante que las máquinas virtuales se implementen muy cerca del almacenamiento de Azure NetApp para conseguir una latencia baja. Para las cargas de trabajo de SAP HANA, una baja latencia resulta fundamental. Trabaje con su representante de Microsoft para asegurarse de que las máquinas virtuales y los volúmenes de Azure NetApp Files se implementan en ubicaciones muy cercanas entre sí.  
- El identificador de usuario para <b>sid</b>adm y el identificador de grupo para `sapsys` en las máquinas virtuales deben coincidir con la configuración de Azure NetApp Files. 

> [!IMPORTANT]
> Para las cargas de trabajo de SAP HANA, una baja latencia resulta fundamental. Trabaje con su representante de Microsoft para asegurarse de que las máquinas virtuales y los volúmenes de Azure NetApp Files se implementan en ubicaciones muy cercanas entre sí.  

> [!IMPORTANT]
> Si hay una discrepancia entre el identificador de usuario para <b>sid</b>adm y el identificador de grupo para `sapsys` entre la máquina virtual y la configuración de Azure NetApp, los permisos de archivos en volúmenes de Azure NetApp, montados en máquinas virtuales, aparecerán como `nobody`. Asegúrese de especificar el identificador de usuario correcto para <b>sid</b>adm y el identificador de grupo para `sapsys` al [incorporar un nuevo sistema](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxjSlHBUxkJBjmARn57skvdUQlJaV0ZBOE1PUkhOVk40WjZZQVJXRzI2RC4u) a Azure NetApp Files.

### <a name="sizing-for-hana-database-on-azure-netapp-files"></a>Ajuste del tamaño de la base de datos HANA en Azure NetApp Files

El rendimiento de un volumen de Azure NetApp es una función del tamaño del volumen y del nivel de servicio, como se documenta en [Nivel de servicio para Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-service-levels). 

Al diseñar la infraestructura de SAP en Azure, debe tener en cuenta los requisitos mínimos de almacenamiento de SAP, que se traducen en características de rendimiento mínimas:

- Habilitación de la lectura o escritura en /hana/log de 250 MB/s con tamaños de E/S de 1 MB  
- Habilitación de la actividad de lectura de al menos 400 MB/s para /hana/data con tamaños de E/S de 16 MB y 64 MB  
- Habilitación de la actividad de escritura de al menos 250 MB/s para /hana/data con tamaños de E/S de 16 MB y 64 MB  

Los [límites de rendimiento de Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-service-levels) por 1 TiB de cuota de volumen son los siguientes:
- Capa Premium Storage: 64 MiB/s  
- Capa de almacenamiento Ultra: 128 MiB/s  

Para cumplir los requisitos de rendimiento mínimo de SAP para los datos y el registro, y de acuerdo con las directrices para `/hana/shared`, los tamaños recomendados serían los siguientes:

| Volumen | Size<br /> Capa Premium Storage | Size<br /> Capa de almacenamiento Ultra |
| --- | --- | --- |
| /hana/log/ | 4 TiB | 2 TiB |
| /hana/data | 6,3 TiB | 3,2 TiB |
| /hana/shared | Max (512 GB, 1xRAM) por cuatro nodos de trabajo | Max (512 GB, 1xRAM) por cuatro nodos de trabajo |

La configuración de SAP HANA para el diseño que se presenta en este artículo, con la capa de almacenamiento Ultra de Azure NetApp Files, tendría el siguiente aspecto:

| Volumen | Size<br /> Capa de almacenamiento Ultra |
| --- | --- |
| /hana/log/mnt00001 | 2 TiB |
| /hana/log/mnt00002 | 2 TiB |
| /hana/data/mnt00001 | 3,2 TiB |
| /hana/data/mnt00002 | 3,2 TiB |
| /hana/shared | 2 TiB |

> [!NOTE]
> Las recomendaciones de tamaño de Azure NetApp Files que se han indicado se dirigen a satisfacer los requisitos mínimos que SAP expresa a sus proveedores de infraestructura. En escenarios reales de implementaciones de clientes y de cargas de trabajo, es posible que no sea suficiente. Utilice estas recomendaciones como punto de partida y adáptelas en función de los requisitos de la carga de trabajo específica.  

> [!TIP]
> Puede cambiar el tamaño de los volúmenes de Azure NetApp Files de manera dinámica, sin necesidad de `unmount` los volúmenes, detener las máquinas virtuales o detener SAP HANA. Esto permite que la aplicación se adapte a las demandas de rendimiento esperadas e imprevistas.

## <a name="deploy-linux-virtual-machines-via-azure-portal"></a>Implementación de máquinas virtuales Linux en Azure Portal

Primero deberá crear los volúmenes de Azure NetApp Files. Cree las [subredes de la red virtual de Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-subnet) en la [red virtual de Azure](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview). Implemente las máquinas virtuales. Cree las interfaces de red adicionales y conecte las interfaces de red a las máquinas virtuales correspondientes. Cada máquina virtual tiene tres interfaces de red, correspondientes a las tres subredes de redes virtuales de Azure (`storage`, `hana` y `client`).  Consulte [Creación de una máquina virtual Linux en Azure con varias tarjetas de interfaz de red](https://docs.microsoft.com/azure/virtual-machines/linux/multiple-nics).  

> [!IMPORTANT]
> Para las cargas de trabajo de SAP HANA, una baja latencia resulta fundamental. Trabaje con su representante de Microsoft para asegurarse de que las máquinas virtuales y los volúmenes de Azure NetApp Files se implementan en ubicaciones muy cercanas entre sí para lograr una baja latencia. Envíe la información necesaria al [incorporar un nuevo sistema SAP HANA](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxjSlHBUxkJBjmARn57skvdUQlJaV0ZBOE1PUkhOVk40WjZZQVJXRzI2RC4u), que usa Azure NetApp Files de SAP HANA.  
> 
En los pasos siguientes se supone que ya ha creado el grupo de recursos, la red virtual de Azure y las tres subredes de la red virtual de Azure: `storage`, `hana` y `client`.  Al implementar las máquinas virtuales, seleccione la subred de almacenamiento, de modo que la interfaz de red de almacenamiento sea la interfaz principal de las máquinas virtuales. Si esto no es posible, debe configurarse una ruta explícita a la subred delegada de Azure NetApp mediante la puerta de enlace de la subred de almacenamiento. 

> [!IMPORTANT]
> Asegúrese de que el sistema operativo seleccionado está certificado por SAP para SAP HANA en los tipos específicos de máquina virtual que use. La lista de tipos de máquina virtual certificados para SAP HANA y las versiones de sistema operativo correspondientes se puede consultar en [Plataformas de IaaS certificadas para SAP HANA](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure). No olvide consultar la información del tipo de máquina virtual mostrado para obtener una lista completa de las versiones de sistema operativo que SAP HANA admite para ese tipo específico de máquina virtual.  

1. Cree un conjunto de disponibilidad para SAP HANA. Asegúrese de establecer el dominio máximo de actualización.  

2. Cree tres máquinas virtuales (**hanadb1**, **hanadb2**, **hanadb3**)  
   - Use una imagen SLES4SAP en la galería de Azure que sea compatible con SAP HANA. En este ejemplo hemos usado la imagen de SLES4SAP 12 SP4.  
   - Seleccione el conjunto de disponibilidad creado anteriormente para SAP HANA.  
   - Seleccione la subred de la red virtual de Azure de almacenamiento. Seleccione [Accelerated Networking](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli).  
Al implementar las máquinas virtuales, el nombre de la interfaz de red se genera automáticamente. Haremos referencia a las interfaces de red, conectadas a la subred la red virtual de Azure de almacenamiento como **hanadb1-storage**, **hanadb2-storage** y **hanadb3-storage**. 
3. Cree tres interfaces de red, una para cada máquina virtual, para la subred de red virtual de `hana`. En este ejemplo: **hanadb1-hana**, **hanadb2-hana** y **hanadb3-hana**.  
4. Cree tres interfaces de red, una para cada máquina virtual, para la subred de red virtual **client**. En este ejemplo: **hanadb1-client**, **hanadb2-client** y **hanadb3-client**.  
5. Conexión de las interfaces de red virtual recién creadas a las máquinas virtuales correspondientes  

    1. Vaya a la máquina virtual en [Azure Portal](https://portal.azure.com/#home).  
    2. En el panel de navegación de la izquierda, seleccione Máquinas virtuales. Filtre por el nombre de la máquina virtual, por ejemplo, **hanadb1**. Haga clic en la máquina virtual.  
    3. Como está en Información general, seleccione Detener para desasignar la máquina virtual.  
    4. Seleccione Redes, Asociar interfaz de red. Seleccione en la lista desplegable de "Asociar interfaz de red" las interfaces de red ya creadas para las subredes **`hana`** y **cliente**.  Haga clic en Save (Guardar). 
    5. Repita para las máquinas virtuales restantes. En nuestro ejemplo, **hanadb2** y **hanadb3**.
    6. Deje las máquinas virtuales en estado detenido por ahora. A continuación, habilitaremos [Accelerated Networking](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli) para todas las interfaces de red que se acaban de conectar.  

6. Habilite Accelerated Networking para las interfaces de red adicionales para las subredes **`hana`** y **`client`** .  

    1. Abra [cloud shell](https://azure.microsoft.com/features/cloud-shell/) en [Azure Portal](https://portal.azure.com/#home).  
    2. Ejecute los siguientes comandos para habilitar Accelerated Networking para las interfaces de red adicionales, conectadas a las subredes **`hana`** y **`client`** .  

    <pre><code>
    az network nic update --id /subscriptions/<b>your subscription</b>/resourceGroups/<b>your resource group</b>/providers/Microsoft.Network/networkInterfaces/<b>hanadb1-hana</b> --accelerated-networking true
    az network nic update --id /subscriptions/<b>your subscription</b>/resourceGroups/<b>your resource group</b>/providers/Microsoft.Network/networkInterfaces/<b>hanadb2-hana</b> --accelerated-networking true
    az network nic update --id /subscriptions/<b>your subscription</b>/resourceGroups/<b>your resource group</b>/providers/Microsoft.Network/networkInterfaces/<b>hanadb3-hana</b> --accelerated-networking true
    
    az network nic update --id /subscriptions/<b>your subscription</b>/resourceGroups/<b>your resource group</b>/providers/Microsoft.Network/networkInterfaces/<b>hanadb1-client</b> --accelerated-networking true
    az network nic update --id /subscriptions/<b>your subscription</b>/resourceGroups/<b>your resource group</b>/providers/Microsoft.Network/networkInterfaces/<b>hanadb2-client</b> --accelerated-networking true
    az network nic update --id /subscriptions/<b>your subscription</b>/resourceGroups/<b>your resource group</b>/providers/Microsoft.Network/networkInterfaces/<b>hanadb3-client</b> --accelerated-networking true
    </code></pre>
7. Inicie la máquina virtual.  

    1. En el panel de navegación de la izquierda, seleccione Máquinas virtuales. Filtre por el nombre de la máquina virtual, por ejemplo, **hanadb1**. Haga clic en la máquina virtual.  
    2. Como está en Información general, seleccione Iniciar.  

## <a name="operating-system-configuration-and-preparation"></a>Configuración y preparación del sistema operativo

Los elementos siguientes tienen el prefijo **[A]** : aplicable a todos los nodos; **[1]** : aplicable solo al nodo 1; **[2]** : aplicable solo al nodo 2 o **[3]** : aplicable solo al nodo 3.

1. **[A]** Mantenga los archivos de hosts en las máquinas virtuales. Incluya las entradas para todas las subredes. En este ejemplo se agregaron las siguientes entradas a `/etc/hosts`.  
    <pre><code>
    # Storage
    10.23.2.4   hanadb1
    10.23.2.5   hanadb2
    10.23.2.6   hanadb3
    # Client
    10.23.0.5   hanadb1-client
    10.23.0.6   hanadb2-client
    10.23.0.7   hanadb3-client
    # Hana
    10.23.3.4   hanadb1-hana
    10.23.3.5   hanadb2-hana
    10.23.3.6   hanadb3-hana
    </code></pre>

2. **[A]** Cambie los valores de configuración de DHCP y de la nube para evitar cambios de nombre de host imprevistos.  
    <pre><code>
    vi /etc/sysconfig/network/dhcp
    #Change the following DHCP setting to "no"
    DHCLIENT_SET_HOSTNAME="no"
    vi /etc/sysconfig/network/ifcfg-eth0
    # Edit ifcfg-eth0 
    #Change CLOUD_NETCONFIG_MANAGE='yes' to "no"
    CLOUD_NETCONFIG_MANAGE='no'
    </code></pre>

3. **[A]** Prepare el sistema operativo para ejecutar SAP HANA en sistemas NetApp con NFS, tal y como se describe en la [guía de configuración de SAP HANA en sistemas AFF de NetApp con NFS](https://www.netapp.com/us/media/tr-4435.pdf). Cree un archivo de configuración para las opciones de configuración de NetApp: `/etc/sysctl.d/netapp-hana.conf`.  

    <pre><code>
    vi /etc/sysctl.d/netapp-hana.conf
    # Add the following entries in the configuration file
    net.core.rmem_max = 16777216
    net.core.wmem_max = 16777216
    net.core.rmem_default = 16777216
    net.core.wmem_default = 16777216
    net.core.optmem_max = 16777216
    net.ipv4.tcp_rmem = 65536 16777216 16777216
    net.ipv4.tcp_wmem = 65536 16777216 16777216
    net.core.netdev_max_backlog = 300000
    net.ipv4.tcp_slow_start_after_idle=0
    net.ipv4.tcp_no_metrics_save = 1
    net.ipv4.tcp_moderate_rcvbuf = 1
    net.ipv4.tcp_window_scaling = 1
    net.ipv4.tcp_timestamps = 1
    net.ipv4.tcp_sack = 1
    </code></pre>

4. **[A]** Cree un archivo de configuración con las opciones de configuración de Microsoft para Azure: `/etc/sysctl.d/ms-az.conf`.  

    <pre><code>
    vi /etc/sysctl.d/ms-az.conf
    # Add the following entries in the configuration file
    ipv6.conf.all.disable_ipv6 = 1
    net.ipv4.tcp_max_syn_backlog = 16348
    net.ipv4.ip_local_port_range = 40000 65300
    net.ipv4.conf.all.rp_filter = 0
    sunrpc.tcp_slot_table_entries = 128
    vm.swappiness=10
    </code></pre>

4. **[A]** Ajuste la configuración de sunrpc como se recomienda en la [guía de configuración de SAP HANA en sistemas AFF de NetApp con NFS](https://www.netapp.com/us/media/tr-4435.pdf).  
    <pre><code>
    vi /etc/modprobe.d/sunrpc.conf
    # Insert the following line
    options sunrpc tcp_max_slot_table_entries=128
    </code></pre>

## <a name="mount-the-azure-netapp-files-volumes"></a>Montaje de los volúmenes de Azure NetApp Files

1. **[A]** Cree puntos de montaje para los volúmenes de bases de datos HANA.  
    <pre><code>
    mkdir -p /hana/data/<b>HN1</b>/mnt00001
    mkdir -p /hana/data/<b>HN1</b>/mnt00002
    mkdir -p /hana/log/<b>HN1</b>/mnt00001
    mkdir -p /hana/log/<b>HN1</b>/mnt00002
    mkdir -p /hana/shared
    mkdir -p /usr/sap/<b>HN1</b>
    </code></pre>

2. **[1]** Cree directorios específicos de nodo para `/usr/sap` en recursos compartidos de **HN1**.  

    <pre><code>
    # Create a temporary directory to mount  <b>HN1</b>-shared
    mkdir /mnt/tmp
    mount <b>10.23.1.4</b>:/<b>HN1</b>-shared /mnt/tmp
    cd /mnt/tmp
    mkdir shared usr-sap-<b>hanadb1</b> usr-sap-<b>hanadb2</b> usr-sap-<b>hanadb3</b>
    # unmount /hana/shared
    cd
    umount /mnt/tmp
    </code></pre>

3. **[A]** Compruebe la configuración del dominio NFS. Asegúrese de que el dominio está configurado como **`localdomain`** y la asignación está establecida en **nobody**.  
    <pre><code>
    sudo cat  /etc/idmapd.conf
    # Example
    [General]
    Verbosity = 0
    Pipefs-Directory = /var/lib/nfs/rpc_pipefs
    Domain = <b>localdomain</b>
    [Mapping]
    Nobody-User = <b>nobody</b>
    Nobody-Group = <b>nobody</b>
    </code></pre>

4. **[A]** Deshabilite la asignación del identificador de NFSv4. Ejecute el comando mount para crear la estructura de directorios donde se encuentra `nfs4_disable_idmapping`.  No podrá crear manualmente un directorio en /sys/modules, ya que el acceso está reservado para el kernel o los controladores.  

    <pre><code>
    
    mkdir /mnt/tmp
    mount 10.23.1.4:/HN1-shared /mnt/tmp
    umount  /mnt/tmp
    # Disable NFSv4 idmapping. 
    echo "N" > /sys/module/nfs/parameters/nfs4_disable_idmapping
    </code></pre>

5. **[A]** Cree el grupo de SAP HANA y el usuario manualmente. Los identificadores del grupo **hn1**adm del usuario y de sapsys deben establecerse en los mismos identificadores que se proporcionan durante la incorporación. En este ejemplo, los identificadores se establecen en **1001**. De lo contrario, no será posible acceder a los volúmenes.  Los identificadores del grupo **hn1**adm y sapadm de las cuentas de usuario deben ser los mismos en todas las máquinas virtuales.  

    <pre><code>
    # Create user group 
    sudo groupadd -g 1001 sapsys
    # Create  users 
    sudo useradd <b>hn1</b>adm -u 1001 -g 1001 -d /usr/sap/<b>HN1</b>/home -c "SAP HANA Database System" -s /bin/sh
    sudo useradd sapadm -u 1002 -g 1001 -d /home/sapadm -c "SAP Local Administrator" -s /bin/sh
    # Set the password  for both user ids
    sudo passwd hn1adm
    sudo passwd sapadm
    </code></pre>

6. **[A]** Monte los volúmenes de Azure NetApp Files compartidos.  

    <pre><code>
    sudo vi /etc/fstab
    # Add the following entries
    10.23.1.5:/<b>HN1</b>-data-mnt00001 /hana/data/<b>HN1</b>/mnt00001  nfs   rw,vers=4,minorversion=1,hard,timeo=600,rsize=262144,wsize=262144,intr,noatime,lock,_netdev,sec=sys  0  0
    10.23.1.6:/<b>HN1</b>-data-mnt00002 /hana/data/<b>HN1</b>/mnt00002  nfs   rw,vers=4,minorversion=1,hard,timeo=600,rsize=262144,wsize=262144,intr,noatime,lock,_netdev,sec=sys  0  0
    10.23.1.4:/<b>HN1</b>-log-mnt00001 /hana/log/<b>HN1</b>/mnt00001  nfs   rw,vers=4,minorversion=1,hard,timeo=600,rsize=262144,wsize=262144,intr,noatime,lock,_netdev,sec=sys  0  0
    10.23.1.6:/<b>HN1</b>-log-mnt00002 /hana/log/HN1/mnt00002  nfs   rw,vers=4,minorversion=1,hard,timeo=600,rsize=262144,wsize=262144,intr,noatime,lock,_netdev,sec=sys  0  0
    10.23.1.4:/<b>HN1</b>-shared/shared /hana/shared  nfs   rw,vers=4,minorversion=1,hard,timeo=600,rsize=262144,wsize=262144,intr,noatime,lock,_netdev,sec=sys  0  0
    # Mount all volumes
    sudo mount -a 
    </code></pre>

7. **[1]** Monte los volúmenes específicos del nodo en **hanadb1**.  
    <pre><code>
    sudo vi /etc/fstab
    # Add the following entries
    10.23.1.4:/<b>HN1</b>-shared/usr-sap-<b>hanadb1</b> /usr/sap/<b>HN1</b>  nfs   rw,vers=4,minorversion=1,hard,timeo=600,rsize=262144,wsize=262144,intr,noatime,lock,_netdev,sec=sys  0  0
    # Mount the volume
    sudo mount -a 
    </code></pre>

8. **[2]** Monte los volúmenes específicos del nodo en **hanadb2**.  
    <pre><code>
    sudo vi /etc/fstab
    # Add the following entries
    10.23.1.4:/<b>HN1</b>-shared/usr-sap-<b>hanadb2</b> /usr/sap/<b>HN1</b>  nfs   rw,vers=4,minorversion=1,hard,timeo=600,rsize=262144,wsize=262144,intr,noatime,lock,_netdev,sec=sys  0  0
    # Mount the volume
    sudo mount -a 
    </code></pre>

9. **[3]** Monte los volúmenes específicos del nodo en **hanadb3**.  
    <pre><code>
    sudo vi /etc/fstab
    # Add the following entries
    10.23.1.4:/<b>HN1</b>-shared/usr-sap-<b>hanadb3</b> /usr/sap/<b>HN1</b>  nfs   rw,vers=4,minorversion=1,hard,timeo=600,rsize=262144,wsize=262144,intr,noatime,lock,_netdev,sec=sys  0  0
    # Mount the volume
    sudo mount -a 
    </code></pre>

10. **[A]** Compruebe que todos los volúmenes de HANA están montados con la versión del protocolo NFS **NFSv4**.  
    <pre><code>
    sudo nfsstat -m
    # Verify that flag vers is set to <b>4.1</b> 
    # Example from <b>hanadb1</b>
    /hana/data/<b>HN1</b>/mnt00001 from 10.23.1.5:/<b>HN1</b>-data-mnt00001
     Flags: rw,noatime,vers=<b>4.1</b>,rsize=262144,wsize=262144,namlen=255,hard,proto=tcp,timeo=600,retrans=2,sec=sys,clientaddr=10.23.2.4,local_lock=none,addr=10.23.1.5
    /hana/log/<b>HN1</b>/mnt00002 from 10.23.1.6:/<b>HN1</b>-log-mnt00002
     Flags: rw,noatime,vers=<b>4.1</b>,rsize=262144,wsize=262144,namlen=255,hard,proto=tcp,timeo=600,retrans=2,sec=sys,clientaddr=10.23.2.4,local_lock=none,addr=10.23.1.6
    /hana/data/<b>HN1</b>/mnt00002 from 10.23.1.6:/<b>HN1</b>-data-mnt00002
     Flags: rw,noatime,vers=<b>4.1</b>,rsize=262144,wsize=262144,namlen=255,hard,proto=tcp,timeo=600,retrans=2,sec=sys,clientaddr=10.23.2.4,local_lock=none,addr=10.23.1.6
    /hana/log/<b>HN1</b>/mnt00001 from 10.23.1.4:/<b>HN1</b>-log-mnt00001
    Flags: rw,noatime,vers=<b>4.1</b>,rsize=262144,wsize=262144,namlen=255,hard,proto=tcp,timeo=600,retrans=2,sec=sys,clientaddr=10.23.2.4,local_lock=none,addr=10.23.1.4
    /usr/sap/<b>HN1</b> from 10.23.1.4:/<b>HN1</b>-shared/usr-sap-<b>hanadb1</b>
     Flags: rw,noatime,vers=<b>4.1</b>,rsize=262144,wsize=262144,namlen=255,hard,proto=tcp,timeo=600,retrans=2,sec=sys,clientaddr=10.23.2.4,local_lock=none,addr=10.23.1.4
    /hana/shared from 10.23.1.4:/<b>HN1</b>-shared/shared
     Flags: rw,noatime,vers=<b>4.1</b>,rsize=262144,wsize=262144,namlen=255,hard,proto=tcp,timeo=600,retrans=2,sec=sys,clientaddr=10.23.2.4,local_lock=none,addr=10.23.1.4
    </code></pre>

## <a name="installation"></a>Instalación  

En este ejemplo para implementar SAP HANA en la configuración de escalabilidad horizontal con un nodo en espera con Azure, usamos HANA 2.0 SP4.  

### <a name="prepare-for-hana-installation"></a>Preparación de la instalación de HANA

1. **[A]** Establezca la contraseña raíz antes de la instalación de HANA (puede deshabilitar la contraseña raíz una vez completada la instalación). Ejecute como `passwd` del comando `root`.  

2. **[1]** Compruebe que puede hacer un ssh en **hanadb2** y **hanadb3**, sin que se le solicite la contraseña.  
    <pre><code>
    ssh root@<b>hanadb2</b>
    ssh root@<b>hanadb3</b>
    </code></pre>

3. **[A]** Instale paquetes adicionales, necesarios para HANA 2.0 SP4. Consulte la nota de SAP [2593824](https://launchpad.support.sap.com/#/notes/2593824) para más información. 
    <pre><code>
    sudo zypper install libgcc_s1 libstdc++6 libatomic1 
    </code></pre>

4. **[2, 3]** Cambie la propiedad de los directorios `data` y `log` de SAP HANA a **hn1**adm.   
    <pre><code>
    # Execute as root
    sudo chown hn1adm:sapsys /hana/data/<b>HN1</b>
    sudo chown hn1adm:sapsys /hana/log/<b>HN1</b>
    </code></pre>

### <a name="hana-installation"></a>Instalación de HANA

1. **[1]** Instale SAP HANA, siguiendo las instrucciones de la [guía de instalación y actualización de SAP HANA 2.0](https://help.sap.com/viewer/2c1988d620e04368aa4103bf26f17727/2.0.04/en-US/7eb0167eb35e4e2885415205b8383584.html). En este ejemplo, se instala la escalabilidad horizontal de SAP HANA con el nodo maestro, un nodo de trabajo y uno en espera.  
   Inicie el programa **hdblcm** desde el directorio de software de instalación de HANA. Use el parámetro `internal_network` y pase el espacio de direcciones a la subred que se usa para la comunicación interna entre nodos de HANA.  

    <pre><code>./hdblcm --internal_network=10.23.3.0/24
    </code></pre>

   Escriba los siguientes valores en el símbolo del sistema.

     * Elija una acción:  Escriba **1** (para la instalación).
     * Seleccione los componentes adicionales para la instalación: Escriba **2, 3**.
     * Escriba la ruta de acceso de instalación:  Presione Entrar (el valor predeterminado es /hana/shared)
     * Escriba el nombre del host local: Presione ENTRAR para aceptar el valor predeterminado.
     * ¿Desea agregar hosts al sistema? Escriba **y**.
     * Escriba los nombres de host separados por comas que se van a agregar: **hanadb2, hanadb3**.
     * Escriba el nombre de usuario raíz [raíz]: presione Entrar para aceptar el valor predeterminado.
     * Escriba la contraseña del usuario raíz. Escriba la contraseña raíz.
     * Seleccione los roles para el host hanadb2: Escriba **1** (para trabajo)
     * Escriba el grupo de conmutación por error de host para el host hanadb2 [valor predeterminado]:  Presione ENTRAR para aceptar el valor predeterminado.
     * Escriba el número de partición de almacenamiento para el host hanadb2 [<<assign automatically>>]: Presione ENTRAR para aceptar el valor predeterminado.
     * Escriba el grupo de trabajo para el host hanadb2 [valor predeterminado]: Presione ENTRAR para aceptar el valor predeterminado.
     * Seleccione los roles para host hanadb3: Escriba **2** (para el nodo de espera)
     * Escriba el grupo de conmutación por error de host para el host hanadb3 [valor predeterminado]: Presione ENTRAR para aceptar el valor predeterminado.
     * Escriba el grupo de trabajo para el host hanadb3 [valor predeterminado]: Presione ENTRAR para aceptar el valor predeterminado.
     * Escriba el identificador del sistema de SAP HANA: Escriba **HN1**.
     * Escriba el número de instancia [00]: Escriba **03**.
     * Escriba el grupo de trabajo de host local [predeterminado]: Presione ENTRAR para aceptar el valor predeterminado.
     * Seleccione Uso del sistema / Escriba el índice [4]: Escriba **4** (para el valor personalizado)
     * Escriba la ubicación de los volúmenes de datos [/hana/data/HN1]:  Presione ENTRAR para aceptar el valor predeterminado.
     * Escriba la ubicación de los volúmenes de registros [/hana/log/HN1]: Presione ENTRAR para aceptar el valor predeterminado.
     * ¿Restringir la asignación de memoria máxima? [n]: Escriba **n.**
     * Escriba el nombre de host del certificado para el host hanadb1 [hanadb1]: Presione ENTRAR para aceptar el valor predeterminado.
     * Escriba el nombre de host del certificado para el host hanadb2 [hanadb2]: Presione ENTRAR para aceptar el valor predeterminado.
     * Escriba el nombre de host del certificado para el host hanadb3 [hanadb3]: Presione ENTRAR para aceptar el valor predeterminado.
     * Escriba la contraseña del administrador del sistema (hn1adm): Especificación de la contraseña
     * Escriba la contraseña del usuario (SYSTEM) de la base de datos: Escriba la contraseña del sistema.
     * Confirme la contraseña del usuario (SYSTEM) de la base de datos: Escriba la contraseña del sistema.
     * ¿Reiniciar el sistema tras el reinicio de la máquina? [n]: Escriba **n**. 
     * ¿Desea continuar (s/n)? Valide el resumen y, si todo es correcto, escriba **y**.


2. **[1]** Compruebe global.ini.  

   Muestre global.ini y asegúrese de que la configuración de la comunicación interna entre nodos de SAP HANA está en su lugar. Compruebe la sección de **comunicación**. Debe tener el espacio de direcciones para la subred **`hana`** y `listeninterface` debe establecerse en `.internal`. Compruebe la sección **internal_hostname_resolution**. Debe tener las direcciones IP de las máquinas virtuales de HANA que pertenezcan a la subred **`hana`** .  

   <pre><code>
    sudo cat /usr/sap/<b>HN1</b>/SYS/global/hdb/custom/config/global.ini
    # Example 
    #global.ini last modified 2019-09-10 00:12:45.192808 by hdbnameserve
    [communication]
    internal_network = <b>10.23.3/24</b>
    listeninterface = .internal
    [internal_hostname_resolution]
    <b>10.23.3.4</b> = <b>hanadb1</b>
    <b>10.23.3.5</b> = <b>hanadb2</b>
    <b>10.23.3.6</b> = <b>hanadb3</b>
   </code></pre>

3. **[1]** Agregue la asignación de host para asegurarse de que las direcciones IP del cliente se utilizan para la comunicación del cliente. Agregue la sección `public_host_resolution` y agregue las direcciones IP correspondientes desde la subred del cliente.  

   <pre><code>
    sudo vi /usr/sap/HN1/SYS/global/hdb/custom/config/global.ini
    #Add the section
    [public_hostname_resolution]
    map_<b>hanadb1</b> = <b>10.23.0.5</b>
    map_<b>hanadb2</b> = <b>10.23.0.6</b>
    map_<b>hanadb3</b> = <b>10.23.0.7</b>
   </code></pre>

4. **[1]** Reinicie SAP HANA para activar los cambios.  
   <pre><code>
    sudo -u <b>hn1</b>adm /usr/sap/hostctrl/exe/sapcontrol -nr <b>03</b> -function StopSystem HDB
    sudo -u <b>hn1</b>adm /usr/sap/hostctrl/exe/sapcontrol -nr <b>03</b> -function StartSystem HDB
   </code></pre>

5. **[1]** Compruebe que la interfaz de cliente utilizará las direcciones IP de la subred **client** para la comunicación.  
   <pre><code>
    sudo -u hn1adm /usr/sap/HN1/HDB03/exe/hdbsql -u SYSTEM -p "<b>password</b>" -i 03 -d SYSTEMDB 'select * from SYS.M_HOST_INFORMATION'|grep net_publicname
    # Expected result
    "<b>hanadb3</b>","net_publicname","<b>10.23.0.7</b>"
    "<b>hanadb2</b>","net_publicname","<b>10.23.0.6</b>"
    "<b>hanadb1</b>","net_publicname","<b>10.23.0.5</b>"
   </code></pre>

   Consulte la nota de SAP [2183363 - Configuration of SAP HANA internal network](https://launchpad.support.sap.com/#/notes/2183363) (2183363 - Configuración de la red interna de SAP HANA) para más información sobre cómo comprobar la configuración.  

6. Para optimizar SAP HANA para el almacenamiento de Azure NetApp Files subyacente, establezca los siguientes parámetros de SAP HANA:

   - `max_parallel_io_requests` **128**
   - `async_read_submit` **activado**
   - `async_write_submit_active` **activado**
   - `async_write_submit_blocks` **todo**

   Para más información, consulte la [guía de configuración de SAP HANA en sistemas AFF de NetApp con NFS](https://www.netapp.com/us/media/tr-4435.pdf). 

   A partir de los sistemas SAP HANA 2.0, puede establecer los parámetros en `global.ini`. Consulte la nota de SAP [1999930](https://launchpad.support.sap.com/#/notes/1999930).  
   En el caso de los sistemas SAP HANA 1.0, en las versiones hasta la SPS12, estos parámetros se pueden establecer durante la instalación, tal y como se describe en la nota de SAP [2267798](https://launchpad.support.sap.com/#/notes/2267798).  

7. El almacenamiento usado por Azure NetApp Files tiene un límite de tamaño de archivo o 16 TB. SAP HANA no es consciente implícitamente de la limitación de almacenamiento y no creará automáticamente un nuevo archivo de datos, cuando se alcance el límite de tamaño de archivo de 16 TB. Cuando SAP HANA intente aumentar el tamaño del archivo más allá de 16 TB, se producirán errores y finalmente se bloqueará el servidor de índice. 

   > [!IMPORTANT]
   > Para evitar que SAP HANA intente aumentar los archivos de datos más allá del [límite de 16 TB](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-resource-limits) del subsistema de almacenamiento, establezca los parámetros siguientes en `global.ini`.  
   > -  datavolume_striping=true
   > - datavolume_striping_size_gb = 15000. Para más información, consulte la nota SAP [2400005](https://launchpad.support.sap.com/#/notes/2400005).
   > Tenga en cuenta la nota de SAP [2631285](https://launchpad.support.sap.com/#/notes/2631285). 

## <a name="test-sap-hana-failover"></a>Prueba de la conmutación por error de SAP HANA 

1. Simulación del bloqueo de nodos en un nodo de trabajo de SAP HANA  

   Ejecute los siguientes comandos como **hn1**adm para capturar el estado del entorno, antes de simular el bloqueo del nodo.  

   <pre><code>
    # Check the landscape status
    python /usr/sap/<b>HN1</b>/HDB<b>03</b>/exe/python_support/landscapeHostConfiguration.py
    | Host    | Host   | Host   | Failover | Remove | Storage   | Storage   | Failover | Failover | NameServer | NameServer | IndexServer | IndexServer | Host    | Host    | Worker  | Worker  |
    |         | Active | Status | Status   | Status | Config    | Actual    | Config   | Actual   | Config     | Actual     | Config      | Actual      | Config  | Actual  | Config  | Actual  |
    |         |        |        |          |        | Partition | Partition | Group    | Group    | Role       | Role       | Role        | Role        | Roles   | Roles   | Groups  | Groups  |
    | ------- | ------ | ------ | -------- | ------ | --------- | --------- | -------- | -------- | ---------- | ---------- | ----------- | ----------- | ------- | ------- | ------- | ------- |
    | hanadb1 | yes    | ok     |          |        |         1 |         1 | default  | default  | master 1   | master     | worker      | master      | worker  | worker  | default | default |
    | hanadb2 | yes    | ok     |          |        |         2 |         2 | default  | default  | master 2   | slave      | worker      | slave       | worker  | worker  | default | default |
    | hanadb3 | yes    | ignore |          |        |         0 |         0 | default  | default  | master 3   | slave      | standby     | standby     | standby | standby | default | -       |
    # Check the instance status
    sapcontrol -nr <b>03</b>  -function GetSystemInstanceList
    GetSystemInstanceList
    OK
    hostname, instanceNr, httpPort, httpsPort, startPriority, features, dispstatus
    hanadb2, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb1, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb3, 3, 50313, 50314, 0.3, HDB|HDB_STANDBY, GREEN
   </code></pre>

   Ejecute el siguiente comando como raíz en el nodo de trabajo, que en este caso es **hanadb2**, para simular un bloqueo de nodo.  
   <pre><code>
    echo b > /proc/sysrq-trigger
   </code></pre>

   Supervise el sistema para la finalización de la conmutación por error. Cuando se haya completado la conmutación por error, capture el estado. Debe parecerse al estado que se muestra a continuación.  
   <pre><code>
    # Check the instance status
    sapcontrol -nr <b>03</b>  -function GetSystemInstanceList
    GetSystemInstanceList
    OK
    hostname, instanceNr, httpPort, httpsPort, startPriority, features, dispstatus
    hanadb1, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb3, 3, 50313, 50314, 0.3, HDB|HDB_STANDBY, GREEN
    hanadb2, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GRAY
    # Check the landscape status
    /usr/sap/HN1/HDB03/exe/python_support> python landscapeHostConfiguration.py
    | Host    | Host   | Host   | Failover | Remove | Storage   | Storage   | Failover | Failover | NameServer | NameServer | IndexServer | IndexServer | Host    | Host    | Worker  | Worker  |
    |         | Active | Status | Status   | Status | Config    | Actual    | Config   | Actual   | Config     | Actual     | Config      | Actual      | Config  | Actual  | Config  | Actual  |
    |         |        |        |          |        | Partition | Partition | Group    | Group    | Role       | Role       | Role        | Role        | Roles   | Roles   | Groups  | Groups  |
    | ------- | ------ | ------ | -------- | ------ | --------- | --------- | -------- | -------- | ---------- | ---------- | ----------- | ----------- | ------- | ------- | ------- | ------- |
    | hanadb1 | yes    | ok     |          |        |         1 |         1 | default  | default  | master 1   | master     | worker      | master      | worker  | worker  | default | default |
    | hanadb2 | no     | info   |          |        |         2 |         0 | default  | default  | master 2   | slave      | worker      | standby     | worker  | standby | default | -       |
    | hanadb3 | yes    | info   |          |        |         0 |         2 | default  | default  | master 3   | slave      | standby     | slave       | standby | worker  | default | default |
   </code></pre>

   > [!IMPORTANT]
   > Para evitar retrasos con la conmutación por error de SAP HANA, cuando un nodo experimenta un estado de pánico del kernel, establezca `kernel.panic` en 20 segundos en **todas** las máquinas virtuales de HANA. La configuración está terminada en `/etc/sysctl`. Reinicie las máquinas virtuales para activar el cambio. La conmutación por error, cuando un nodo está experimentando un estado de pánico del kernel, puede tardar 10 minutos o más, si no se realiza este cambio.  

2. Eliminación del servidor de nombres  
   Ejecute los siguientes comandos como **hn1**adm para comprobar el estado del entorno, antes de la prueba:  

   <pre><code>
    #Landscape status 
    python /usr/sap/HN1/HDB03/exe/python_support/landscapeHostConfiguration.py
    | Host    | Host   | Host   | Failover | Remove | Storage   | Storage   | Failover | Failover | NameServer | NameServer | IndexServer | IndexServer | Host    | Host    | Worker | Worker  |
    |         | Active | Status | Status   | Status | Config    | Actual    | Config   | Actual   | Config     | Actual     | Config      | Actual      | Config  | Actual  | Config  | Actual  |
    |         |        |        |          |        | Partition | Partition | Group    | Group    | Role       | Role       | Role        | Role        | Roles   | Roles   | Groups  | Groups  |
    | ------- | ------ | ------ | -------- | ------ | --------- | --------- | -------- | -------- | ---------- | ---------- | ----------- | ----------- | ------- | ------- | ------- | ------- |
    | hanadb1 | yes    | ok     |          |        |         1 |         1 | default  | default  | master 1   | master     | worker      | master      | worker  | worker  | default | default |
    | hanadb2 | yes    | ok     |          |        |         2 |         2 | default  | default  | master 2   | slave      | worker      | slave       | worker  | worker  | default | default |
    | hanadb3 | no     | ignore |          |        |         0 |         0 | default  | default  | master 3   | slave      | standby     | standby     | standby | standby | default | -       |
    # Check the instance status
    sapcontrol -nr 03  -function GetSystemInstanceList
    GetSystemInstanceList
    OK
    hostname, instanceNr, httpPort, httpsPort, startPriority, features, dispstatus
    hanadb2, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb1, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb3, 3, 50313, 50314, 0.3, HDB|HDB_STANDBY, GRAY
   </code></pre>

   Ejecute los siguientes comandos como **hn1**adm en el nodo principal activo, que en este caso es **hanadb1**.  

   <pre><code>
    hn1adm@hanadb1:/usr/sap/HN1/HDB03> HDB kill
   </code></pre>

   El nodo en espera **hanadb3** asumirá el control como nodo maestro. Estado del recurso una vez completada la prueba de conmutación por error:  

   <pre><code>
    # Check the instance status
    sapcontrol -nr 03 -function GetSystemInstanceList
    GetSystemInstanceList
    OK
    hostname, instanceNr, httpPort, httpsPort, startPriority, features, dispstatus
    hanadb2, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb1, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GRAY
    hanadb3, 3, 50313, 50314, 0.3, HDB|HDB_STANDBY, GREEN
    # Check the landscape status
    python /usr/sap/HN1/HDB03/exe/python_support/landscapeHostConfiguration.py
    | Host    | Host   | Host   | Failover | Remove | Storage   | Storage   | Failover | Failover | NameServer | NameServer | IndexServer | IndexServer | Host    | Host    | Worker  | Worker  |
    |         | Active | Status | Status   | Status | Config    | Actual    | Config   | Actual   | Config     | Actual     | Config      | Actual      | Config  | Actual  | Config  | Actual  |
    |         |        |        |          |        | Partition | Partition | Group    | Group    | Role       | Role       | Role        | Role        | Roles   | Roles   | Groups  | Groups  |
    | ------- | ------ | ------ | -------- | ------ | --------- | --------- | -------- | -------- | ---------- | ---------- | ----------- | ----------- | ------- | ------- | ------- | ------- |
    | hanadb1 | no     | info   |          |        |         1 |         0 | default  | default  | master 1   | slave      | worker      | standby     | worker  | standby | default | -       |
    | hanadb2 | yes    | ok     |          |        |         2 |         2 | default  | default  | master 2   | slave      | worker      | slave       | worker  | worker  | default | default |
    | hanadb3 | yes    | info   |          |        |         0 |         1 | default  | default  | master 3   | master     | standby     | master      | standby | worker  | default | default |
   </code></pre>

   Vuelva a iniciar la instancia de HANA en **hanadb1**, es decir, en la misma máquina virtual donde se ha eliminado el servidor de nombres. El nodo **hanadb1** volverá a unirse al entorno y mantendrá su rol de nodo en espera.  
   <pre><code>
    hn1adm@hanadb1:/usr/sap/HN1/HDB03> HDB start
   </code></pre>

   Espere el siguiente estado, después de que se haya iniciado SAP HANA en **hanadb1**:  
   <pre><code>
    # Check the instance status
    sapcontrol -nr 03 -function GetSystemInstanceList
    GetSystemInstanceList
    OK
    hostname, instanceNr, httpPort, httpsPort, startPriority, features, dispstatus
    hanadb1, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb2, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb3, 3, 50313, 50314, 0.3, HDB|HDB_STANDBY, GREEN
    # Check the landscape status
    python /usr/sap/HN1/HDB03/exe/python_support/landscapeHostConfiguration.py
    | Host    | Host   | Host   | Failover | Remove | Storage   | Storage   | Failover | Failover | NameServer | NameServer | IndexServer | IndexServer | Host    | Host    | Worker  | Worker  |
    |         | Active | Status | Status   | Status | Config    | Actual    | Config   | Actual   | Config     | Actual     | Config      | Actual      | Config  | Actual  | Config  | Actual  |
    |         |        |        |          |        | Partition | Partition | Group    | Group    | Role       | Role       | Role        | Role        | Roles   | Roles   | Groups  | Groups  |
    | ------- | ------ | ------ | -------- | ------ | --------- | --------- | -------- | -------- | ---------- | ---------- | ----------- | ----------- | ------- | ------- | ------- | ------- |
    | hanadb1 | yes    | info   |          |        |         1 |         0 | default  | default  | master 1   | slave      | worker      | standby     | worker  | standby | default | -       |
    | hanadb2 | yes    | ok     |          |        |         2 |         2 | default  | default  | master 2   | slave      | worker      | slave       | worker  | worker  | default | default |
    | hanadb3 | yes    | info   |          |        |         0 |         1 | default  | default  | master 3   | master     | standby     | master      | standby | worker  | default | default |
   </code></pre>

   Ahora, vuelva a eliminar el servidor de nombres en el nodo maestro actualmente activo, es decir, en hanadb3.  
   <pre><code>
    hn1adm@hanadb3:/usr/sap/HN1/HDB03> HDB kill
   </code></pre>

   El nodo **hanadb1** reanudará el rol del nodo maestro. El estado, una vez completada la prueba de conmutación por error, tendrá el siguiente aspecto:
   <pre><code>
    # Check the instance status
    sapcontrol -nr 03  -function GetSystemInstanceList & python /usr/sap/HN1/HDB03/exe/python_support/landscapeHostConfiguration.py
    GetSystemInstanceList
    OK
    hostname, instanceNr, httpPort, httpsPort, startPriority, features, dispstatus
    GetSystemInstanceList
    OK
    hostname, instanceNr, httpPort, httpsPort, startPriority, features, dispstatus
    hanadb1, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb2, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb3, 3, 50313, 50314, 0.3, HDB|HDB_STANDBY, GRAY
    # Check the landscape status
    python /usr/sap/HN1/HDB03/exe/python_support/landscapeHostConfiguration.py
    | Host    | Host   | Host   | Failover | Remove | Storage   | Storage   | Failover | Failover | NameServer | NameServer | IndexServer | IndexServer | Host    | Host    | Worker  | Worker  |
    |         | Active | Status | Status   | Status | Config    | Actual    | Config   | Actual   | Config     | Actual     | Config      | Actual      | Config  | Actual  | Config  | Actual  |
    |         |        |        |          |        | Partition | Partition | Group    | Group    | Role       | Role       | Role        | Role        | Roles   | Roles   | Groups  | Groups  |
    | ------- | ------ | ------ | -------- | ------ | --------- | --------- | -------- | -------- | ---------- | ---------- | ----------- | ----------- | ------- | ------- | ------- | ------- |
    | hanadb1 | yes    | ok     |          |        |         1 |         1 | default  | default  | master 1   | master     | worker      | master      | worker  | worker  | default | default |
    | hanadb2 | yes    | ok     |          |        |         2 |         2 | default  | default  | master 2   | slave      | worker      | slave       | worker  | worker  | default | default |
    | hanadb3 | no     | ignore |          |        |         0 |         0 | default  | default  | master 3   | slave      | standby     | standby     | standby | standby | default | -       |
   </code></pre>

   Inicie SAP HANA en **hanadb3**: estará preparado para servir como nodo en espera.  
   <pre><code>
    hn1adm@hanadb3:/usr/sap/HN1/HDB03> HDB start
   </code></pre>

   Estado después de que SAP HANA se haya iniciado en **hanadb3**.  
   <pre><code>
    # Check the instance status
    sapcontrol -nr 03  -function GetSystemInstanceList & python /usr/sap/HN1/HDB03/exe/python_support/landscapeHostConfiguration.py
    GetSystemInstanceList
    OK
    hostname, instanceNr, httpPort, httpsPort, startPriority, features, dispstatus
    GetSystemInstanceList
    OK
    hostname, instanceNr, httpPort, httpsPort, startPriority, features, dispstatus
    hanadb1, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb2, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb3, 3, 50313, 50314, 0.3, HDB|HDB_STANDBY, GRAY
    # Check the landscape status
    python /usr/sap/HN1/HDB03/exe/python_support/landscapeHostConfiguration.py
    | Host    | Host   | Host   | Failover | Remove | Storage   | Storage   | Failover | Failover | NameServer | NameServer | IndexServer | IndexServer | Host    | Host    | Worker  | Worker  |
    |         | Active | Status | Status   | Status | Config    | Actual    | Config   | Actual   | Config     | Actual     | Config      | Actual      | Config  | Actual  | Config  | Actual  |
    |         |        |        |          |        | Partition | Partition | Group    | Group    | Role       | Role       | Role        | Role        | Roles   | Roles   | Groups  | Groups  |
    | ------- | ------ | ------ | -------- | ------ | --------- | --------- | -------- | -------- | ---------- | ---------- | ----------- | ----------- | ------- | ------- | ------- | ------- |
    | hanadb1 | yes    | ok     |          |        |         1 |         1 | default  | default  | master 1   | master     | worker      | master      | worker  | worker  | default | default |
    | hanadb2 | yes    | ok     |          |        |         2 |         2 | default  | default  | master 2   | slave      | worker      | slave       | worker  | worker  | default | default |
    | hanadb3 | no     | ignore |          |        |         0 |         0 | default  | default  | master 3   | slave      | standby     | standby     | standby | standby | default | -       |
   </code></pre>

## <a name="next-steps"></a>Pasos siguientes

* [Planeamiento e implementación de Azure Virtual Machines para SAP][planning-guide]
* [Implementación de Azure Virtual Machines para SAP][deployment-guide]
* [Implementación de DBMS de Azure Virtual Machines para SAP][dbms-guide]
* Para saber cómo establecer una estrategia de alta disponibilidad y recuperación ante desastres de SAP 
* HANA en Azure (instancias grandes), consulte [Alta disponibilidad y recuperación ante desastres de SAP HANA en Azure (instancias grandes)](hana-overview-high-availability-disaster-recovery.md).
* Para más información sobre cómo establecer la alta disponibilidad y planear la recuperación ante desastres de SAP HANA en Azure Virtual Machines, consulte [Alta disponibilidad de SAP HANA en las máquinas virtuales de Azure][sap-hana-ha].
