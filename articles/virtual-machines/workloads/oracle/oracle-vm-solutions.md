---
title: Soluciones de Oracle en máquinas virtuales de Azure | Microsoft Docs
description: Obtenga información sobre las configuraciones admitidas y las limitaciones de las imágenes de máquina virtual de Oracle en Microsoft Azure.
services: virtual-machines-linux
documentationcenter: ''
author: romitgirdhar
manager: jeconnoc
tags: azure-resource-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/23/2019
ms.author: rogirdh
ms.custom: seodec18
ms.openlocfilehash: b62b35320ba1f4473e9b3a039d181d6a2fb58257
ms.sourcegitcommit: 7042ec27b18f69db9331b3bf3b9296a9cd0c0402
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66743626"
---
# <a name="oracle-vm-images-and-their-deployment-on-microsoft-azure"></a>Imágenes de máquina virtual de Oracle y su implementación en Microsoft Azure

Este artículo contiene información acerca de las soluciones de Oracle basadas en imágenes de máquina virtual publicadas por Oracle en Azure Marketplace. Si está interesado en soluciones de aplicación de toda la nube con la infraestructura de nube de Oracle, vea [soluciones de aplicación de Oracle, la integración de Microsoft Azure y la infraestructura de nube de Oracle](oracle-oci-overview.md).

Para obtener una lista de imágenes disponibles actualmente, ejecute el siguiente comando:

```azurecli-interactive
az vm image list --publisher oracle -o table --all
```

A partir de mayo de 2019 están disponibles las siguientes imágenes:

```bash
Offer                   Publisher    Sku                     Urn                                                          Version
----------------------  -----------  ----------------------  -----------------------------------------------------------  -------------
Oracle-Database-Ee      Oracle       12.1.0.2                Oracle:Oracle-Database-Ee:12.1.0.2:12.1.20170220             12.1.20170220
Oracle-Database-Ee      Oracle       12.2.0.1                Oracle:Oracle-Database-Ee:12.2.0.1:12.2.20180725             12.2.20180725
Oracle-Database-Ee      Oracle       18.3.0.0                Oracle:Oracle-Database-Ee:18.3.0.0:18.3.20181213             18.3.20181213
Oracle-Database-Se      Oracle       12.1.0.2                Oracle:Oracle-Database-Se:12.1.0.2:12.1.20170220             12.1.20170220
Oracle-Database-Se      Oracle       12.2.0.1                Oracle:Oracle-Database-Se:12.2.0.1:12.2.20180725             12.2.20180725
Oracle-Database-Se      Oracle       18.3.0.0                Oracle:Oracle-Database-Se:18.3.0.0:18.3.20181213             18.3.20181213
Oracle-Linux            Oracle       6.10                    Oracle:Oracle-Linux:6.10:6.10.20190506                       6.10.20190506
Oracle-Linux            Oracle       6.8                     Oracle:Oracle-Linux:6.8:6.8.20190506                         6.8.20190506
Oracle-Linux            Oracle       6.9                     Oracle:Oracle-Linux:6.9:6.9.20190506                         6.9.20190506
Oracle-Linux            Oracle       7.3                     Oracle:Oracle-Linux:7.3:7.3.20190506                         7.3.20190506
Oracle-Linux            Oracle       7.4                     Oracle:Oracle-Linux:7.4:7.4.20190506                         7.4.20190506
Oracle-Linux            Oracle       7.5                     Oracle:Oracle-Linux:7.5:7.5.20181207                         7.5.20181207
Oracle-Linux            Oracle       7.5                     Oracle:Oracle-Linux:7.5:7.5.20190506                         7.5.20190506
Oracle-Linux            Oracle       7.6                     Oracle:Oracle-Linux:7.6:7.6.20181207                         7.6.20181207
Oracle-Linux            Oracle       7.6                     Oracle:Oracle-Linux:7.6:7.6.20190506                         7.6.20190506
Oracle-WebLogic-Server  Oracle       Oracle-WebLogic-Server  Oracle:Oracle-WebLogic-Server:Oracle-WebLogic-Server:12.1.2  12.1.2
```

