---
title: Informática de alto rendimiento (Azure Virtual Machines)| Microsoft Docs
description: Más información acerca de la informática de alto rendimiento en Azure.
services: virtual-machines
documentationcenter: ''
author: vermagit
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 05/07/2019
ms.author: amverma
ms.openlocfilehash: e8ff4147130dfeff14be41ed292b51ed34966df0
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2019
ms.locfileid: "67537669"
---
# <a name="optimization-for-linux"></a>Optimización para Linux

En este artículo se muestran algunas técnicas importante para optimizar la imagen de un sistema operativo. Obtenga más información acerca de la [habilitación de InfiniBand](enable-infiniband.md) y la optimización de imágenes del sistema operativo.

## <a name="update-lis"></a>Actualización de LIS

Si va a realizar la implementación mediante una imagen personalizada (por ejemplo, un sistema operativo antiguo como RHEL/CentOS 7.4 o 7.5), actualice LIS en la máquina virtual.

```bash
wget https://aka.ms/lis
tar xzf lis
pushd LISISO
./upgrade.sh
```

## <a name="reclaim-memory"></a>Reclamación de memoria

Mejore la eficacia reclamando automáticamente memoria, con el fin de evitar el acceso remoto a la memoria.

```bash
echo 1 >/proc/sys/vm/zone_reclaim_mode
```

Para hacer que se mantenga después de que se reinicie la máquina virtual:

```bash
echo "vm.zone_reclaim_mode = 1" >> /etc/sysctl.conf sysctl -p
```

## <a name="disable-firewall-and-selinux"></a>Deshabilitación del firewall y de SELinux

Deshabilite el firewall y SELinux.

```bash
systemctl stop iptables.service
systemctl disable iptables.service
systemctl mask firewalld
systemctl stop firewalld.service
systemctl disable firewalld.service
iptables -nL
sed -i -e's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
```

## <a name="disable-cpupower"></a>Deshabilitación de cpupower

Deshabilite cpupower.

```bash
service cpupower status
if enabled, disable it:
service cpupower stop
sudo systemctl disable cpupower
```

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información acerca de la [habilitación de InfiniBand](enable-infiniband.md) y la optimización de imágenes del sistema operativo.

* Obtenga más información acerca de [HPC](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/) en Azure.
