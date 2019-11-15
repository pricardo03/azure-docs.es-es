---
title: Implementación de FarmBeats de Azure
description: Describe cómo implementar FarmBeats
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: c609285b727414b4849c9ef6654406a035005bb1
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2019
ms.locfileid: "73797245"
---
# <a name="deploy-farmbeats"></a>Implementación de FarmBeats

En este artículo se describe cómo configurar FarmBeats de Azure.

FarmBeats de Azure es una solución extensible y específica del sector para la agricultura controlada por datos que permite la conectividad sin interrupciones del aprovisionamiento y los dispositivos de sensor a la nube de Azure, la recopilación de datos de telemetría y la agregación. FarmBeats de Azure tiene varios sensores, como cámaras, drones, sensores de suelo y la administración de dispositivos desde la nube, que incluye la infraestructura y los servicios de Azure para los dispositivos listos para IoT (Internet de las cosas) a una aplicación web y móvil ampliable con el fin de proporcionar visualización, alertas e información.

> [!NOTE]
> FarmBeats de Azure solo se admite en entornos de nube pública. Para obtener más información sobre el entorno de nube, vea [Azure](https://azure.microsoft.com/overview/what-is-a-public-cloud/).

FarmBeats de Azure tiene los dos componentes siguientes:

- **Centro de datos**: el centro de datos es el nivel de plataforma de FarmBeats de Azure que permite compilar, almacenar y procesar datos, así como extraer información de las canalizaciones de datos nuevas o existentes. Este nivel de plataforma es útil para ejecutar y compilar canalizaciones y modelos de datos de agricultura.

- **Acelerador**: el acelerador es el nivel de solución de FarmBeats de Azure que tiene una aplicación integrada para ilustrar las capacidades de FarmBeats de Azure mediante los modelos de agricultura creados previamente. Este nivel de solución permite crear límites de granjas y extraer información de los datos de la agricultura en el contexto del límite de la granja.

Una implementación rápida de FarmBeats de Azure debe tardar menos de una hora. Los costos del centro de datos y del acelerador varían en función del uso.

## <a name="deployed-resources"></a>Recursos implementados

La implementación de FarmBeats de Azure crea los recursos siguientes en la suscripción:

|S.No  |Nombre de recurso  |Componente de FarmBeats de Azure  |
|---------|---------|---------|
|1  |       Azure Cosmos DB   |  Centro de datos       |
|2  |    Application Insights      |     Centro de datos/acelerador     |
|3  |Azure Cache for Redis   |Centro de datos   |
|4  |       Azure KeyVault    |  Centro de datos/acelerador        |
|5  |    Time Series Insights       |     Centro de datos      |
|6 |      Espacio de nombres de EventHub    |  Centro de datos       |
|7  |    Azure Data Factory V2       |     Centro de datos/acelerador      |
|8  |Cuenta de Batch    |Centro de datos   |
|9  |       Cuenta de almacenamiento     |  Centro de datos/acelerador        |
|10  |    Aplicación lógica        |     Centro de datos      |
|11  |    Conexión de API        |     Centro de datos      |
|12|      App Service      |  Centro de datos/acelerador       |
|13 |    Plan del Servicio de aplicaciones        |     Centro de datos/acelerador      |
|14 |Cuenta de Azure Maps     |Acelerador    |
|15 |       Time Series Insights      |  Centro de datos     |

FarmBeats de Azure está disponible para su descarga en Azure Marketplace. Puede tener acceso directamente desde Azure Portal.  

## <a name="prepare"></a>Preparación

Necesita los permisos siguientes para implementar FarmBeats de Azure:

- Inquilino: acceso de lectura
- Suscripción: colaborador a propietario
- Grupo de recursos: propietario

## <a name="before-you-begin"></a>Antes de empezar

Antes de iniciar la implementación, asegúrese de que tiene lo siguiente:

- Cuenta de Sentinel
- Azure Active Directory (registro de aplicación)
- FarmBeats de Azure

## <a name="create-a-sentinel-account"></a>Creación de una cuenta de Sentinel    

Una cuenta con Sentinel ayuda a descargar la imagen de satélite de Sentinel desde su sitio web oficial al dispositivo. Siga estos pasos para crear una cuenta gratuita:

1. Vaya a https://scihub.copernicus.eu/dhus/#/self-registration. En la página Registro, proporcione un nombre, un apellido, un nombre de usuario, una contraseña y un correo.
2. En el menú desplegable **Seleccionar dominio**, seleccione la opción **Terrenos**.
3. En el menú desplegable **Seleccionar uso**, seleccione la opción **Educación**.
4. En el menú desplegable **Seleccionar país**, seleccione su país.
5. Seleccione **Registrar** para completar el proceso de registro.

Se enviará un correo de comprobación a la dirección de correo electrónico registrada para su confirmación. Seleccione el vínculo y confirme. El proceso de registro se he completado.

## <a name="create-azure-ad-app-registration"></a>Creación de un registro de aplicación de Azure AD

Para la autenticación y autorización en FarmBeats de Azure, debe tener un registro de aplicación de Azure Active Directory que cumpla lo siguiente:

- Caso 1: el instalador pueda crear automáticamente (siempre que tenga los permisos de acceso de inquilino, suscripción y grupo de recursos necesarios).
- Caso 2: pueda crear y configurar antes de implementar FarmBeats de Azure (requiere pasos manuales).

**Caso 1**: el instalador presupone que se tienen los derechos para crear un registro de aplicación de Azure Active Directory dentro de la suscripción de su elección. Para registrarse, inicie sesión en el portal, vaya a **Azure Active Directory** > **Registro de aplicación** > **Nuevo registro**.

Si ya tiene una suscripción, puede pasar directamente al siguiente procedimiento.

**Caso 2**: Este método es el paso preferido cuando no se tienen suficientes derechos para crear y configurar un registro de aplicación de Azure AD en la suscripción. Solicite al administrador que use el [script personalizado](https://aka.ms/FarmBeatsAADScript), que ayudará a los administradores de TI a generar y configurar automáticamente el registro de aplicación de Azure AD en Azure Portal. Como resultado de la ejecución de este script personalizado con el entorno de PowerShell, el administrador de TI debe compartir un id. de cliente de aplicación y un secreto de contraseña de Azure Active Directory con usted. Anote estos valores.

Siga los pasos siguientes para ejecutar el script del registro de aplicación de Azure AD:

1. Obtenga el [script](https://aka.ms/FarmBeatsAADScript) de registro.
2. Inicie sesión en Azure Portal y seleccione la suscripción y el inquilino de AD.
3. Inicie Cloud Shell en la navegación superior de Azure Portal.

    ![Proyecto de FarmBeats](./media/prepare-for-deployment/navigation-bar-1.png)


4. A los usuarios principiantes se les pedirá que seleccionen una suscripción para crear una cuenta de almacenamiento y un recurso compartido de Microsoft Azure Files. Seleccione **Crear almacenamiento**.
5. A los usuarios principiantes se les pedirá que elijan la experiencia de shell de su preferencia: Bash o PowerShell. Elija PowerShell.
6. En Cloud Shell, escriba los comandos siguientes para ejecutar el script.

    ```powershell
    ./create_aad_script.ps1
    ```
7. Tome nota del id. de aplicación de Azure AD y el secreto de cliente para compartirlo con la persona que implemente FarmBeats de Azure.

## <a name="create-azure-farmbeats-offer-on-marketplace"></a>Creación de una oferta de FarmBeats de Azure en Marketplace

Siga estos pasos para crear una oferta de FarmBeats de Azure en Marketplace:

1. Inicie sesión en **Azure Portal**, seleccione su cuenta en la esquina superior derecha y cambie al inquilino de Azure AD en el que quiera implementar FarmBeats de Microsoft Azure.
2. Seleccione **Buscar/Marketplace** o **Crear recursos**. Escriba **FarmBeats** para ver los detalles de la oferta. Descargue las guías disponibles a través de los hipervínculos de aka.ms en esta página antes de continuar con los pasos siguientes.
3. Seleccione **Crear** y escriba la información siguiente:

   - Escriba el nombre de la suscripción.
   - Escriba un nombre de grupo de recursos existente (solo grupo de recursos en blanco) o cree un grupo de recursos nuevo para la implementación FarmBeats de Azure. Tome nota de este grupo de recursos para escribirlo en el archivo input.json en una fase posterior.
   - Escriba la región en la que quiere instalar FarmBeats de Azure. FarmBeats se puede instalar actualmente en estas regiones: Centro de EE. UU., Oeste de Europa, Este de EE. UU. 2, Norte de Europa, Oeste de EE. UU., Sudeste Asiático, Este de EE. UU., Este de Australia, Oeste de EE. UU. 2.
4. Seleccione **Aceptar**, lo que le redirigirá a la página Condiciones de uso. Revise las condiciones estándar de Marketplace o seleccione el hipervínculo para revisar las Condiciones de uso.
5. Seleccione **Cerrar**, luego la casilla "Acepto" y, después, seleccione **Crear**.
6. Ya ha firmado correctamente el Contrato de licencia para el usuario final (CLUF) de FarmBeats de Azure en Marketplace. Para continuar con la implementación, siga los pasos siguientes de esta guía.

### <a name="prepare-inputjson-file"></a>Preparación del archivo input.json

Este es el archivo de entrada para Azure Cloud Shell y, durante la implementación, no se le pedirán los parámetros cuyos valores se especifican en este archivo antes de la carga, por lo que se ahorrará tiempo.  

> [!NOTE]
> Este archivo introduce valores en Azure Cloud Shell.  Para ahorrar tiempo, durante la implementación no se le pedirán los parámetros que agregue a este archivo. Se le pedirán los parámetros que falten.

Revise los parámetros antes de preparar el archivo.

|Get-Help | DESCRIPCIÓN|
|--- | ---|
|"sku"  | Proporciona una opción para descargar uno de los componentes de FarmBeats de Azure, o ambos. Especifica los componentes que se van a descargar. Para instalar solo el centro de datos, use "onlydatabhub". Para instalar el centro de datos y el acelerador, use "both".|
|“subscriptionId”  | Especifica la suscripción para la instalación de FarmBeats.|
|"datahubResourceGroup"  | Nombre del grupo de recursos para los recursos del centro de datos.|
|"datahubLocation" | Ubicación en la que quiere almacenar los recursos del centro de datos.|
|“acceleratorWebsiteName”  |Prefijo único de dirección URL para asignar un nombre al centro de datos.
Sitio web de Swagger. El valor predeterminado es el nombre del grupo de recursos del centro de datos. Presione Entrar para continuar con el valor predeterminado.|
|"acceleratorResourceGroup"  | Nombre del grupo de recursos para los recursos del centro de datos. |
|"acceleratorLocation"  | Ubicación para almacenar los recursos del centro de datos.
"acceleratorWebsiteName"  | Prefijo único de la dirección URL para asignar un nombre al sitio web del acelerador. El valor predeterminado es acelerador. Presione Entrar para continuar con el valor predeterminado.|
|''sentinelUsername'' | Nombre de usuario con el que iniciar sesión: https://scihub.copernicus.eu/dhus/#/self-registration.|
|"sentinelPassword"  | Contraseña con la que iniciar sesión: https://scihub.copernicus.eu/dhus/#/self-registration.|
|"farmbeatsAppId"  | Valores que compartirá el equipo de FarmBeats de Azure.  |
|"farmbeatsPassword"  | Valores que compartirá el equipo de FarmBeats de Azure.|
|"notificationEmailAddress"  | Dirección de correo electrónico para recibir las notificaciones de las alertas configuradas en el centro de datos.|
|"upda"aadAppClientId" "  |[**Opcional**] Parámetro que se va a incluir en input.json solo si la aplicación de Azure AD ya existe.  - True o False. False para una primera instalación y True para el escenario de actualización.|
|"aadAppClientId"  | [**Opcional**] Parámetro que se va a incluir en input.json solo si la aplicación de Azure AD ya existe.  |
|"aadAppClientSecret"   | [**Opcional**] Parámetro que se va a incluir en input.json solo si la aplicación de Azure AD ya existe.|


Entrada JSON de ejemplo:

```json
{  
   "sku":"both", 
   "subscriptionId": "da9xxxec-dxxf-4xxc-xxx21-xxx3ee7xxxxx", 
   "datahubResourceGroup": "dummy-test-dh1", 
   "datahubLocation": "westus2", 
   "datahubWebsiteName": "dummy-test-dh1", 
   "acceleratorResourceGroup": "dummy-test-acc1", 
   "acceleratorLocation": "westus2", 
   "acceleratorWebsiteName": "dummy-test-acc1", 
   "sentinelUsername": "dummy-dev", 
   "farmbeatsAppId": "c3cb3xxx-27xx-4xxb-8xx6-3xxx2xxdxxx5c", 
   "notificationEmailAddress": "dummy@microsoft.com", 
   "updateIfExists": true
}
```
## <a name="deploy-within-cloud-shell-browser-based-command-line"></a>Implementación en la línea de comandos basada en el explorador de Cloud Shell

Como parte del flujo de trabajo de Marketplace, se ha creado un grupo de recursos y firmado el Contrato de licencia para el usuario final, que se puede revisar una vez más como parte de la implementación real. La implementación se realizará a través de Azure Cloud Shell (línea de comandos basada en explorador) con el entorno Bash.  

> [!NOTE]
> Las sesiones de Cloud Shell inactivas expiran después de 20 minutos. Intente completar la implementación durante este tiempo.

1. Inicie sesión en **Azure Portal** y seleccione la suscripción y el inquilino de AD de su elección.
2. Inicie **Cloud Shell** en la navegación superior de **Azure Portal**.

   ![Proyecto de FarmBeats](./media/prepare-for-deployment/navigation-bar-1.png)

3. Seleccione una suscripción para crear una cuenta de almacenamiento y un recurso compartido de Microsoft Azure Files.
4. Seleccione **Crear almacenamiento**.
5. Seleccione el menú desplegable del entorno que se encuentra en el lado izquierdo de la ventana del shell (Bash).

   ![Proyecto de FarmBeats](./media/prepare-for-deployment/bash-1-1.png)

### <a name="deployment-scenario-1"></a>Escenario de implementación 1

El instalador crea Azure AD (se tienen permisos de lectura de inquilinos de AD).  

1. Descargue la plantilla [input.json](https://aka.ms/PPInputJsonTemplate). Incluya el id. de cliente y la contraseña de Aplicación de Azure en el archivo input.json y guárdelo.
2. Abra el archivo descargado en un bloc de notas y rellénelo especificando los valores.

    ```json
    {
    "sku":"both",
    "subscriptionId":"daxx9xxx-d18f-4xxc-9c21-5xx3exxxxx45",
    "datahubResourceGroup":"dummy-test-dh1",  
    "location":"westus2",  
    "datahubWebsiteName":"dummy-test-dh1",  
    "acceleratorResourceGroup":"dummy-test-acc1",  
    "acceleratorWebsiteName":"dummy-test-acc1",  
    "sentinelUsername":"dummy-dev",
    "notificationEmailAddress":"dummyuser@org1.com",
    "updateIfExists":true
    }

    ```

    > [!NOTE]
    > Puede omitir este paso. En tal caso, las entradas se solicitarán en tiempo de ejecución.

3. Guarde el archivo y tome nota de la ruta de acceso (en el equipo local).  
4. Vaya a Azure Cloud Shell y, después de una autenticación correcta, seleccione la carga (vea el icono resaltado en la imagen siguiente) y cargue el archivo input.json en el almacenamiento de Cloud Shell. No es necesario pasar el parámetro de Azure AD en el archivo JSON, ya que el instalador creará y configurará el registro de aplicación de Azure AD.

   ![Proyecto de FarmBeats](./media/prepare-for-deployment/bash-2-1.png)

5. Copie y pegue el "Comando de implementación"en Cloud Shell. Asegúrese de modificar la ruta de acceso al archivo input.json y presione Entrar.  

    ```
    wget -N -O farmbeats-installer.sh https://aka.ms/FB_1.2.0 && bash farmbeats-installer.sh> /home/dummyuser/input.json

    ```
   El script descarga automáticamente todas las dependencias y compila el implementador. Después, se le pedirá que acepte el Contrato de licencia para el usuario final (CLUF) de FarmBeats de Azure.

   - Escriba "S" si está de acuerdo y continuará con el paso siguiente.
   - Escriba "N" si no está de acuerdo con las condiciones y la implementación finalizará.

   Luego, se le pedirá que introduzca un token de acceso para la implementación. Copie el código generado e inicie sesión en https://microsoft.com/devicelogin con sus credenciales de Azure.

   > [!NOTE]
   > El código expira después de 60 minutos. Cuando expire, se puede reiniciar al escribir el comando de implementación.

6. En la siguiente petición de datos, escriba la contraseña de la cuenta Sentinel.
7. El instalador ahora valida e inicia la implementación, que puede tardar unos 20 minutos.
8. Una vez que la implementación se ha realizado correctamente, recibirá los siguientes vínculos de salida:
    - **URL del centro de datos**: vínculo de Swagger para probar las API de FarmBeats de Azure.
    - **URL del acelerador**: interfaz de usuario para explorar el acelerador de granja inteligente de FarmBeats de Azure.
    - **Archivo de registro del implementador**: archivo de registro creado durante la implementación. Se puede usar para solucionar problemas.

    - Escriba "S" si está de acuerdo y continuará con el paso siguiente.
    - Escriba "N" si no está de acuerdo con las condiciones y la implementación finalizará.

   Luego, se le pedirá que introduzca un token de acceso para la implementación. Copie el código generado e inicie sesión en https://microsoft.com/devicelogin con la credenciales de Azure.

   > [!NOTE]
    > Tome nota de esta información y mantenga la ruta de acceso del archivo de registro de la implementación a mano para su uso en el futuro.


### <a name="deployment-scenario-2"></a>Escenario de implementación 2

El registro de aplicación existente de Azure Active Directory se usa para implementar.

1. Descargue el [JSON de entrada](https://aka.ms/PPInputJsonTemplate) Incluya el id. de cliente y la contraseña de Aplicación de Azure en el archivo input.json y guárdelo.

    ```json
       {
       "sku":"both",
       "subscriptionId":"daxx9xxx-d18f-4xxc-9c21-5xx3exxxxx45",
       "datahubResourceGroup":"dummy-test-dh1",  
       "location":"westus2",  
       "datahubWebsiteName":"dummy-test-dh1",  
       "acceleratorResourceGroup":"dummy-test-acc1",  
       "acceleratorWebsiteName":"dummy-test-acc1",  
       "sentinelUsername":"dummy-dev",
       "notificationEmailAddress":"dummyuser@org1.com",
       "updateIfExists": true,
       "aadAppClientId": "lmtlemlemylmylkmerkywmkm823",
       "aadAppClientSecret": "Kxxxqxxxxu*kxcxxx3Yxxu5xx/db[xxx"
       }
     ```

     Siga el resto de pasos:

2. Tome nota de la ruta de acceso al archivo input.json (en el equipo local).
3. Vaya a Azure Cloud Shell de nuevo y verá que está autenticado de forma correcta, seleccione el botón de carga (vea el icono resaltado en la imagen siguiente) y cargue el archivo input.json en el almacenamiento de Cloud Shell.

    ![Proyecto de FarmBeats](./media/prepare-for-deployment/bash-2-1.png)

4. Copie o pegue el *Comando de implementación* en Cloud Shell. Asegúrese de modificar la ruta de acceso al archivo input.json y presione Entrar.

    ```
    wget -N -O farmbeats-installer.sh https://aka.ms/FB_1.2.0 && bash farmbeats-installer.sh> /home/dummyuser/input.json

    ```
   Siga el resto de pasos:

5. El script descarga automáticamente todas las dependencias y compila el implementador.
6. Se le pedirá que lea y acepte el Contrato de licencia para el usuario final (CLUF) de FarmBeats de Azure.

   - Escriba "S" si está de acuerdo y continuará con el paso siguiente.
   - Escriba "N" si no está de acuerdo con las condiciones y la implementación finalizará.

7. Luego, se le pedirá que introduzca un token de acceso para la implementación. Copie el código generado e inicie sesión en https://microsoft.com/devicelogin con la credenciales de Azure.
8. El instalador validará y empezará ahora a crear los recursos, lo que puede tardar unos 20 minutos. Mantenga la sesión activa en Cloud Shell durante este tiempo.
9. Cuando la implementación se haya realizado correctamente, recibirá los siguientes vínculos de salida:
   - **URL del centro de datos**: vínculo de Swagger para probar las API de FarmBeats.
   - **URL del acelerador**: interfaz de usuario para explorar el acelerador de granja inteligente de FarmBeats.
   - **Archivo de registro del implementador**: guarda los registros durante la implementación y se puede usar para solucionar problemas.

Si tiene alguna incidencia, revise [Solución de problemas](troubleshoot-project-farmbeats.md).


## <a name="validate-deployment"></a>Validación de la implementación

### <a name="data-hub"></a>Centro de datos

Una vez completada la instalación del centro de datos, recibirá la dirección URL para acceder a las API de FarmBeats de Azure a través de la interfaz de Swagger en el formato siguiente: https://\<nombre_del_sitio_web_del_centro_de_datos>.azurewebsites.net.

1. Para iniciar sesión a través de Swagger, copie y pegue la dirección URL en el explorador.
2. Inicie sesión con las credenciales de Azure Portal.
3. Prueba de integridad (opcional)

     - Se puede iniciar sesión correctamente en el portal de Swagger mediante el vínculo del centro de datos que se ha recibido como salida a una implementación correcta.
     - API Get de tipos ampliados: seleccione "Probar/Ejecutar".
     - Debe recibir el código de respuesta del servidor 200 y no una excepción, como 403 "usuario no autorizado".

### <a name="accelerator"></a>Acelerador

Una vez completada la instalación del acelerador, recibirá la dirección URL para acceder a la interfaz de usuario de FarmBeats en el formato siguiente: https://\<nombre_del_sitio_web_del_acelerador>.azurewebsites.net

1. Para iniciar sesión a través del acelerador, copie y pegue la dirección URL en el explorador.
2. Inicie sesión con las credenciales de Azure Portal.
3. Ejecute una prueba de integridad opcional.

    - Compruebe si ha podido iniciar sesión correctamente en el portal del acelerador mediante el vínculo del acelerador que se ha recibido como salida a una implementación correcta.
    - Seleccione **Crear granja**.
    - En el icono "?", abra las guías de FarmBeats con el botón **Empezar**.

## <a name="upgrade"></a>Actualizar

Los pasos para la actualización son parecidos a los de la primera instalación. Siga estos pasos:

1. Inicie sesión en Azure Portal y seleccione la suscripción y el inquilino de AD deseados.
2. Inicie Cloud Shell en la navegación superior de Azure Portal.

   ![Proyecto de FarmBeats](./media/prepare-for-deployment/navigation-bar-1.png)

3. Seleccione una suscripción para crear una cuenta de almacenamiento y un recurso compartido de Azure Files.
4. Seleccione **Crear almacenamiento**.
5. Seleccione el entorno desde el menú desplegable que se encuentra en el lado izquierdo del shell (Bash).
6. Realice cambios en el archivo input.json solo si es necesario y cárguelo en Azure Cloud Shell. Por ejemplo, puede actualizar la dirección de correo electrónico de la notificación que quiere recibir.
7. Tome nota de la ruta de acceso del archivo input.json (en el equipo local).
8. Vaya a Azure Cloud Shell. Cuando esté autenticado de forma correcta, seleccione el botón de carga (vea el icono resaltado en la imagen siguiente) y cargue el archivo input.json en el almacenamiento de Cloud Shell.

   ![Proyecto de FarmBeats](./media/prepare-for-deployment/bash-2-1.png)

9. Copie o pegue el **Comando de implementación** en Cloud Shell. Asegúrese de modificar la ruta de acceso al archivo input.json y presione Entrar.

    ```
    wget -N -O farmbeats-installer.sh https://aka.ms/FB_1.2.0 && bash farmbeats-installer.sh> /home/dummyuser/input.json

    ```
    Siga el resto de pasos:

10. El instalador solicita automáticamente las entradas necesarias en tiempo de ejecución:

    - Introduzca un token de acceso para la implementación. Copie el código generado e inicie sesión en https://microsoft.com/devicelogin con la credenciales de Azure.

     > [!NOTE]
     > El código expira después de 60 minutos. Cuando expire, se puede reiniciar al escribir el comando de implementación.

     - Contraseña de Sentinel

11. El instalador validará y empezará ahora a crear los recursos, lo que puede tardar unos 20 minutos.
12. Una vez que la implementación se ha realizado correctamente, recibirá los siguientes vínculos de salida:

    - **URL del centro de datos**: vínculo de Swagger para probar las API de FarmBeats.
    - **URL del acelerador**: interfaz de usuario para explorar el acelerador de granja inteligente de FarmBeats.
    - **Archivo de registro del implementador**: guarda los registros durante la implementación. Se puede usar para solucionar problemas.

    > [!NOTE]
    > Tome nota de los vínculos siguientes y mantenga la ruta de acceso del archivo de registro de la implementación a mano para su uso en el futuro.

## <a name="uninstall"></a>Desinstalación

Actualmente no se admite la desinstalación automatizada de FarmBeats mediante el instalador. Para quitar el centro de datos o el acelerador, en Azure Portal, elimine el grupo de recursos en el que están instalados estos componentes o elimine los recursos manualmente.

Por ejemplo, si se ha implementado el centro de datos y el acelerador en dos grupos de recursos distintos, elimine los grupos de recursos de la siguiente manera:

1. Inicie sesión en Azure Portal.
2. Seleccione la cuenta en la esquina superior derecha y cambie al inquilino de Azure AD en el que quiera implementar FarmBeats de Azure.

   > [!NOTE]
   > Para que el acelerador funcione correctamente, es necesario el centro de datos. No se recomienda desinstalar el centro de datos sin desinstalar el acelerador.

3. Seleccione Grupos de recursos y escriba el nombre del centro de datos o del grupo de recursos del acelerador que quiera eliminar.
4. Seleccione el nombre del grupo de recursos. Escriba el nombre de nuevo para comprobarlo y haga clic en Eliminar para quitar el grupo de recursos y todos sus recursos subyacentes.
5. También puede eliminar cada recurso manualmente, pero no se recomienda.
7. Para eliminar o desinstalar el centro de datos, vaya directamente a Grupo de recursos en Azure y elimine el grupo de recursos desde ahí.

## <a name="next-steps"></a>Pasos siguientes

Ya se ha implementado FarmBeats de Azure. Ahora, obtenga información sobre cómo [crear granjas](manage-farms.md#create-farms).
