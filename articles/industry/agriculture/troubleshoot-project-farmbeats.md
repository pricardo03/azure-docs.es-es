---
title: solución de problemas
description: Procedimientos para solucionar problemas de FarmBeats de Azure
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: 22c304b26eddaee4444f6eb12957e2a6fedf7810
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74793285"
---
# <a name="troubleshooting"></a>solución de problemas

En las secciones siguientes se describen los problemas comunes de FarmBeats de Azure y cómo corregirlos.

Para obtener ayuda adicional, escríbanos a farmbeatssupport@microsoft.com, e incluya el archivo deploy.log en este correo.

 Siga estos pasos para descargar el archivo deployer.log:

1. El icono resaltado y seleccione la opción **Descargar** en la lista desplegable.

    ![Proyecto de FarmBeats](./media/troubleshooting-farmbeats/download-deployer-log-1.png)

2. En la pantalla siguiente, escriba la ruta de acceso en el archivo deployer.log. Por ejemplo, farmbeats-deployer. log.

## <a name="sensor-telemetry"></a>Telemetría del sensor

### <a name="telemetry-not-seen"></a>No se ha detectado la telemetría

**Síntoma**: los dispositivos o sensores se implementan, y se ha vinculado FarmBeats con el asociado del dispositivo; pero no se pueden obtener o ver datos de telemetría en FarmBeats.

**Acción correctiva**: visite Azure Portal y siga estos pasos:

1. Vaya al grupo de recursos del centro de datos de FarmBeats.   
2. Seleccione el **Centro de eventos** (DatafeedEventHubNamespace...) y compruebe el número de mensajes entrantes.   
3. En caso de que no haya **ningún mensaje entrante**, póngase en contacto con el asociado del dispositivo.  
4. En caso de que haya **mensajes entrantes**, póngase en contacto en la dirección de correo farmbeatssupport@microsoft.com con el centro de datos, los registros del acelerador y la telemetría capturada.

