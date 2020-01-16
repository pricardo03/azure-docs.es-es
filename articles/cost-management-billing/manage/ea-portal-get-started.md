---
title: Introducción al portal del Contrato Enterprise de Azure
description: En este artículo se explica cómo usan los clientes el portal del Contrato Enterprise de Azure.
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 01/03/2020
ms.topic: conceptual
ms.service: cost-management-billing
manager: boalcsva
ms.openlocfilehash: ace3c251d979a67666d2aaf01dca01e257bed66b
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2020
ms.locfileid: "75985732"
---
# <a name="get-started-with-the-azure-ea-portal"></a>Introducción al portal del Contrato Enterprise de Azure

Este artículo ayuda a los clientes directos e indirectos de Azure a empezar a usar el [portal del Contrato Enterprise de Azure](https://ea.azure.com) con información básica sobre:

- La estructura del portal del Contrato Enterprise de Azure.
- Los roles que se usan en el portal del Contrato Enterprise de Azure.
- Cómo empezar a crear suscripciones.
- El análisis de costos en el portal del Contrato Enterprise de Azure y en Azure Portal.

Este es un vídeo que muestra una sesión completa de acceso al portal del Contrato Enterprise de Azure:

[Vídeo de acceso al portal del Contrato Enterprise de Azure](https://www.youtube.com/watch?v=OiZ1GdBpo-I)

>[!VIDEO https://www.youtube.com/embed/OiZ1GdBpo-I]

## <a name="azure-ea-portal-hierarchy"></a>Jerarquía del portal del Contrato Enterprise de Azure

La jerarquía del portal del Contrato Enterprise de Azure consta de:

**Portal del Contrato Enterprise de Microsoft Azure**: el portal del Contrato Enterprise de Azure es un portal de administración en línea que ayuda a administrar los costos de los servicios de Azure EA. Se usa para crear una jerarquía de Azure EA que incluye departamentos, cuentas y suscripciones. También se usa para conciliar los costos de los servicios consumidos, descargar informes de uso y ver listas de precios. Además, se crean claves de API para la inscripción.

**Departamentos**: los departamentos se crean para ayudar a dividir los costos en agrupaciones lógicas y, después, establecer un presupuesto o una cuota en el nivel de departamento.

**Cuentas**: las cuentas son unidades organizativas del portal del Contrato Enterprise de Azure y se usan para administrar suscripciones. Las cuentas también se usan para la generación de informes.

**Suscripciones**: las suscripciones son la unidad más pequeña del portal del Contrato Enterprise de Azure. Son contenedores para los servicios de Azure que administra el administrador de servicios.

En el siguiente diagrama se ilustran las jerarquías simples de Azure EA.

![Diagrama de jerarquías simples de Azure EA](./media/ea-portal-get-started/ea-hierarchies.png)

## <a name="enterprise-user-roles"></a>Roles de los usuarios de empresa

Para administrar los servicios de Azure en su inscripción, hay cinco roles de usuario administrativo de empresa distintos:

- Administrador de empresa
- Administrador de departamentos
- Propietario de cuenta
- Administrador de servicios
- Contacto para notificaciones

Los roles se usan para completar tareas en dos portales de Microsoft Azure diferentes. El portal del Contrato Enterprise de Azure (https://ea.azure.com) se usa para ayudarle a administrar la facturación y los costos. Azure Portal (https://portal.azure.com) se utiliza para administrar los servicios de Azure.

Los roles de usuario están asociados a una cuenta de usuario. Para validar la autenticidad de los usuarios, todos ellos deben tener una cuenta profesional, educativa o de Microsoft válida. Asegúrese de que cada cuenta está asociada a una dirección de correo electrónico que se supervisa activamente. Las notificaciones de las cuentas se envían a la dirección de correo electrónico.

Al configurar usuarios, puede asignar varias cuentas profesionales, educativas o de Microsoft al rol Administrador de empresa. Sin embargo, solo puede asignar una cuenta profesional, educativa o de Microsoft al rol Propietario de la cuenta. Además, una sola cuenta profesional, educativa o de Microsoft puede tener aplicados los roles Administrador de empresa y Propietario de la cuenta.

### <a name="enterprise-administrator"></a>Administrador de empresa

El rol Administrador de empresa tiene el nivel de acceso más alto. Los usuarios con el rol pueden:

- Administrar cuentas y propietarios de cuentas
- Administrar otros administradores de empresa
- Administrar administradores de departamento
- Administrar contactos de notificación
- Visualizar el uso en todas las cuentas
- Visualizar los cargos no facturados en todas las cuentas

Puede tener varios administradores de empresa en una inscripción empresarial. Puede conceder acceso de solo lectura a los administradores de empresa. Estos heredan el rol de administrador de departamento.

### <a name="department-administrator"></a>Administrador de departamentos

Los usuarios con el rol pueden:

- Crear y administrar departamentos
- Crear nuevos propietarios de cuentas
- Ver detalles de uso de los departamentos que administran
- Ver los costos, si se les han concedido los permisos necesarios

Puede tener varios administradores de departamento en cada inscripción empresarial.

Puede conceder acceso de solo lectura a los administradores de departamento. Para conceder acceso de solo lectura, edite o cree un administrador de departamento y establezca la opción de solo lectura en **Sí**.

### <a name="account-owner"></a>Propietario de cuenta

Los usuarios con el rol pueden:

- Crear y administrar suscripciones
- Administrar administradores de servicios
- Ver el uso de las suscripciones

Todas las cuentas requieren una cuenta profesional, educativa o de Microsoft única. Para más información acerca de los roles administrativos del portal del Contrato Enterprise de Azure, consulte [Roles administrativos del Contrato Enterprise de Azure en Azure](understand-ea-roles.md).

### <a name="service-administrator"></a>Administrador de servicios

El administrador de servicios tiene permisos para administrar servicios en Azure Portal y asignar a los usuarios el rol de coadministrador.

### <a name="notification-contact"></a>Contacto para notificaciones

El contacto para notificaciones recibe las notificaciones de uso relacionadas con la inscripción.

## <a name="activate-your-enrollment"></a>Activación de la inscripción

Para activar un servicio, el administrador de empresa inicial abre el portal del Contrato Enterprise de Azure en [https://ea.azure.com](https://ea.azure.com) e inicia sesión con la dirección del correo electrónico de invitación.

Si se ha configurado como administrador de EA, no es preciso que reciba el correo electrónico de activación para iniciar sesión en Azure EA Portal. Puede continuar con [https://ea.azure.com](https://ea.azure.com) e iniciar sesión con su dirección de correo electrónico (ya sea profesional, educativa o Live ID) y la contraseña.

Si tiene más de una inscripción, elija la que desea activar. De forma predeterminada, solo se muestran las inscripciones activas. Para ver el historial de inscripciones, desactive la opción **Active** (Activo) en la parte superior derecha del portal del Contrato Enterprise de Azure.

En Enrollment (Inscripción), aparece el estado **Active** (Activo).

![Ejemplo en el que se muestra una inscripción activa](./media/ea-portal-get-started/ea-enrollment-status.png)

Los administradores de empresa de Azure existentes son los únicos que pueden crear otros administradores de empresa.

### <a name="create-another-enterprise-admin"></a>Creación de otro administrador de empresa

- Inicie sesión en el [portal del Contrato Enterprise de Azure](https://ea.azure.com) y vaya a **Manage (Administrar)**  > **Enrollment Detail** (Detalles de la inscripción) y haga clic en **+ Add Administrator (+Agregar administrador)** en la esquina superior derecha de la página.

Asegúrese de que tiene las direcciones de correo electrónico del usuario y el método de autenticación preferido, como autenticación profesional o educativa, o una cuenta Microsoft. Esta información es necesaria para agregar a cualquier usuario.

Si no es administrador de EA, póngase en contacto con uno de ellos y solicítele que le agregue a una inscripción. Una vez que se haya agregado a una inscripción, recibirá un correo electrónico de activación.

Si el administrador de EA no puede ayudarle, cree una [solicitud de soporte técnico en el portal del Contrato Enterprise de Azure](https://support.microsoft.com/supportrequestform/cf791efa-485b-95a3-6fad-3daf9cd4027c). Proporcione la siguiente información:

- Número de inscripción
- Dirección de correo electrónico que se debe agregar y tipo de autenticación (cuenta profesional, educativa o de Microsoft)
- Aprobación del correo electrónico por parte de un administrador de EA existente
  - Si el administrador de EA existente no está disponible, póngase en contacto con su asociado o asesor de software para solicitarle que cambie los detalles del contacto a través de la herramienta VLSC.

Para más información acerca de los roles administrativos de la empresa, consulte [Roles administrativos del Contrato Enterprise de Azure en Azure](understand-ea-roles.md).

## <a name="create-an-azure-ea-department"></a>Creación de un departamento de Azure EA

Los administradores de empresa y los administradores de departamento usan departamentos para organizar e informar no solo acerca de los servicios empresariales de Azure sino también de su uso por departamento y centro de costos. El administrador de empresa puede:

- Agregar o quitar departamentos
- Asociar una cuenta a un departamento
- Crear administradores de departamento
- Permitir a los administradores de departamento ver el precio y los costos

Un administrador de departamento puede agregar cuentas a sus departamentos. Puede quitar cuentas de sus departamentos, pero no de la inscripción.

Para agregar un departamento:

1. En el área de navegación de la izquierda, haga clic en **Manage** (Administrar).
2. Haga clic en la pestaña **Departament** (Departamento) y, después, en **+ Add Department** (+ Agregar departamento) y escriba la información necesaria.
3. El nombre del departamento es el único campo obligatorio. Debe tener tres caracteres como mínimo.
4. Cuando haya terminado, haga clic en **Add** (Agregar).

## <a name="add-a-department-admin"></a>Incorporación de un administrador de departamento

Una vez creado un departamento, el administrador de empresa de Azure puede agregar administradores de departamento y asociar cada uno de ellos a un departamento. El administrador de departamento puede:

- Crear otros administradores de departamento
- Ver y editar propiedades de departamento como el nombre o el centro de costos
- Agregar una cuenta a sus departamentos
- Quitar cuentas de sus departamentos
- Descargar detalles de uso de sus departamentos
- Ver el uso mensual y los cargos de su departamento si un administrador de empresa le ha concedido el permiso <sup>1</sup>

### <a name="to-add-a-department-admin"></a>Para agregar un administrador de departamento

Como administrador de empresa:

1. En el área de navegación de la izquierda, haga clic en **Manage** (Administrar).
2. Haga clic en la pestaña **Department** (Departamento) y, después, haga clic en el departamento.
3. Haga clic en **+ Add Administrator** (+ Agregar administrador) y agregue la información necesaria.
4. Para que el acceso sea de solo lectura, establezca la opción **Read-Only** (Solo lectura) en **Sí** y haga clic en **Add** (Agregar).

![Ejemplo que muestra el cuadro de diálogo Add Department Administrator (Agregar administrador de departamento)](./media/ea-portal-get-started/ea-create-add-department-admin.png)

### <a name="to-set-read-only-access"></a>Para establecer acceso de solo lectura

Puede conceder acceso de solo lectura a los administradores de departamento. Al crear un administrador de departamento:

- Establezca la opción de solo lectura en **Sí**.

Para editar un administrador de departamento existente:

1. Seleccione un departamento y, después, haga clic en el símbolo del lápiz situado junto al **administrador de departamento** que desea editar.
2. Establezca la opción de solo lectura en **Sí** y haga clic en **Save** (Guardar).

Los usuarios con el rol de administrador de empresa obtienen automáticamente permisos de administrador de departamento.

<sup>1</sup> Si tiene permiso para ver los cargos y el uso mensual del departamento pero no puede verlos, póngase en contacto con su asociado.

## <a name="add-an-account"></a>Agregar una cuenta

La estructura de cuentas y suscripciones afecta al modo en que se administran y aparecen en las facturas e informes. Entre los ejemplos de organización típica se incluye la estructuración por divisiones empresariales, equipos funcionales y ubicaciones geográficas.

Para agregar una cuenta:

1. En el portal del Contrato Enterprise de Azure, haga clic en **Manage** (Administrar) en el área de navegación de la izquierda.
2. Haga clic en la pestaña **Account** (Cuenta) y, después, en la página **Account** (Cuenta), haga clic en  **+Add Account** (+Agregar cuenta).
3. Seleccione un departamento o déjelo como sin asignar y, después, seleccione el tipo de autenticación que desea.
4. Escriba el nombre descriptivo que va a usar para identificar la cuenta en los informes.
5. Escriba la dirección de **Account Owner Email** (Dirección de correo electrónico del propietario de la cuenta) que se va a asociar a la nueva cuenta.
6. Confirme la dirección de correo electrónico y haga clic en **Add** (Agregar).

![Ejemplo que muestra la lista de cuentas y la opción + Add Account (+ Agregar cuenta)](./media/ea-portal-get-started/create-ea-add-an-account.png)

Puede agregar otra cuenta haciendo clic en **Add Another Account** (Agregar otra cuenta), o bien puede hacer clic en **Add** (Agregar) en la esquina inferior derecha de la barra de herramientas izquierda.

Para confirmar la propiedad de la cuenta:

1. Inicie sesión en el portal del Contrato Enterprise de Azure.
1. Para confirmar la propiedad de la cuenta, vea su estado. El estado debe cambiar de **Pending** (Pendiente) a **Start/End date** (Fecha de inicio y de finalización). La fecha de inicio y de finalización es la fecha en que el usuario inició sesión por primera vez y la fecha de finalización del contrato.
1. Cuando aparece el mensaje "Warning" (Advertencia), es necesario que el propietario de la cuenta haga clic en **Continue** (Continuar) para activar la cuenta la primera vez que inicie sesión en Azure EA Portal.


## <a name="change-account-owner"></a>Cambiar el propietario de la cuenta

Los administradores de empresa pueden usar el portal del Contrato Enterprise de Azure para transferir la propiedad de la cuenta de suscripción en una inscripción. La acción mueve todas las suscripciones de una cuenta de usuario de origen a una cuenta de usuario de destino.

Puntos importantes acerca de la transferencia de información de las cuentas de usuario:

- Se admiten las transferencias de una cuenta profesional o educativa a otra cuenta profesional o educativa.
- Se admiten las transferencias de una cuenta Microsoft a una cuenta profesional o educativa.
- No se admiten las transferencias de una cuenta profesional o educativa a una cuenta Microsoft.
- Se admiten las transferencias de una cuenta Microsoft a otra cuenta Microsoft. La cuenta de destino debe ser una cuenta de Azure Commerce válida para ser un destino válido para las transferencias. En las nuevas cuentas, se le pide que cree una cuenta de Azure Commerce al iniciar sesión en el portal del Contrato Enterprise de Azure. En el caso de las cuentas existentes, para que la cuenta sea apta es preciso crear antes una suscripción de Azure.
- Cuando se completa una transferencia de suscripción, Microsoft actualiza el propietario de la cuenta.

Directivas de control de acceso basado en rol:

- Las transferencias de suscripciones de Azure entre dos identificadores de organización que se encuentren en el mismo inquilino son las únicas que conservan las directivas de control de acceso basado en rol (RBAC) existentes de Azure, las asignaciones de roles de administrador de servicios y las asignaciones de roles de coadministrador. Otras transferencias de suscripciones dan como resultado la pérdida de las directivas de RBAC y las asignaciones de roles de coadministrador y administrador de servicios. Las directivas y los roles de administrador no se transfieren entre los distintos directorios. Los administradores de servicios se actualizan al propietario de la cuenta de destino.
- Cuando se realizan transferencias de suscripciones entre dos identificadores de organización que se encuentran en el mismo inquilino, se conservan tanto las directivas de RBAC como los roles de administrador y coadministrador de servicios existentes.

Antes de cambiar el propietario de una cuenta:

1. Vea la pestaña **Account** (Cuenta) e identifique la cuenta de origen. La cuenta de origen debe estar activa.
2. Identifique la cuenta de destino. Debe estar activa.

Para transferir la propiedad de la cuenta para todas las suscripciones:

1. En el área de navegación de la izquierda, haga clic en **Manage** (Administrar).
2. Haga clic en la pestaña **Account** (Cuenta) y mantenga el puntero sobre una cuenta.
3. Haga clic en el símbolo de cambiar el propietario de la cuenta, que está a la derecha. El símbolo se parece a una persona.
4. Seleccione una cuenta válida y, después, haga clic en **Next** (Siguiente).
5. Confirme la transferencia y haga clic en **Submit**  (Enviar).

![Imagen que muestra el símbolo Cambiar propietario de cuenta.](./media/ea-portal-get-started/create-ea-create-sub-transfer-account-ownership-of-sub.png)

Para transferir la propiedad de la cuenta para una sola suscripción:

1. En el área de navegación de la izquierda, haga clic en **Manage** (Administrar).
2. Haga clic en la pestaña **Account** (Cuenta) y mantenga el puntero sobre una cuenta.
3. Haga clic en el símbolo de transferir suscripciones, que se encuentra a la derecha. El símbolo se parece a una página.
4. Seleccione una suscripción apta y haga clic en **Next** (Siguiente).
5. Confirme la transferencia y haga clic en **Submit** (Enviar).

![Imagen que muestra el símbolo Transferir suscripciones](./media/ea-portal-get-started/ea-transfer-subscriptions.png)

Este es un vídeo que muestra la administración de usuarios en el portal del Contrato Enterprise de Azure:

[Vídeo de administración de usuarios en el portal del Contrato Enterprise de Azure](https://www.youtube.com/watch?v=621jVkvmwm8)

>[!VIDEO https://www.youtube.com/embed/621jVkvmwm8]

## <a name="create-a-subscription"></a>una suscripción

Los propietarios de cuentas pueden ver y administrar suscripciones. Las suscripciones se pueden usar para dar acceso a los equipos de la organización a proyectos y entornos de desarrollo. Por ejemplo, prueba, producción, desarrollo y almacenamiento provisional. La creación de distintas suscripciones para cada entorno de aplicación ayuda a proteger cada entorno. También se puede asignar una cuenta de administrador de servicios diferente a cada suscripción. Se pueden asociar las suscripciones a cualquier número de servicios. El propietario de la cuenta crea suscripciones y asigna una cuenta de administrador de servicios a cada una de las suscripciones de su cuenta.

### <a name="add-a-subscription"></a>Agregar una suscripción

Use la siguiente información para agregar una suscripción.

La primera vez que se agrega una suscripción a una cuenta, se pide que se acepten tanto el Contrato Microsoft Online Subscription como un plan de tarifas. Aunque ni el contrato ni el plan se aplican a los clientes con Contrato Enterprise, son necesarios para crear la suscripción. La inscripción del Contrato Enterprise de Microsoft Azure sustituye a los elementos anteriores y la relación contractual no cambia. Cuando se le solicite, seleccione el cuadro que indica que acepta los términos.

Las suscripciones nuevas se crean con el nombre de suscripción predeterminado de  _Microsoft Azure Enterprise_. Este nombre se puede actualizar tanto para diferenciarlo de las restantes suscripciones de la inscripción como para asegurarse de que se reconoce en los informes en el nivel de empresa.

Para agregar una suscripción:

1. En el portal del Contrato Enterprise de Azure, inicie sesión en la cuenta.
2. Haga clic en la pestaña **Admin** y, después, haga clic en **Subscription** (Suscripción) en la parte superior de la página.
2. Compruebe que ha iniciado sesión como propietario de la cuenta.
3. Haga clic en **+Add Subscription** (+Agregar suscripción) y, después, en **Purchase** (Comprar).
  La primera vez que se agrega una suscripción a una cuenta, es preciso especificar la información de contacto. Si se agregan más suscripciones, la información de contacto se agrega automáticamente.
4. Haga clic en **Subscriptions** (Suscripciones), seleccione la suscripción que ha creado y, después haga clic en **Edit Subscription Details** (Editar detalles de suscripción).
5. Actualice los valores de **Subscription Name** (Nombre de la suscripción) y **Service Administrator** (Administrador de servicio) y, después, seleccione la marca de verificación.
  El nombre de la suscripción aparece en los informes y es el nombre del proyecto asociado a la suscripción en el portal de desarrollo.

Las nuevas suscripciones pueden tardar hasta 24 horas en aparecer en la lista de suscripciones. Después de crear una suscripción, puede:

- [Editar los detalles de la suscripción](https://account.azure.com/Subscriptions)
- [Administrar los servicios de la suscripción](https://portal.azure.com/#home)

## <a name="transfer-ea-subscription-to-pay-as-you-go-subscription"></a>Transferencia de una suscripción de Contrato Enterprise a una suscripción de pago por uso

Para transferir una suscripción de Contrato Enterprise a una suscripción individual con las tarifas de pago por uso, debe crear una nueva solicitud de soporte técnico en Azure EA Portal. Para crear una solicitud de soporte técnico, haga clic en **+ Nueva solicitud de soporte técnico** en el área Ayuda y soporte técnico.

## <a name="associate-an-existing-account-with-your-pay-as-you-go-subscription"></a>Asociación de una cuenta existente con la suscripción de pago por uso

Si ya tiene una cuenta de Microsoft Azure existente en Microsoft Azure Portal, escriba la cuenta de Microsoft o la cuenta profesional o educativa asociadas con el fin de asociarla a la inscripción del Contrato Enterprise.

### <a name="associate-an-existing-account"></a>Asociación de una cuenta existente

1. En Enterprise Portal, haga clic en **Manage** (Administrar).
1. Haga clic en la pestaña **Account** (Cuenta).
1. Haga clic en **+Add an account** (+Agregar una cuenta).
1. Escriba la cuenta de Microsoft o la cuenta profesional o educativa asociada con la cuenta existente.
1. Confirme la cuenta de Microsoft o la cuenta profesional o educativa asociada con la cuenta existente.
1. Proporcione el nombre que desea usar para identificar esta cuenta en los informes.
1. Haga clic en **Agregar**.
1. Puede agregar una cuenta adicional seleccionando de nuevo la opción **+Add an Account** (+Agregar una cuenta) o volver a la página principal; para ello, seleccione el botón **Admin** (Administración).
1. Si hace clic para ver la página **Account** (Cuenta), la cuenta recién agregada aparecerá con estado **Pending** (Pendiente).

### <a name="confirm-account-ownership"></a>Confirmación de la propiedad de la cuenta

1. Inicie sesión en la cuenta de correo electrónico asociada a la cuenta de Microsoft o a la cuenta profesional o educativa que ha proporcionado.
1. Abra la notificación de correo electrónico titulada _"Invitation to Activate your Account on the Microsoft Azure Service from Microsoft Volume Licensing"_ (Invitación para activar su cuenta en el servicio Microsoft Azure de licencias por volumen de Microsoft).
1. Haga clic en el vínculo **Log into the Microsoft Azure Enterprise Portal** (Iniciar sesión en Microsoft Azure Enterprise Portal) de la invitación.
1. Haga clic en **Iniciar sesión**.
1. Escriba la cuenta de Microsoft o la cuenta profesional o educativa y escriba la contraseña para iniciar sesión y confirmar la propiedad de la cuenta.

### <a name="azure-marketplace"></a>Azure Marketplace

Aunque la mayoría de las suscripciones se convierten desde el entorno de pago por uso a Azure Enterprise, los servicios de Azure Marketplace no lo hacen. Para tener una vista única de todas las suscripciones y los cargos, se recomienda agregar los servicios de Azure Marketplace a Enterprise Portal:

1. Haga clic en **Manage** (Administrar) en el panel de navegación izquierdo.
1. Haga clic en la pestaña **Enrollment** (Inscripción).
1. Observe la sección Enrollment Detail (Detalles de la inscripción).
1. A la derecha del campo Azure Marketplace, haga clic en el icono de lápiz para habilitarlo y haga clic en **Save** (Guardar).

Ahora, el propietario de la cuenta puede comprar suscripciones de Azure Marketplace que se hayan incorporado anteriormente en el plan de pago por uso.

Una vez que se activen las nuevas suscripciones de Azure Marketplace en su inscripción, cancele las suscripciones de Marketplace creadas en el entorno de pago por uso. Este paso es fundamental para que las suscripciones de Marketplace no entren en un estado incorrecto cuando expire el instrumento de pago del entorno de pago por uso.

### <a name="msdn"></a>MSDN

Las suscripciones a MSDN se convierten automáticamente a Desarrollo/pruebas de MSDN y la oferta de EA perderá cualquier crédito monetario existente.

### <a name="azure-in-open"></a>Azure bajo licencia Open

Al asociar una suscripción de Azure bajo licencia Open con un Contrato Enterprise, se perderán todos los créditos no consumidos de Azure bajo licencia Open. Para evitar la posible pérdida de crédito, es recomendable que los clientes consuman todo el crédito de una suscripción de Azure bajo licencia Open antes de agregar la cuenta a sus Contratos Enterprise.  

### <a name="accounts-with-support-subscriptions"></a>Cuentas con suscripciones de soporte técnico

Al agregar cuentas existentes a Enterprise Portal que tienen una suscripción de soporte técnico (y aún no tienen una suscripción de soporte técnico de Contrato Enterprise), tenga en cuenta que la suscripción de soporte técnico de MOSA no se transfiere automáticamente y es necesario volver a comprarla en el Contrato Enterprise. Se proporcionará un período de gracia para la cobertura de soporte técnico hasta el final del mes siguiente para dar tiempo a hacer un nuevo pedido de soporte técnico.

## <a name="view-usage-summary-and-download-reports"></a>Visualización de resumen de uso e informes de las descargas

Los administradores de empresa pueden ver un resumen de sus datos de uso, el compromiso monetario consumido y los cargos asociados con el uso adicional en el portal del Contrato Enterprise de Azure. Los cargos se presentan en el nivel de resumen en todas las cuentas y suscripciones.

Para ver el uso detallado de cuentas específicas:

Descargue el informe de detalles de uso. Haga clic en **Reports** (Informes) y, después, haga clic en la pestaña **Download Usage** (Descargar uso). En la lista de informes, haga clic en **Download** (Descargar) en el informe mensual que desee obtener.

El informe no incluye ningún impuesto aplicable. Puede haber una latencia de hasta ocho horas desde el momento en que se realizó el uso al momento en que se refleja en el informe.

Para ver los informes y gráficos del resumen de uso:

1. En el portal del Contrato Enterprise de Azure, en el área de navegación, haga clic en **Reports** (Informes) y vea la pestaña **Usage Summary** (Resumen de uso).  
  ![Crear y ver el resumen de uso y descargar informes](./media/ea-portal-get-started/create-ea-view-usage-summary-and-download-reports.png)
2. Seleccione un plazo de compromiso.
3. Alterne entre **M** (mensual) **y** C (personalizado) en la parte superior derecha de la página para ver el **resumen de uso** con las fechas de inicio y finalización personalizadas.  
  ![Crear y ver el resumen de uso y descargar informes en la vista personalizada](./media/ea-portal-get-started/create-ea-view-usage-summary-and-download-reports-custom-view.png)
4. Seleccione un periodo en el gráfico para ver detalles adicionales.
5. El gráfico muestra el uso mes a mes con un desglose del uso realizado, el cargo extra por los servicios, los cargos facturados por separado y cargos por Marketplace.
6. En el mes seleccionado, filtre por departamentos, cuentas y suscripciones debajo del gráfico.
7. Alterne entre **Charge by Services** (Cargo por servicios) y **Charge by Hierarchy** (Cargo por jerarquía).
8. Expanda y contraiga entre **Azure Service** (Servicio de Azure), **Charges Billed Separately** (Cargos facturados por separado) y **Azure Marketplace** para ver los detalles.

Este es un vídeo que muestra cómo ver el uso:

[Vídeo de uso del portal del Contrato Enterprise de Azure](https://www.youtube.com/watch?v=Cv2IZ9QCn9E)

>[!VIDEO https://www.youtube.com/embed/Cv2IZ9QCn9E]

### <a name="download-csv-reports"></a>Descarga de informes CSV

Los administradores de empresa usan la página de descarga de informes mensuales para descargar varios informes como archivos CSV. Incluyen:

- Saldo y cargo
- Detalle de uso
- Cargos de Marketplace
- Hoja de precios

Para descargar informes:


1. En el portal del Contrato Enterprise de Azure, haga clic en **Reports** (Informes).
2. Haga clic en **Download Usage** (Descargar uso) en la parte superior de la página.
3. Seleccione **Download** (Descargar) junto al informe del mes.

Puede haber una latencia de hasta cinco días entre la fecha en que se realizó el uso y el momento en que el uso se muestra en los informes.

Los usuarios que descarguen archivos CSV con Safari en Excel pueden encontrarse con errores de formato. Para evitar errores, abra el archivo con un editor de texto.

![Ejemplo que muestra la página Descargar uso](./media/ea-portal-get-started/create-ea-download-csv-reports.png)

Este es un vídeo que muestra cómo descargar información de uso:

[Vídeo de uso del portal del Contrato Enterprise de Azure](https://www.youtube.com/watch?v=eY797htT1qg)

>[!VIDEO https://www.youtube.com/embed/eY797htT1qg]

### <a name="advanced-report-download"></a>Descarga de informes avanzada

Para los informes de cuentas o intervalos de fechas específicos, se puede usar la descarga de informes avanzados. A partir del 30 de agosto de 2016, el formato del archivo de salida cambia de .xlsx a .csv para alojar conjuntos de registros mayores.

1. Seleccione **Advanced Report Download** (Descarga de informes avanzada).
1. Seleccione **Appropriate Date Range** (Intervalo de fechas adecuado).
1. Seleccione **Appropriate Accounts** (Cuentas adecuadas).
1. Seleccione **Request Usage Data** (Solicitar datos de uso).
1. Seleccione el botón **Refresh** (Actualizar) hasta que el estado del informe se actualice a **Download** (Descargar).
1. Descargue el informe.

## <a name="ea-term-glossary"></a>Glosario de términos del Contrato Enterprise

- **Cuenta**: una unidad organizativa de Azure EA Portal que se usa para administrar las suscripciones y para la creación de informes.
- **Propietario de la cuenta**: persona identificada para administrar suscripciones y administradores de servicios en Microsoft Azure. Pueden ver los datos de uso de esta cuenta y sus suscripciones asociadas.
- **Suscripción de enmienda**: una suscripción de un solo año o coincidente de la enmienda de la inscripción.
- **Compromiso**: compromiso de una cantidad monetaria anual de servicios de Microsoft Azure con una tasa comprometida de descuento por el uso de este pago por adelantado.
- **Administrador de departamento**: personas identificadas para administrar departamentos, crear cuentas nuevas y propietarios de cuentas, ver los detalles de uso de los departamentos que administran y ver los costos cuando se les conceden permisos.
- **Número de inscripción**: identificador único proporcionado por Microsoft para identificar la inscripción específica asociada a un Contrato Enterprise.
- **Administrador de empresa**: personas identificadas para administrar departamentos y propietarios de departamentos, cuentas y propietarios de cuentas en Microsoft Azure. Tienen la capacidad de administrar los administradores de empresa, así como ver los datos de uso, las cantidades facturadas y los cargos no facturados en todas las cuentas y suscripciones asociadas a la inscripción de la empresa.
- **Contrato Enterprise**: un contrato de licencias de Microsoft para clientes con adquisición centralizada que desean estandarizar toda la organización en la tecnología de Microsoft y mantener una infraestructura de tecnología de la información en un estándar de software de Microsoft.
- **Inscripción de Contrato Enterprise**: inscripción en el programa de Contrato Enterprise que proporciona productos de Microsoft en volumen con tarifas con descuento.
- **Cuenta de Microsoft**: Servicio basado en web que permite a los sitios participantes autenticar a un usuario con un único conjunto de credenciales.
- **Enmienda de inscripción de Contrato Enterprise de Microsoft Azure (enmienda de inscripción)** : enmienda firmada por una empresa que les proporciona acceso a Microsoft Azure como parte de su inscripción empresarial.
- **Azure EA Portal**: portal que usan los clientes empresariales para administrar sus cuentas de Microsoft Azure y las suscripciones relacionadas.
- **Cantidad de recursos consumidos**: cantidad de un servicio individual de Microsoft Azure que se usó en un mes.
- **Administrador del servicio**: persona identificada para acceder y administrar suscripciones y proyectos de desarrollo en Azure EA Portal.
- **Suscripción**: Representa una suscripción de Azure EA Portal y es un contenedor de servicios de Microsoft Azure administrados por el mismo administrador de servicios.
- **Cuenta profesional o educativa**: para organizaciones que han configurado Active Directory con Federación en la nube y todas las cuentas están en un solo inquilino.

### <a name="enrollment-statuses"></a>Estados de inscripción:

- **Pending**: el administrador de la inscripción debe iniciar sesión en Azure EA Portal. Una vez iniciada la sesión, la inscripción cambiará a estado Activa.
- **Activa**: la inscripción está activa y se pueden crear cuentas y suscripciones en Azure EA Portal. La inscripción permanecerá activa hasta la fecha de finalización del Contrato Enterprise.
- **Período extendido indefinido**: el período extendido indefinido tiene lugar después de que se haya alcanzado la fecha de finalización del Contrato Enterprise. Permite a los clientes de Contrato Enterprise que participan en el período extendido seguir usando Azure indefinidamente al final de su Contrato Enterprise. Antes de que la inscripción de Contrato Enterprise alcance la fecha de finalización del Contrato Enterprise, el administrador de la inscripción debe decidir si renovará la inscripción mediante la adición de un compromiso monetario adicional, la transferirá a una nueva inscripción, la migrará al Programa de suscripción en línea de Microsoft (MOSP) o confirmará la deshabilitación de todos los servicios asociados a la inscripción.
- **Expirada**: el cliente de Contrato Enterprise no participa en el período extendido y la inscripción del Contrato Enterprise ha alcanzado la fecha de finalización del Contrato Enterprise, la inscripción expirará y se deshabilitarán todos los servicios asociados.
- **Transferida**: las inscripciones en las que se hayan transferido todas las cuentas y servicios asociados a una nueva inscripción aparecerán con estado Transferida. Tenga en cuenta que las inscripciones no se transfieren automáticamente si se genera un nuevo número de inscripción durante la renovación. El número de inscripción anterior debe incluirse en la solicitud de renovación del cliente para facilitar la transferencia automática.

## <a name="get-started-on-azure-ea-faq"></a>Introducción a las preguntas más frecuentes sobre el Contrato Enterprise de Azure

En este documento se proporcionan detalles sobre las preguntas típicas que han preguntado los clientes durante el proceso de incorporación.  

### <a name="can-i-associate-my-existing-azure-account-to-enterprise-enrollment"></a>¿Puedo asociar mi cuenta de Azure existente con la inscripción empresarial?

Sí, puede hacerlo. Es importante recordar que todas las suscripciones de Azure en las que usted es el propietario de la cuenta se convertirán al Contrato Enterprise. Esto incluye las suscripciones que usan crédito mensual (por ejemplo, Visual Studio, AzurePass, MPN, BizSpark, etc.), lo que significa que perderá el crédito mensual al hacerlo.

### <a name="i-accidentally-associated-my-existing-azure-account-with-enterprise-enrollment-as-a-result-i-lost-my-monthly-credit-is-it-possible-to-get-my-monthly-credit-back"></a>He asociado accidentalmente mi cuenta de Azure existente con el Contrato Enterprise. Como resultado, perdí mi crédito mensual. ¿Es posible volver a obtener el crédito mensual?

Para recuperar la ventaja de la suscripción de Azure individual de Visual Studio después de autenticarse como propietario de una cuenta de Contrato Enterprise y después de haber usado el mismo inicio de sesión para el Contrato Enterprise que la suscripción de Visual Studio, debe:
1. Eliminar el propietario de la cuenta en EA Portal, después de eliminar o mover las suscripciones de Azure que posea y de registrarlas en las ventajas individuales de Azure en Visual Studio.
 O BIEN
1. Eliminar el suscriptor de Visual Studio del sitio de administración en VLSC y reasignar la suscripción con un inicio de sesión diferente en este momento; a continuación, estas pueden registrarse para obtener las ventajas individuales de Azure en Visual Studio.

### <a name="what-type-of-subscription-should-i-create"></a>¿Qué tipo de suscripción debo crear?

EA Portal ofrece dos tipos de suscripciones para clientes empresariales:

- Microsoft Azure Enterprise, ideal para:
  - Todos los usos de producción
  - Mejores precios según el gasto en infraestructura
  - Puede encontrar más detalles en https://azure.microsoft.com/pricing/enterprise-agreement/.
- Desarrollo/pruebas - Enterprise, ideal para:
  - Todas las cargas de trabajo de desarrollo y pruebas del equipo
  - Cargas de trabajo de medianas a pesadas para desarrollo y pruebas individuales
  - Acceso a imágenes especiales de MSDN y tarifas de servicio preferentes
  - Puede encontrar más detalles en https://azure.microsoft.com/offers/ms-azr-0148p/.

### <a name="is-it-possible-to-transfer-subscription-ownership-to-another-account"></a>¿Es posible transferir la propiedad de la suscripción a otra cuenta?

Sí, es posible transferir la propiedad de la suscripción a una cuenta diferente. Por ejemplo, si la cuenta A tiene tres suscripciones, el administrador de la empresa podría transferir una suscripción a la cuenta B, una a la cuenta C y otra a la cuenta D o todas a la cuenta E.

Puede ir a EA y hacer clic en Manage > Account (Administrar > Cuenta), desplazar el puntero sobre **Account** (Cuenta) en el extremo derecho y verá las opciones Transfer Ownership (Transferir propiedad) (icono de retrato) y Transfer Subscription (Transferir suscripción) (icono de lista).

Esta opción solo estará visible para las cuentas activas.

### <a name="i-see-subscription-name-defaults-to-offer-name-should-i-change-the-subscription-name-to-something-meaningful-to-my-organization"></a>Veo que el nombre de la suscripción tiene como valor predeterminado el nombre de la oferta, ¿debería cambiar el nombre de la suscripción a algo que sea significativo para mi organización?

Cualquier suscripción creada tendrá como valor predeterminado el tipo de oferta que elija. Se recomienda cambiar el nombre de la suscripción a algo que facilite el seguimiento de la suscripción.

**Para cambiar el nombre:**
1. Inicie sesión en [https://account.windowsazure.com](https://account.windowsazure.com).
1. Haga clic en la lista de suscripciones.
1. Seleccione la suscripción.
1. Haga clic en el icono **Manage Subscription** (Administrar suscripción).
1. Edite los detalles de la suscripción.

### <a name="how-can-i-track-cost-incurred-by-cost-center"></a>¿Cómo puedo realizar un seguimiento de los costos en los que se incurre por centro de costos?

Para realizar un seguimiento del costo por centro de costos, debe definir el centro de costos en cualquiera de los siguientes niveles:
- department
- Cuenta
- Subscription

En función de sus necesidades, puede usar el mismo centro de costos para realizar un seguimiento del uso y los costos asociados a un centro de costos determinado.

Por ejemplo, para realizar un seguimiento de los costos de un proyecto especial en el que participan varios departamentos, puede que desee utilizar el centro de costos en un nivel de suscripción para realizar el seguimiento del uso y el costo.

No se puede definir el centro de costos en el nivel de servicio y, en caso de que desee realizar un seguimiento del uso en el nivel de servicio, puede usar la característica "Etiqueta" disponible en el nivel de servicio.

### <a name="how-do-i-track-usage-and-spend-by-different-departments-in-my-organization"></a>¿Cómo puedo realizar un seguimiento del uso y el gasto de los distintos departamentos de mi organización?

Puede crear tantos departamentos como necesite en su inscripción de Contrato Enterprise. Para realizar el seguimiento del uso correctamente, debe asegurarse de que las suscripciones no se comparten entre departamentos.

Una vez que se hayan creado el departamento y la suscripción, puede ver la información en el informe de uso, que le ayudará a realizar un seguimiento del uso y a administrar los costos y gastos en el nivel de departamento.

También puede acceder al uso mediante la información detallada de la API y el código de ejemplo disponible en [https://ea.azure.com/helpdocs/reportingAPI](https://ea.azure.com/helpdocs/reportingAPI).

### <a name="can-i-set-the-spending-quota-and-get-alerts-as-i-approach-my-limit"></a>¿Puedo establecer la cuota de gasto y obtener alertas a medida que me acerque al límite?

Puede establecer la cuota de gasto en el nivel de departamento y el sistema le notificará automáticamente a medida que los límites de gasto lleguen al 50 %, 75 %, 90 % y al 100 % de la cuota que defina.

Para definir la cuota de gasto, haga clic en el departamento al que desea agregar un límite de gasto y haga clic en el icono de edición. Haga clic en **Save** (Guardar) para guardar los detalles.

### <a name="i-used-resource-groups-rgs-to-implement-rbac-and-track-usage-how-can-i-view-the-associated-usage-details"></a>He usado grupos de recursos (RG) para implementar RBAC y realizar un seguimiento del uso, ¿cómo puedo ver los detalles de uso asociados?

Si se utilizan, se realiza un seguimiento de la información como "grupos de recursos" y "etiqueta" en el nivel de servicio y la información está disponible en el archivo de descarga del uso detallado (CSV), que se puede descargar desde Azure EA Portal [https://ea.azure.com/report/downloadusage](https://ea.azure.com/report/downloadusage).

También puede acceder al uso mediante la información detallada de la API y el código de ejemplo disponible en [https://ea.azure.com/helpdocs/reportingAPI](https://ea.azure.com/helpdocs/reportingAPI).

Solo puede aplicar etiquetas a recursos que admiten operaciones de Resource Manager. Si creó una máquina virtual, una red virtual o un almacenamiento mediante el modelo de implementación clásica (por ejemplo, a través del portal clásico), no podrá aplicar una etiqueta a ese recurso. Debe volver a implementar estos recursos mediante Resource Manager para admitir el etiquetado. Todos los demás recursos admiten el etiquetado.

### <a name="can-i-perform-analyses-using-power-bi"></a>¿Puedo realizar análisis mediante Power BI?

Sí. Con el paquete de contenido de Microsoft Azure Enterprise para Power BI, puede importar y analizar rápidamente el consumo de Azure de su inscripción empresarial, averiguar qué departamento, cuenta o suscripción consumió la mayor cantidad de uso o qué servicio se usó más en su empresa o hacer un seguimiento de las tendencias de gastos y uso.

**Vaya al sitio web de Power BI:**

 1. Inicie sesión con una cuenta profesional o educativa válida.
    - La cuenta profesional o educativa puede ser la misma o distinta de la que se usa para acceder a la inscripción mediante Azure EA Portal.
 1. En el panel de servicios, elija:
    - Icono de Microsoft Azure Enterprise.
    - Haga clic en **Conectar**.
 1. En la pantalla "Conectar con Azure Enterprise", elija:
    - Dirección URL del entorno de Azure: [https://ea.azure.com](https://ea.azure.com).
    - Número de meses: elija entre 1 y 36.
    - Número de inscripción: escriba el número de inscripción.
    - Haga clic en **Next**.
 1. Escriba la clave de API en el cuadro Clave de autenticación. Puede obtener la clave de API en Azure EA Portal en la pestaña "Download Usage" (Descargar uso); para ello, haga clic en **API Access Key** (Clave de acceso de la API).
    - Copie y pegue la clave en el cuadro "Clave de cuenta".
    - Los datos tardarán aproximadamente de 5 a 30 minutos en cargarse en Power BI, según el tamaño del conjunto de datos.

Los informes de Power BI están disponibles para clientes directos, asociados e indirectos de Contrato Enterprise que puedan ver la información de facturación.

## <a name="next-steps"></a>Pasos siguientes

- Los administradores del portal del Contrato Enterprise de Azure deben leer el artículo acerca de la [administración del portal del Contrato Enterprise de Azure](ea-portal-administration.md) para obtener información sobre las tareas administrativas comunes.
- Si necesita ayuda para solucionar problemas con el portal del Contrato Enterprise de Azure, consulte [Solución de problemas de acceso al portal del Contrato Enterprise de Azure](ea-portal-troubleshoot.md).
- Para obtener una guía de incorporación de Azure Contrato Enterprise, consulte [Guía de incorporación de Azure Contrato Enterprise](https://ea.azure.com/api/v3Help/v2AzureEAOnboardingGuide).
