---
title: Uso de directivas de asignación personalizadas con IoT Hub Device Provisioning Service | Microsoft Docs
description: Uso de directivas de asignación personalizadas con IoT Hub Device Provisioning Service
author: wesmc7777
ms.author: wesmc
ms.date: 04/10/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: philmea
ms.openlocfilehash: 1e672e7bd43dcd05d048d22205939749c1d96579
ms.sourcegitcommit: e72073911f7635cdae6b75066b0a88ce00b9053b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/19/2019
ms.locfileid: "68348068"
---
# <a name="how-to-use-custom-allocation-policies"></a>Uso de directivas de asignación personalizadas


Una directiva de asignación personalizada le ofrece más control sobre cómo se asignan los dispositivos a un centro de IoT. Esto se logra mediante el uso de código personalizado en una [función de Azure](../azure-functions/functions-overview.md) para asignar dispositivos a un centro de IoT. El servicio de aprovisionamiento de dispositivos llama al código de función de Azure y proporciona toda la información pertinente sobre el dispositivo y la inscripción. El código de la función se ejecuta y devuelve la información del centro de IoT para aprovisionar el dispositivo.

Mediante el uso de directivas de asignación personalizada, puede definir sus propias directivas de asignación cuando las directivas proporcionadas por el servicio de aprovisionamiento de dispositivos no cumplan con los requisitos de su escenario.

Por ejemplo, quizás desee examinar el certificado que está usando un dispositivo durante el aprovisionamiento y asignar el dispositivo a un centro de IoT basado en una propiedad del certificado. Es posible que tenga la información almacenada en una base de datos para los dispositivos y necesite consultarla para determinar a qué centro de IoT se debe asignar el dispositivo.


En este artículo se muestra una directiva de asignación personalizada mediante una función de Azure escrita en C#. Se crean dos centros de IoT para representar una *División de tostadoras Contoso* y una *División de bombas de calor Contoso*. Los dispositivos que soliciten aprovisionamiento deben tener un identificador de registro con uno de los siguientes sufijos para que se acepte el aprovisionamiento:

- **-contoso-tstrsd-007**: División de tostadoras Contoso
- **-contoso-hpsd-088**: División de bombas de calor de Contoso

