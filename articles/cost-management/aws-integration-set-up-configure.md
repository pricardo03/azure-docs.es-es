---
title: Instalar y configurar la integración de informes de uso y costos de AWS con Azure Cost Management
description: Este artículo le guiará a través de cómo instalar y configurar la integración de informes de uso y costos de AWS con Azure Cost Management.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 05/21/2019
ms.topic: conceptual
ms.service: cost-management
manager: ormaoz
ms.custom: ''
ms.openlocfilehash: 951178a82e0975f5f2af71bd48cf0f931246ae37
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2019
ms.locfileid: "66002123"
---
# <a name="set-up-and-configure-aws-cost-and-usage-report-integration"></a>Instalar y configurar la integración de informes de uso y costos de AWS

Con Amazon Web Services (AWS) costo y uso de la integración de informes (actual), puede supervisar y controlar los gastos de AWS en Azure Cost Management. La integración permite una sola ubicación en el portal de Azure donde puede supervisar y el control de gasto de Azure y AWS. En este artículo se explica cómo configurar la integración y configúrelo para que use las características de administración de costos de Azure para analizar los costos y revisar los presupuestos.

Costo los procesos de administración en el informe de uso y costos de AWS almacenado en un depósito de S3 con sus credenciales de acceso AWS para obtener las definiciones de informe y descargar archivos GZIP CSV del informe.

## <a name="create-a-cost-and-usage-report-in-aws"></a>Crear un informe de costo y uso en AWS

Uso de un informe de costo y uso es la manera recomendada de AWS para recopilar y procesar los costos de AWS. Para obtener más información, consulte el [informe de uso y costos de AWS](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-reports-costusage.html) documentación.

Use la **informes de uso y costo** página de la consola de administración de costos y facturación de AWS para crear un informe de costo y uso con los pasos siguientes:

