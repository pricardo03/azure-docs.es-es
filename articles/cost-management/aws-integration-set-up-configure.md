---
title: Instalar y configurar la integración de informes de uso y costos de AWS con Azure Cost Management
description: Este artículo le guiará aunque cómo instalar y configurar la integración de informes de uso y costos de AWS con Azure Cost Management.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 06/06/2019
ms.topic: conceptual
ms.service: cost-management
manager: ormaoz
ms.custom: ''
ms.openlocfilehash: a7a020284f44eda0da62f307866c74b0a8df493d
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65205700"
---
# <a name="set-up-and-configure-aws-cost-and-usage-report-integration"></a>Instalar y configurar la integración de informes de uso y costos de AWS

Con la integración de informes de uso y costo de Amazon Web Services, puede supervisar y controlar los gastos de AWS en Azure Cost Management. La integración permite una sola ubicación en el portal de Azure donde puede supervisar y el control de gasto de Azure y AWS. En este artículo se explica cómo configurar la integración y configúrelo para que use las características de administración de costos para analizar los costos y revisar los presupuestos.

Costo los procesos de administración en el informe de uso y costos de AWS almacenado en un depósito de S3 con sus credenciales de acceso AWS para obtener las definiciones de informe y descargar archivos GZIP CSV del informe.

## <a name="create-a-cost-and-usage-report-in-aws"></a>Crear un informe de costo y uso en AWS

Usa un informe de costo y uso es la forma recomendada de AWS para recopilar y procesar los costos de AWS. Para obtener más información, consulte el [informe de uso y costos de AWS](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-reports-costusage.html) artículo de documentación.

Use la **informes** página de la consola de administración de costos y facturación de AWS para crear un informe de costo y uso con los pasos siguientes.

1. Inicie sesión en la consola de administración de AWS y abra la consola de administración de costos y facturación en https://console.aws.amazon.com/billing.
2. En el panel de navegación, haga clic en **informes**.
3. Haga clic en **crear informe**.
4. Para **nombre del informe**, escriba un nombre para el informe.
5. Para **unidad de tiempo**, elija **por hora**.
6. Para **Include**, agregue los identificadores de cada recurso en el informe y seleccione **identificadores de recursos**.
7. Para **habilitar la compatibilidad con**, se requiere ninguna selección.
8. Para **configuración de actualización de datos**, seleccione **actualizar automáticamente el costo &amp; cerrado de informe de uso cuando se detectan los cargos de los meses anteriores con facturas**.
9. Haga clic en **Next**.
10. Para **depósito de Amazon S3**, escriba el nombre del depósito de Amazon S3 en la que desea entregar a los informes y haga clic en **compruebe**. El cubo debe tener los permisos adecuados para ser válido. Para obtener más información acerca de cómo agregar permisos para el cubo, vea [depósito de configuración y los permisos de acceso del objeto](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/set-permissions.html).
11. Para **prefijo de ruta de acceso del informe**, escriba el prefijo de ruta de acceso del informe que desea que se agrega al nombre del informe.
12. Para **compresión**, seleccione **GZIP**.
13. Haga clic en **Next**.
14. Después de haber revisado la configuración para el informe, haga clic en **revisar y completar**.
    Tenga en cuenta la **nombre del informe**. Se usará en pasos posteriores.

Puede tardar hasta 24 horas para AWS empezar a entregar los informes en el depósito de Amazon S3. Cuando se inicia la entrega, AWS actualiza los archivos de informe de uso y costos de AWS al menos una vez al día. Puede seguir para configurar un entorno de AWS sin tener que esperar para que su entrega a iniciar.

## <a name="create-a-role-and-policy-in-aws"></a>Crear un rol y una directiva en AWS

Azure Cost Management tiene acceso el depósito de S3, donde el costo y uso se encuentra informe varias veces al día. Cost Management necesita acceso a las credenciales que se va a comprobar si hay nuevos datos. Crear un rol y una directiva en AWS para permitir el acceso a Cost Management.

Para habilitar el acceso basado en roles para una cuenta de AWS en Azure Cost Management, se crea el rol en la consola de AWS. Debe tener la _ARN de rol_ y _Id. externo_ desde la consola AWS. Más adelante, usarlos en el crear una página del conector AWS en Azure Cost Management.

Use el Asistente Crear un nuevo rol:

1. Inicie sesión en la consola AWS y seleccione **servicios**.
2. En la lista de servicios, seleccione **IAM**.
3. Seleccione **Roles** y, a continuación, haga clic en **Create Role**.
4. En la siguiente página, seleccione **cuenta de AWS otro**.
5. En **Id. de cuenta** , escriba _432263259397_.
6. En **opciones**, seleccione **requieren el Id. externo (práctica recomendada cuando se supondrá que este rol de un tercero)**.
7. En **Id. externo**, escriba el identificador externo. El identificador externo es un código de acceso compartido entre el rol de AWS y Azure Cost Management. También se usa el mismo Id. externo en la página nuevo conector de administración de costos. Por ejemplo, se parece un Id. externo _Companyname1234567890123_.
    No cambie la selección de **exigencia de MFA**. Debe permanecer desactivado.
