---
title: Flujo de trabajo de Windows Azure VM arquitectura | Microsoft Docs
description: Este artículo proporciona información general de los procesos de flujo de trabajo al implementar un servicio.
services: cloud-services
documentationcenter: ''
author: genlin
manager: Willchen
editor: ''
tags: top-support-issue
ms.assetid: 9f2af8dd-2012-4b36-9dd5-19bf6a67e47d
ms.service: cloud-services
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 04/08/2019
ms.author: kwill
ms.openlocfilehash: 7c8459a6694663a49203b6ec21a760d3e6bd60c3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60480759"
---
#    <a name="workflow-of-windows-azure-classic-vm-architecture"></a>Flujo de trabajo de la arquitectura de máquina virtual clásica de Windows Azure 
En este artículo se proporciona información general de los procesos de flujo de trabajo que se producen al implementar o actualizar un recurso de Azure como una máquina virtual. 

> [!NOTE]
>Azure tiene dos modelos de implementación diferentes para crear recursos y trabajar con ellos: Resource Manager y clásico. Este artículo trata del modelo de implementación clásico.

El diagrama siguiente presenta la arquitectura de los recursos de Azure.

![Flujo de trabajo de Azure](./media/cloud-services-workflow-process/workflow.jpg)

## <a name="workflow-basics"></a>Conceptos básicos del flujo de trabajo
   
**R**. RDFE / FFE es la ruta de comunicación del usuario en el tejido. RDFE (RedDog Front-End) es la API expuesta públicamente que es el front-end para el Portal de administración y la API de administración de servicios como Visual Studio, MMC de Azure y así sucesivamente.  Todas las solicitudes del usuario se van a través de RDFE. FFE (Fabric Front-End) es la capa que convierte las solicitudes de RDFE en los comandos de fabric. Todas las solicitudes de RDFE pasan por el FFE para alcanzar al tejido de controladores.

**B**. Fabric controller es responsable del mantenimiento y supervisión de todos los recursos en el centro de datos. Se comunica con los agentes de host de tejido en el tejido de envío de información del sistema operativo, como la versión del SO invitado, paquete de servicio, configuración de servicio y estado del servicio.

**C**. El agente de Host se encuentra en el Host de OSsystem y es responsable de configurar el sistema operativo invitado y comunicarse con el agente de invitado (WindowsAzureGuestAgent) con el fin de actualizar el rol hacia un estado de objetivo y realice latido comprueba con el agente invitado. Si el agente de Host no recibe respuesta de latido durante 10 minutos, el agente de Host se reinicia el sistema operativo invitado.

**C2**. WaAppAgent es responsable de instalar, configurar y actualizar WindowsAzureGuestAgent.exe.

**D**.  WindowsAzureGuestAgent es responsable de lo siguiente:

1. Configurar el SO invitado como firewall, las ACL, recursos LocalStorage, paquete de servicio y configuración y los certificados. Configurando el SID de la cuenta de usuario que se ejecutará el rol.
2. Comunicar el estado del rol en el tejido.
3. Iniciar WaHostBootstrapper y supervisión para asegurarse de que el rol está en estado de objetivo.

**E**. WaHostBootstrapper es responsable de:

1. Leer la configuración de rol y, a partir de todas las tareas apropiadas y procesos para configurar y ejecutar el rol.
2. Supervisión de todos sus procesos secundarios.
3. Provoca el evento StatusCheck en el proceso de host de rol.

**F**. IISConfigurator se ejecuta si el rol está configurado como un rol web de IIS completa (no se ejecutará para los roles de HWC 1.2 de SDK). Es responsable de:

1. Iniciando los servicios IIS estándares
2. Configuración del módulo de reescritura en la configuración web
3. Configurar el grupo de aplicaciones para la función configurada en el modelo de servicio
4. Configurar el registro de IIS para que apunte a la carpeta DiagnosticStore LocalStorage
5. Configurar las ACL y permisos
6. El sitio Web se encuentra en % roleroot%:\sitesroot\0 y los puntos de apppool en esta ubicación para ejecutar IIS. 

