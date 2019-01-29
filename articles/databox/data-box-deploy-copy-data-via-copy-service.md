---
title: Copia de datos a un dispositivo Microsoft Azure Data Box mediante SMB | Microsoft Docs
description: Aprenda a copiar datos a un dispositivo Azure Data Box mediante el servicio de copia de datos
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 01/10/2019
ms.author: alkohli
ms.openlocfilehash: a71635abd036bb89546dd3421af97cd9b88f4327
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54440052"
---
# <a name="tutorial-use-data-copy-service-to-directly-ingest-data-into-azure-data-box-preview"></a>Tutorial: Uso del servicio de copia de datos para ingerir directamente datos en Azure Data Box (versión preliminar)

Este tutorial describe cómo realizar la ingesta de datos con el servicio de copia de datos sin necesidad de un host intermedio. El servicio de copia de datos se ejecuta localmente en Data Box, se conecta al dispositivo de almacenamiento conectado a la red a través de SMB y copia los datos en Data Box.

Use el servicio de copia de datos:

- En los entornos de almacenamiento conectado a la red (NAS) donde los hosts intermedios pueden no estar disponibles.
- Con archivos pequeños que demoran durante semanas la ingesta y carga de datos. Este servicio mejora significativamente el tiempo de ingesta y de carga.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Copia de datos a un dispositivo Data Box
> * Preparación del envío del dispositivo Data Box

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, asegúrese de que:

1. Ha completado el [Tutorial: Instalación de un dispositivo Azure Data Box](data-box-deploy-set-up.md).
2. Ha recibido su dispositivo Data Box y el estado del pedido en el portal se actualiza a **Delivered** (Entregado).
3. Tiene las credenciales del dispositivo NAS origen que se conecta para la copia de datos.
4. Está conectado a una red de alta velocidad. Es muy recomendable tener una conexión de 10 GbE como mínimo. Si no hay disponible una conexión 10 GbE, use un vínculo de datos de 1 GbE, pero las velocidades de copia se verán afectadas.

## <a name="copy-data-to-data-box"></a>Copia de datos a un dispositivo Data Box

Una vez que esté conectado al almacenamiento conectado a la red, el siguiente paso es copiar los datos. Antes de comenzar la copia de datos, revise las consideraciones siguientes:

- Al copiar los datos, asegúrese de que el tamaño de los datos se ajusta a los límites descritos en los [límites de almacenamiento de Azure y de Data Box](data-box-limits.md).
- Si los datos que va a cargar Data Box los están cargando a la vez otras aplicaciones fuera de Data Box, podría provocar errores en el trabajo de carga y daños en los datos.
- Si los datos se están renovando mientras el servicio de datos los está leyendo, podrían aparecer errores o daños en los datos.

Para copiar datos utilizando el servicio de copia de datos, tendrá que crear un trabajo. Siga estos pasos para crear un trabajo para copiar datos.

1. En la interfaz de usuario de la web local de su instancia de Data Box, vaya a **Administrar > Copiar datos**.
2. En la página **Copiar datos**, haga clic en **Crear**.

    ![Haga clic en **Crear** en la página **Copiar datos**](media/data-box-deploy-copy-data-via-copy-service/click-create.png)

