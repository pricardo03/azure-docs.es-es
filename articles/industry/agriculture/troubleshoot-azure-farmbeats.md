---
title: Solución de problemas de Azure FarmBeats
description: En este artículo se describe cómo solucionar problemas de Azure FarmBeats.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: fb4b06eca0d6df6848e2e215d8890569701f7596
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2020
ms.locfileid: "76705622"
---
# <a name="troubleshoot"></a>Solución de problemas

En este artículo se proporcionan soluciones a problemas comunes de Azure FarmBeats.

Si necesita ayuda adicional, póngase en contacto con nosotros en farmbeatssupport@microsoft.com. Asegúrese de incluir el archivo **deploy.log** en el correo electrónico.

Para descargar el archivo **deploy.log**, haga lo siguiente:

1. Inicie sesión en **Azure Portal** y seleccione la suscripción y el inquilino de AD.
2. Inicie Cloud Shell en la navegación superior de Azure Portal.
3. Seleccione **Bash** como experiencia de Cloud Shell preferida.
4. Seleccione el icono resaltado y, luego, en la lista desplegable, seleccione **Descargar**.

    ![Proyecto FarmBeats](./media/troubleshoot-azure-farmbeats/download-deployer-log-1.png)

5. En el panel siguiente, escriba la ruta de acceso al archivo **deployer.log**. Por ejemplo, escriba **farmbeats-deployer.log**.

## <a name="sensor-telemetry"></a>Telemetría del sensor

### <a name="cant-view-telemetry-data"></a>No se ven los datos de telemetría

**Síntoma**: los dispositivos o sensores están implementados, y se ha vinculado FarmBeats con el asociado del dispositivo; sin embargo, no se pueden obtener ni ver datos de telemetría en FarmBeats.

**Acción correctiva**:

1. vaya al grupo de recursos Datahub de FarmBeats.   
2. Seleccione el **centro de eventos** (DatafeedEventHubNamespace) y compruebe el número de mensajes entrantes.
3. Realice cualquiera de las siguientes acciones:   
   - En caso de que no haya *ningún mensaje entrante*, póngase en contacto con el asociado del dispositivo.  
   - Si hay *mensajes entrantes*, póngase en contacto con farmbeatssupport@microsoft.com. Adjunte los registros de Datahub y Accelerator y la telemetría capturada.