1. Inicie sesión la consola de administración de AWS y abra el [consola de administración de costos y facturación](https://console.aws.amazon.com/billing).
2. En el panel de navegación, seleccione **informes de uso y costo**.
3. Seleccione **crear informe**.
4. Para **nombre del informe**, escriba un nombre para el informe.
5. Para **detalles del informe adicional** , para incluir los identificadores de cada recurso en el informe y seleccione **incluir identificadores de recursos**.
6. Para **configuración de actualización de datos**, seleccione si desea que el informe de uso y costos de AWS para actualizar si AWS aplica reembolsos, créditos, o admitir cuotas a su cuenta después de finalizar la factura. Cuando se actualiza un informe, se carga un nuevo informe a Amazon S3. Se recomienda dejar esta configuración en.
7. Seleccione **Next** (Siguiente).
8. Para **depósito de S3**, elija **configurar**.
9. En el cuadro de diálogo Configurar el cubo de S3, realice una de las siguientes acciones:
    1. Seleccione un cubo existente en la lista desplegable y elija **siguiente**.
    2. Escriba un nombre de cubo y la región donde desea crear un nuevo cubo y elegir **siguiente**.
10. Seleccione que he confirmado que esta directiva es corregir y seleccione Guardar.
11. (Opcional) Prefijo de ruta de acceso del informe, escriba el prefijo de ruta de acceso del informe que desee antepuesto al nombre del informe.
Si no se especifica un prefijo, el prefijo predeterminado es el nombre que especificó para el informe en el paso 4 y el intervalo de fechas para el informe, en el formato siguiente: `/report-name/date-range/`
12. Para **unidad de tiempo**, elija **por hora**.
13. Para **control de versiones de informe**, elija si desea que cada versión del informe para sobrescribir la versión anterior del informe o para entregarse además de las versiones anteriores.
14. Para **habilitar la integración de datos para**, se requiere ninguna selección.
15. Para **compresión**, seleccione **GZIP**.
16. Seleccione **Next** (Siguiente).
17. Una vez que haya revisado la configuración para el informe, seleccione **revisar y completar**.

    Tenga en cuenta el nombre del informe. Se usará en pasos posteriores.

Puede tardar hasta 24 horas para AWS empezar a entregar los informes en el depósito de Amazon S3. Cuando se inicia la entrega, AWS actualiza los archivos de informe de uso y costos de AWS al menos una vez al día. Puede seguir para configurar un entorno de AWS sin tener que esperar para que su entrega a iniciar.

## <a name="create-a-role-and-policy-in-aws"></a>Crear un rol y una directiva en AWS

Azure Cost Management tiene acceso el depósito de S3, donde el costo y uso se encuentra informe varias veces al día. El servicio necesita acceso a las credenciales que se va a comprobar si hay nuevos datos. Crear un rol y una directiva en AWS para permitir la administración de costos acceder a él.

Para habilitar el acceso basado en roles para una cuenta de AWS en Cost Management, se crea el rol en la consola de AWS. Debe tener la _ARN de rol_ y _Id. externo_ desde la consola AWS. Más adelante, usa en el **crear un conector de AWS** página de administración de costos.

Use el Asistente Crear un nuevo rol:

1. Inicie sesión la consola AWS y seleccione **servicios**.
2. En la lista de servicios, seleccione **IAM**.
3. Seleccione **Roles** y, a continuación, seleccione **Create Role**.
4. En la siguiente página, seleccione **cuenta de AWS otro**.
5. En **Id. de cuenta**, escriba **432263259397**.
6. En **opciones**, seleccione **requieren el Id. externo (práctica recomendada cuando se supondrá que este rol de un tercero)**.
7. En **Id. externo**, escriba el Id. externo. El identificador externo es un código de acceso compartido entre el rol AWS y Azure Cost Management. También se utiliza el mismo Id. externo en el **nuevo conector** página de administración de costos. Por ejemplo, se parece un Id. externo _Companyname1234567890123_.

    > [!NOTE]
    > No cambie la selección de **exigencia de MFA**. Debe permanecer desactivado.
8. Seleccione **Siguiente: Permisos**.
9. Seleccione **Crear directiva**. Se abre una nueva pestaña del explorador. Es donde se crea una directiva.
10. Seleccione **elegir un servicio**.

Configurar los permisos para el informe de costo y uso:

1. Escriba **informe de uso y costo**.
2. Seleccione **nivel de acceso** > **lectura** > **DescribeReportDefinitions**. Este paso permite la administración de costos leer se definen qué informes CUR y determinar si coinciden con los requisitos previos de definición de informe.
3. Seleccione **agregar permisos adicionales**.

Configurar los permisos para el cubo de S3 y objetos:

1. Seleccione **elegir un servicio**.
2. Escriba **S3**.
3. Seleccione **nivel de acceso** > **lista** > **ListBucket**. Esta acción obtiene la lista de objetos en el depósito de S3.
4. Seleccione **nivel de acceso** > **lectura** > **GetObject**. Esta acción permite la descarga de archivos de facturación.
5. Seleccione **recursos**.
6. Seleccione **depósito: agregar ARN**.
7. En **nombre del depósito**, escriba el depósito que se usa para almacenar los archivos CUR.
8. Seleccione **objeto: agregar ARN**.
9. En **nombre del depósito**, escriba el depósito que se usa para almacenar los archivos CUR.
10. En **nombre de objeto**, seleccione **cualquier**.
11. Seleccione **agregar permisos adicionales**.

Configurar los permisos para el Explorador de costo:

1. Seleccione **elegir un servicio**.
2. Escriba **costo servicio Explorer**.
3. Seleccione **acciones de todos los costos de servicio de explorador (ce:\*)**. Esta acción Valida que la colección es correcta.
4. Seleccione **agregar permisos adicionales**.

Agregue el permiso para las organizaciones de AWS:

1. Escriba **organizaciones**.
2. Seleccione **nivel de acceso** > **lista** > **ListAccounts**. Esta acción obtiene los nombres de las cuentas.
3. En **revisar directiva**, escriba un nombre para la nueva directiva. Compruebe que escribió la información correcta y, a continuación, seleccione **Crear directiva**.
4. Vuelva a la pestaña anterior y actualizar la página Web de su explorador. En la barra de búsqueda, busque la nueva directiva.
5. Seleccione **siguiente: revisión**.
6. Escriba un nombre para el nuevo rol. Compruebe que escribió la información correcta y, a continuación, seleccione **Create Role**.

    Tenga en cuenta el rol ARN y el Id. externo usan en los pasos anteriores cuando creó el rol. Deberá usar más adelante cuando configure el conector de Azure Cost Management.

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

Use la siguiente información para crear un conector de AWS y comenzar a supervisar los costos AWS:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Vaya a **costo Management + facturación** > **administración de costos**.
3. En **configuración**, seleccione **conectores (versión preliminar) en nube**.  
    ![Ejemplo que muestra los conectores en la nube (versión preliminar) configuración)](./media/aws-integration-setup-configure/cloud-connectors-preview01.png).
4. Seleccione **+ agregar** en la parte superior de la página para crear un conector.
5. En el **crear un conector de AWS** página **nombre para mostrar**, escriba un nombre para el conector.  
    ![Ejemplo de la página para crear un conector de AWS](./media/aws-integration-setup-configure/create-aws-connector01.png)
6. Opcionalmente, seleccione el grupo de administración predeterminado. Se van a almacenar todos los detectados cuentas vinculadas. Se puede configurar más adelante.
7. En el **facturación** sección, seleccione **le cobra automáticamente en el 1% en disponibilidad general** si desea garantizar el funcionamiento continuo cuando caduca la versión preliminar. Si selecciona la opción automática, debe seleccionar una suscripción de facturación.
8. Para **ARN de rol**, escriba el valor que usó al configurar el rol de AWS.
9. Para **Id. externo**, escriba el valor que usó al configurar el rol de AWS.
10. Para **nombre del informe**, escriba el nombre que ha creado en AWS.
11. Seleccione **siguiente** y, a continuación, seleccione **crear**.

Puede tardar unas horas para los nuevos ámbitos AWS, el AWS consolidados cuenta y las cuentas AWS vinculado y que aparezcan sus datos de costo.