3. En el cuadro de diálogo **Configure and start** (Configurar e iniciar), proporcione los siguientes datos de entrada.
    
    |Campo                          |Valor    |
    |-------------------------------|---------|
    |Nombre del trabajo                       |Un nombre exclusivo de menos de 230 caracteres para el trabajo. En el nombre del trabajo no se permiten los siguientes caracteres: \<, \>, \|, \?, \*, \\, \:, \/, y \\\.         |
    |Ubicación de origen                |Proporcione la ruta de acceso SMB para el origen de datos en el formato: `\\<ServerIPAddress>\<ShareName>` o `\\<ServerName>\<ShareName>`.        |
    |Nombre de usuario                       |Nombre de usuario para acceder al origen de datos.        |
    |Contraseña                       |Contraseña para acceder al origen de datos.           |
    |Cuenta de almacenamiento de destino    |Seleccione la cuenta de almacenamiento de destino para cargar datos desde la lista desplegable.         |
    |Tipo de almacenamiento de destino       |Seleccione el tipo de almacenamiento de destino del blob en bloques, blob en páginas o Azure Files.        |
    |Contenedor o recurso compartido de destino    |Escriba el nombre del contenedor o el recurso compartido para cargar datos en la cuenta de almacenamiento de destino. El nombre puede ser un nombre de recurso compartido o de contenedor. Por ejemplo, `myshare` o `mycontainer`. Además, puede escribir en el formato `sharename\directory_name` o `containername\virtual_directory_name` en la nube.        |
    |Copiar los archivos que coincidan con el patrón    | Escriba el patrón de coincidencia de nombre de archivo en las dos maneras siguientes.<ul><li>**Use expresiones de caracteres comodín**: solo `*` y `?` se admiten en expresiones de caracteres comodín. Por ejemplo, esta expresión `*.vhd` coincide con todos los archivos que tienen la extensión .vhd. De forma similar, `*.dl?` coincide con todos los archivos cuya extensión sea `.dl` o `.dll`. Además, `*foo` coincidirá con todos los archivos cuyos nombres terminan con `foo`.<br>Puede especificar directamente la expresión de caracteres comodín en el campo. De forma predeterminada, el valor especificado en el campo se trata como una expresión comodín.</li><li>**Use expresiones regulares**: se admiten expresiones regulares basadas en POSIX. Por ejemplo, una expresión regular `.*\.vhd` coincidirá con todos los archivos que tienen una extensión `.vhd`. Para la expresión regular, proporcione el `<pattern>` directamente como `regex(<pattern>)`. <li>Para más información sobre las expresiones regulares, acuda a [Lenguaje de expresiones regulares - Referencia rápida](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference).</li><ul>|
    |Optimización de archivos              |Cuando se habilita, los archivos se empaquetan en la ingesta. Esto acelera la copia de datos para archivos pequeños.        |
 
4. Haga clic en **Iniciar**. Se validan las entradas y si la validación es correcta, a continuación, se inicia un trabajo. El trabajo puede tardar unos minutos en iniciarse.

    ![Iniciar un trabajo desde el cuadro de diálogo para configurar e iniciar un trabajo](media/data-box-deploy-copy-data-via-copy-service/configure-and-start.png)