Para saber cómo descargar registros, vaya a la sección ["Recopilación de registros de forma manual"](#collect-logs-manually).  

### <a name="cant-view-telemetry-data-after-ingesting-historicalstreaming-data-from-your-sensors"></a>No se pueden ver los datos de telemetría después de ingerir datos históricos o de streaming de los sensores

**Síntoma**: hay dispositivos o sensores implementados, y ha creado los dispositivos o sensores en FarmBeats y ha ingerido la telemetría en EventHub, pero no puede obtener ni ver los datos de telemetría en FarmBeats.

**Acción correctiva**:

1. Asegúrese de que ha realizado correctamente el registro del asociado. Para comprobarlo, vaya a su swagger del centro de datos, vaya a /Partner API, realice una operación get y compruebe si el asociado está registrado. Si no es así, siga los [pasos que se indican aquí](get-sensor-data-from-sensor-partner.md#enable-device-integration-with-farmbeats) para agregar un asociado.
2. Asegúrese de que ha utilizado el formato de mensaje de telemetría correcto:

```json
{
"deviceid": "<id of the Device created>",
"timestamp": "<timestamp in ISO 8601 format>",
"version" : "1",
"sensors": [
    {
      "id": "<id of the sensor created>",
      "sensordata": [
        {
          "timestamp": "< timestamp in ISO 8601 format >",
          "<sensor measure name (as defined in the Sensor Model)>": <value>
        },
        {
          "timestamp": "<timestamp in ISO 8601 format>",
          "<sensor measure name (as defined in the Sensor Model)>": <value>
        }
      ]
    }
 ]
}
```

### <a name="dont-have-the-azure-event-hubs-connection-string"></a>No tiene la cadena de conexión de Azure Event Hubs

**Acción correctiva**:

1. En Swagger de Datahub, vaya a la API del asociado.
2. Seleccione **Get** > **Try it out** > **Execute** (Obtener > Probar > Ejecutar).
3. Anote el identificador del asociado del sensor que le interesa.
4. Vuelva a la API del asociado y seleccione **Get/\<ID>** (Obtener/Id.>).
5. Especifique el identificador del asociado del paso 3 y, luego, seleccione **Execute** (Ejecutar).

   La respuesta de la API debe tener la cadena de conexión de Event Hubs.

### <a name="device-appears-offline"></a>El dispositivo aparece sin conexión

**Síntomas**: los dispositivos están instalados y ha vinculado FarmBeats con el asociado del dispositivo. Los dispositivos están en línea y envían datos de telemetría, pero aparecen sin conexión.

**Acción correctiva**: el intervalo de informes no está configurado para este dispositivo. Póngase en contacto con el fabricante del dispositivo para establecer el intervalo de informes. 

### <a name="error-deleting-a-device"></a>Error al eliminar un dispositivo

Al eliminar un dispositivo, podría encontrarse con uno de los siguientes escenarios de error comunes:  

**Mensaje**: "Se hace referencia al dispositivo en los sensores: hay uno o varios sensores asociados con el dispositivo. Elimine los sensores y, después, elimine el dispositivo".  

**Significado**: el dispositivo está asociado con varios sensores implementados en la granja.   

**Acción correctiva**:  

1. elimine los sensores asociados con el dispositivo mediante Accelerator.  
2. En caso de que quiera asociar los sensores a otro dispositivo, solicite a su asociado de dispositivo que haga lo mismo.  
3. Elimine el dispositivo mediante una llamada `DELETE API` y establezca el parámetro force como *true*.  

**Mensaje**: "Se hace referencia al dispositivo en los dispositivos como ParentDeviceId: hay uno o más dispositivos que están asociados con este dispositivo como dispositivos secundarios. Elimínelos y, después, elimine este dispositivo".  

**Significado**: el dispositivo tiene otros dispositivos asociados.  

**Acción correctiva**:

1. elimine los dispositivos que están asociados a este dispositivo específico.  
2. Elimine el dispositivo específico.  

    > [!NOTE]
    > No se puede eliminar un dispositivo si hay sensores asociados. Para más información sobre cómo eliminar sensores asociados, consulte la sección "Eliminación del sensor" en [Obtención de datos de sensor de los asociados de sensor](get-sensor-data-from-sensor-partner.md).


## <a name="issues-with-jobs"></a>Incidencias con trabajos

### <a name="farmbeats-internal-error"></a>Error interno de FarmBeats

**Mensaje**: "Error interno de FarmBeats. Vea la Guía de solución de problemas para obtener más detalles".

**Acción correctiva**: este problema puede deberse a un error temporal en la canalización de datos. Vuelva a crear el trabajo. Si el error persiste, publique el mensaje de error en el foro de FarmBeats o póngase en contacto en la dirección FarmBeatsSupport@microsoft.com.

## <a name="accelerator-troubleshooting"></a>Solución de problemas del acelerador

### <a name="access-control"></a>Control de acceso

**Problema:** recibe un error mientras agrega una asignación de roles.

**Mensaje**: "No se encuentran usuarios que coincidan".

**Acción correctiva**: compruebe el identificador de correo electrónico para el que intenta agregar una asignación de roles. Este debe coincidir exactamente con el registrado para ese usuario en Active Directory. Si el error persiste, publique el mensaje de error en el foro de FarmBeats o póngase en contacto en la dirección FarmBeatsSupport@microsoft.com.

### <a name="unable-to-log-in-to-accelerator"></a>No se puede iniciar sesión en el acelerador

**Mensaje**: "Error: no tiene autorización para llamar al servicio. Póngase en contacto con el administrador para obtener autorización".

**Acción correctiva**: pida al administrador que le autorice el acceso a la implementación de FarmBeats. Esto se puede hacer mediante una solicitud POST de las API RoleAssignment o a través de Access Control en el panel **Configuración** de Accelerator.  

Si ya se le ha concedido acceso y se encuentra con este error, actualice la página y vuelva a intentarlo. Si el error persiste, publique el mensaje de error en el foro de FarmBeats o póngase en contacto en la dirección FarmBeatsSupport@microsoft.com.

![Proyecto FarmBeats](./media/troubleshoot-azure-farmbeats/accelerator-troubleshooting-1.png)

### <a name="accelerator-issues"></a>Problemas con Accelerator  

**Problema:** ha recibido un error de Accelerator de causa indeterminada.

**Mensaje**: "Error: se ha producido un error desconocido".

**Acción correctiva**: este error se produce si deja la página inactiva durante demasiado tiempo. Actualice la página.  

Si el error persiste, publique el mensaje de error en el foro de FarmBeats o póngase en contacto en la dirección FarmBeatsSupport@microsoft.com.

**Problema:** Accelerator de FarmBeats no muestra la versión más reciente incluso después de actualizar FarmBeatsDeployment.

**Acción correctiva**: este error se debe a la persistencia del trabajo de servicio en el explorador. Haga lo siguiente:
1. cierre todas las pestañas del explorador que tengan abierto Accelerator y cierre la ventana del explorador.
2. Inicie una nueva instancia del explorador y vuelva a cargar el URI de Accelerator. Esta acción carga la nueva versión de Accelerator.

## <a name="sentinel-imagery-related-issues"></a>Sentinel: problemas relacionados con imágenes

### <a name="wrong-username-or-password"></a>Nombre de usuario o contraseña incorrectos

**Mensaje de error del trabajo**: "Se requiere autenticación completa para tener acceso a este recurso".

**Acción correctiva**:

Realice una de las siguientes acciones:
- Vuelva a ejecutar el programa de instalación para actualizar Datahub con el nombre de usuario y la contraseña correctos.
- Vuelva a ejecutar el trabajo con errores o ejecute un trabajo de índices por satélite para un intervalo de fechas de 5 a 7 días; luego, compruebe si el trabajo se ha realizado correctamente.

### <a name="sentinel-hub-wrongurlor-site-not-accessible"></a>Concentrador de Sentinel: Dirección URL o sitio incorrectos no accesibles 

**Mensaje de error del trabajo**: "Vaya, hubo un problema. La página a la que intenta acceder no se encuentra temporalmente disponible". 

**Acción correctiva**:
1. abra [Sentinel](https://scihub.copernicus.eu/dhus/) en el explorador para ver si el sitio web es accesible. 
2. Si el sitio web no es accesible, compruebe si algún firewall, red de empresa u otro software de bloqueo están impidiendo el acceso al sitio web y, luego, siga los pasos necesarios para permitir la dirección URL de Sentinel. 
3. Vuelva a ejecutar el trabajo con errores o ejecute un trabajo de índices por satélite para un intervalo de fechas de 5 a 7 días; luego, compruebe si el trabajo se ha realizado correctamente.  

### <a name="sentinel-server-down-for-maintenance"></a>Servidor de Sentinel: inactivo por mantenimiento

**Mensaje de error del trabajo**: "Copernicus Open Access Hub volverá a estar operativo muy pronto". Lamentamos las molestias, estamos llevando a cabo tareas de mantenimiento en este momento. Pronto volveremos a estar en línea". 

**Acción correctiva**:

este problema puede producirse si se realizan actividades de mantenimiento en el servidor de Sentinel.

1. Si se produce un error en algún trabajo o canalización porque se están realizando tareas de mantenimiento, vuelva a enviar el trabajo más tarde. 

   Para información sobre las actividades de mantenimiento de Sentinel planeadas o no planeadas, vaya al sitio web [Copernicus Open Access Hub News](https://scihub.copernicus.eu/news/).  
2. Vuelva a ejecutar el trabajo con errores o ejecute un trabajo de índices por satélite para un intervalo de fechas de 5 a 7 días; luego, compruebe si el trabajo se ha realizado correctamente.

### <a name="sentinel-maximum-number-of-connections-reached"></a>Sentinel: se ha alcanzado el número máximo de conexiones

**Mensaje de error del trabajo**: "El usuario '\<username>' ha alcanzado el número máximo de dos flujos de simultáneos".

**Significado**: si se produce un error en un trabajo porque se ha alcanzado el número máximo de conexiones, significa que se está usando la misma cuenta de Sentinel en otra implementación de software.

**Acción correctiva**: pruebe alguna de estas soluciones:
* Cree una cuenta de Sentinel y vuelva a ejecutar el programa de instalación para actualizar Datahub mediante un nombre de usuario y una contraseña nuevos de Sentinel.  
* Vuelva a ejecutar el trabajo con errores o ejecute un trabajo de índices por satélite para un intervalo de fechas de 5 a 7 días y, luego, compruebe si el trabajo se ha realizado correctamente.

### <a name="sentinel-server-refused-connection"></a>Servidor de Sentinel: conexión rechazada 

**Mensaje de error del trabajo**: "el servidor ha rechazado la conexión en: http://172.30.175.69:8983/solr/dhus". 

**Acción correctiva**: este problema puede producirse si se realizan actividades de mantenimiento en el servidor de Sentinel. 
1. Si se produce un error en algún trabajo o canalización porque se están realizando tareas de mantenimiento, vuelva a enviar el trabajo más tarde. 

   Para información sobre las actividades de mantenimiento de Sentinel planeadas o no planeadas, vaya al sitio web [Copernicus Open Access Hub News](https://scihub.copernicus.eu/news/).  
2. Vuelva a ejecutar el trabajo con errores o ejecute un trabajo de índices por satélite para un intervalo de fechas de 5 a 7 días; luego, compruebe si el trabajo se ha realizado correctamente.

## <a name="collect-logs-manually"></a>Recopilación de registros de forma manual

[Instale e implemente el Explorador de Azure Storage]( https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows).

### <a name="collect-azure-data-factory-job-logs-in-datahub"></a>Recopilación de registros de trabajos de Azure Data Factory en Datahub
1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. En el cuadro **Buscar**, busque el grupo de recursos Datahub de FarmBeats.

    > [!NOTE]
    > Seleccione el grupo de recursos Datahub que especificó durante la instalación de FarmBeats.

3. En el panel **Grupo de recursos**, busque la cuenta de almacenamiento *datahublogs\** . Por ejemplo, busque **datahublogsmvxmq**.  
4. En la columna **Nombre**, seleccione la cuenta de almacenamiento para ver el panel **Cuenta de almacenamiento**.
5. En el panel **datahubblogs\*** , seleccione **Abrir en el Explorador** para ver la aplicación **Abrir Explorador de Azure Storage**.
6. En el panel izquierdo, seleccione **Contenedores de blobs** y, luego, **job-logs**.
7. En la pestaña **job-logs**, seleccione **Descargar**.
8. Descargue los registros en una carpeta local de la máquina.
9. Envíe por correo el archivo ZIP descargado a farmbeatssupport@microsoft.com.

    ![Proyecto FarmBeats](./media/troubleshoot-azure-farmbeats/collecting-logs-manually-1.png)

### <a name="collect-azure-data-factory-job-logs-in-accelerator"></a>Recopilación de registros de trabajo de Azure Data Factory en Accelerator

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. En el cuadro de texto **Buscar**, busque el grupo de recursos Accelerator de FarmBeats.

    > [!NOTE]
    > Seleccione el grupo de recursos Accelerator que especificó durante la instalación de FarmBeats.

3. En el panel **Grupo de recursos**, busque la cuenta de almacenamiento *storage\** . Por ejemplo, busque **storagedop4k\*** .
4. Seleccione la cuenta de almacenamiento en la columna **Nombre** para ver el panel **Cuenta de almacenamiento**.
5. En el panel **storage\*** , seleccione **Abrir en el Explorador** para abrir la aplicación Explorador de Azure Storage.
6. En el panel izquierdo, seleccione **Contenedores de blobs** y, luego, **job-logs**.
7. En la pestaña **job-logs**, seleccione **Descargar**.
8. Descargue los registros en una carpeta local de la máquina.
9. Envíe por correo el archivo ZIP descargado a farmbeatssupport@microsoft.com.


### <a name="collect-datahub-app-service-logs"></a>Recopilación de registros de App Service de Datahub

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. En el cuadro **Buscar**, busque el grupo de recursos Datahub de FarmBeats.

    > [!NOTE]
    > Seleccione el grupo de recursos Datahub que especificó durante la instalación de FarmBeats.

3. En el grupo de recursos, busque la cuenta de almacenamiento *datahublogs\** . Por ejemplo, busque **fordatahublogsmvxmq\*** .
4. Seleccione la cuenta de almacenamiento en la columna **Nombre** para ver el panel **Cuenta de almacenamiento**.
5. En el panel **datahubblogs\*** , seleccione **Abrir en el Explorador** para abrir la aplicación Explorador de Azure Storage.
6. En el panel izquierdo, seleccione **Contenedores de blobs** y, luego, **appinsights-logs**.
7. En el panel **appinsights-logs**, seleccione **Descargar**.
8. Descargue los registros en una carpeta local de la máquina.
9. Envíe por correo el archivo ZIP descargado a farmbeatssupport@microsoft.com.

### <a name="collect-accelerator-app-service-logs"></a>Recopilación de registros de App Service de Accelerator

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. En el cuadro de texto **Buscar**, busque el grupo de recursos Accelerator de FarmBeats.

    > [!NOTE]
    > Seleccione el grupo de recursos Accelerator de FarmBeats que se ha proporcionado durante la instalación de FarmBeats.

3. En el grupo de recursos, busque la cuenta de almacenamiento *storage\** . Por ejemplo, busque **storagedop4k\*** .
4. Seleccione la cuenta de almacenamiento en la columna **Nombre** para ver el panel **Cuenta de almacenamiento**.
5. En el panel **storage\*** , seleccione **Abrir en el Explorador** para abrir la aplicación Explorador de Azure Storage.
6. En el panel izquierdo, seleccione **Contenedores de blobs** y, luego, **appinsights-logs**.
7. En el panel **appinsights-logs**, seleccione **Descargar**.
8. Descargue los registros en una carpeta local de la máquina.
9. Envíe por correo electrónico la carpeta descargada a farmbeatssupport@microsoft.com.

## <a name="known-issues"></a>Problemas conocidos

## <a name="batch-related-issues"></a>Incidencias relacionadas con lotes

**Mensaje de error**: "Se ha alcanzado la cuota de cuenta regional de las cuentas de Batch para la suscripción especificada".

**Acción correctiva**: aumente la cuota o elimine las cuentas de Batch sin usar y vuelva a ejecutar la implementación.

### <a name="azure-active-directory-azure-ad-related-issues"></a>Problemas relacionados con Azure Active Directory (Azure AD)

**Mensaje de error**: "No se ha podido actualizar la configuración necesaria en la aplicación de Azure AD d41axx40-xx21-4fbd-8xxf-97xxx9e2xxc0: No tiene privilegios suficientes para completar la operación. Asegúrese de que los valores anteriores están configurados correctamente para la aplicación de Azure AD".

**Significado**: la configuración del registro de aplicación de Azure AD no se completó correctamente.  

**Acción correctiva**: pida al administrador de TI (la persona con acceso de lectura de inquilino) que use nuestro [script](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect/tree/master/AppCreationScripts) para crear el registro de aplicación de Azure AD. Este script también se ocupará automáticamente de los pasos de configuración.

**Mensaje de error**: "No se pudo crear la aplicación de Active Directory '\<nombre de aplicación\>' en este inquilino: ya existe otro objeto con el mismo valor para los URI de identificador de propiedad".

**Significado**: ya existe un registro de aplicación de Azure AD con el mismo nombre.

**Acción correctiva**: elimine el registro de aplicación de Azure AD existente o vuelva a usarlo para la instalación. Si va a reutilizar el registro de aplicación de Azure AD existente, pase el identificador de aplicación y el secreto de cliente al instalador y vuelva a realizar la implementación.

## <a name="issues-with-the-inputjson-file"></a>Problemas con el archivo input.json

**Error**: hay un error al leer la entrada del archivo *input.json*.

**Acción correctiva**: este problema suele surgir debido a un error al especificar la ruta de acceso o el nombre correctos del archivo *input.json* en el instalador. Realice las correcciones apropiadas y vuelva a intentar la implementación.

**Error**: se produce un error al analizar los valores del archivo *input.json*.

**Acción correctiva**: este problema surge principalmente debido a la existencia de valores incorrectos en el archivo *input.json*. Realice las correcciones apropiadas y vuelva a intentar la implementación.

## <a name="high-cpu-usage"></a>Uso elevado de CPU

**Error**: recibe una alerta por correo electrónico que hace referencia a una **alerta de uso elevado de CPU**. 

**Acción correctiva**: 
1. vaya al grupo de recursos Datahub de FarmBeats.
2. Seleccione **App Service**.  
3. Vaya a la [página de precios de App Service](https://azure.microsoft.com/pricing/details/app-service/windows/) de escalabilidad vertical y seleccione un plan de tarifa adecuado.

## <a name="next-steps"></a>Pasos siguientes

Si sigue teniendo problemas con FarmBeats, acuda al [foro de soporte técnico](https://social.msdn.microsoft.com/Forums/home?forum=ProjectFarmBeats).
