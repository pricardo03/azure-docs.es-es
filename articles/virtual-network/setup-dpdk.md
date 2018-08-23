---
title: DPDK en una máquina virtual Linux de Azure | Microsoft Docs
description: Aprenda a configurar DPDK en una máquina virtual Linux.
services: virtual-network
documentationcenter: na
author: laxmanrb
manager: gedegrac
editor: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: ''
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/27/2018
ms.author: labattul
ms.openlocfilehash: 205a1e399eadd268ffaa390a7ebb4397fda9feff
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2018
ms.locfileid: "42444660"
---
# <a name="setup-dpdk-in-a-linux-virtual-machine"></a>Configuración de DPDK en una máquina virtual Linux

Data Plane Development Kit (DPDK) de Azure ofrece una plataforma más rápida de procesamiento de paquetes de espacio de usuario para aplicaciones de rendimiento intensivo que omiten la pila de red de kernel de la máquina virtual.

El procesamiento normal de paquetes mediante la pila de red de kernel está controlado por interrupciones. Cada vez que la interfaz de red recibe paquetes entrantes, hay una interrupción en el kernel para procesar el cambio de paquete y de contexto del espacio del kernel al espacio del usuario. DPDK elimina el cambio de contextos y el método controlado por interrupciones en favor de una implementación de espacio de usuario que usa controladores en modo sondeo para un rápido procesamiento de paquetes.

DPDK consta de un conjunto de bibliotecas de espacio de usuario que proporciona acceso a los recursos de nivel más bajo como hardware, núcleos lógicos, administración de memoria y controladores en modo sondeo para las tarjetas de interfaz de red.

DPDK se puede ejecutar en máquinas virtuales de Azure que admitan varias distribuciones del sistema operativo. DPDK proporciona una diferencia de rendimiento clave a la hora de controlar implementaciones de virtualización de funciones de red, en forma de aplicaciones virtuales de red (NVA) como, por ejemplo, un enrutador virtual, firewall, VPN, equilibrador de carga, núcleo de paquete evolucionado y aplicaciones de denegación de servicio (DDoS).

## <a name="benefit"></a>Ventaja

**Más paquetes por segundo (PPS)**: si se omite el kernel y se toma el control de los paquetes en el espacio del usuario se reduce el recuento de ciclos mediante la eliminación del cambio de contexto y se mejora la tasa de paquetes procesados por segundo en máquinas virtuales Linux de Azure.


## <a name="supported-operating-systems"></a>Sistemas operativos compatibles

Se admiten las siguientes distribuciones desde la galería de Azure:

| SO Linux     | Versión de kernel        |
|--------------|----------------       |
| Ubuntu 16.04 | 4.15.0-1015-azure     |
| Ubuntu 18.04 | 4.15.0-1015-azure     |
| SLES 15      | 4.12.14-5.5-azure     |
| RHEL 7.5     | 3.10.0-862.9.1.el7    |
| CentOS 7.5   | 3.10.0-862.3.3.el7    |

**Compatibilidad de kernel personalizado**

