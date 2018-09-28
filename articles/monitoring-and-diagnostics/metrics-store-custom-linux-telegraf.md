---
title: Recopilación de métricas personalizadas para una máquina virtual de Linux con el agente de InfluxData Telegraf
description: Recopilación de métricas personalizadas para una máquina virtual de Linux con el agente de InfluxData Telegraf
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: howto
ms.date: 09/24/2018
ms.author: ancav
ms.component: metrics
ms.openlocfilehash: 651706b269cf24eca85e0601384ef63cb6ed06a2
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46990712"
---
# <a name="collect-custom-metrics-for-a-linux-vm-with-the-influxdata-telegraf-agent"></a>Recopilación de métricas personalizadas para una máquina virtual de Linux con el agente de InfluxData Telegraf

Azure Monitor le permite recopilar métricas personalizadas a través de los datos de telemetría de aplicación, un agente que se ejecuta en sus recursos de Azure o, incluso, fuera del sistema de supervisión y enviarlas directamente a Azure Monitor. En este artículo, nos centramos en proporcionar instrucciones sobre cómo implementar al agente de [InfluxData](https://www.influxdata.com/) Telegraf en una máquina virtual de Linux en Azure y configurar el agente para que publique las métricas en Azure Monitor. 

## <a name="influxdata-telegraf-agent"></a>Agente de InfluxData Telegraf 

[Telegraf](https://docs.influxdata.com/telegraf/v1.7/) es un agente controlado por complemento que habilita la recopilación de métricas más de 150 orígenes diferentes. Según las cargas de trabajo que se ejecuten en la máquina virtual (p. ej., MySQL, NGINX, Apache, etcétera.), puede configurar al agente para sacar provecho de los complementos de entrada especializados para recopilar métricas. A continuación, los complementos de salida permiten que el agente escriba en destinos de su elección. El agente de Telegraf se integra directamente con la API de REST de métricas personalizadas de Azure Monitor y es compatible con un "complemento de salida de Azure Monitor". Esto permite que el agente recopile métricas específicas a la carga de trabajo de la máquina virtual de Linux y las envíe como métricas personalizadas en Azure Monitor. 

 ![Vista general del agente de Telegraf](./media/metrics-store-custom-linux-telegraf/telegraf-agent-overview.png)

## <a name="send-custom-metrics"></a>Envío de métricas personalizadas 

Para este tutorial, implementamos una máquina virtual de Linux que ejecuta el sistema operativo Ubuntu 16.04 LTS. El agente de Telegraf es compatible con la mayoría de los sistemas operativos Linux. En el portal de descarga de InfluxData están disponibles los paquetes Debian y RPM, además de los archivos binarios de Linux sin empaquetar. Consulte esta guía de instalación para conocer instrucciones y opciones de instalación de Telegraf adicionales. 

Inicio de sesión en [Azure Portal](https://portal.azure.com)

Cree una nueva máquina virtual de Linux: 

1. Haga clic en la opción  **Crear un recurso** en el panel de navegación izquierdo. 
1. Busque *Máquina virtual*.  
1. Seleccione *Ubuntu 16.04 LTS* y haga clic en **Crear**. 
1. Especifique un nombre de máquina virtual como  *MyTelegrafVM*.  
1. Deje el tipo de disco como **SSD** y proporcione un **nombre de usuario**, como  *azureuser*. 
1. Para *Tipo de autenticación*, seleccione **Contraseña** y, a continuación, escriba una contraseña que utilizará más adelante para conectarse mediante SSH a la máquina virtual. 
1. Elija  **Crear nuevo grupo de recursos** y proporcione un nombre, como  *myResourceGroup*.  Elija su ubicación deseada y seleccione  **Aceptar**. 

     ![Creación de la máquina virtual de Ubuntu](./media/metrics-store-custom-linux-telegraf/create-vm.png)

1. Seleccione un tamaño para la máquina virtual. Puede filtrar por tipo de proceso o tipo de disco, por ejemplo. 

     ![Vista general del tamaño de máquina virtual en el agente de Telegraf](./media/metrics-store-custom-linux-telegraf/vm-size.png)

1. En la  **página Configuración**, en  **Red** > **Grupo de seguridad de red** > **Seleccionar puertos de entrada públicos**, seleccione *HTTP* y *SSH (22)*. Deje el resto de valores predeterminados y seleccione  **Aceptar**. 

1. En la página de resumen, seleccione Crear para iniciar la implementación de la máquina virtual. 

1. La máquina virtual se ancla al panel de Azure Portal. Una vez completada la implementación, se abrirá automáticamente el resumen de la máquina virtual. 

1. En la hoja de la máquina virtual, navegue hasta la ficha **Identidad** y asegúrese de que la máquina virtual tiene una identidad asignada por el sistema *activada*. 
 
![fillin](./media/metrics-store-custom-linux-telegraf/connect-to-VM.png)
 
## <a name="connect-to-the-vm"></a>Conexión a la máquina virtual 

Cree una conexión SSH con la máquina virtual. Seleccione el botón Conectar en la página de información general de la máquina virtual. 

![fillin](./media/metrics-store-custom-linux-telegraf/connect-VM-button2.png)

En la página Connect to virtual machine (Conexión a una máquina virtual), mantenga las opciones predeterminadas para conectarse por nombre DNS a través del puerto 22. En Login using VM local account (Iniciar sesión mediante la cuenta local de máquina virtual), se muestra un comando de conexión. Haga clic en el botón para copiar el comando. En el ejemplo siguiente se muestra el aspecto que tiene el comando de conexión SSH: 

```cmd
ssh azureuser@XXXX.XX.XXX 
```

Pegue el comando de conexión SSH en un shell, como Azure Cloud Shell, Bash en Ubuntu o Windows o use el cliente de SSH de su elección para crear la conexión. 

## <a name="install-and-configure-telegraf"></a>Instalación y configuración de Telegraf 

Para instalar el paquete de Telegraf para Debian en la máquina virtual, ejecute los siguientes comandos desde la sesión SSH: 

```cmd
# download the package to the VM 
wget https://dl.influxdata.com/telegraf/releases/telegraf_1.8.0~rc1-1_amd64.deb 
# install the package 
sudo dpkg -i telegraf_1.8.0~rc1-1_amd64.deb
```
El archivo de configuración de Telegraf define las operaciones de Telegraf. De forma predeterminado, se instala un archivo de configuración de ejemplo en la ruta de acceso "/etc/telegraf/telegraf.conf". El archivo de configuración enumera todos los posibles complementos de entrada y salida. Sin embargo, vamos a crear un archivo de configuración personalizado y haremos que el agente lo use al ejecutar los siguientes comandos. 

```cmd
# generate the new Telegraf config file in the current directory 
telegraf --input-filter cpu:mem --output-filter azure_monitor config > azm-telegraf.conf 

# replace the example config with the new generated config 
sudo cp azm-telegraf.conf /etc/telegraf/telegraf.conf 
```

> [!NOTE]
> El código anterior solo habilita dos complementos de entrada, "cpu" y "mem", no dude en agregar más complementos de entrada (Docker, MySQL, NGINX, etc.) según la carga de trabajo que se ejecute en el equipo. Aquí encontrará una lista completa de los complementos de entrada. 

Por último, para que el agente se inicie con la nueva configuración, forzamos la detención e inicio del agente mediante los siguientes comandos. 

```cmd
# stop the telegraf agent on the VM 
sudo systemctl stop telegraf 
# start the telegraf agent on the VM to ensure it picks up the latest configuration 
sudo systemctl start telegraf 
```
Ahora, el agente recopilará las métricas de cada uno de los complementos de entrada especificados y los emitirá a Azure Monitor. 

## <a name="plot-your-telegraf-metrics-in-the-azure-portal"></a>Trazado de las métricas de Telegraf en Azure Portal 

1. Abra [Azure Portal](https://portal.azure.com). 

1. Vaya a la nueva pestaña Monitor y seleccione  **Métricas**.  
     ![fillin](./media/metrics-store-custom-linux-telegraf/metrics.png)

1. Seleccione la máquina virtual en el selector de recursos

     ![fillin](./media/metrics-store-custom-linux-telegraf/metric-chart.png)

1. Seleccione el espacio de nombres *Telegraf/CPU* y la métrica *usage_system*. Puede elegir filtrar por las dimensiones de esta métrica, o dividir según dichas dimensiones.  

     ![fillin](./media/metrics-store-custom-linux-telegraf/VM-resource-selector.png)

## <a name="additional-configuration"></a>Configuración adicional 

En el tutorial anterior, se proporciona información sobre cómo configurar el agente de Telegraf para recopilar métricas de algunos complementos básicos de entrada. El agente de Telegraf es compatible con más de 150 complementos de entrada, con algunas opciones de configuración adicionales de compatibilidad. InfluxData ha publicado una [lista de complementos compatibles](https://docs.influxdata.com/telegraf/v1.7/plugins/inputs/) e instrucciones en inglés sobre [cómo configurarlas](https://docs.influxdata.com/telegraf/v1.7/administration/configuration/).  

Además, este tutorial le ha ayudado a usar el agente de Telegraf para emitir métricas acerca de la máquina virtual en la que se implementó el agente. El agente de Telegraf también se puede usar como un recopilador y centro de reenvío de las métricas para otros recursos. Para obtener más información sobre cómo configurar el agente para que emita métricas para otros recursos de Azure, consulte [Azure Monitor Custom Metric Output for Telegraf](https://github.com/influxdata/telegraf/blob/fb704500386214655e2adb53b6eb6b15f7a6c694/plugins/outputs/azure_monitor/README.md) (Salida de métricas personalizadas de Azure Monitor para Telegraf).  

## <a name="clean-up-resources"></a>Limpieza de recursos 

Cuando ya no los necesite, puede eliminar el grupo de recursos, la máquina virtual y todos los recursos relacionados. Para ello, seleccione el grupo de recursos de la máquina virtual, seleccione Eliminar y luego confirme el nombre del grupo de recursos para eliminar. 

## <a name="next-steps"></a>Pasos siguientes
- Más información acerca de las [métricas personalizadas](metrics-custom-overview.md).


