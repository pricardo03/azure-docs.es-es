---
title: Prueba de la latencia de red de Azure Virtual Machines en una red virtual de Azure | Microsoft Docs
description: Obtenga información sobre cómo probar la latencia de red entre máquinas virtuales de Azure en una red virtual
services: virtual-network
documentationcenter: na
author: steveesp
manager: Marina Lipshteyn
editor: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/29/2019
ms.author: steveesp
ms.openlocfilehash: 760a181b4459db28d3a6eee5022cc0f984c4bee0
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73587085"
---
# <a name="testing-network-latency"></a>Prueba de la latencia de red

La medición de la latencia de red con una herramienta diseñada para la tarea proporcionará los resultados más precisos. SockPerf (para Linux) y Latte (para Windows) son ejemplos de herramientas disponibles públicamente que permiten aislar y medir la latencia de red, a la vez que excluyen otros tipos de latencia, como la de las aplicaciones. Estas herramientas se centran en el tipo de tráfico de red que afecta al rendimiento de la aplicación, es decir, TCP y UDP. Otras herramientas de conectividad comunes, como ping, a veces se pueden usar para medir la latencia, pero esos resultados pueden no ser representativos del tráfico de red que se usa en cargas de trabajo reales. Esto se debe a que la mayoría de estas herramientas emplean el protocolo ICMP, que puede tratarse de forma diferente al tráfico de la aplicación, y es posible que los resultados no sean aplicables a las cargas de trabajo en las que se usa TCP y UDP. Para realizar pruebas de latencia de red precisas de los protocolos usados por la mayoría de las aplicaciones, SockPerf (para Linux) y Latte (para Windows) producen los resultados más relevantes. En este documento se describen las dos herramientas.

## <a name="overview"></a>Información general

Mediante dos máquinas virtuales, una como emisor y otra como receptor, se crea un canal de comunicaciones bidireccional para enviar y recibir paquetes en ambas direcciones para medir el tiempo de ida y vuelta (RTT).

Estos pasos se pueden usar para medir la latencia de red entre dos máquinas virtuales (VM) o incluso entre dos equipos físicos. Las medidas de latencia pueden ser útiles para los escenarios siguientes:

- Establecer una prueba comparativa para la latencia de red entre las máquinas virtuales implementadas
- Comparar los efectos de los cambios en la latencia de red después de que se realicen cambios relacionados en:
  - El SO o el software de la pila de red, incluidos cambios de configuración
  - El método de implementación de la máquina virtual, como la implementación en una zona o PPG
  - Las propiedades de máquina virtual, como redes aceleradas o cambios de tamaño
  - La red virtual, como el enrutamiento o el filtrado de cambios

### <a name="tools-for-testing"></a>Herramientas para pruebas
Para medir la latencia, hay dos opciones diferentes, una para los sistemas basados en Windows y otra para los basados en Linux