Consulte las [revisiones para la compilación de un kernel de Linux adaptado a Azure](https://github.com/microsoft/azure-linux-kernel) para cualquier versión de kernel de Linux que no aparezca, o si desea obtener más información, póngase en contacto con [azuredpdk@microsoft.com](mailto:azuredpdk@microsoft.com). 

## <a name="region-support"></a>Regiones admitidas

Todas las regiones de Azure admiten DPDK.

## <a name="prerequisites"></a>Requisitos previos

Se deben habilitar las redes aceleradas en una máquina virtual Linux. La máquina virtual debe tener al menos dos interfaces de red, con una interfaz para la administración. Aprenda a [crear una máquina virtual Linux con redes aceleradas habilitadas](create-vm-accelerated-networking-cli.md).

## <a name="install-dpdk-dependencies"></a>Instalación de dependencias de DPDK

### <a name="ubuntu-1604"></a>Ubuntu 16.04

```bash
sudo add-apt-repository ppa:canonical-server/dpdk-azure -y
sudo apt-get update
sudo apt-get install -y librdmacm-dev librdmacm1 build-essential libnuma-dev libmnl-dev
```

### <a name="ubuntu-1804"></a>Ubuntu 18.04

```bash
sudo apt-get update
sudo apt-get install -y librdmacm-dev librdmacm1 build-essential libnuma-dev libmnl-dev
```

### <a name="rhel75centos-75"></a>RHEL7.5/CentOS 7.5

```bash
yum -y groupinstall "Infiniband Support"
sudo dracut --add-drivers "mlx4_en mlx4_ib mlx5_ib" -f
yum install -y gcc kernel-devel-`uname -r` numactl-devel.x86_64 librdmacm-devel libmnl-devel
```

### <a name="sles-15"></a>SLES 15

**Kernel de Azure**

```bash
zypper  \
  --no-gpg-checks \
  --non-interactive \
  --gpg-auto-import-keys install kernel-azure kernel-devel-azure gcc make libnuma-devel numactl librdmacm1 rdma-core-devel
```

**Kernel predeterminado**

```bash
zypper \
  --no-gpg-checks \
  --non-interactive \
  --gpg-auto-import-keys install kernel-default-devel gcc make libnuma-devel numactl librdmacm1 rdma-core-devel
```

## <a name="setup-virtual-machine-environment-once"></a>Configuración del entorno de la máquina virtual (una vez)

1. [Descargue la versión de DPDK más reciente](https://core.dpdk.org/download). Se requiere la versión 18.02 o posterior para Azure.
2. En primer lugar, cree la configuración predeterminada con `make config T=x86_64-native-linuxapp-gcc`.
3. Habilite Mellanox PMDs en la configuración generada con `sed -ri 's,(MLX._PMD=)n,\1y,' build/.config`.
4. Compile con `make`.
5. Instale con `make install DESTDIR=<output folder>`.

# <a name="configure-runtime-environment"></a>Configuración del entorno de tiempo de ejecución

Ejecute los siguientes comandos una vez, después de reiniciar:

1. Macropáginas

   * Configure la macropágina ejecutando el siguiente comando, una vez para todos los nodos NUMA:

     ```bash
     echo 1024 | sudo tee
     /sys/devices/system/node/node*/hugepages/hugepages-2048kB/nr_hugepages
     ```

   *  Cree un directorio para el montaje con `mkdir /mnt/huge`.
   *  Monte macropáginas con `mount -t hugetlbfs nodev /mnt/huge`.
   *  Compruebe que las macropáginas están reservadas con `grep Huge /proc/meminfo`.

     > [!NOTE]
     > Hay una manera de modificar el archivo GRUB para que las macropáginas se reserven en el arranque siguiendo las [instrucciones](http://dpdk.org/doc/guides/linux_gsg/sys_reqs.html#use-of-hugepages-in-the-linux-environment) de DPDK. La instrucción está en la parte inferior de la página. Si la ejecución se realiza en una máquina virtual Linux de Azure, modifique en su lugar los archivos de /etc/config/grub.d, para reservar las macropáginas en los reinicios.

2. Direcciones MAC e IP: use `ifconfig –a` para ver la dirección IP y MAC de las interfaces de red. La interfaz de red *VF* y la interfaz de red *NETVSC* tienen la misma dirección MAC, pero solo la interfaz de red *NETVSC* tiene una dirección IP. Las interfaces VF se ejecutan como interfaces subordinadas de las interfaces NETVSC.

3. Direcciones PCI

   * Averigüe qué dirección PCI usar para *VF* con `ethtool -i <vf interface name>`.
   * Asegúrese de que testpmd no toma accidentalmente el control del dispositivo PCI de VF para *eth0*, si *eth0* tiene las redes aceleradas habilitadas. Si la aplicación DPDK ha tomado accidentalmente el control de la interfaz de red de administración y provoca la pérdida de la conexión SSH, use la consola serie para terminar la aplicación DPDK, o para detener o iniciar la máquina virtual.

4. Cargue *ibuverbs* en cada reinicio con `modprobe -a ib_uverbs`. Solo para SLES 15, cargue también *mlx4_ib* con `modprobe -a mlx4_ib`.

## <a name="failsafe-pmd"></a>PMD a prueba de errores

Las aplicaciones de DPDK se deben ejecutar sobre el PMD a prueba de errores que se expone en Azure. Si la aplicación se ejecuta directamente sobre el PMD de VF, no recibirá **todos** los paquetes destinados a la máquina virtual, ya que algunos paquetes se mostrarán sobre la interfaz sintética. La ejecución sobre el PMD a prueba de errores garantiza que la aplicación recibirá todos los paquetes destinados a ella y también garantiza que la aplicación continuará ejecutándose en modo DPDK aunque se revoque VF cuando el host esté en proceso de mantenimiento. Para más información sobre PMD a prueba de errores, consulte la [biblioteca de controladores de modo de sondeo a prueba de errores](http://doc.dpdk.org/guides/nics/fail_safe.html).

## <a name="run-testpmd"></a>Ejecutar testpmd

Use `sudo` antes del comando *testpmd* para que la ejecución se realice en modo raíz.

### <a name="basic-sanity-check-failsafe-adapter-initialization"></a>Básico: comprobación de integridad, inicialización del adaptador a prueba de errores

1. Ejecute los comandos siguientes para iniciar una aplicación testpmd de puerto único:

   ```bash
   testpmd -w <pci address from previous step> \
     --vdev="net_vdev_netvsc0,iface=eth1" \
     -- -i \
     --port-topology=chained
    ```

2. Ejecute los comandos siguientes para iniciar una aplicación testpmd de puerto dual:

   ```bash
   testpmd -w <pci address nic1> \
   -w <pci address nic2> \
   --vdev="net_vdev_netvsc0,iface=eth1" \
   --vdev="net_vdev_netvsc1,iface=eth2" \
   -- -i
   ```

   Si la ejecución se realiza con más de 2 NIC, el argumento `--vdev` seguirá este patrón: `net_vdev_netvsc<id>,iface=<vf’s pairing eth>`.

3.  Una vez iniciado, ejecute `show port info all` para comprobar la información de puerto. Debería ver uno o dos puertos DPDK que son net_failsafe (no *net_mlx4*).
4.  Use `start <port> /stop <port>` para iniciar el tráfico.

Los comandos anteriores inician *testpmd* en modo interactivo, lo cual es recomendable, para probar algunos comandos testpmd.

### <a name="basic-single-sendersingle-receiver"></a>Básico: remitente y receptor únicos

Los siguientes comandos imprimen periódicamente las estadísticas de paquetes por segundo:

1. En el lado de TX, ejecute el comando siguiente:

   ```bash
   testpmd \
     -l <core-list> \
     -n <num of mem channels> \
     -w <pci address of the device intended to use> \
     --vdev="net_vdev_netvsc<id>,iface=<the iface to attach to>" \
     -- --port-topology=chained \
     --nb-cores <number of cores to use for test pmd> \
     --forward-mode=txonly \
     --eth-peer=<port id>,<receiver peer MAC address> \
     --stats-period <display interval in seconds>
   ```

2. En el lado de RX, ejecute el comando siguiente:

   ```bash
   testpmd \
     -l <core-list> \
     -n <num of mem channels> \
     -w <pci address of the device intended to use> \
     --vdev="net_vdev_netvsc<id>,iface=<the iface to attach to>" \
     -- --port-topology=chained \
     --nb-cores <number of cores to use for test pmd> \
     --forward-mode=rxonly \
     --eth-peer=<port id>,<sender peer MAC address> \
     --stats-period <display interval in seconds>
   ```

Si se ejecutan los comandos anteriores en una máquina virtual, cambie *IP_SRC_ADDR* y *IP_DST_ADDR* a `app/test-pmd/txonly.c` para que coincida con la dirección IP real de las máquinas virtuales antes de realizar la compilación. En caso contrario, se descartarán los paquetes antes de alcanzar el receptor.

### <a name="advanced-single-sendersingle-forwarder"></a>Avanzado: remitente y reenviador únicos
Los siguientes comandos imprimen periódicamente las estadísticas de paquetes por segundo:

1. En el lado de TX, ejecute el comando siguiente:

   ```bash
   testpmd \
     -l <core-list> \
     -n <num of mem channels> \
     -w <pci address of the device intended to use> \
     --vdev="net_vdev_netvsc<id>,iface=<the iface to attach to>" \
     -- --port-topology=chained \
     --nb-cores <number of cores to use for test pmd> \
     --forward-mode=txonly \
     --eth-peer=<port id>,<receiver peer MAC address> \
     --stats-period <display interval in seconds>
    ```

2. En el lado de FWD, ejecute el comando siguiente:

   ```bash
   testpmd \
     -l <core-list> \
     -n <num of mem channels> \
     -w <pci address NIC1> \
     -w <pci address NIC2> \
     --vdev="net_vdev_netvsc<id>,iface=<the iface to attach to>" \
     --vdev="net_vdev_netvsc<2nd id>,iface=<2nd iface to attach to>" (you need as many --vdev arguments as the number of devices used by testpmd, in this case) \
     -- --nb-cores <number of cores to use for test pmd> \
     --forward-mode=io \
     --eth-peer=<recv port id>,<sender peer MAC address> \
     --stats-period <display interval in seconds>
    ```

Si se ejecutan los comandos anteriores en una máquina virtual, cambie *IP_SRC_ADDR* y *IP_DST_ADDR* a `app/test-pmd/txonly.c` para que coincida con la dirección IP real de las máquinas virtuales antes de realizar la compilación. En caso contrario, se descartarán los paquetes antes de alcanzar el reenviador. No podrá hacer que una tercera máquina virtual reciba el tráfico reenviado, porque el reenviador de *testpmd* no modifica las direcciones de la capa 3, a menos que realice algunos cambios.

## <a name="references"></a>Referencias

* [Opciones EAL](https://dpdk.org/doc/guides/testpmd_app_ug/run_app.html#eal-command-line-options)
* [Comandos testpmd](https://dpdk.org/doc/guides/testpmd_app_ug/run_app.html#testpmd-command-line-options)