8. Haga clic en **Siguiente: Permisos**.
9. Haga clic en **Create policy** (Crear directiva). Se abre una nueva pestaña del explorador donde se crea una nueva directiva.
10. Haga clic en **elegir un servicio**.

Configurar los permisos de informe de uso y costo:

1. Tipo **informe de uso y costo**.
2. Seleccione **nivel de acceso**, **lectura** > **DescribeReportDefinitions**. Esto permite el que costo de administración, consulte ¿Qué informes CUR se definen y determinar si coinciden con los requisitos previos de definición de informe.
3. Haga clic en **agregar permisos adicionales**.

Configurar el permiso de cubo y los objetos S3:

1. Haga clic en **elegir un servicio**.
2. Tipo _S3_.
3. Seleccione **nivel de acceso**, **lista** > **ListBucket**. Esta acción obtiene la lista de objetos en el depósito de S3.
4. Seleccione **nivel de acceso**, **lectura** > **GetObject**. Esta acción permite la descarga de archivos de facturación.
5. Seleccione **recursos**.
6. Seleccione **depósito: agregar ARN**.
7. En **nombre del depósito**, escriba el depósito que se usa para almacenar los archivos CUR.
8. Seleccione **objeto: agregar ARN**.
9. En **nombre del depósito**, escriba el depósito que se usa para almacenar los archivos CUR.
10. En **nombre de objeto**, seleccione **cualquier**.
11. Haga clic en **agregar permisos adicionales**.

Configurar los permisos del explorador de costos:

1. Haga clic en **elegir un servicio**.
2. Tipo _costo servicio Explorer_.
3. Seleccione **acciones de todos los costos de servicio de explorador (ce:\*)**. Esta acción Valida que la colección es correcta.
4. Haga clic en **agregar permisos adicionales**.

Agregue el permiso de las organizaciones:

1. Tipo **organizaciones**.
2. Seleccione **nivel de acceso, lista** > **ListAccounts**. Esta acción obtiene los nombres de las cuentas.
3. En **revisar directiva**, escriba un nombre para la nueva directiva. Comprobación para asegurarse de que escribió la información correcta y, a continuación, haga clic en **Crear directiva**.
4. Vuelva a la pestaña anterior y actualice la página del explorador web. En la barra de búsqueda, busque la nueva directiva.
5. Seleccione **siguiente: revisión**.
6. Escriba un nombre para el nuevo rol. Comprobación para asegurarse de que escribió la información correcta y, a continuación, haga clic en **Create Role**.
    Tenga en cuenta la **ARN de rol** y **Id. externo** utilizado en los pasos anteriores cuando creó el rol. Se usarán más adelante cuando configure el conector de Azure Cost Management.

La directiva JSON debe parecerse al ejemplo siguiente. Reemplace _bucketname_ con el nombre del cubo S3.

```JSON
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
"organizations:ListAccounts",
            "ce:*",
            "cur:DescribeReportDefinitions"
            ],
            "Resource": "*"
        },
        {
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::bucketname",
                "arn:aws:s3:::bucketname/*"
            ]
        }
    ]
}
```

## <a name="set-up-a-new-aws-connector-in-azure"></a>Configurar un nuevo conector AWS en Azure

Use la información siguiente para crear un nuevo conector AWS y comenzar a supervisar los costos AWS.

1. Inicie sesión en el portal de Azure en https://portal.azure.com.
2. Vaya a **costo Management + facturación** > **administración de costos**.
3. En **configuración**, haga clic en **conectores (versión preliminar) en nube**.  
    ![Ejemplo que muestra los conectores en la nube (configuración de vista previa)](./media/aws-integration-setup-configure/cloud-connectors-preview01.png).
4. Haga clic en **+ agregar** en la parte superior de la página para crear un nuevo conector.
5. En la creación de una página del conector AWS, escriba un **nombre para mostrar** nombrar el conector.  
    ![Ejemplo de la creación de una página del conector de AWS](./media/aws-integration-setup-configure/create-aws-connector01.png)
6. Opcionalmente, seleccione el grupo de administración predeterminado. Se van a almacenar todos los detectados cuentas vinculadas. Se puede configurar más adelante.
7. En el **facturación** sección, seleccione **le cobra automáticamente en el 1% en disponibilidad general** si desea garantizar el funcionamiento continuo cuando caduca la versión preliminar. Si selecciona la opción automática, debe seleccionar una facturación **suscripción**.
8. Escriba el **ARN de rol**. Es el valor que usó al configurar el rol de AWS.
9. Escriba el **Id. externo**. Es el valor que usó al configurar el rol de AWS.
10. Escriba el **nombre del informe** que creó en AWS.
11. Haga clic en **siguiente** y, a continuación, haga clic en **crear**.

Puede tardar unas horas para los nuevos ámbitos AWS, la cuenta de consolidado de AWS y las cuentas vinculadas de AWS y sus datos de costos que aparezca.

