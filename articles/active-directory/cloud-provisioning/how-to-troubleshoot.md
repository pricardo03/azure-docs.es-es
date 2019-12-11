---
title: Solución de problemas de aprovisionamiento en la nube de Azure AD Connect
description: En este documento se describe cómo solucionar los problemas que pueden surgir con el agente de aprovisionamiento en la nube.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 12/02/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4100886e0a24fa961b9085bd507ae3f4ebfdd6eb
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74795451"
---
# <a name="cloud-provisioning-troubleshooting"></a>Solución de problemas de aprovisionamiento en la nube

El aprovisionamiento en la nube afecta a muchos aspectos distintos y tiene muchas dependencias diferentes.  Como es lógico, esto puede dar lugar a diversos problemas.  Este documento está diseñado para ayudarle a empezar a solucionar estos problemas.  En este documento se presentan las áreas típicas en las que se debe centrar, cómo recopilar información adicional y diversas técnicas que se pueden usar para realizar un seguimiento de los problemas.  


## <a name="common-troubleshooting-areas"></a>Áreas comunes de solución de problemas

|NOMBRE|DESCRIPCIÓN|
|-----|-----|
|[Problemas del agente](#agent-issues)|Compruebe que el agente se ha instalado correctamente y se está comunicando con Azure AD.|
|[Problemas de sincronización de objetos](#object-synchronization-issues)|Use registros de aprovisionamiento para solucionar problemas de sincronización de objetos.|
|[Aprovisionamiento de problemas en cuarentena](#provisioning-quarantined-issues)|Obtenga información sobre el aprovisionamiento de problemas en cuarentena y cómo corregirlos.|


## <a name="agent-issues"></a>Problemas del agente
Algunas de los primeros aspectos que debe comprobar con el agente son las siguientes:


-  ¿Está instalado?
-  ¿Se ejecuta localmente?
-  ¿Está en el portal?
-  ¿Está marcado como correcto?  

Estos aspectos se pueden comprobar en Azure Portal y en el servidor local que ejecuta el agente.

### <a name="azure-portal-agent-verification"></a>Comprobación del agente en Azure Portal

Para comprobar que Azure ve el agente y que este es correcto, siga estos pasos:

1. Inicie sesión en el Portal de Azure.
2. A la izquierda, seleccione **Azure Active Directory**, haga clic en **Azure AD Connect** y, en el centro, seleccione **Administración del aprovisionamiento (versión preliminar)** .
3.  En la pantalla **Aprovisionamiento de Azure AD (versión preliminar)** , haga clic en **Revisar todos los agentes**.
 ![Aprovisionamiento de Azure AD](media/how-to-install/install7.png)</br>
 
4. En la pantalla **On-premises provisioning agents** (Agentes de aprovisionamiento locales) verá los agentes que ha instalado.  Compruebe que el agente en cuestión está ahí y que se ha marcado como **Correcto**.
 ![Agentes de aprovisionamiento](media/how-to-install/install8.png)</br>

### <a name="verify-the-port"></a>Comprobación del puerto

Para comprobar que Azure está escuchando en el puerto 443 y que el agente puede comunicarse con él, puede utilizar la siguiente herramienta:

https://aadap-portcheck.connectorporttest.msappproxy.net/ 

En esta prueba se comprobará que los agentes pueden comunicarse con Azure a través del puerto 443.  Abra un explorador y vaya a la dirección URL anterior desde el servidor en el que está instalado el agente.
 ![Servicios](media/how-to-install/verify2.png)

### <a name="on-the-local-server"></a>En el servidor local

Para comprobar que el agente se ejecuta, siga estos pasos:

1.  En el servidor con el agente instalado, abra **Servicios**. Para ello, vaya ahí o a Inicio/Ejecutar/Services.msc.
2.  En **Servicios**, asegúrese de que tanto el **Actualizador del Agente de Microsoft Azure AD Connect** como el **Agente de aprovisionamiento de Microsoft Azure AD Connect** están ahí y que su estado es **En ejecución**.
 ![Servicios](media/how-to-troubleshoot/troubleshoot1.png)

### <a name="common-agent-installation-issues"></a>Problemas comunes de instalación del agente

A continuación se muestran algunos problemas comunes de instalación del agente y la solución típica.

#### <a name="agent-failed-to-start"></a>El agente no se pudo iniciar

Si recibe un mensaje de error que indique lo siguiente:

**No se pudo iniciar el servicio "Microsoft Azure AD Connect Provisioning Agent".  Compruebe que dispone de suficientes privilegios para iniciar servicios del sistema".** 

Esto suele deberse a que una directiva de grupo impidió que se aplicaran permisos al servicio NT local de cuenta de inicio de sesión creado por el instalador (NT SERVICE\AADConnectProvisioningAgent). Estos permisos son necesarios para iniciar el servicio.

Para resolver este problema, siga estos pasos.

1.  Inicie sesión en el servidor con una cuenta de administrador.
2.  Abra **Servicios**. Para ello, vaya ahí o a Inicio/Ejecutar/Services.msc.
3.  En **Servicios**, haga doble clic en **Microsoft Azure AD Connect Provisioning Agent**
4. En la pestaña, cambie la cuenta de inicio de sesión por un administrador de dominio y reinicie el servicio. 

 ![Services](media/how-to-troubleshoot/troubleshoot3.png)

#### <a name="agent-times-out-or-certificate-is-invalid"></a>El agente agota el tiempo de espera o el certificado no es válido

Si está intentando registrar el agente, es posible que obtenga los siguientes errores.

 ![Services](media/how-to-troubleshoot/troubleshoot4.png)

Esto suele deberse a que el agente no puede conectarse al servicio de identidad híbrida y requiere configurar el proxy HTTP.  Para solucionar este error, configure un proxy de salida. 

El agente de aprovisionamiento admite el uso de un proxy de salida. Puede configurarlo al editar el archivo de configuración del agente **C:\Archivos de programa\Microsoft Azure AD Connect Provisioning Agent\AADConnectProvisioningAgent.exe.config**. Agregue al archivo las siguientes líneas, hacia el final del archivo, justo antes de la etiqueta de cierre `</configuration>`.
Reemplace las variables [proxy-server] y [proxy-port] con los valores del puerto y el nombre del servidor proxy.

```xml
    <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
            <proxy
                usesystemdefault="true"
                proxyaddress="http://[proxy-server]:[proxy-port]"
                bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

#### <a name="agent-registration-fails-with-security-error"></a>Error de seguridad al registrar el agente

Es posible que vea el siguiente error al instalar el agente de aprovisionamiento en la nube.

Esto suele deberse a que el agente no puede ejecutar los scripts de registro de PowerShell debido a las directivas de ejecución locales de PowerShell.

Para solucionar este error, cambie las directivas de ejecución de PowerShell en el servidor. Debe establecer Machine and User policies (Máquina y directivas de usuario) en "Undefined" (No definido) o "RemoteSigned" (Firma remota). Si está en "Unrestricted" (Sin restricciones), verá este error.  Para más información, consulte las [directivas de ejecución de PowerShell](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-6). 

### <a name="log-files"></a>Archivos de registro

De forma predeterminada, el agente emite mensajes de error mínimos e información de seguimiento de la pila. Puede encontrar estos registros de seguimiento en la carpeta: **C:\ProgramData\Microsoft\Azure AD Connect Provisioning Agent\Trace**

Use los pasos siguientes para recopilar información adicional para solucionar problemas relacionados con el agente.

1. Detenga el servicio **Microsoft Azure AD Connect Provisioning Agent**.
2. Cree una copia del archivo de configuración original: **C:\Archivos de programa\Microsoft Azure AD Connect Provisioning Agent\AADConnectProvisioningAgent.exe.config**
3. Reemplace la sección <system.diagnostics> existente por lo siguiente y todos los mensajes de seguimiento irán al archivo **ProvAgentTrace.log.**

      ```xml
        <system.diagnostics>
            <sources>
            <source name="AAD Connect Provisioning Agent">
                <listeners>
                <add name="console"/>
                <add name="etw"/>
                <add name="textWriterListener"/>
                </listeners>
            </source>
            </sources>
            <sharedListeners>
            <add name="console" type="System.Diagnostics.ConsoleTraceListener" initializeData="false"/>
            <add name="etw" type="System.Diagnostics.EventLogTraceListener" initializeData="Azure AD Connect Provisioning Agent">
                <filter type="System.Diagnostics.EventTypeFilter" initializeData="All"/>
            </add>
            <add name="textWriterListener" type="System.Diagnostics.TextWriterTraceListener" initializeData="C:/ProgramData/Microsoft/Azure AD Connect Provisioning Agent/Trace/ProvAgentTrace.log"/>
            </sharedListeners>
        </system.diagnostics>

      ```
4. Inicie el servicio **Microsoft Azure AD Connect Provisioning Agent**.
5. Puede usar el siguiente comando para terminar el archivo y depurar problemas: 
    ```
    Get-Content “C:/ProgramData/Microsoft/Azure AD Connect Provisioning Agent/Trace/ProvAgentTrace.log” -Wait
    ```
## <a name="object-synchronization-issues"></a>Problemas de sincronización de objetos

La siguiente sección contiene información sobre la solución de problemas de sincronización de objetos.

### <a name="provisioning-logs"></a>Registros de aprovisionamiento

En Azure Portal, se pueden usar registros de aprovisionamiento para ayudar a realizar un seguimiento de los problemas de sincronización de objetos y solucionarlos.  Para ver los registros, seleccione **Registros**.
 ![Registros de aprovisionamiento](media/how-to-troubleshoot/log1.png)

Los registros de aprovisionamiento proporcionan una gran cantidad de información sobre el estado de los objetos que se sincronizan entre el entorno local de AD y Azure.

 ![Registros de aprovisionamiento](media/how-to-troubleshoot/log2.png)

Puede utilizar las listas desplegables de la parte superior de la página para filtrar la vista para centrarse en problemas específicos, fechas, etc.  Al hacer doble clic en un evento individual, se proporcionará información detallada adicional.

 ![Registros de aprovisionamiento](media/how-to-troubleshoot/log3.png)

Esta información proporcionará pasos detallados e indicará dónde se está produciendo el problema de sincronización.  Por lo tanto, le permitirá centrarse en el punto exacto del problema y localizarlo.


## <a name="provisioning-quarantined-issues"></a>Aprovisionamiento de problemas en cuarentena

El aprovisionamiento en la nube supervisa el estado de la configuración y establece los objetos incorrectos en un estado de "cuarentena". Si todas o la mayoría de las llamadas realizadas al sistema de destino no tienen éxito sistemáticamente debido a un error (como en el caso de credenciales de administrador no válidas), el trabajo de aprovisionamiento se establece en un estado de "cuarentena".

 ![Cuarentena](media/how-to-troubleshoot/quarantine1.png)

Al hacer clic en el estado, puede ver información adicional acerca de la cuarentena.  También puede obtener el código de error y el mensaje.

 ![Cuarentena](media/how-to-troubleshoot/quarantine2.png)

### <a name="resolving-a-quarantine"></a>Resolución de una cuarentena

- Use Azure Portal para reiniciar el trabajo de aprovisionamiento. En la página de configuración del agente, seleccione **Reiniciar aprovisionamiento**.

  ![Cuarentena](media/how-to-troubleshoot/quarantine3.png)

- Use Microsoft Graph para [reiniciar el trabajo de aprovisionamiento](https://docs.microsoft.com/graph/api/synchronization-synchronizationjob-restart?view=graph-rest-beta&tabs=http). Así tendrá control total sobre lo que reinicie. Puede optar por borrar los elementos en custodia (para reiniciar el contador de custodia que se acumula en el estado de cuarentena), borrar la cuarentena (para quitar la aplicación de la cuarentena) o borrar las marcas de agua. Use la siguiente solicitud:
 
  `POST /servicePrincipals/{id}/synchronization/jobs/{jobId}/restart`

## <a name="next-steps"></a>Pasos siguientes 

- [¿Qué es el aprovisionamiento?](what-is-provisioning.md)
- [¿Qué es el aprovisionamiento en la nube de Azure AD Connect?](what-is-cloud-provisioning.md)