Para Windows: latte.exe (Windows) [https://gallery.technet.microsoft.com/Latte-The-Windows-tool-for-ac33093b](https://gallery.technet.microsoft.com/Latte-The-Windows-tool-for-ac33093b)

Para Linux: SockPerf (Linux) [https://github.com/mellanox/sockperf](https://github.com/mellanox/sockperf)

El uso de estas herramientas garantiza que solo se miden los tiempos de entrega de la carga TCP o UDP, y no ICMP (ping) u otros tipos de paquetes que no usen las aplicaciones y que no afecten a su rendimiento.

### <a name="tips-for-an-optimal-vm-configuration"></a>Sugerencias para la configuración óptima de las máquinas virtuales:

- Use la versión más reciente de Windows o Linux
- Habilite redes aceleradas para obtener los mejores resultados
- Implemente las máquinas virtuales con [Grupo de selección de ubicación de proximidad de Azure](https://docs.microsoft.com/azure/virtual-machines/linux/co-location)
- Las máquinas virtuales más grandes generalmente funcionan mejor que las más pequeñas

### <a name="tips-for-analysis"></a>Sugerencias para el análisis

- Establezca una línea de base pronto, en cuanto se complete la implementación, la configuración y las optimizaciones
- Compare siempre los nuevos resultados con una línea de base o de otro modo de una prueba a otra con cambios controlados
- Repita las pruebas cada vez que se observen o se planeen cambios


## <a name="testing-vms-running-windows"></a>Pruebas de máquinas virtuales que ejecutan Windows

## <a name="get-latteexe-onto-the-vms"></a>Cargue Latte.exe en las máquinas virtuales

Descargue la versión más reciente: [https://gallery.technet.microsoft.com/Latte-The-Windows-tool-for-ac33093b](https://gallery.technet.microsoft.com/Latte-The-Windows-tool-for-ac33093b)

Considere la posibilidad de colocar Latte.exe en una carpeta independiente, como c:\tools

## <a name="allow-latteexe-through-the-windows-firewall"></a>Permitir Latte.exe mediante el firewall de Windows

En el RECEPTOR, cree una regla Permitir en el Firewall de Windows para permitir que llegue el tráfico de Latte.exe. Es más fácil permitir todo el programa Latte.exe por su nombre en lugar de determinados puertos TCP de entrada.

Permita Latte.exe a través del Firewall de Windows de esta forma:

netsh advfirewall firewall add rule program=\<PATH\>\\latte.exe name=&quot;Latte&quot; protocol=any dir=in action=allow enable=yes profile=ANY


Por ejemplo, si ha copiado latte.exe a la carpeta &quot;c:\\tools&quot;, este sería el comando:

```cmd
netsh advfirewall firewall add rule program=c:\tools\latte.exe name="Latte" protocol=any dir=in action=allow enable=yes profile=ANY
```

## <a name="running-latency-tests"></a>Ejecución de pruebas de latencia

Inicie Latte.exe en el receptor (se ejecuta desde CMD, no desde PowerShell):

latte -a &lt;dirección IP del receptor&gt;:&lt;puerto&gt; -i &lt;iteraciones&gt;

Es suficiente alrededor de 65 000 iteraciones para obtener unos resultados representativos.

Cualquier número de puerto disponible es válido.

Si la máquina virtual tiene la dirección IP 10.0.0.4, tendría este aspecto:

```cmd
latte -a 10.0.0.4:5005 -i 65100
```

Inicie Latte.exe en el EMISOR (se ejecuta desde CMD, no desde PowerShell):

latte -c -a \<dirección IP del receptor\>:\<puerto\> -i \<iteraciones\>


El comando resultante es el mismo que en el receptor, excepto con la adición de &quot;-c&quot; para indicar que se trata del &quot;cliente&quot; o emisor:

```cmd
latte -c -a 10.0.0.4:5005 -i 65100
```

Espere a que se muestren los resultados. Dependiendo de la distancia que haya entre las máquinas virtuales, podría llevar unos minutos completarlos. Considere comenzar con menos iteraciones para probar si se ha realizado correctamente antes de ejecutar pruebas más largas.



## <a name="testing-vms-running-linux"></a>Pruebas de máquinas virtuales que ejecutan Linux

Use SockPerf. Está disponible en [https://github.com/mellanox/sockperf](https://github.com/mellanox/sockperf)

### <a name="install-sockperf-on-the-vms"></a>Instalación de SockPerf en las máquinas virtuales

En las máquinas virtuales Linux (tanto en el EMISOR como en el RECEPTOR), ejecute estos comandos para preparar SockPerf en las máquinas virtuales. Se proporcionan comandos para las principales distribuciones.

#### <a name="for-rhel--centos-follow-these-steps"></a>Para RHEL y CentOS, siga estos pasos:
```bash
#CentOS / RHEL - Install GIT and other helpful tools
    sudo yum install gcc -y -q
    sudo yum install git -y -q
    sudo yum install gcc-c++ -y
    sudo yum install ncurses-devel -y
    sudo yum install -y automake
    sudo yum install -y autoconf
```

#### <a name="for-ubuntu-follow-these-steps"></a>Para Ubuntu, siga estos pasos:
```bash
#Ubuntu - Install GIT and other helpful tools
    sudo apt-get install build-essential -y
    sudo apt-get install git -y -q
    sudo apt-get install -y autotools-dev
    sudo apt-get install -y automake
    sudo apt-get install -y autoconf
```

#### <a name="for-all-distros-copy-compile-and-install-sockperf-according-to-the-following-steps"></a>Para todas las distribuciones, copie, compile e instale SockPerf según los pasos siguientes:
```bash
#Bash - all distros

#From bash command line (assumes git is installed)
git clone https://github.com/mellanox/sockperf
cd sockperf/
./autogen.sh
./configure --prefix=

#make is slower, may take several minutes
make

#make install is fast
sudo make install
```

### <a name="run-sockperf-on-the-vms"></a>Ejecución de SockPerf en las máquinas virtuales

Una vez finalizada la instalación de SockPerf, las máquinas virtuales están listas para ejecutar las pruebas de latencia. 

En primer lugar, inicie SockPerf en el RECEPTOR.

Cualquier número de puerto disponible es válido. En este ejemplo, se usa el puerto 12345:
```bash
#Server/Receiver - assumes server's IP is 10.0.0.4:
sudo sockperf sr --tcp -i 10.0.0.4 -p 12345
```
Ahora que el servidor está a la escucha, el cliente puede empezar a enviarle paquetes en el puerto en el que realiza la escucha, en este caso 12345.

Bastan aproximadamente 100 segundos para devolver resultados representativos, como se muestra en el ejemplo siguiente:
```bash
#Client/Sender - assumes server's IP is 10.0.0.4:
sockperf ping-pong -i 10.0.0.4 --tcp -m 350 -t 101 -p 12345  --full-rtt
```

Espere a que se muestren los resultados. En función de la distancia entre las máquinas virtuales, el número de iteraciones variará. Considere comenzar con pruebas más cortas, de aproximadamente cinco segundos, para comprobar si realizan de forma correcta antes de ejecutar pruebas más largas.

En este ejemplo de SockPerf se usa un tamaño de mensaje de 350 bytes porque es un paquete de tamaño medio típico. El tamaño del mensaje se puede aumentar o reducir para lograr resultados que representen con mayor precisión la carga de trabajo que se ejecutará en las máquinas virtuales.


## <a name="next-steps"></a>Pasos siguientes
* Implemente la latencia con [Grupo de selección de ubicación de proximidad de Azure](https://docs.microsoft.com/azure/virtual-machines/linux/co-location)
* Obtenga información sobre cómo [optimizar las redes para máquinas virtuales](../virtual-network/virtual-network-optimize-network-bandwidth.md) para su escenario.
* Obtenga información acerca de cómo [se asigna el ancho de banda a las máquinas virtuales](../virtual-network/virtual-machine-network-throughput.md)
* Obtenga más información con las [Preguntas más frecuentes (P+F) acerca de Azure Virtual Network](../virtual-network/virtual-networks-faq.md)