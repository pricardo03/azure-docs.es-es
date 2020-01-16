---
title: Instalación de Azure FarmBeats
description: En este artículo se describe cómo instalar Azure FarmBeats en la suscripción de Azure.
author: usha-rathnavel
ms.topic: article
ms.date: 12/11/2019
ms.author: usrathna
ms.openlocfilehash: d1a6bdfb38431e18eb305b223ce8ee2467804052
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75475746"
---
# <a name="install-azure-farmbeats"></a>Instalación de Azure FarmBeats

En este artículo se describe cómo instalar Azure FarmBeats en la suscripción de Azure.

Azure FarmBeats es una oferta de negocio a negocio disponible en Azure Marketplace. Permite la agregación de conjuntos de datos de agricultura entre proveedores y la generación de información procesable. Azure FarmBeats permite crear modelos de inteligencia artificial (AI) o de aprendizaje automático (ML) basados en conjuntos de datos combinados. Los dos componentes principales de Azure FarmBeats son:

- **Centro de datos**: Una capa de API que permite la agregación, normalización y contextualización de varios conjuntos de datos agrícolas entre distintos proveedores.

- **Acelerador**: Una aplicación web de ejemplo que se basa en el centro de datos. Sirve de inicio en el desarrollo y visualización del modelo. El acelerador usa las API de Azure FarmBeats para mostrar la visualización de los datos ingeridos de los sensores como gráficos y la visualización de la salida del modelo como mapas.

## <a name="before-you-start"></a>Antes de comenzar
### <a name="components-installed"></a>Componentes instalados

Al instalar Azure FarmBeats, se aprovisionan los siguientes recursos en la suscripción de Azure:

| Recursos de Azure instalados  | Componente de Azure FarmBeats  |
|---------|---------|
| Application Insights   |      Centro de datos y acelerador      |
| App Service     |     Centro de datos y acelerador     |
| Plan de servicio de aplicación   | Centro de datos y acelerador  |
| API Connection    |  Centro de datos       |
| Azure Cache for Redis       | Centro de datos      |
| Azure Cosmos DB   |  Centro de datos       |
| Azure Data Factory V2       |     Centro de datos y acelerador      |
| Cuenta de Azure Batch    | Centro de datos   |
| Azure Key Vault |  Centro de datos y acelerador        |
| Cuenta de Azure Maps       |     Acelerador    |
| Espacio de nombres del centro de eventos    |     Centro de datos      |
| Aplicación lógica      |  Centro de datos       |
| Cuenta de almacenamiento      |     Centro de datos y acelerador      |
| Time Series Insights     |    Centro de datos    |

### <a name="costs-incurred"></a>Costos en los que se incurre