Estas imágenes se consideran "Traiga su propia licencia" y, por tanto, solo se le cobrarán los costos de proceso, almacenamiento y red derivados de la ejecución de una máquina virtual.  Se supone que tiene la licencia correcta para usar software Oracle y que cuenta con un acuerdo de soporte técnico actual con Oracle. Oracle ha garantizado la movilidad de licencias del entorno local a Azure. Para más información sobre la movilidad de licencias, consulte la nota publicada de [Oracle y Microsoft](https://www.oracle.com/technetwork/topics/cloud/faq-1963009.html). 

Los individuos pueden elegir también basar sus soluciones en una imagen personalizada que crean desde cero en Azure o carga su imagen personalizada de su entorno local.

## <a name="support-for-jd-edwards"></a>Compatibilidad con JD Edwards
Según la nota de soporte técnico de Oracle [Id. de documento 2178595.1](https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=573435677515785&id=2178595.1&_afrWindowMode=0&_adf.ctrl-state=o852dw7d_4), versiones de JD Edwards EnterpriseOne 9.2 y superiores se admiten en **oferta de nube de todos los miembros públicos** que cumpla sus específicos `Minimum Technical Requirements` (MTR).  Debe crear imágenes personalizadas que cumplan sus especificaciones de MTR para la compatibilidad con el sistema operativo y la aplicación de software. 

## <a name="oracle-database-vm-images"></a>Imágenes de máquina virtual de base de datos de Oracle
Oracle admite la ejecución de las ediciones Oracle DB 12.1 Standard y Enterprise en Azure en imágenes de máquina virtual basadas en Oracle Linux.  Para conseguir el mejor rendimiento para las cargas de trabajo de producción de Oracle DB en Azure, asegúrese de ajustar correctamente el tamaño de la imagen de máquina virtual y de usar discos administrados que estén respaldados por Premium Storage. Para obtener instrucciones sobre cómo conseguir poner en marcha rápidamente una instancia de Oracle DB en Azure mediante la imagen de máquina virtual publicada, [pruebe el tutorial de inicio rápido de Oracle DB](oracle-database-quick-create.md).

### <a name="attached-disk-configuration-options"></a>Opciones de configuración de discos conectados

Los discos conectados se basan en el servicio de almacenamiento de blobs de Azure. Teóricamente, cada disco estándar puede realizar aproximadamente 500 operaciones de entrada/salida por segundo (IOPS). Nuestra oferta de disco premium es preferible para las cargas de trabajo de base de datos de alto rendimiento y puede alcanzar las 5000 IOPS disco. Puede usar un único disco siempre que cubra sus necesidades de rendimiento. Sin embargo, puede mejorar el rendimiento de IOPS efectivo si usa varios discos conectados, repartir los datos de la base de datos a través de ellos y, a continuación, usar Automatic Storage Management (ASM). Consulte la [información general sobre Oracle Automatic Storage](https://www.oracle.com/technetwork/database/index-100339.html) para más información específica de Oracle ASM. Para obtener un ejemplo de cómo instalar y configurar Oracle ASM en una máquina virtual Linux de Azure, consulte el [instalar y configurar Oracle Automated Storage Management](configure-oracle-asm.md) tutorial.

## <a name="oracle-real-application-cluster-oracle-rac"></a>Clúster de aplicaciones reales de Oracle (Oracle RAC)
Oracle RAC está diseñado para mitigar el error de un único nodo en una configuración local de clúster de varios nodos. Se basa en dos tecnologías locales que no son nativas para los entornos de nube pública de hiperescala: multidifusión y disco compartido de red. Si la solución de base de datos requiere Oracle RAC en Azure, necesita otro software de terceros para habilitar estas tecnologías. Para obtener más información sobre RAC de Oracle, vea el [FlashGrid SkyCluster página](https://www.flashgrid.io/oracle-rac-in-azure/).

## <a name="high-availability-and-disaster-recovery-considerations"></a>Consideraciones sobre la alta disponibilidad y la recuperación ante desastres
Al utilizar las bases de datos de Oracle en Azure, usted es responsable de implementar una solución de recuperación ante desastres y disponibilidad alta para evitar los tiempos de inactividad. 

Se puede lograr alta disponibilidad y recuperación ante desastres para Oracle Database Enterprise Edition (sin basarse en Oracle RAC) en Azure mediante [protección de datos, protección de datos activa](https://www.oracle.com/technetwork/articles/oem/dataguardoverview-083155.html), o [Oracle GoldenGate](https://www.oracle.com/technetwork/middleware/goldengate), con dos bases de datos en dos máquinas virtuales independientes. Ambas máquinas virtuales deben estar en la misma [red virtual](https://azure.microsoft.com/documentation/services/virtual-network/) para asegurarse de que pueden acceder entre sí a través de la dirección IP privada persistente.  Además, recomendamos colocar las máquinas virtuales en el mismo conjunto de disponibilidad para permitir a Azure colocarlas en dominios de error y de actualización independientes. Si desea tener redundancia geográfica, configure las dos bases de datos para replicar entre dos regiones diferentes y conectar las dos instancias con una puerta de enlace de VPN.

El tutorial [Implement Oracle DataGuard en Azure](configure-oracle-dataguard.md) le guiará a través del procedimiento de configuración básica en Azure.  

Con la Protección de datos de Oracle, se puede lograr alta disponibilidad con una base de datos principal en una máquina virtual, una base de datos secundaria (de reserva) en otra máquina virtual y la replicación unidireccional establecida entre ellos. El resultado es un acceso de lectura a la copia de la base de datos. Con Oracle GoldenGate, puede configurar la replicación bidireccional entre las dos bases de datos. Para más información sobre cómo configurar una solución de alta disponibilidad para bases de datos mediante estas herramientas, consulte la documentación de [Protección de datos activa](https://www.oracle.com/technetwork/database/features/availability/data-guard-documentation-152848.html) y [GoldenGate](https://docs.oracle.com/goldengate/1212/gg-winux/index.html) en el sitio web de Oracle. Si se necesita acceso de lectura-escritura a la copia de la base de datos, puede usar [Protección de datos activa de Oracle](https://www.oracle.com/uk/products/database/options/active-data-guard/overview/index.html).

El tutorial [Implement Oracle GoldenGate en Azure](configure-oracle-golden-gate.md) le guiará a través del procedimiento de configuración básica en Azure.

Además de tener una solución de alta disponibilidad y recuperación ante desastres diseñada en Azure, debe tener una estrategia de copia de seguridad para restaurar la base de datos. El tutorial [copias de seguridad y recuperar una base de datos de Oracle](oracle-backup-recovery.md) le guiará por el procedimiento básico para establecer una copia de seguridad coherente.

## <a name="oracle-weblogic-server-virtual-machine-images"></a>Imágenes de máquina virtual de Oracle WebLogic Server

* **La agrupación en clústeres solo se admite en Enterprise Edition.** Tiene licencia para usar la agrupación en clústeres de WebLogic solo cuando utiliza la versión Enterprise Edition de WebLogic Server. No use la agrupación en clústeres con WebLogic Server Standard Edition.
* **Tampoco se admite la multidifusión UDP.** Azure admite la unidifusión UDP, pero no admite la multidifusión ni la difusión. WebLogic Server es capaz de depender de las capacidades de unidifusión UDP de Azure. Para obtener mejores resultados que depender de unidifusión UDP, se recomienda que el tamaño del clúster WebLogic se mantenga estático o mantenerse con no más de 10 servidores administrados.
* **WebLogic Server espera que los puertos públicos y privados sean los mismos para el acceso de T3 (por ejemplo, al usar Enterprise JavaBeans).** Considere un escenario de varios niveles, donde una aplicación de servicio de capa (EJB) se está ejecutando en un clúster de WebLogic Server que consta de dos o más VM en una red virtual denominada *SLWLS*. El nivel de cliente está en una subred diferente en la misma red virtual, ejecutando un programa Java simple que intenta llamar a EJB en el nivel de servicio. Dado que es necesario equilibrar el nivel de servicio, un punto de conexión público con equilibrio de carga debe crearse para las máquinas virtuales en el clúster de WebLogic Server. Si el puerto privado que especifique es diferente del puerto público (por ejemplo, 7006:7008), se producirá un error como el siguiente:

       [java] javax.naming.CommunicationException [Root exception is java.net.ConnectException: t3://example.cloudapp.net:7006:

       Bootstrap to: example.cloudapp.net/138.91.142.178:7006' over: 't3' got an error or timed out]

   Esto se debe a que para cualquier acceso remoto T3, WebLogic Server espera que el puerto con equilibrador de carga y el puerto del servidor WebLogic administrado sean el mismo. En el caso anterior, el cliente tiene acceso al puerto 7006 (el puerto del equilibrador de carga) y el servidor administrado está escuchando en 7008 (el puerto privado). Esta restricción se aplica solo al acceso T3, no a HTTP.

   Para evitar este problema, use una de las siguientes soluciones:

  * Use los mismos números de puerto públicos y privados para los extremos de equilibrio de carga dedicados al acceso a T3.
  * Incluya el siguiente parámetro JVM al iniciar WebLogic Server:

    ```
    -Dweblogic.rjvm.enableprotocolswitch=true
    ```

Para información relacionada, consulte el artículo de KB **860340.1** en <https://support.oracle.com>.

* **Agrupación en clústeres dinámica y limitaciones de equilibrio de carga.** Supongamos que desea usar un clúster dinámico en WebLogic Server y exponerlo a través de un único extremo con equilibrio de carga público en Azure. Esto puede hacerse siempre que use un número de puerto fijo para cada uno de los servidores administrados (no asignados dinámicamente a partir un rango) y no inicie más servidores administrados que máquinas que esté siguiendo el administrador. Es decir, hay no más de un servidor administrado por máquina virtual). Si la configuración provoca más servidores WebLogic que se está iniciando que hay máquinas virtuales (es decir, donde varias instancias de WebLogic Server compartirán la misma máquina virtual), entonces no es posible que más de una de esas instancias de servidores de WebLogic Para enlazar a un número de puerto determinado. Los demás de esa máquina virtual producirá un error.

   Si configura el servidor de administración para asignar automáticamente números de puerto exclusivos para los servidores administrados, el equilibrio de carga no será posible porque Azure no admite la asignación de un único puerto público a varios puertos privados, como sería necesario para esta configuración.
* **Varias instancias de WebLogic Server en una máquina virtual.** Según los requisitos de su implementación, puede considerar que se ejecutan varias instancias de WebLogic Server en la misma máquina virtual, si la máquina virtual es suficientemente grande. Por ejemplo, en un máquina virtual de tamaño medio que contiene dos núcleos, puede elegir ejecutar dos instancias de WebLogic Server. Sin embargo, todavía se recomienda evitar introducir puntos únicos de error en la arquitectura, que sería el caso si usa una sola máquina virtual que se está ejecutando varias instancias de WebLogic Server. Con al menos dos máquinas virtuales podría ser un mejor enfoque y, cada máquina virtual, a continuación, puede ejecutar varias instancias de WebLogic Server. Cada instancia de WebLogic Server podría ser todavía parte del mismo clúster. Sin embargo, actualmente no es posible usar Azure a los extremos de equilibrio de carga que están expuestos por tales implementaciones de WebLogic Server dentro de la misma máquina virtual, ya que el equilibrador de carga de Azure requiere que los servidores con equilibrio de carga se distribuyan entre único máquinas virtuales.

## <a name="oracle-jdk-virtual-machine-images"></a>Imágenes de máquina virtual de Oracle JDK
* **Actualizaciones más recientes de JDK 6 y 7.** Aunque se recomienda usar la última versión compatible pública de Java (actualmente Java 8), Azure también ofrece imágenes de JDK 6 y 7. Esto está pensado para las aplicaciones heredadas que aun no están preparadas para actualizarse a JDK 8. Mientras que es posible que las actualizaciones a imágenes de JDK anteriores ya no estén disponibles para el público general, dada la asociación de Microsoft con Oracle, las imágenes de JDK 6 y 7 ofrecidas por Azure están diseñadas para contener una actualización más reciente no pública que la ofrecida normalmente por Oracle para tan solo un grupo selecto de clientes compatibles con Oracle. Con el tiempo habrá disponibles nuevas versiones de las imágenes de JDK con versiones actualizadas de JDK 6 y 7.

   El JDK disponible en el JDK 6 y 7 imágenes y las máquinas virtuales y las imágenes derivadas de éstas solo puede usarse dentro de Azure.
* **JDK de 64 bits.** Las imágenes de máquina virtual de Oracle WebLogic Server y las imágenes de máquina virtual de JDK de Oracle proporcionadas por Azure contienen las versiones de 64 bits de Windows Server y el JDK.

## <a name="next-steps"></a>Pasos siguientes
Ahora tiene una visión general de las soluciones actuales de Oracle basadas en imágenes de máquina virtual en Microsoft Azure. El siguiente paso es implementar su primera base de datos de Oracle en Azure.

> [!div class="nextstepaction"]
> [Creación de una base de datos de Oracle en Azure](oracle-database-quick-create.md)
