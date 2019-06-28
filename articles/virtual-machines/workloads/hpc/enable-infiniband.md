---
title: Habilitación de InifinBand con SR-IOV (Microsoft Azure Virtual Machines) | Microsoft Docs
description: Aprenda a habilitar InfiniBand con SR-IOV.
services: virtual-machines
documentationcenter: ''
author: vermagit
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 05/15/2019
ms.author: amverma
ms.openlocfilehash: 81acb804ed2ebb9e88bc7d8281a7fa52359d4455
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66810088"
---
# <a name="enable-infiniband-with-sr-iov"></a>Habilitación de InfiniBand con SR-IOV


La manera más sencilla y recomendada de configurar una imagen de máquina virtual personalizada con InfiniBand (IB) es agregar la extensión de máquina virtual InfiniBandDriverLinux o InfiniBandDriverWindows a la implementación.
Aprenda a usar estas extensiones de máquina virtual con [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-hpc#rdma-capable-instances) y [Windows](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-hpc#rdma-capable-instances)

Para configurar manualmente InfiniBand en máquinas virtuales con SR-IOV habilitado (actualmente, las series HB y HC), siga estos pasos. Estos pasos son solo para RHEL/CentOS. En el caso de Ubuntu (16.04 y 18.04) y SLES (12 SP4 y 15), los controladores integrados funcionan bien. Para Ubuntu, 


## <a name="manually-install-ofed"></a>Instalación manual de OFED

Instale los controladores más recientes de MLNX_OFED para ConnectX-5 desde [Mellanox](http://www.mellanox.com/page/products_dyn?product_family=26).

Para RHEL/CentOS (el ejemplo siguiente es para la versión 7.6):
```bash
sudo yum install -y kernel-devel python-devel
sudo yum install -y redhat-rpm-config rpm-build gcc-gfortran gcc-c++
sudo yum install -y gtk2 atk cairo tcl tk createrepo
wget --retry-connrefused --tries=3 --waitretry=5 http://content.mellanox.com/ofed/MLNX_OFED-4.5-1.0.1.0/MLNX_OFED_LINUX-4.5-1.0.1.0-rhel7.6-x86_64.tgz
tar zxvf MLNX_OFED_LINUX-4.5-1.0.1.0-rhel7.6-x86_64.tgz
sudo ./MLNX_OFED_LINUX-4.5-1.0.1.0-rhel7.6-x86_64/mlnxofedinstall --add-kernel-support
```

Para Windows, descargue e instale los controladores WinOF-2 para ConnectX-5 de [Mellanox](http://www.mellanox.com/page/products_dyn?product_family=32&menu_section=34)

## <a name="assign-an-ip-address"></a>Asignación de una dirección IP

Asigne una dirección IP a la interfaz ib0, para lo que debe:

- Asignar manualmente una dirección IP a la interfaz ib0 (como raíz).

    ```bash
    ifconfig ib0 $(sed '/rdmaIPv4Address=/!d;s/.*rdmaIPv4Address="\([0-9.]*\)".*/\1/' /var/lib/waagent/SharedConfig.xml)/16
    ```

OR

- Utilizar WALinuxAgent para asignar una dirección IP y configurarla para que persista.

    ```bash
    yum install -y epel-release
    yum install -y python-pip
    python -m pip install --upgrade pip setuptools wheel
    wget "https://github.com/Azure/WALinuxAgent/archive/release-2.2.36.zip"
    unzip release-2.2.36.zip
    cd WALinuxAgent*
    python setup.py install --register-service --force
    sed -i -e 's/# OS.EnableRDMA=y/OS.EnableRDMA=y/g' /etc/waagent.conf
    sed -i -e 's/# AutoUpdate.Enabled=y/AutoUpdate.Enabled=y/g' /etc/waagent.conf
    systemctl restart waagent
    ```

## <a name="next-steps"></a>Pasos siguientes

Más información acerca de [HPC](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/) en Azure.
