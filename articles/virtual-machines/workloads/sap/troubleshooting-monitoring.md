---
title: Supervisión de SAP HANA en Azure (instancias grandes) | Microsoft Docs
description: Supervisión de SAP HANA en Azure (Instancias grandes).
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
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
ms.openlocfilehash: f046304121e0aed8efa1bbc2535d34186eba3496
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60713714"
---
# <a name="how-to-monitor-sap-hana-large-instances-on-azure"></a>Procedimiento para supervisar SAP HANA en Azure (instancias grandes)

SAP HANA en Azure (Instancias grandes) no se diferencia de cualquier otra implementación de IaaS: necesita supervisar lo que están haciendo el sistema operativo y la aplicación y cómo consumen las aplicaciones los siguientes recursos:

- CPU
- Memoria
- Ancho de banda de red
- Espacio en disco

Con Azure Virtual Machines, debe averiguar si las clases de recursos indicadas anteriormente serán suficientes o se agotarán. A continuación se proporcionan más detalles sobre cada una de las clases:

**Consumo de recursos de CPU:** La relación que SAP definido para ciertas cargas de trabajo con HANA se exige para asegurarse de que debe haber suficientes recursos de CPU disponibles para trabajar con los datos que se almacenan en memoria. No obstante, puede haber casos en los que HANA consuma una gran cantidad de CPU al ejecutar consultas debido a que falten índices o problemas similares. Esto significa que debe supervisar el consumo de los recursos de CPU por parte de la unidad de Instancias grandes de HANA, así como los recursos de CPU utilizados por servicios específicos de HANA.

**Consumo de memoria:** Es importante supervisarlo desde dentro y fuera de HANA en la unidad. Dentro de HANA, supervise cómo los datos utilizan la memoria asignada a HANA con el fin de que se mantengan dentro de las directrices de tamaño obligatorias de SAP. También puede supervisar el consumo de memoria en el nivel de Instancias grandes para asegurarse de que el software instalado no perteneciente a HANA no utiliza demasiada memoria y compite por la memoria con HANA.

**Ancho de banda de red:** La puerta de enlace de red virtual de Azure está limitada en el ancho de banda de los datos que se transfieren en la red virtual de Azure, por lo que es útil supervisar los datos recibidos por todas las máquinas virtuales de Azure dentro de una red virtual para averiguar si está cerca de los límites de la puerta de enlace Azure SKU seleccionada. En la unidad de Instancias grandes de HANA, tiene sentido supervisar también el tráfico de red de entrada y de salida, así como realizar un seguimiento de los volúmenes que se administran a lo largo del tiempo.

**Espacio en disco:** Consumo de espacio en disco suele aumenta con el tiempo. Las causas más comunes son el aumento del volumen de datos, la ejecución de copias de seguridad de los registros de transacciones, el almacenamiento de los archivos de seguimiento y la realización de instantáneas de almacenamiento. Por lo tanto, es importante supervisarlo y administrar el espacio en disco asociado a la unidad de Instancias grandes de HANA.

En las **SKU de tipo II** de las instancias grandes de HANA, el servidor viene ya con las herramientas de diagnóstico del sistema cargadas. Puede usar estas herramientas de diagnóstico para realizar la comprobación de mantenimiento del sistema. Ejecute el siguiente comando para generar el archivo de registro de comprobación de mantenimiento en /var/log/health_check.
```
/opt/sgi/health_check/microsoft_tdi.sh
```
Cuando trabaje con el equipo de Soporte técnico de Microsoft para solucionar un problema, también se le pedirá que proporcione los archivos de registro mediante el uso de estas herramientas de diagnóstico. Puede comprimir el archivo con el comando siguiente.
```
tar  -czvf health_check_logs.tar.gz /var/log/health_check
```

**Pasos siguientes**

- Consulte [Procedimiento para supervisar SAP HANA en Azure (instancias grandes)](troubleshooting-monitoring.md).