Vea [Sección de registros](#collect-logs-manually) del documento para saber cómo descargar registros.  

### <a name="dont-have-the-eventhub-connection-string"></a>No tiene la cadena de conexión de EventHub

**Acción correctiva**: vaya al Swagger del centro de datos y siga estos pasos:
1. Vaya a la API de partner.
2. Haga clic en GET -> Try it Out -> Execute (OBTENER -> Probar -> Ejecutar).
3. Tome nota del id. de partner del partner de sensores que le interese.
4. Vuelva a la API de partner y haga clic en GET/{id}.
5. Especifique el identificador del paso 3 y haga clic en Execute (Ejecutar).
6. La respuesta de la API debe tener la cadena de conexión de EventHub.

### <a name="device-appears-offline"></a>El dispositivo aparece sin conexión

**Síntomas**: se instalan los dispositivos y se ha vinculado FarmBeats con el asociado del dispositivo. Los dispositivos están en línea y envían datos de telemetría, pero aparecen sin conexión.

**Acción correctiva**: el intervalo de informes no está configurado para este dispositivo. Póngase en contacto con el fabricante del dispositivo para establecer el intervalo de informes. 

### <a name="error-deleting-a-resource"></a>Error al eliminar un recurso

A continuación se muestran los escenarios de error habituales al eliminar un dispositivo:  

**Mensaje**: se hace referencia al dispositivo en los sensores: hay uno o varios sensores asociados con el dispositivo. Elimine los sensores y, después, elimine el dispositivo.  

**Significado**: el dispositivo está asociado con varios sensores implementados en la granja.   

**Acción correctiva**:  

1. Elimine los sensores asociados con el dispositivo a través del acelerador.  
2. En caso de que quiera asociar los sensores a otro dispositivo, solicite a su asociado de dispositivo que haga lo mismo.  
3. Elimine el dispositivo mediante una llamada API DELETE estableciendo el parámetro force como "true".  

**Mensaje**: se hace referencia al dispositivo en los dispositivos como ParentDeviceId: hay uno o más dispositivos que están asociados con este dispositivo como dispositivos secundarios. Elimínelos y, después, elimine este dispositivo.  

**Significado**: el dispositivo tiene otros dispositivos asociados.  

**Acción correctiva**:

1. Elimine los dispositivos asociados con el dispositivo específico.  
2. Elimine el dispositivo específico.  

    > [!NOTE]
    > No se puede eliminar un dispositivo si hay sensores asociados. Vea [Eliminación de los sensores](get-sensor-data-from-sensor-partner.md) en el capítulo Obtención de datos del sensor para obtener más información sobre cómo eliminar sensores asociados.


## <a name="issues-with-jobs"></a>Incidencias con trabajos

### <a name="farmbeats-internal-error"></a>Error interno de FarmBeats

**Mensaje**: error interno de FarmBeats. Vea la Guía de solución de problemas para obtener más detalles.

**Acción correctiva**: esto puede deberse a un error temporal en la canalización de datos. Vuelva a crear el trabajo. Si el error persiste, agregue el error en una publicación en el foro de FarmBeats o póngase en contacto en la dirección FarmBeatsSupport@microsoft.com.

## <a name="accelerator-troubleshooting"></a>Solución de problemas del acelerador

### <a name="access-control"></a>Control de acceso

**Error al agregar asignación de roles**

**Mensaje**: no se encuentran usuarios que coincidan.

**Acción correctiva**: compruebe el id. de correo para el que está intentando realizar una asignación de roles. El id. de correo debe coincidir exactamente con el registrado para ese usuario en Active Directory. Si el error persiste, agregue el error en una publicación en el foro de FarmBeats o póngase en contacto en la dirección FarmBeatsSupport@microsoft.com.

### <a name="unable-to-log-in-to-accelerator"></a>No se puede iniciar sesión en el acelerador

**Mensaje**: Error: no tiene autorización para llamar al servicio. Póngase en contacto con el administrador para obtener autorización.

**Acción correctiva**: pida al administrador que le autorice el acceso a la implementación de FarmBeats. Esto se puede hacer mediante un método POST de las API de RoleAssignment o a través de Access Control en el panel Configuración del acelerador.  

Si ya se ha agregado y se encuentra con este error, vuelva a intentarlo actualizando la página.  Si el error persiste, agregue el error en una publicación en el foro de FarmBeats o póngase en contacto en la dirección FarmBeatsSupport@microsoft.com.

![Proyecto de FarmBeats](./media/troubleshooting-farmbeats/accelerator-troubleshooting-1.png)

### <a name="accelerator---an-unknown-error-occurred"></a>Acelerador: se ha producido un error desconocido  

**Mensaje**: Error: se ha producido un error desconocido.

**Acción correctiva**: esto sucede si deja la página inactiva durante demasiado tiempo. Actualice la página.  

Si el error todavía persiste, agregue el error en una publicación en el foro de FarmBeats o póngase en contacto en la dirección FarmBeatsSupport@microsoft.com.

**El acelerador de FarmBeats no muestra la versión más reciente incluso después de actualizar FarmBeatsDeployment**

**Acción correctiva**: esto sucede debido a la persistencia del trabajo de servicio en el explorador.
Cierre todas las pestañas del explorador que tengan el acelerador abierto y cierre la ventana del explorador. Inicie una nueva instancia del explorador y vuelva a cargar el URI del acelerador. Esto cargará la nueva versión del acelerador.

## <a name="sentinel-imagery-related-issues"></a>Incidencias relacionadas con imágenes de Sentinel

### <a name="sentinel-wrong-username-or-password"></a>Nombre de usuario o contraseña de Sentinel incorrectos

**Mensaje de error de trabajo**: se requiere autenticación completa para tener acceso a este recurso.

**Acción de corrección**: vuelva a ejecutar el programa de instalación para actualizar el centro de datos con el nombre de usuario y la contraseña correctos.

**Acción correctiva**: vuelva a ejecutar el trabajo que ha producido el error o ejecute un trabajo de índices satélite para un rango de fechas de 5 a 7 días y compruebe si el trabajo se ha realizado correctamente.

### <a name="sentinel-hub-wrong-url-or-not-accessible"></a>Dirección URL incorrecta del centro de Sentinel o no accesible 

**Mensaje de error del trabajo**: vaya, algo ha fallado. La página a la que estaba intentando acceder se encuentra no disponible temporalmente. 

**Acción correctiva**:
1.  abra la dirección URL de Sentinel (https://scihub.copernicus.eu/dhus/) en el explorador y compruebe si el sitio web es accesible. 
2.  Si el sitio web no es accesible, compruebe si hay algún firewall, red de la empresa, etc., que está bloqueando el sitio web y adopte los pasos necesarios para permitir la dirección URL anterior. 
3.  Vuelva a ejecutar el trabajo que ha producido el error o ejecute un trabajo de índices satélite para un rango de fechas de 5 a 7 días y compruebe si el trabajo se ha realizado correctamente.  

### <a name="sentinel-server-down-for-maintenance"></a>Servidor de Sentinel fuera de servicio por mantenimiento

**Mensaje de error del trabajo**: La central de acceso abierto de Copernicus estará de vuelta próximamente. Lamentamos las molestias, estamos llevando a cabo tareas de mantenimiento en este momento. Pronto volveremos a estar en línea. 

**Acción correctiva**:

1.  Esta incidencia puede producirse si se realizan actividades de mantenimiento en el servidor de Sentinel. 
2.  Si se produce un error en algún trabajo o canalización con el motivo anterior, vuelva a enviar el trabajo más tarde. 
3.  El usuario puede visitar https://scihub.copernicus.eu/news/ para comprobar la información sobre las actividades de mantenimiento de Sentinel planeadas o no planeadas.  
4.  Vuelva a ejecutar el trabajo que ha producido el error o ejecute un trabajo de índices satélite para un rango de fechas de 5 a 7 días y compruebe si el trabajo se ha realizado correctamente.

### <a name="sentinel-maximum-number-of-connections-reached"></a>Se ha alcanzado el número máximo de conexiones de Sentinel

**Mensaje de error del trabajo**: el usuario "<username>" ha alcanzado el número máximo de dos flujos simultáneos. 

**Acción correctiva**:
1.  Si se produce un error en algún trabajo con el motivo anterior, significa que se está usando la misma cuenta de Sentinel en otra implementación o software. 
2.  El usuario puede crear una cuenta de Sentinel y volver a ejecutar el instalador para actualizar el centro de datos con el nuevo nombre de usuario y contraseña de Sentinel.  
3.  Vuelva a ejecutar el trabajo que ha producido el error o ejecute un trabajo de índices satélite para un rango de fechas de 5 a 7 días y compruebe si el trabajo se ha realizado correctamente.

### <a name="sentinel-server-refused-connection"></a>El servidor de Sentinel ha rechazado la conexión 

**Mensaje de error del trabajo**:

El servidor ha rechazado la conexión en: http://172.30.175.69:8983/solr/dhus 

**Acción correctiva**: esta incidencia puede producirse si se realizan actividades de mantenimiento en el servidor de Sentinel. 
1.  Si se produce un error en algún trabajo o canalización con el motivo anterior, vuelva a enviar el trabajo más tarde. 
2.  El usuario puede visitar https://scihub.copernicus.eu/news/ para comprobar la información sobre las actividades de mantenimiento de Sentinel planeadas o no planeadas.  
3.  Vuelva a ejecutar el trabajo que ha producido el error o ejecute un trabajo de índices satélite para un rango de fechas de 5 a 7 días y compruebe si el trabajo se ha realizado correctamente.


## <a name="collect-logs-manually"></a>Recopilación de registros de forma manual

[Instale y configure]( https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows) el Explorador de Azure Storage.

### <a name="how-to-collect-data-hub-adf-job-logs"></a>Procedimientos para recopilar registros de trabajo de ADF del centro de datos
1. Inicie sesión en https://portal.azure.com.
2. En el cuadro de texto **Buscar**, busque el grupo de recursos del centro de datos de FarmBeats.

    > [!NOTE]
    > Seleccione el grupo de recursos del centro de datos que se ha especificado en el momento de la implementación de FarmBeats.

En el panel del grupo de recursos, busque la cuenta de almacenamiento (datahublogs...). Por ejemplo, datahublogsmvxmq.  

1.  Seleccione la cuenta de almacenamiento en la columna **Nombre** para ver el panel **Cuenta de almacenamiento**.
2.  En la página (datahubblogs...), seleccione **Abrir en el explorador** para ver la aplicación **Abrir el Explorador de Azure Storage**.
3.  En el panel de la izquierda, (datahubblogs...), **Contenedores de blobs**, seleccione **job-logs**.
4.  En la pestaña **job-logs**, seleccione **Descargar**.
5.  Seleccione la ubicación para descargar los registros en una carpeta local de la máquina.
6.  Envíe por correo el archivo ZIP descargado a farmbeatssupport@microsoft.com.

    ![Proyecto de FarmBeats](./media/troubleshooting-farmbeats/collecting-logs-manually-1.png)

### <a name="how-to-collect-accelerator-adf-job-logs"></a>Procedimientos para recopilar registros de trabajo de ADF del acelerador

1.  Inicie sesión en https://portal.azure.com.
2.  En el cuadro de texto **Buscar**, busque el grupo de recursos del acelerador de FarmBeats.

    > [!NOTE]
    > Seleccione el grupo de recursos del acelerador que se ha especificado en el momento de la implementación de FarmBeats.

3.  En el panel del grupo de recursos, busque storage... cuenta de almacenamiento. Por ejemplo, storagedop4k.
4.  Seleccione la cuenta de almacenamiento en la columna **Nombre** para ver el panel Cuenta de almacenamiento.
5.  En la página (storage...), seleccione **Abrir en el explorador** para ver la aplicación Abrir el Explorador de Azure Storage.
6.  En el panel de la izquierda, (storage...), **Contenedores de blobs**, seleccione **job-logs**.
7.  En la pestaña **job-logs**, seleccione **Descargar**.
8.  Seleccione la ubicación para descargar los registros en una carpeta local de la máquina.
9.  Envíe por correo el archivo ZIP descargado a farmbeatssupport@microsoft.com.


### <a name="how-to-collect-data-hub-app-service-logs"></a>Procedimientos para recopilar registros de trabajo de la aplicación del centro de datos

1.  Inicie sesión en https://portal.azure.com.
2.  En el cuadro de texto **Buscar**, busque el grupo de recursos del centro de datos de FarmBeats.

    > [!NOTE]
    > Seleccione el grupo de recursos del centro de datos que se ha especificado en el momento de la implementación de FarmBeats.

3.  En el grupo de recursos, busque la cuenta de almacenamiento (datahublogs...). Por ejemplo, datahublogsmvxmq.
4.  Seleccione la cuenta de almacenamiento en la columna **Nombre** para ver el panel **Cuenta de almacenamiento**.
5.  En la página (datahubblogs...), seleccione **Abrir en el explorador** para ver la aplicación **Abrir el Explorador de Azure Storage**.
6.  En el panel de la izquierda, (datahubblogs...), **Contenedores de blobs**, seleccione appinsights-logs.
7.  En la pestaña appinsights-logs, seleccione **Descargar**.
8.  Seleccione la ubicación para descargar los registros en una carpeta local de la máquina.
9.  Envíe por correo la carpeta descargada a farmbeatssupport@microsoft.com.

### <a name="how-to-collect-accelerator-app-service-logs"></a>Procedimientos para recopilar registros de servicio de la aplicación del acelerador

1.  Inicie sesión en https://portal.azure.com.
2.  En **Buscar**, busque el grupo de recursos del acelerador de FarmBeats.

    > [!NOTE]
    > Seleccione el grupo de recursos del acelerador de FarmBeats que se ha proporcionado en el momento de la implementación de FarmBeats.

3.  En el grupo de recursos, busque la cuenta de almacenamiento (storage...). Por ejemplo, storagedop4k.
4.  Seleccione la cuenta de almacenamiento en la columna **Nombre** para ver el panel **Cuenta de almacenamiento**.
5.  En la página (storage...), seleccione **Abrir en el explorador** para ver la aplicación **Abrir el Explorador de Azure Storage**.
6.  En el panel de la izquierda, (storage...), **Contenedores de blobs**, seleccione appinsights-logs.
7.  En la pestaña appinsights-logs, seleccione **Descargar**.
8.  Seleccione la ubicación para descargar los registros en una carpeta local de la máquina.
9.  Envíe por correo la carpeta descargada a farmbeatssupport@microsoft.com.

## <a name="known-issues"></a>Problemas conocidos

## <a name="batch-related-issues"></a>Incidencias relacionadas con lotes

**Error**: se ha alcanzado la cuota de cuenta regional de las cuentas de lote para la suscripción especificada.

**Acción correctiva**: aumente la cuota o elimine las cuentas de lote sin usar y vuelva a ejecutar la implementación.

### <a name="azure-active-directory-related-issues"></a>Incidencias relacionados con Azure Active Directory

**Error**: no se ha podido actualizar la configuración necesaria en la aplicación de Azure AD d41axx40-xx21-4fbd-8xxf-97xxx9e2xxc0: No tiene privilegios suficientes para completar la operación. Asegúrese de que los valores anteriores están configurados correctamente para la aplicación de Azure AD.

**Significado**: la configuración del registro de aplicación de Azure AD no se ha realizado correctamente.  

**Acción correctiva**: pida al administrador de TI (que tiene acceso de lectura de inquilino) que use nuestro [script](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect/tree/master/AppCreationScripts) para crear el registro de aplicación de Azure AD. Este script también se ocupará automáticamente de los pasos de configuración.

**Error**: no se ha podido crear la nueva aplicación de Active Directory "dummyname" en este inquilino: ya existe otro objeto con el mismo valor para los URI de identificador de propiedad.

**Significado**: ya existe un registro de aplicación de Azure AD con el mismo nombre.

**Acción correctiva**: elimine el registro de aplicación de Azure AD existente o vuelva a usarlo para la instalación. Si va a reutilizar el registro de Azure AD existente, pase el id. de aplicación y el secreto de cliente al instalador y vuelva a implementarlo.

## <a name="inputjson-related-issues"></a>Incidencias relacionados con input.json

**Error** al leer la entrada del archivo Input.json

**Acción correctiva**: esta incidencia surge principalmente debido a un error al especificar la ruta de acceso o el nombre JSON de entrada correctos al instalador. Realice las correcciones apropiadas y vuelva a intentar la implementación.

**Error al analizar la entrada JSON**

**Acción correctiva**: esta incidencia surge principalmente debido a la existencia de valores incorrectos en el archivo JSON de entrada. Realice las correcciones apropiadas y vuelva a intentar la implementación.

## <a name="high-cpu-usage"></a>Uso elevado de CPU

**Error**: recibirá una alerta de correo que hace referencia a una alerta de uso intensivo de la CPU. 

**Acción correctiva**: 
1.  Vaya al grupo de recursos del centro de datos de FarmBeats.
2.  Seleccione la App Service.  
3.  Vaya a escalar verticalmente (plan de App Service) y seleccione un [plan de tarifa](https://azure.microsoft.com/pricing/details/app-service/windows/) adecuado.

## <a name="next-steps"></a>Pasos siguientes

Si sigue encontrándose con incidencias, póngase en contacto con nosotros en nuestro [Foro de soporte técnico](https://social.msdn.microsoft.com/Forums/home?forum=ProjectFarmBeats).