**G**. Las tareas de inicio están definidas por el modelo de roles y se inicia con WaHostBootstrapper. Las tareas de inicio pueden configurarse para ejecutarse de forma asincrónica en segundo plano y el arranque del host se inicie la tarea de inicio y, a continuación, continúe con otras tareas de inicio. Las tareas de inicio también pueden configurarse para ejecutarse en modo Simple (predeterminado) en el que se esperará el arranque del host para que la tarea de inicio terminar de ejecutarse y devolver un código de salida correcto (0) antes de continuar con la siguiente tarea de inicio.

**H**. Estas tareas forman parte del SDK y se definen como complementos en la definición del servicio de rol (.csdef). Cuando se expande en las tareas de inicio, la **DiagnosticsAgent** y **RemoteAccessAgent** son únicos en que cada uno de ellos define dos tareas de inicio, una normal y que tiene un **/blockStartup** parámetro. La tarea de inicio normal se define como una tarea de inicio en segundo plano para que pueda ejecutar en segundo plano mientras se está ejecutando el propio rol. El **/blockStartup** tarea de inicio se define como una tarea de inicio Simple para que WaHostBootstrapper espera a que salga antes de continuar. El **/blockStartup** tarea espera que la tarea periódica para finalizar la inicialización, y, a continuación, se cierra y permitir que el programa previo de host continuar. Esto se hace para que se pueden configurar diagnósticos y el acceso RDP antes del inicio de los procesos de rol (Esto se hace a través de la tarea /blockStartup). Esto también permite el acceso RDP para seguir ejecutándose después de que el programa previo de host ha finalizado las tareas de inicio (Esto se hace a través de la tarea Normal) y el diagnóstico.

**I**. WaWorkerHost es el proceso de host estándar para los roles de trabajo normal. Este proceso de host hospeda los archivos DLL y código de punto de entrada, como OnStart y ejecute todos los roles.

**J**. WaWebHost es el proceso de host estándar para los roles web si están configurados para usar el SDK compatible con 1.2 de Web hospedable (HWC). Roles pueden habilitar el modo HWC quitando el elemento de la definición de servicio (.csdef). En este modo, ejecute el código y archivos DLL de todos los servicios desde el proceso WaWebHost. No usa IIS (w3wp) y no hay ningún AppPools configurada en el Administrador de IIS porque IIS se hospeda dentro de WaWebHost.exe.

**K**. WaIISHost es el proceso de host para el código de punto de entrada de rol para roles web que usan IIS completo. Este proceso carga el primer archivo DLL que se encuentra que usa el **RoleEntryPoint** clase y se ejecuta el código de esta clase (OnStart, ejecución, OnStop). Cualquier **RoleEnvironment** se generan eventos (por ejemplo, StatusCheck y Changed) que se crean en la clase RoleEntryPoint en este proceso.

**L**. W3wp es el proceso de trabajo IIS estándar que se usa si el rol está configurado para usar IIS completo. Esto ejecuta el grupo de aplicaciones que se configura desde IISConfigurator. En este proceso, se generan los eventos de RoleEnvironment (por ejemplo, StatusCheck y Changed) que se crean aquí. Tenga en cuenta que los eventos de RoleEnvironment se activarán en ambas ubicaciones (WaIISHost y w3wp.exe) si se suscribe a eventos en ambos procesos.

## <a name="workflow-processes"></a>Procesos de flujo de trabajo