Después de crear el conector, se recomienda que asignar el control de acceso a él. Los usuarios se les asignan permisos a los ámbitos recién detectados: AWS consolidados cuenta y las cuentas de AWS vinculado. El usuario que crea el conector es el propietario del conector, la cuenta consolidada y todas las cuentas vinculadas.

Asignar permisos de conector a los usuarios después de que se produce la detección no asigne permisos a los ámbitos AWS existentes. En su lugar, solo nuevas cuentas vinculadas se les asignan permisos.

## <a name="take-additional-steps"></a>Realizar pasos adicionales

- [Configurar grupos de administración](../governance/management-groups/index.md#initial-setup-of-management-groups), si no lo ha hecho ya.
- Compruebe que se agregan nuevos ámbitos para el selector de ámbito. Seleccione **actualizar** para ver los datos más recientes.
- En el **en la nube conectores** , seleccione el conector y seleccione **vaya a la cuenta de facturación** para asignar la cuenta vinculada a grupos de administración.

## <a name="manage-cloud-connectors"></a>Administrar conectores en la nube

Cuando selecciona un conector en el **en la nube conectores** página, puede:

- Seleccione **vaya a la cuenta de facturación** ver información de la AWS cuenta consolidada.
- Seleccione **Control de acceso** para administrar la asignación de roles para el conector.
- Seleccione **editar** para actualizar el conector. No se puede cambiar el número de cuenta AWS, ya que aparece en el ARN de rol. Pero puede crear un nuevo conector.
- Seleccione **compruebe** para volver a ejecutar la prueba de comprobación para asegurarse de que Cost Management podrá recopilar datos mediante el uso de la configuración del conector.

![Ejemplo de lista de conectores creados de AWS](./media/aws-integration-setup-configure/list-aws-connectors.png)

## <a name="set-up-azure-management-groups"></a>Configurar grupos de administración de Azure

Para crear un único lugar para ver información del proveedor de toda la nube, deberá colocar sus suscripciones de Azure y cuentas de AWS vinculado en el mismo grupo de administración. Si ya no ha configurado el entorno de Azure con grupos de administración, consulte [configuración de grupos de administración inicial](../governance/management-groups/index.md#initial-setup-of-management-groups).

Si desea separar los costos, puede crear un grupo de administración que contiene solo las cuentas de AWS vinculado.

## <a name="set-up-an-aws-consolidated-account"></a>Configurar AWS cuenta consolidada

La cuenta de AWS consolidado combina la facturación y pago para varias cuentas de AWS. También actúa como una cuenta de AWS vinculado.

![Cuenta consolidan de los detalles de ejemplo de AWS](./media/aws-integration-setup-configure/aws-consolidated-account01.png)

En la página, hacer lo siguiente:

- Seleccione **actualizar** para actualización masiva la asociación de AWS vinculado cuentas con un grupo de administración.
- Seleccione **Control de acceso** para establecer la asignación de roles para el ámbito.

### <a name="permissions-for-an-aws-consolidated-account"></a>Cuenta consolidan de los permisos de AWS

De forma predeterminada, los permisos para una cuenta de AWS consolidado se establecen durante la creación de la cuenta, en función de los permisos de conector AWS. El creador del conector es el propietario.

Administrar el nivel de acceso mediante la **nivel de acceso** cuenta consolidada de página de la AWS. Sin embargo, AWS vinculado cuentas no heredan permisos a la cuenta AWS consolidado.

## <a name="set-up-an-aws-linked-account"></a>Configurar una cuenta AWS vinculado

La cuenta AWS vinculado es donde se crean y administran los recursos de AWS. Una cuenta vinculada también actúa como un límite de seguridad.

Desde esta página, hacer lo siguiente:

- Seleccione **actualizar** actualizar la asociación de AWS vinculado cuenta con un grupo de administración.
- Seleccione **Control de acceso** para establecer una asignación de roles para el ámbito.

![Ejemplo de la página de la cuenta vinculada de AWS](./media/aws-integration-setup-configure/aws-linked-account01.png)

### <a name="permissions-for-an-aws-linked-account"></a>Permisos para AWS cuenta vinculada

De forma predeterminada, se establecen los permisos para una cuenta de AWS vinculado tras su creación, según los permisos de conector AWS. El creador del conector es el propietario. Administrar el nivel de acceso mediante la **nivel de acceso** cuenta vinculada de página de la AWS. AWS vinculado cuentas no heredan los permisos de una cuenta de AWS consolidado.

Cuentas AWS vinculadas siempre heredan los permisos del grupo de administración que pertenecen.

## <a name="next-steps"></a>Pasos siguientes

- Ahora que se haya instalado y configurado la integración de informes de uso y costos de AWS, continúe con [uso y los costos de AWS administrar](aws-integration-manage.md).
- Si no está familiarizado con el análisis de costos, consulte [Explore y analice los costos con análisis de costos](quick-acm-cost-analysis.md) Guía de inicio rápido.
- Si no está familiarizado con los presupuestos en Azure, consulte [crear y administrar presupuestos Azure](tutorial-acm-create-budgets.md).