El costo de Azure FarmBeats es una agregación del costo de los servicios de Azure subyacentes. La información de precios de los servicios de Azure se puede calcular con la [Calculadora de precios](https://azure.microsoft.com/pricing/calculator). El costo real de la instalación total variará en función del uso. El costo del estado estable para los dos componentes es:

* Centro de datos: menos de 10 $ al día
* Acelerador: menos de 2 $ al día

### <a name="regions-supported"></a>Regiones admitidas

Actualmente, Azure FarmBeats se admite en entornos de nube pública en las siguientes regiones:
* Este de Australia
* Centro de EE. UU.
* East US
* Este de EE. UU. 2
* Oeste de EE. UU.
* Oeste de EE. UU. 2
* Europa del Norte
* Europa occidental
* Sudeste asiático

### <a name="time-taken"></a>Tiempo empleado

La configuración completa de Azure FarmBeats, incluidas la preparación y la instalación, tardará menos de una hora.

## <a name="prerequisites"></a>Prerequisites    

Antes de iniciar la instalación real de Azure FarmBeats, debe completar los pasos siguientes:

### <a name="create-sentinel-account"></a>Creación de una cuenta de Sentinel
La instalación de Azure FarmBeats le permite obtener imágenes gratuitas de la misión del satélite [Sentinel-2](https://scihub.copernicus.eu/) de la Agencia espacial europea de la granja. Para configurar esta instalación, necesita una cuenta de Sentinel.

Siga estos pasos para crear una cuenta gratuita de Sentinel:

1. Vaya a la página oficial de [registro](https://scihub.copernicus.eu/dhus/#/self-registration).
2. Proporcione los detalles necesarios (nombre, apellido, nombre de usuario, contraseña e identificador de correo electrónico) y rellene el formulario.
3. Se enviará un vínculo de comprobación al identificador de correo electrónico registrado. Seleccione el vínculo proporcionado en el correo electrónico y complete la comprobación.

Una vez finalizada la comprobación, se completa el proceso de registro. Anote el **Nombre de usuario de Sentinel** y la **Contraseña de Sentinel**.

### <a name="decide-subscription-and-region"></a>Elección de la suscripción y la región

Necesitará el identificador de suscripción de Azure y la región en la que desea instalar Azure FarmBeats. Elija una de las regiones que aparecen en la sección [Regiones admitidas](#regions-supported).

Anote el **Identificador de suscripción de Azure** y la **Región de Azure**.

### <a name="verify-permissions"></a>Comprobación de los permisos

Deberá comprobar si tiene privilegios y permisos suficientes en el inquilino de Azure en el que desea instalar Azure FarmBeats.

Puede comprobar los permisos de acceso en Azure Portal según las instrucciones descritas en [Control de acceso basado en rol](https://docs.microsoft.com/azure/role-based-access-control/check-access).

Necesita los permisos siguientes para instalar Azure FarmBeats:
- Inquilino: acceso de lectura
- Suscripción: colaborador o propietario
- Grupo de recursos: propietario.

Además, Azure FarmBeats requiere registros de aplicación de Azure Active Directory. Hay dos maneras de completar la configuración de Azure AD necesaria:

**Caso 1**: tiene permisos de **escritura** en el inquilino de Azure en el que va a realizar la instalación. Este caso significa que tiene los permisos necesarios para crear el registro de aplicación de AAD de forma dinámica durante la instalación.

Puede continuar directamente con la sección [Completar el registro de Marketplace](#complete-azure-marketplace-sign-up).


**Caso 2**: NO tiene permisos de **escritura** en el inquilino de Azure. Este caso es común cuando se intenta instalar Azure FarmBeats en la suscripción de Azure de la empresa y el acceso de **escritura** está restringido solo al grupo de recursos de su propiedad.
En este caso, solicite al administrador de TI que siga los pasos que se indican a continuación para generar y completar automáticamente el registro de aplicación de Azure AD en Azure Portal.

1. Descargue y extraiga el [script del generador de aplicaciones de AAD](https://aka.ms/FarmBeatsAADScript) en el equipo local.
2. Inicie sesión en Azure Portal y seleccione la suscripción y el inquilino de Azure AD.
3. Inicie Cloud Shell en la barra de herramientas de la parte superior de Azure Portal.

    ![Proyecto FarmBeats](./media/install-azure-farmbeats/navigation-bar-1.png)

4. Elija PowerShell como la experiencia de shell preferida. A los usuarios por primera vez se les pedirá que seleccionen una suscripción y que creen una cuenta de almacenamiento. Complete la instalación tal como se indica.
5. Cargue el script (del paso 1) en Cloud Shell y anote la ubicación del archivo cargado.

    > [!NOTE]
    > De forma predeterminada, el archivo se carga en el directorio principal.

6. Vaya al directorio principal con el comando "cd" y ejecute el siguiente script:

      ```azurepowershell-interactive
            ./create_aad_script.ps1
      ```
7. Escriba el nombre del **sitio web del centro de datos** y el nombre del **sitio web del acelerador**. Tome nota de la salida del script y compártala con la persona que va a instalar Azure FarmBeats.

Una vez que el administrador de TI le proporcione los detalles necesarios, anote el **identificador de cliente de AAD, el secreto de cliente de AAD, el nombre del sitio web del centro de datos y el nombre del sitio web del acelerador**.

   > [!NOTE]
   > Si sigue las instrucciones del caso 2, no olvide agregar el identificador de cliente de AAD y el secreto de cliente de AAD como parámetros independientes en el [archivo de parámetros](#prepare-parameters-file).

Ahora, tiene toda la información necesaria para continuar con la siguiente sección.

### <a name="complete-azure-marketplace-sign-up"></a>Registro en Azure Marketplace

Tiene que completar la suscripción a la oferta de Azure FarmBeats en Azure Marketplace antes de poder iniciar el proceso de instalación basado en Cloud Shell. Siga los pasos que se indican a continuación para completar el registro:

1.  Inicie sesión en Azure Portal. Seleccione la cuenta en la esquina superior derecha y cambie al inquilino de Azure AD en el que quiere instalar Azure FarmBeats.

2.  Vaya a Azure Marketplace en el portal y busque **Azure FarmBeats** en Marketplace.

3.  Aparece una nueva ventana con información general sobre Azure FarmBeats. Seleccione el botón **Crear**.

4.  Aparece una nueva ventana. Complete el proceso de registro; para ello, elija la suscripción, el grupo de recursos y la ubicación correctos en los que desea instalar Azure FarmBeats.

5.  Una vez validados los detalles especificados, seleccione **Aceptar**. Aparece la página Términos de uso. Revise los términos y seleccione **Crear** para completar el proceso de registro.

Este paso completa el proceso de registro en Azure Marketplace. Ahora está listo para comenzar la preparación del archivo de parámetros.

### <a name="prepare-parameters-file"></a>Preparación del archivo de parámetros
El último paso de la fase de requisitos previos consiste en crear un archivo JSON que servirá como entrada durante la instalación de Cloud Shell. Se deben reemplazar los parámetros del archivo JSON por los valores adecuados.

A continuación se proporciona un archivo JSON de ejemplo. Descargue el ejemplo y actualice los detalles necesarios.

```json
{  
    "sku":"both",
    "subscriptionId":"da9xxxec-dxxf-4xxc-xxx21-xxx3ee7xxxxx",
    "datahubResourceGroup":"dummy-test-dh1",
    "location":"westus2",
    "datahubWebsiteName":"dummy-test-dh1",
    "acceleratorResourceGroup":"dummy-test-acc1",
    "acceleratorWebsiteName":"dummy-test-acc1",
    "sentinelUsername":"dummy-dev",
    "notificationEmailAddress":"dummy@yourorg.com",
    "updateIfExists":true
}
```

Puede consultar la siguiente tabla de parámetros para más información sobre cada uno de los parámetros.

| Parámetro | Value|
|--- | ---|
|sku  | Proporciona al usuario la opción de instalar el centro de datos y el acelerador o solo el centro de datos. Para instalar solo el centro de datos, use "datahub". Para instalar el centro de datos y el acelerador, use "both".|
|subscriptionId | Especifica la suscripción para la instalación de Azure FarmBeats.|
|datahubResourceGroup| Especifica el nombre del grupo de recursos para los recursos del centro de datos. Escriba aquí el nombre del grupo de recursos que creó en Azure Marketplace.|
|ubicación |Ubicación o región de Azure en la que desea instalar Azure FarmBeats.|
|datahubWebsiteName  | Prefijo de dirección URL único para la aplicación web del centro de datos. |
|acceleratorResourceGroup  | Especifica el nombre del grupo de recursos para los recursos del acelerador.|
|acceleratorWebsiteName |Prefijo de dirección URL único para la aplicación web del acelerador.|
|sentinelUsername | Nombre de usuario para obtener las imágenes del satélite Sentinel.|
|notificationEmailAddress  | Dirección de correo electrónico para recibir las notificaciones de las alertas configuradas en el centro de datos.|
|updateIfExists| (Opcional) Parámetro que se incluirá en el archivo de parámetros solo si desea actualizar una instancia existente de Azure FarmBeats. Para una actualización, otros detalles, como los nombres y las ubicaciones de los grupos de recursos, deben ser iguales.|
|aadAppClientId | (Opcional) Parámetro que se incluirá en el archivo de parámetros solo si se usa una aplicación de AAD creada previamente. Consulte el caso 2 en la sección [Comprobación de los permisos](#verify-permissions) para más detalles. |
|aadAppClientSecret  | (Opcional) Parámetro que se incluirá en el archivo de parámetros solo si se usa una aplicación de AAD creada previamente. Consulte el caso 2 en la sección [Comprobación de los permisos](#verify-permissions) para más detalles.|

En función de la tabla anterior y el archivo JSON de ejemplo, cree el archivo JSON de parámetros y guárdelo en el equipo local.

## <a name="install"></a>Instalar

La instalación real de los recursos de Azure FarmBeats se produce en la interfaz de la línea de comandos basada en explorador de Cloud Shell con el entorno Bash. Siga estas instrucciones para instalar Azure FarmBeats:

1. Inicie sesión en Azure Portal. Seleccione la suscripción de Azure y el inquilino en el que quiere instalar Azure FarmBeats.
2. Inicie **Cloud Shell** en la barra de herramientas de la esquina superior derecha de Azure Portal.
3. Elija Bash como experiencia de shell preferida. Seleccione el botón **Cargar** (resaltado en la siguiente imagen) y cargue el archivo JSON de parámetros preparado.

      ![Proyecto FarmBeats](./media/install-azure-farmbeats/bash-2-1.png)

4. **Copie** el comando siguiente y **reemplace \<username>** por el valor correcto para que el comando señale a la ruta de acceso correcta del archivo cargado.

    ```bash
          wget -O farmbeats-installer.sh https://aka.ms/AzureFarmbeatsInstallerScript && bash farmbeats-installer.sh /home/<username>/input.json
    ```
5. Ejecute el comando modificado para iniciar el proceso de instalación. Se le pedirá que:
 - Acepte los términos de la **licencia de Azure FarmBeats**. Escriba "Y" para continuar con el paso siguiente si está de acuerdo con los Términos de uso. Escriba "N" para finalizar la instalación, si no está de acuerdo con los Términos de uso.

 - A continuación, se le pedirá que especifique un token de acceso para la instalación. Copie el código generado e inicie sesión en la [página de inicio de sesión del dispositivo](https://microsoft.com/devicelogin) con sus **credenciales de Azure**.

 - Una vez que el inicio de sesión se haya completado correctamente, el instalador le solicitará la contraseña de la cuenta de Sentinel. Escriba la **contraseña de la cuenta de Sentinel**.

6. Se valida el archivo de parámetros y se inicia la instalación de los recursos de Azure. La instalación tarda aproximadamente **25 minutos** en completarse.    
> [!NOTE]
> Las sesiones de Cloud Shell inactivas expiran después de **20 minutos**. Mantenga activa la sesión de Cloud Shell mientras el instalador implementa los recursos de Azure. Si se agota el tiempo de espera de la sesión, tendrá que reiniciar el proceso de instalación.

Una vez completada la instalación, recibirá los siguientes vínculos de salida:
* **URL del centro de datos**: vínculo de Swagger para acceder a las API del centro de datos.
* **URL del acelerador**: aplicación web para explorar el acelerador de Azure FarmBeats.
* **Archivo de registro del instalador**: archivo de registro que contiene los detalles de la instalación. Este archivo de registro se puede usar para solucionar problemas de la instalación, si es necesario.

Puede comprobar la finalización de la instalación de Azure FarmBeats con las siguientes comprobaciones:

**Centro de datos**
1. Inicie sesión en la dirección URL del centro de datos proporcionada (con el formato **https://\<nombre-del-sitio-web-del-centro-de-datos>.azurewebsites.net/swagger**) con sus credenciales de Azure.
2. Debería poder ver los distintos objetos de la API de FarmBeats y realizar operaciones REST en las API.

**Acelerador**
1. Inicie sesión en la dirección URL del acelerador proporcionada (con el formato **https://\<nombre-del-sitio-web-del-acelerador>.azurewebsites.net/swagger**) con sus credenciales de Azure.
2. Debería poder ver la interfaz de usuario del acelerador con una opción para crear granjas en el explorador.

La capacidad de realizar las operaciones anteriores indica una instalación correcta de Azure FarmBeats.

## <a name="upgrade"></a>Actualizar
En la versión preliminar pública, para actualizar una instalación existente de Azure FarmBeats, tendrá que volver a ejecutar el comando de instalación en Cloud Shell, con el parámetro "**updateIfExists**" adicional en el archivo de parámetros establecido en "**true**". Consulte la última línea del ejemplo JSON siguiente para el parámetro de actualización.

```json
{
    "sku":"both",
    "subscriptionId":"da9xxxec-dxxf-4xxc-xxx21-xxx3ee7xxxxx",
    "datahubResourceGroup":"dummy-test-dh1",
    "location":"westus2",
    "datahubWebsiteName":"dummy-test-dh1",
    "acceleratorResourceGroup":" dummy-test-acc1",
    "acceleratorWebsiteName":" dummy-test-acc1",
    "sentinelUsername":"dummy-dev",
    "notificationEmailAddress":"dummy@yourorg.com",
    "updateIfExists":true
}
```
El comando actualiza la instalación existente de Azure FarmBeats a la versión más reciente y proporciona los vínculos de salida.

## <a name="uninstall"></a>Desinstalación

Para desinstalar el centro de datos o el acelerador de Azure FarmBeats, complete los pasos siguientes:

1.  Inicie sesión en Azure Portal y **elimine los grupos de recursos** en los que se instalaron estos componentes.

2.  Vaya a Azure Active Directory y **elimine la aplicación de Azure AD** vinculada a la instalación de Azure FarmBeats. Se eliminará la instalación de Azure FarmBeats de la suscripción de Azure.

## <a name="next-steps"></a>Pasos siguientes
Ha aprendido cómo instalar Azure FarmBeats en la suscripción de Azure. Ahora, obtenga información sobre cómo [agregar usuarios](manage-users-in-azure-farmbeats.md#manage-users) a la instancia de Azure FarmBeats.