Después de crear el conector, se recomienda que asignar el control de acceso al conector. Los usuarios se les asignan permisos a los ámbitos recién detectados: AWS consolidados cuenta y AWS cuentas vinculadas. El usuario que crea el conector es el propietario del conector, cuenta consolidada y cuentas vinculadas todas.

Asignar permisos de conector a los usuarios después de que se produce la detección no asigne permisos a los ámbitos AWS existentes. En su lugar, solo nuevas cuentas vinculadas se les asignan permisos.

## <a name="additional-steps"></a>Pasos adicionales

- [Configurar grupos de administración](../governance/management-groups/index.md#initial-setup-of-management-groups), si no lo ha hecho ya.
- Compruebe que se agregan nuevos ámbitos para el selector de ámbito. No olvide hacer clic en **actualizar** para ver los datos más recientes.
- En la página del conector en la nube, seleccione el conector y haga clic en **vaya a la cuenta de facturación** para asignar la cuenta vinculada a grupos de administración.

## <a name="manage-cloud-connectors"></a>Administrar conectores en la nube

Cuando se selecciona un conector en la página de conectores en la nube, puede:

- Haga clic en **vaya a la cuenta de facturación** para ver la información de cuenta de AWS consolidada.
- Haga clic en **Control de acceso** para administrar la asignación de roles para el conector.
- Haga clic en **editar** para actualizar el conector. No se puede cambiar el número de cuenta de AWS, tal como aparece en el ARN de rol. Sin embargo, puede crear un nuevo conector.
- Haga clic en **compruebe** para volver a ejecutar la prueba de comprobación para asegurarse de que Cost Management podrá recopilar datos mediante la configuración del conector.

![Ejemplo que muestra la lista de conectores creados de AWS](./media/aws-integration-setup-configure/list-aws-connectors.png)

## <a name="role-of-azure-management-groups"></a>Función de los grupos de administración de Azure

Para crear un único lugar para ver información del proveedor de toda la nube, deberá colocar sus suscripciones de Azure y cuentas de AWS vinculado en el mismo grupo de administración. Si ya no ha configurado el entorno de Azure con grupos de administración, consulte [configuración de grupos de administración inicial](../governance/management-groups/index.md#initial-setup-of-management-groups).

Si desea separar los costos, puede crear un grupo de administración que contiene solo las cuentas de AWS vinculado.

## <a name="aws-consolidated-account"></a>Cuenta de AWS consolidado

La cuenta de AWS consolidados se utiliza para consolidar la facturación y pago para varias cuentas de AWS. La cuenta de AWS consolidado también actúa como una cuenta vinculada de AWS.

![Ejemplo de detalles de la cuenta de AWS consolidado](./media/aws-integration-setup-configure/aws-consolidated-account01.png)

Desde la página, puede:

- Haga clic en **actualizar** para la asociación de las cuentas de AWS vinculado con un grupo de administración de actualización en masa.
- Haga clic en **Control de acceso** para establecer la asignación de roles para el ámbito.

### <a name="aws-consolidated-account-permissions"></a>Permisos de cuenta consolidado de AWS

De forma predeterminada, el AWS consolidado cuenta los permisos se establecen durante la creación, según los permisos de conector AWS. El creador del conector es el propietario.

Administrar el nivel de acceso mediante el uso de la página de nivel de acceso de la cuenta de AWS consolidado. Sin embargo, los permisos para el AWS consolidado cuenta no son heredadas por las cuentas vinculadas de AWS.

## <a name="aws-linked-account"></a>Cuenta vinculada de AWS

La cuenta de AWS vinculada es donde se crean y administran los recursos de AWS. Una cuenta vinculada también actúa como un límite de seguridad.

Desde esta página, puede:

- Haga clic en **actualizar** para actualizar la asociación de cuenta de AWS vinculado con un grupo de administración.
- Haga clic en **Control de acceso** para establecer una asignación de roles para el ámbito.

![Ejemplo de la página de la cuenta vinculada de AWS](./media/aws-integration-setup-configure/aws-linked-account01.png)

### <a name="aws-linked-account-permissions"></a>Permisos de cuenta de AWS vinculado

De forma predeterminada, se establecen los permisos de cuenta de AWS vinculado tras su creación, según los permisos de conector AWS. El creador del conector es el propietario. Administrar el nivel de acceso mediante el uso de la página de nivel de acceso de la cuenta vinculada de AWS. Cuentas de AWS vinculadas no heredan permisos para la cuenta de AWS consolidado.

Cuentas de AWS vinculadas siempre heredan los permisos del grupo de administración que pertenecen.

## <a name="next-steps"></a>Pasos siguientes

- Ahora que se haya instalado y configurado la integración de informes de uso y costos de AWS, continúe con [uso y los costos de AWS administrar](aws-integration-manage.md).
- Si no está familiarizado con el análisis de costos, consulte [Explore y analice los costos con análisis de costos](quick-acm-cost-analysis.md) Guía de inicio rápido.
- Si no está familiarizado con los presupuestos en Azure, consulte [crear y administrar presupuestos Azure](tutorial-acm-create-budgets.md) tutorial.