5. Se crea un trabajo con la configuración especificada. Seleccione la casilla de verificación y, a continuación, puede pausar y reanudar, cancelar o reiniciar un trabajo.

    ![Administrar un trabajo a través de la página de copia de datos](media/data-box-deploy-copy-data-via-copy-service/select-job.png)
    
    - Puede realizar una pausa en este trabajo si afecta a los recursos de almacenamiento conectado a la red durante las horas punta.

        ![Pausar un trabajo](media/data-box-deploy-copy-data-via-copy-service/pause-job.png)

        Puede reanudar el trabajo más adelante durante las horas con menos actividad.

        ![Reanudar un trabajo](media/data-box-deploy-copy-data-via-copy-service/resume-job.png)

    - Puede cancelar un trabajo en cualquier momento.

        ![Cancelar un trabajo](media/data-box-deploy-copy-data-via-copy-service/cancel-job.png) Se necesita una confirmación cuando se cancela un trabajo.

        ![Confirmar la cancelación de un trabajo](media/data-box-deploy-copy-data-via-copy-service/confirm-cancel-job.png)

        Si decide cancelar un trabajo, los datos que ya se han copiado no se eliminan. Para eliminar cualquier dato que haya copiado en la instancia de Data Box, restablezca el dispositivo.

        ![Restablecer el dispositivo](media/data-box-deploy-copy-data-via-copy-service/reset-device.png)

        >[!NOTE]
        > Si se cancela o pausa un trabajo, los archivos de gran tamaño que se estén copiando pueden quedarse a medio copiar. Estos archivos se cargan en el mismo estado en Azure. Cuando vaya a cancelar o a realizar una pausa, asegúrese de que los archivos se copian correctamente. Para validar los archivos, examine los recursos compartidos de SMB o descargue el archivo BOM.

    - Puede reiniciar un trabajo si se ha producido un fallo repentino debido a un error transitorio, como un problema de red. Un trabajo no se puede reiniciar si ha alcanzado un estado terminal, como finalizado correctamente o finalizado con errores. Los errores podrían deberse a problemas de nomenclatura o tamaño de los archivos. Estos errores se registran, pero no se puede reiniciar el trabajo una vez que se ha completado.

        ![Reiniciar un trabajo con error](media/data-box-deploy-copy-data-via-copy-service/restart-failed-job.png)

        Si experimenta un error y no se puede reiniciar el trabajo, descargue los registros de error y examine el error en los archivos de registro. Una vez que haya corregido el problema, puede crear un nuevo trabajo para copiar los archivos. También puede [copiar los archivos mediante SMB](data-box-deploy-copy-data.md).
    
    - En esta versión no puede eliminar un trabajo.
    
    - Puede crear un número ilimitado de trabajos, pero solo puede ejecutar un máximo de 10 trabajos en paralelo en un momento dado.
    - Si la optimización de archivos está activada, los archivos pequeños se empaquetan en la ingesta para mejorar el rendimiento de copia. En estos casos, verá un archivo empaquetado (GUID como nombre) como se muestra en la captura de pantalla siguiente.

        ![Ejemplo de un archivo empaquetado](media/data-box-deploy-copy-data-via-copy-service/packed-file-on-ingest.png)

6. Mientras el trabajo está en curso, en la página **Copiar datos**:

    - Puede ver el estado del trabajo de copia en la columna **Estado**. El estado puede ser:
        - **Ejecución**
        - **Erróneo**
        - **Correcto**
        - **Pausando**
        - **En pausa**
        - **Cancelando**
        - **Canceled**
        - **Completed with errors** (Completado con errores)
    - En la columna **Archivos**, puede ver el número y el tamaño del total de archivos que se va a copiar.
    - En la columna **Procesado**, puede ver el número y el tamaño del total de archivos que se va a procesar.
    - En la columna **Detalles**, haga clic en **Ver** para ver los detalles del trabajo.
    - Si tiene se produce algún error durante el proceso de copia como se muestra en la columna **# errores** (Nº de errores), vaya a la columna **Registro de errores** y descargue los registros de error para la solución de problemas.

Espere a que finalicen los trabajos de copia. Como algunos errores solo se registran en la página **Connect and copy** (Conectar y copiar), asegúrese de que los trabajos de copia han finalizado sin errores antes de ir al paso siguiente.

![No hay errores en la página **Connect and copy**](media/data-box-deploy-copy-data-via-copy-service/verify-no-errors-on-connect-and-copy.png)

Para garantizar la integridad de los datos, la suma de comprobación se calcula a medida que los datos se copian. Una vez completada la copia, compruebe el espacio utilizado y el espacio disponible en el dispositivo.
    
![Comprobación del espacio libre y utilizado en el panel](media/data-box-deploy-copy-data-via-copy-service/verify-used-space-dashboard.png)

Una vez finalizado el trabajo de copia, puede ir a **preparación para el envío**.

>[!NOTE]
> Preparación para el envío no se puede ejecutar mientras que los trabajos de copia están en curso.

## <a name="prepare-to-ship"></a>Preparación para el envío

[!INCLUDE [data-box-prepare-to-ship](../../includes/data-box-prepare-to-ship.md)]


## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha obtenido información acerca de varios temas relacionados con Azure Data Box, como:

> [!div class="checklist"]
> * Copia de datos a un dispositivo Data Box
> * Preparación del envío a Data Box

En el siguiente tutorial aprenderá a enviar su dispositivo Data Box a Microsoft.

> [!div class="nextstepaction"]
> [Envío de un dispositivo Data Box a Microsoft](./data-box-deploy-picked-up.md)