Los dispositivos se aprovisionarán según el sufijo que esté presente en el identificador de registro. Estos dispositivos se simularán mediante un ejemplo de aprovisionamiento incluido en el [SDK de Azure IoT para C](https://github.com/Azure/azure-iot-sdk-c). 

En este artículo, llevará a cabo los siguientes pasos:

* Usará la CLI de Azure para crear dos centros de IoT para divisiones de Contoso (**división de tostadoras Contoso** y **división de bombas de calor Contoso**)
* Creará una nueva inscripción de grupo mediante una función de Azure para la directiva de asignación personalizada
* Creará claves de dispositivo para dos simulaciones de dispositivo.
* Configurará el entorno de desarrollo del SDK de Azure IoT para C
* Simulará los dispositivos para comprobar que se aprovisionen según el código de ejemplo de la directiva de asignación personalizada


[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

* Finalización de la guía de inicio rápido [Configuración de Azure IoT Hub Device Provisioning Service con Azure Portal](./quick-setup-auto-provision.md).
* [Visual Studio](https://visualstudio.microsoft.com/vs/) 2015 o posterior con la carga de trabajo ["Desarrollo para el escritorio con C++"](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/) habilitada.
* Tener instalada la versión más reciente de [Git](https://git-scm.com/download/).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-two-divisional-iot-hubs"></a>Crear dos centros de IoT de división

En esta sección, usará Azure Cloud Shell para crear dos nuevos centros de IoT que representen la **división de tostadoras Contoso** y la **división de bombas de calor Contoso**.

1. Use Azure Cloud Shell para crear un grupo de recursos con el comando [az group create](/cli/azure/group#az-group-create). Un grupo de recursos de Azure es un contenedor lógico en el que se implementan y se administran los recursos de Azure. 

    En el ejemplo siguiente, se crea un grupo de recursos denominado *contoso-us-resource-group* en la región *eastus*. Le recomendamos que use este grupo para todos los recursos que crearemos en el artículo. De esta forma, será más fácil limpiar al finalizar.

    ```azurecli-interactive 
    az group create --name contoso-us-resource-group --location eastus
    ```

2. Use Azure Cloud Shell para crear el centro de IoT para la **división de tostadoras Contoso** con el comando [az iot hub create](/cli/azure/iot/hub#az-iot-hub-create). El centro de IoT se agregará al grupo *contoso-us-resource-group*.

    En el ejemplo siguiente, se crea un centro de IoT denominado *contoso-toasters-hub-1098* en la ubicación *eastus*. Debe utilizar su propio nombre único de centro. Cree su propio sufijo para reemplazar a **1098** en el nombre del centro. El código de ejemplo para la directiva de asignación personalizada requiere que se añada `-toasters-` en el nombre del centro.

    ```azurecli-interactive 
    az iot hub create --name contoso-toasters-hub-1098 --resource-group contoso-us-resource-group --location eastus --sku S1
    ```
    
    Este comando puede tardar varios minutos en completarse.

3. Use Azure Cloud Shell para crear el centro de IoT para la **división de bombas de calor Contoso** con el comando [az iot hub create](/cli/azure/iot/hub#az-iot-hub-create). Este centro de IoT también se agregará a *contoso-us-resource-group*.

    En el ejemplo siguiente, se crea un centro de IoT denominado *contoso-heatpumps-hub-1098* en la ubicación *eastus*. Debe utilizar su propio nombre único de centro. Cree su propio sufijo para reemplazar a **1098** en el nombre del centro. El código de ejemplo para la directiva de asignación personalizada requiere que se añada `-heatpumps-` en el nombre del centro.

    ```azurecli-interactive 
    az iot hub create --name contoso-heatpumps-hub-1098 --resource-group contoso-us-resource-group --location eastus --sku S1
    ```
    
    Este comando puede tardar varios minutos en completarse.




## <a name="create-the-enrollment"></a>Creación de la inscripción

En esta sección, creará un nuevo grupo de inscripciones que use la directiva de asignación personalizada. Para que sea más sencillo, en este artículo se usa la [atestación de clave simétrica](concepts-symmetric-key-attestation.md) con la inscripción. Con el fin de obtener una solución más segura, considere la posibilidad de usar la [atestación de certificado X.509](concepts-security.md#x509-certificates) con una cadena de confianza.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y abra la instancia de Device Provisioning Service.

2. Seleccione la pestaña **Administrar inscripciones** y, luego, haga clic en el botón **Agregar grupo de inscripciones**, situado en la parte superior de la página. 

3. En **Agregar grupo de inscripciones**, escriba la siguiente información y haga clic en el botón **Guardar**.

    **Nombre del grupo**: escriba **contoso-custom-allocated-devices**.

    **Tipo de atestación**: seleccione **Clave simétrica**.

    **Generar claves automáticamente**: ya debe estar activada esta casilla.

    **Seleccione cómo quiere asignar los dispositivos a los centros**: Seleccione **Personalizado (use la función de Azure)**

    ![Adición de un grupo de inscripciones de asignación personalizado para la atestación de clave simétrica](./media/how-to-use-custom-allocation-policies/create-custom-allocation-enrollment.png)


4. En **Agregar grupo de inscripciones**, haga clic en **Link a new IoT hub** (Vincular un nuevo centro de IoT) para vincular sus dos centros de IoT de división. 

    Debe ejecutar este paso para sus dos IoT Hub de división.

    **Suscripción**: si tiene varias suscripciones, elija aquella en la que ha creado los centros de IoT de división.

    **IoT hub**: seleccione uno de los centros de división que ha creado.

    **Directiva de acceso**: elija **iothubowner**.

    ![Vinculación de los centros de IoT de división con el servicio de aprovisionamiento](./media/how-to-use-custom-allocation-policies/link-divisional-hubs.png)


5. En **Agregar grupo de inscripciones**, una vez que haya vinculado ambos centros de IoT de división, debe seleccionarlos para que sean el grupo de IoT Hub para el grupo de inscripciones, tal y como se muestra a continuación:

    ![Creación del grupo de centros de división para la inscripción](./media/how-to-use-custom-allocation-policies/enrollment-divisional-hub-group.png)


6. En **Agregar grupo de inscripciones**, desplácese hacia abajo hasta la sección **Seleccionar función de Azure** y haga clic en **Crear una nueva aplicación de función**.

7. En la página de creación de **Function App** que se abre, escriba las siguientes opciones de configuración para la nueva función y haga clic en **Crear**:

    **Nombre de la aplicación**: escriba un nombre único de aplicación de función. Se muestra **contoso-function-app-1098** como ejemplo.

    **Grupo de recursos**: seleccione **Usar existente** y **contoso-us-resource-group** para mantener juntos todos los recursos creados en este artículo.

    **Application Insights**: para este ejercicio puede desactivar esta opción.

    ![Crear la aplicación de función](./media/how-to-use-custom-allocation-policies/function-app-create.png)


8. En la página **Agregar grupo de inscripciones**, asegúrese de que está seleccionada la nueva aplicación de función. Es posible que deba volver a seleccionar la suscripción para actualizar la lista de aplicaciones de función.

    Una vez seleccionada la nueva aplicación de función, haga clic en **Create a new function** (Crear una nueva función).

    ![Crear la aplicación de función](./media/how-to-use-custom-allocation-policies/click-create-new-function.png)

    se abrirá la nueva aplicación de función.

9. En la aplicación de función, haga clic para crear una nueva función

    ![Crear la aplicación de función](./media/how-to-use-custom-allocation-policies/new-function.png)

    Para la nueva función, use la configuración predeterminada para crear un nuevo **Webhook y API** con el lenguaje **CSharp**. A continuación, haga clic en **Crear esta función**.

    Esto crea una nueva función de C# denominada **HttpTriggerCSharp1**.

10. Reemplace la función de C# por el siguiente código y, a continuación, haga clic en **Guardar**:    

    ```csharp
    #r "Newtonsoft.Json"
    using System.Net;
    using System.Text;
    using Newtonsoft.Json;

    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
    {
        // Just some diagnostic logging
        log.Info("C# HTTP trigger function processed a request.");
        log.Info("Request.Content:...");    
        log.Info(req.Content.ReadAsStringAsync().Result);

        // Get request body
        dynamic data = await req.Content.ReadAsAsync<object>();

        // Get registration ID of the device
        string regId = data?.deviceRuntimeContext?.registrationId;

        string message = "Uncaught error";
        bool fail = false;
        ResponseObj obj = new ResponseObj();

        if (regId == null)
        {
            message = "Registration ID not provided for the device.";
            log.Info("Registration ID : NULL");
            fail = true;
        }
        else
        {
            string[] hubs = data?.linkedHubs.ToObject<string[]>();

            // Must have hubs selected on the enrollment
            if (hubs == null)
            {
                message = "No hub group defined for the enrollment.";
                log.Info("linkedHubs : NULL");
                fail = true;
            }
            else
            {
                // This is a Contoso Toaster Model 007
                if (regId.Contains("-contoso-tstrsd-007"))
                {
                    //Find the "-toasters-" IoT hub configured on the enrollment
                    foreach(string hubString in hubs)
                    {
                        if (hubString.Contains("-toasters-"))
                            obj.iotHubHostName = hubString;
                    }

                    if (obj.iotHubHostName == null)
                    {
                        message = "No toasters hub found for the enrollment.";
                        log.Info(message);
                        fail = true;
                    }
                }
                // This is a Contoso Heat pump Model 008
                else if (regId.Contains("-contoso-hpsd-088"))
                {
                    //Find the "-heatpumps-" IoT hub configured on the enrollment
                    foreach(string hubString in hubs)
                    {
                        if (hubString.Contains("-heatpumps-"))
                            obj.iotHubHostName = hubString;
                    }

                    if (obj.iotHubHostName == null)
                    {
                        message = "No heat pumps hub found for the enrollment.";
                        log.Info(message);
                        fail = true;
                    }
                }
                // Unrecognized device.
                else
                {
                    fail = true;
                    message = "Unrecognized device registration.";
                    log.Info("Unknown device registration");
                }
            }
        }

        return (fail)
            ? req.CreateResponse(HttpStatusCode.BadRequest, message)
            : new HttpResponseMessage(HttpStatusCode.OK)
            {
                Content = new StringContent(JsonConvert.SerializeObject(obj, Formatting.Indented), Encoding.UTF8, "application/json")
            };
    }    

    public class DeviceTwinObj
    {
        public string deviceId {get; set;}
    }

    public class ResponseObj
    {
        public string iotHubHostName {get; set;}
        public string IoTHub {get; set;}
        public DeviceTwinObj initialTwin {get; set;}
        public string[] linkedHubs {get; set;}
        public string enrollment {get; set;}
    }
    ```


11. Vuelva a la página **Agregar grupo de inscripciones** y asegúrese de que está seleccionada la nueva función. Es posible que deba volver a seleccionar la aplicación de función para actualizar la lista de funciones.

    Una vez seleccionada la nueva función, haga clic en **Guardar** para guardar el grupo de inscripciones.

    ![Guarde el grupo de inscripciones](./media/how-to-use-custom-allocation-policies/save-enrollment.png)


12. Después de guardar la inscripción, vuelva a abrirla y apunte la **clave principal**. Primero debe guardar la inscripción para que se generen las claves. Esta clave se usará para generar claves de dispositivo únicas para los dispositivos simulados más adelante.


## <a name="derive-unique-device-keys"></a>Derivación de las claves de dispositivo únicas

En esta sección, creará dos claves de dispositivo únicas. Una de las claves se usará para la tostadora simulada. La otra, se usará para la bomba de calor simulada.

Para generar la clave del dispositivo, usará la **clave principal** que anotó anteriormente para calcular el [HMAC-SHA256](https://wikipedia.org/wiki/HMAC) del identificador de registro de cada dispositivo y convertirá el resultado al formato Base64. Para obtener más información sobre cómo crear claves de dispositivo derivadas con grupos de inscripción, consulte la sección de inscripciones de grupo de [Atestación de clave simétrica](concepts-symmetric-key-attestation.md).

Para el ejemplo de este artículo, use los dos identificadores de registro de dispositivo que se muestran a continuación y calcule una clave de dispositivo para ambos. Ambos identificadores de registro tienen un sufijo válido para funcionar con el código de ejemplo de la directiva de asignación personalizada:

- **breakroom499-contoso-tstrsd-007**
- **mainbuilding167-contoso-hpsd-088**

#### <a name="linux-workstations"></a>Estaciones de trabajo de Linux

Si utiliza una estación de trabajo de Linux, puede usar openssl para generar las claves de dispositivo derivadas tal y como se muestra en el ejemplo siguiente.

1. Reemplace el valor de **KEY** por el de la **clave principal** que ha apuntado anteriormente.

    ```bash
    KEY=oiK77Oy7rBw8YB6IS6ukRChAw+Yq6GC61RMrPLSTiOOtdI+XDu0LmLuNm11p+qv2I+adqGUdZHm46zXAQdZoOA==

    REG_ID1=breakroom499-contoso-tstrsd-007
    REG_ID2=mainbuilding167-contoso-hpsd-088

    keybytes=$(echo $KEY | base64 --decode | xxd -p -u -c 1000)
    devkey1=$(echo -n $REG_ID1 | openssl sha256 -mac HMAC -macopt hexkey:$keybytes -binary | base64)
    devkey2=$(echo -n $REG_ID2 | openssl sha256 -mac HMAC -macopt hexkey:$keybytes -binary | base64)

    echo -e $"\n\n$REG_ID1 : $devkey1\n$REG_ID2 : $devkey2\n\n"
    ```

    ```bash
    breakroom499-contoso-tstrsd-007 : JC8F96eayuQwwz+PkE7IzjH2lIAjCUnAa61tDigBnSs=
    mainbuilding167-contoso-hpsd-088 : 6uejA9PfkQgmYylj8Zerp3kcbeVrGZ172YLa7VSnJzg=
    ```


#### <a name="windows-based-workstations"></a>Estaciones de trabajo basadas en Windows

Si utiliza una estación de trabajo basada en Windows, puede usar PowerShell para generar las claves de dispositivo derivadas tal y como se muestra en el ejemplo siguiente.

1. Reemplace el valor de **KEY** por el de la **clave principal** que ha apuntado anteriormente.

    ```powershell
    $KEY='oiK77Oy7rBw8YB6IS6ukRChAw+Yq6GC61RMrPLSTiOOtdI+XDu0LmLuNm11p+qv2I+adqGUdZHm46zXAQdZoOA=='

    $REG_ID1='breakroom499-contoso-tstrsd-007'
    $REG_ID2='mainbuilding167-contoso-hpsd-088'

    $hmacsha256 = New-Object System.Security.Cryptography.HMACSHA256
    $hmacsha256.key = [Convert]::FromBase64String($key)
    $sig1 = $hmacsha256.ComputeHash([Text.Encoding]::ASCII.GetBytes($REG_ID1))
    $sig2 = $hmacsha256.ComputeHash([Text.Encoding]::ASCII.GetBytes($REG_ID2))
    $derivedkey1 = [Convert]::ToBase64String($sig1)
    $derivedkey2 = [Convert]::ToBase64String($sig2)

    echo "`n`n$REG_ID1 : $derivedkey1`n$REG_ID2 : $derivedkey2`n`n"
    ```

    ```powershell
    breakroom499-contoso-tstrsd-007 : JC8F96eayuQwwz+PkE7IzjH2lIAjCUnAa61tDigBnSs=
    mainbuilding167-contoso-hpsd-088 : 6uejA9PfkQgmYylj8Zerp3kcbeVrGZ172YLa7VSnJzg=
    ```


Los dispositivos simulados usarán las claves de dispositivo derivadas con cada Id. de registro para realizar la atestación de clave simétrica.




## <a name="prepare-an-azure-iot-c-sdk-development-environment"></a>Preparación de un entorno de desarrollo del SDK de Azure IoT para C

En esta sección, preparará un entorno de desarrollo para compilar el [SDK de Azure IoT para C](https://github.com/Azure/azure-iot-sdk-c). El SDK incluye el código de ejemplo para el dispositivo simulado. Este dispositivo simulado tratará de aprovisionarse durante la secuencia de arranque del dispositivo.

En esta sección, nos enfocamos en una estación de trabajo basada en Windows. Para obtener un ejemplo de Linux, consulte la configuración de máquinas virtuales en [How to provision for multitenancy](how-to-provision-multitenant.md) (Cómo aprovisionar para varios inquilinos).

1. Descargue el [sistema de compilación CMake](https://cmake.org/download/).

    **Antes** de comenzar la instalación de `CMake`, es importante que los requisitos previos de Visual Studio (Visual Studio y la carga de trabajo de desarrollo de escritorio con C++) estén instalados en la máquina. Una vez que los requisitos previos están en su lugar, y se ha comprobado la descarga, instale el sistema de compilación de CMake.

2. Abra un símbolo del sistema o el shell de Bash de Git. Ejecute el siguiente comando para clonar el repositorio de GitHub del SDK de Azure IoT para C:
    
    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive
    ```
    Esta operación puede tardar varios minutos en completarse.


3. Cree un subdirectorio `cmake` en el directorio raíz del repositorio de Git y vaya a esa carpeta. 

    ```cmd/sh
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

4. Ejecute el siguiente comando para compilar una versión del SDK específica para su plataforma de cliente de desarrollo. Se generará una solución de Visual Studio para el dispositivo simulado en el directorio `cmake`. 

    ```cmd
    cmake -Dhsm_type_symm_key:BOOL=ON -Duse_prov_client:BOOL=ON  ..
    ```
    
    Si `cmake` no encuentra el compilador de C++, es posible que obtenga errores de compilación durante la ejecución del comando anterior. Si sucede, intente ejecutar este comando en el [símbolo del sistema de Visual Studio](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs). 

    Una vez realizada la compilación, las últimas líneas de salida tendrán un aspecto similar al siguiente:

    ```cmd/sh
    $ cmake -Dhsm_type_symm_key:BOOL=ON -Duse_prov_client:BOOL=ON  ..
    -- Building for: Visual Studio 15 2017
    -- Selecting Windows SDK version 10.0.16299.0 to target Windows 10.0.17134.
    -- The C compiler identification is MSVC 19.12.25835.0
    -- The CXX compiler identification is MSVC 19.12.25835.0

    ...

    -- Configuring done
    -- Generating done
    -- Build files have been written to: E:/IoT Testing/azure-iot-sdk-c/cmake
    ```




## <a name="simulate-the-devices"></a>Simulación de los dispositivos

En esta sección, actualizará el ejemplo de aprovisionamiento denominado **prov\_dev\_client\_sample** que está ubicado en el SDK de Azure IoT para C que ha configurado anteriormente. 

El código de ejemplo simula una secuencia de arranque de dispositivo que envía la solicitud de aprovisionamiento a la instancia de Device Provisioning Service. La secuencia de arranque hará que se reconozca y se asigne la tostadora al centro de IoT según la directiva de asignación personalizada.

1. En Azure Portal, seleccione la pestaña **Información general** para su servicio Device Provisioning y anote el valor de **_Ámbito de id_** .

    ![Extracción de información del punto de conexión del servicio Device Provisioning desde la hoja del portal](./media/quick-create-simulated-device-x509/extract-dps-endpoints.png) 

2. En Visual Studio, abra el archivo de solución **azure_iot_sdks.sln** que se ha generado anteriormente al ejecutar CMake. El archivo de solución debe estar en la siguiente ubicación:

    ```
    \azure-iot-sdk-c\cmake\azure_iot_sdks.sln
    ```

3. En la ventana del *Explorador de soluciones* de Visual Studio, desplácese hasta la carpeta **Aprovisionar\_Ejemplos**. Expanda el proyecto de ejemplo denominado **prov\_dev\_client\_sample**. Expanda **Archivos de código fuente** y abra **prov\_dev\_cliente\_sample.c**.

4. Busque la constante `id_scope` y reemplace el valor por el valor de **Ámbito de id.** que copió anteriormente. 

    ```c
    static const char* id_scope = "0ne00002193";
    ```

5. Busque la definición de la función `main()` en el mismo archivo. Asegúrese de que la variable `hsm_type` está establecida en `SECURE_DEVICE_TYPE_SYMMETRIC_KEY`, tal como se muestra a continuación:

    ```c
    SECURE_DEVICE_TYPE hsm_type;
    //hsm_type = SECURE_DEVICE_TYPE_TPM;
    //hsm_type = SECURE_DEVICE_TYPE_X509;
    hsm_type = SECURE_DEVICE_TYPE_SYMMETRIC_KEY;
    ```

6. Haga clic con el botón derecho en el proyecto **prov\_dev\_client\_sample** y seleccione **Set as Startup Project** (Establecer como proyecto de inicio). 


#### <a name="simulate-the-contoso-toaster-device"></a>Simulación de la tostadora de Contoso

1. Para simular el dispositivo tostadora, encuentre la llamada a `prov_dev_set_symmetric_key_info()` en **prov\_dev\_client\_sample.c** que se marcó como comentario.

    ```c
    // Set the symmetric key if using they auth type
    //prov_dev_set_symmetric_key_info("<symm_registration_id>", "<symmetric_Key>");
    ```

    Quite la marca del comentario en las llamadas de función y reemplace los valores de marcador de posición (incluidos los corchetes angulares) por el identificador de registro de la tostadora y la clave de dispositivo derivada que ha generado antes. Solo se proporciona el valor de clave **JC8F96eayuQwwz + PkE7IzjH2lIAjCUnAa61tDigBnSs =** como ejemplo.

    ```c
    // Set the symmetric key if using they auth type
    prov_dev_set_symmetric_key_info("breakroom499-contoso-tstrsd-007", "JC8F96eayuQwwz+PkE7IzjH2lIAjCUnAa61tDigBnSs=");
    ```
   
    Guarde el archivo.

2. En el menú de Visual Studio, seleccione **Depurar** > **Iniciar sin depurar** para ejecutar la solución. En la solicitud para volver a compilar el proyecto, haga clic en **Sí** para recompilar el proyecto antes de ejecutarlo.

    La salida siguiente ejemplifica el arranque correcto de la tostadora simulada y su conexión a la instancia del servicio de aprovisionamiento para asignarse al centro de IoT de tostadoras según la directiva de asignación personalizada:

    ```cmd
    Provisioning API Version: 1.2.9

    Registering Device

    Provisioning Status: PROV_DEVICE_REG_STATUS_CONNECTED
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING

    Registration Information received from service: contoso-toasters-hub-1098.azure-devices.net, deviceId: breakroom499-contoso-tstrsd-007

    Press enter key to exit:
    ```


#### <a name="simulate-the-contoso-heat-pump-device"></a>Simulación de la bomba de calor de Contoso

1. Para simular el dispositivo de la bomba de calor, actualice la llamada a `prov_dev_set_symmetric_key_info()` en **prov\_dev\_client\_sample.c** de nuevo con el identificador de registro de la bomba de calor y la clave de dispositivo derivada que ha generado antes. Solo se proporciona el valor de la clave **6uejA9PfkQgmYylj8Zerp3kcbeVrGZ172YLa7VSnJzg =** como ejemplo.

    ```c
    // Set the symmetric key if using they auth type
    prov_dev_set_symmetric_key_info("mainbuilding167-contoso-hpsd-088", "6uejA9PfkQgmYylj8Zerp3kcbeVrGZ172YLa7VSnJzg=");
    ```
   
    Guarde el archivo.

2. En el menú de Visual Studio, seleccione **Depurar** > **Iniciar sin depurar** para ejecutar la solución. En la solicitud para volver a compilar el proyecto, haga clic en **Sí** para recompilar el proyecto antes de ejecutarlo.

    La salida siguiente ejemplifica el arranque correcto de la bomba de calor simulada y su conexión a la instancia del servicio de aprovisionamiento para asignarse al centro de IoT de bombas de calor según la directiva de asignación personalizada:

    ```cmd
    Provisioning API Version: 1.2.9

    Registering Device

    Provisioning Status: PROV_DEVICE_REG_STATUS_CONNECTED
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING

    Registration Information received from service: contoso-heatpumps-hub-1098.azure-devices.net, deviceId: mainbuilding167-contoso-hpsd-088

    Press enter key to exit:
    ```


## <a name="troubleshooting-custom-allocation-policies"></a>Solución de problemas de las directivas de asignación personalizadas

La siguiente tabla muestra los escenarios esperados y los códigos de error de resultados que pueden surgir. Use esta tabla para ayudar a solucionar errores en las directivas de asignación personalizadas con Azure Functions.


| Escenario | Resultado del registro del servicio de aprovisionamiento | Resultados del SDK de aprovisionamiento |
| -------- | --------------------------------------------- | ------------------------ |
| El webhook devuelve 200 OK con el valor de 'iotHubHostName' establecido en un nombre de host de centro de IoT válido | Estado del resultado: Asignado  | SDK devuelve PROV_DEVICE_RESULT_OK junto con información del centro |
| El webhook devuelve 200 OK con el valor de 'iotHubHostName' en la respuesta, pero se establece en una cadena vacía o null | Estado del resultado: Con error<br><br> Código de error: CustomAllocationIotHubNotSpecified (400208) | SDK devuelve PROV_DEVICE_RESULT_HUB_NOT_SPECIFIED |
| El webhook devuelve 401 no autorizado | Estado del resultado: Con error<br><br>Código de error: CustomAllocationUnauthorizedAccess (400209) | SDK devuelve PROV_DEVICE_RESULT_UNAUTHORIZED |
| Se ha creado una inscripción individual para deshabilitar el dispositivo | Estado del resultado: Disabled | SDK devuelve PROV_DEVICE_RESULT_DISABLED |
| El webhook devuelve el código de error > = 429 | La orquestación de DPS volverá a intentarlo un par de veces. La directiva de reintentos actual es:<br><br>&nbsp;&nbsp;- Número de reintentos: 10<br>&nbsp;&nbsp;- Intervalo inicial: 1 s<br>&nbsp;&nbsp;- Incremento: 9s | El SDK omitirá el error y enviará otro mensaje para obtener el estado en el tiempo especificado |
| El webhook devuelve cualquier otro código de estado | Estado del resultado: Con error<br><br>Código de error: CustomAllocationFailed (400207) | SDK devuelve PROV_DEVICE_RESULT_DEV_AUTH_ERROR |


## <a name="clean-up-resources"></a>Limpieza de recursos

Si tiene previsto seguir trabajando con los recursos creados en este artículo, puede dejarlos. Si no tiene la intención de seguir usando el recurso, siga estos pasos para eliminar todos los recursos creados en este artículo para evitar cargos innecesarios.

En estos pasos se supone que ha creado todos los recursos de este artículo como se ha indicado en el mismo grupo de recursos denominado **contoso-us-resource-group**.

> [!IMPORTANT]
> La eliminación de un grupo de recursos es irreversible. El grupo de recursos y todos los recursos contenidos en él se eliminan permanentemente. Asegúrese de no eliminar por accidente el grupo de recursos o los recursos equivocados. Si ha creado una instancia de IoT Hub en un grupo de recursos ya existente que contiene recursos que desea conservar, puede eliminar solo esa instancia en lugar de eliminar todo el grupo de recursos.
>

Para eliminar el grupo de recursos por nombre:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y haga clic en **Grupos de recursos**.

2. En el cuadro de texto **Filtrar por nombre...** , escriba el nombre del grupo de recursos que contiene los recursos: **contoso-us-resource-group**. 

3. A la derecha del grupo de recursos de la lista de resultados, haga clic en **...** y, a continuación, en **Eliminar grupo de recursos**.

4. Se le pedirá que confirme la eliminación del grupo de recursos. Escriba otra vez el nombre del grupo de recursos para confirmar y haga clic en **Eliminar**. Transcurridos unos instantes, el grupo de recursos y todos los recursos que contiene se eliminan.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información sobre el reaprovisionamiento, consulte [Conceptos sobre el reaprovisionamiento de dispositivos de IoT Hub](concepts-device-reprovision.md). 
- Para obtener más información sobre el desaprovisionamiento, consulte [Desaprovisionamiento de dispositivos aprovisionados automáticamente](how-to-unprovision-devices.md). 