1. Un usuario realiza una solicitud, como cargar archivos cspkg y cscfg, indicando a un recurso para detener o hacer un cambio de configuración, y así sucesivamente. Esto puede hacerse mediante el portal de Azure o una herramienta que usa la API de administración de servicios, como la característica de publicación de Visual Studio. Esta solicitud se pasa al RDFE hacer toda la suscripción relacionados con trabajarán y, a continuación, comunican FFE la solicitud. El resto de estos pasos de flujo de trabajo son un nuevo paquete de implementación e iniciarlo.
2. FFE busca el grupo de máquinas correcto (según la entrada de cliente, tal como el grupo de afinidad o ubicación geográfica más entradas del tejido, tales como la disponibilidad de la máquina) y se comunica con el controlador de tejido maestra en ese grupo de máquinas.
3. El controlador de tejido encuentra un host que tenga núcleos de CPU disponibles (o gira hasta un nuevo host). El paquete de servicio y la configuración se copian en el host y el controlador de tejido se comunica con el agente de host en el sistema operativo para implementar el paquete de host (configurar DIP, puertos, sistema operativo invitado y así sucesivamente).
4. El agente de host inicia el sistema operativo invitado y se comunica con el agente de invitado (WindowsAzureGuestAgent). El host envía los latidos del invitado para asegurarse de que el rol está trabajando en su estado de objetivo.
5. WindowsAzureGuestAgent configura el sistema operativo (servidor de seguridad, las ACL, LocalStorage y así sucesivamente) invitado, copia un nuevo archivo de configuración XML en c:\Config y, a continuación, inicia el proceso de WaHostBootstrapper.
6. Para los roles web de IIS completo, WaHostBootstrapper comienza IISConfigurator y le indica que eliminar los grupos de aplicaciones existente para el rol web de IIS.
7. WaHostBootstrapper lee el **inicio** las tareas de E:\RoleModel.xml y comienza a ejecutar las tareas de inicio. WaHostBootstrapper espera hasta que termine de todas las tareas de inicio Simple y devuelve un mensaje "correcto".
8. Para los roles web de IIS completo, WaHostBootstrapper indica IISConfigurator para configurar el grupo de aplicaciones de IIS y señala el sitio para `E:\Sitesroot\<index>`, donde `<index>` es un índice basado en 0 en el número de <Sites> elementos definidos para el servicio.
9. WaHostBootstrapper se iniciará el proceso de host según el tipo de rol:
    1. **Rol de trabajo**: WaWorkerHost.exe se inicia. WaHostBootstrapper ejecuta el método OnStart(). Tras la devolución, WaHostBootstrapper empieza a ejecutar el método Run() y, a continuación, al mismo tiempo marca la función como listas y los coloca en la rotación del equilibrador de carga (si se define InputEndpoints). WaHostBootsrapper, a continuación, entra en un bucle de comprobación del estado del rol.
    1. **Rol Web de SDK 1.2 HWC**: WaWebHost se inicia. WaHostBootstrapper ejecuta el método OnStart(). Tras la devolución, WaHostBootstrapper y comienza a ejecutar el método Run(), a continuación, marca simultáneamente el rol como listo y los coloca en la rotación del equilibrador de carga. WaWebHost emite una solicitud de preparación (/do.rd_runtime_init GET). Todas las solicitudes web se envían a WaWebHost.exe. WaHostBootsrapper, a continuación, entra en un bucle de comprobación del estado del rol.
    1. **Completa el rol Web de IIS**: aIISHost se inicia. WaHostBootstrapper ejecuta el método OnStart(). Tras la devolución, se empieza a ejecutar el método Run() y, a continuación, marca simultáneamente el rol como listo y lo coloca en la rotación del equilibrador de carga. WaHostBootsrapper, a continuación, entra en un bucle de comprobación del estado del rol.
10. Las solicitudes web entrantes a una completa de IIS web desencadenadores de rol IIS para iniciar el proceso W3WP y atender la solicitud, igual como lo haría en un entorno de IIS local.

## <a name="log-file-locations"></a>Ubicaciones de archivos de registro

**WindowsAzureGuestAgent**

- C:\Logs\AppAgentRuntime.Log.  
Este registro contiene cambios en el servicio como las nuevas configuraciones, se detiene y se inicia. Si el servicio no cambia, puede esperar a ver grandes espacios de tiempo en este archivo de registro.
- C:\Logs\WaAppAgent.Log.  
Este registro contiene las actualizaciones de estado y notificaciones de latido y se actualiza cada 2 a 3 segundos.  Este registro contiene una vista histórica del estado de la instancia y le indicará cuando la instancia no estaba en estado listo.
 
**WaHostBootstrapper**

C:\Resources\Directory\<deploymentID>.<role>.DiagnosticStore\WaHostBootstrapper.log
 
**WaWebHost**

C:\Resources\Directory\<guid>.<role>\WaWebHost.log
 
**WaIISHost**

C:\Resources\Directory\<deploymentID>.<role>\WaIISHost.log
 
**IISConfigurator**

C:\Resources\Directory\<deploymentID>.<role>\IISConfigurator.log
 
**Registros de IIS**

C:\Resources\Directory\<guid>.<role>.DiagnosticStore\LogFiles\W3SVC1
 
**Registros de eventos de Windows**

D:\Windows\System32\Winevt\Logs
 



