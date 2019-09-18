---
title: Introducción al portal del Contrato Enterprise de Azure
description: En este artículo se explica cómo usan los clientes el portal del Contrato Enterprise de Azure.
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 09/09/2019
ms.topic: conceptual
ms.service: billing
manager: boalcsva
ms.openlocfilehash: 1882b283f376a1bb8706132263c83e1a24ec0705
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2019
ms.locfileid: "70900937"
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

![Diagrama de jerarquías simples de Azure EA](./media/billing-ea-portal-get-started/ea-hierarchies.png)

## <a name="enterprise-user-roles"></a>Roles de los usuarios de la empresa

Para administrar los servicios de Azure en su inscripción, hay cuatro roles de usuario administrativo de empresa distintos:

- Administrador de empresa
- Administrador de departamentos
- Propietario de cuenta
- Administrador de servicios

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

Todas las cuentas requieren una cuenta profesional, educativa o de Microsoft única. Para más información acerca de los roles administrativos del portal del Contrato Enterprise de Azure, consulte [Roles administrativos del Contrato Enterprise de Azure en Azure](billing-understand-ea-roles.md).

### <a name="service-administrator"></a>Administrador de servicios

El administrador de servicios tiene permisos para administrar servicios en Azure Portal y asignar a los usuarios el rol de coadministrador.

## <a name="activate-your-enrollment"></a>Activación de la inscripción

Para activar un servicio, el administrador de empresa inicial abre el portal del Contrato Enterprise de Azure en [https://ea.azure.com](https://ea.azure.com) e inicia sesión con la dirección del correo electrónico de invitación.

Si tiene más de una inscripción, elija la que desea activar. De forma predeterminada, solo se muestran las inscripciones activas. Para ver el historial de inscripciones, desactive la opción **Active** (Activo) en la parte superior derecha del portal del Contrato Enterprise de Azure.

En Enrollment (Inscripción), aparece el estado **Active** (Activo).

![Ejemplo en el que se muestra una inscripción activa](./media/billing-ea-portal-get-started/ea-enrollment-status.png)

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

Para más información acerca de los roles administrativos de la empresa, consulte [Roles administrativos del Contrato Enterprise de Azure en Azure](billing-understand-ea-roles.md).

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

![Ejemplo que muestra el cuadro de diálogo Add Department Administrator (Agregar administrador de departamento)](./media/billing-ea-portal-get-started/ea-create-add-department-admin.png)

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

![Ejemplo que muestra la lista de cuentas y la opción + Add Account (+ Agregar cuenta)](./media/billing-ea-portal-get-started/create-ea-add-an-account.png)

Puede agregar otra cuenta haciendo clic en **Add Another Account** (Agregar otra cuenta), o bien puede hacer clic en **Add** (Agregar) en la esquina inferior derecha de la barra de herramientas izquierda.

Para confirmar la propiedad de la cuenta:

1. Inicie sesión en el portal del Contrato Enterprise de Azure.
2. Para confirmar la propiedad de la cuenta, vea su estado. El estado debe cambiar de **Pending** (Pendiente) a **Start/End date** (Fecha de inicio y de finalización). La fecha de inicio y de finalización es la fecha en que el usuario inició sesión por primera vez y la fecha de finalización del contrato.


## <a name="change-account-owner"></a>Cambio del propietario de la cuenta

Los administradores de empresa pueden usar el portal del Contrato Enterprise de Azure para transferir la propiedad de la cuenta de suscripción en una inscripción. La acción mueve todas las suscripciones de una cuenta de usuario de origen a una cuenta de usuario de destino.

Puntos importantes acerca de la transferencia de información de las cuentas de usuario:

- Se admiten las transferencias de una cuenta profesional o educativa a otra cuenta profesional o educativa.
- Se admiten las transferencias de una cuenta Microsoft a una cuenta profesional o educativa.
- No se admiten las transferencias de una cuenta profesional o educativa a una cuenta Microsoft.
- Se admiten las transferencias de una cuenta Microsoft a otra cuenta Microsoft. La cuenta de destino debe ser una cuenta de Azure Commerce válida para ser un destino válido para las transferencias. En las nuevas cuentas, se le pide que cree una cuenta de Azure Commerce al iniciar sesión en el portal del Contrato Enterprise de Azure. En el caso de las cuentas existentes, para que la cuenta sea apta es preciso crear antes una suscripción de Azure.
- Cuando se completa una transferencia de suscripción, Microsoft actualiza el propietario de la cuenta.

Directivas de RBAC:

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

![Imagen que muestra el símbolo Cambiar propietario de cuenta.](./media/billing-ea-portal-get-started/create-ea-create-sub-transfer-account-ownership-of-sub.png)

Para transferir la propiedad de la cuenta para una sola suscripción:

1. En el área de navegación de la izquierda, haga clic en **Manage** (Administrar).
2. Haga clic en la pestaña **Account** (Cuenta) y mantenga el puntero sobre una cuenta.
3. Haga clic en el símbolo de transferir suscripciones, que se encuentra a la derecha. El símbolo se parece a una página.
4. Seleccione una suscripción apta y haga clic en **Next** (Siguiente).
5. Confirme la transferencia y haga clic en **Submit** (Enviar).

![Imagen que muestra el símbolo Transferir suscripciones](./media/billing-ea-portal-get-started/ea-transfer-subscriptions.png)

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

## <a name="transfer-pay-as-you-go-subscription-to-ea-subscription"></a>Transferencia de la suscripción de pago por uso a una suscripción de Contrato Enterprise

Para transferir una suscripción individual con tarifas de pago por uso a una suscripción de Contrato Enterprise, es preciso crear una solicitud de soporte técnico en Azure Portal. Para crear una solicitud de soporte técnico, haga clic en **+ Nueva solicitud de soporte técnico** en el área Ayuda y soporte técnico.


## <a name="view-usage-summary-and-download-reports"></a>Visualización de resumen de uso e informes de las descargas

Los administradores de empresa pueden ver un resumen de sus datos de uso, el compromiso monetario consumido y los cargos asociados con el uso adicional en el portal del Contrato Enterprise de Azure. Los cargos se presentan en el nivel de resumen en todas las cuentas y suscripciones.

Para ver el uso detallado en cuentas específicas

Descargue el informe de detalles de uso. Haga clic en **Reports** (Informes) y, después, haga clic en la pestaña **Download Usage** (Descargar uso). En la lista de informes, haga clic en **Download** (Descargar) en el informe mensual que desee obtener.

El informe no incluye ningún impuesto aplicable. Puede haber una latencia de hasta ocho horas desde el momento en que se realizó el uso al momento en que se refleja en el informe.

Para ver los informes y gráficos de Resumen de uso:

1. En el portal del Contrato Enterprise de Azure, en el área de navegación, haga clic en **Reports** (Informes) y vea la pestaña **Usage Summary** (Resumen de uso).  
  ![](./media/billing-ea-portal-get-started/create-ea-view usage-summary-and-download-reports.png)
2. Seleccione un plazo de compromiso.
3. Alterne entre **M** (mensual) **y** C (personalizado) en la parte superior derecha de la página para ver el **resumen de uso** con las fechas de inicio y finalización personalizadas.  
  ![](./media/billing-ea-portal-get-started/create-ea-view-usage-summary-and-download-reports-custom-view.png)
4. Seleccione un periodo en el gráfico para ver detalles adicionales.
5. El gráfico muestra el uso mes a mes con un desglose del uso realizado, el cargo extra por los servicios, los cargos facturados por separado y cargos por Marketplace.
6. En el mes seleccionado, filtre por departamentos, cuentas y suscripciones debajo del gráfico.
7. Alterne entre **Charge by Services** (Cargo por servicios) y **Charge by Hierarchy** (Cargo por jerarquía).
8. Expanda y contraiga entre **Azure Service** (Servicio de Azure), **Charges Billed Separately** (Cargos facturados por separado) y **Azure Marketplace** para ver los detalles.

Este es un vídeo que muestra cómo ver el uso:

[Vídeo de uso del portal del Contrato Enterprise de Azure](https://www.youtube.com/watch?v=Cv2IZ9QCn9E)

>[!VIDEO https://www.youtube.com/embed/Cv2IZ9QCn9E]

### <a name="download-csv-reports"></a>Descarga de informes CSV

Los administradores de empresa usan la página de descarga de informes mensuales para descargar varios informes como archivos CSV. Entre ellos se incluyen los siguientes:

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

![Ejemplo que muestra la página Descargar uso](./media/billing-ea-portal-get-started/create-ea-download-csv-reports.png)

Este es un vídeo que muestra cómo descargar información de uso:

[Vídeo de uso del portal del Contrato Enterprise de Azure](https://www.youtube.com/watch?v=eY797htT1qg)

>[!VIDEO https://www.youtube.com/embed/eY797htT1qg]

## <a name="schedule-an-onboarding-call"></a>Programación de una llamada de incorporación

Si desea programar una sesión de incorporación de clientes en persona, cree una solicitud de soporte técnico en la sección de [soporte técnico del portal del Contrato Enterprise de Azure](https://support.microsoft.com/supportrequestform/e114582c-4e51-af46-10b1-1f0cc141e133). Seleccione **Incorporación** en **Categoría del problema**.

## <a name="next-steps"></a>Pasos siguientes
- Los administradores del portal del Contrato Enterprise de Azure deben leer el artículo acerca de la [administración del portal del Contrato Enterprise de Azure](billing-ea-portal-administration.md) para obtener información sobre las tareas administrativas comunes.
- Si necesita ayuda para solucionar problemas del portal del Contrato Enterprise de Azure, consulte el artículo acerca de la [solución de problemas de acceso al portal del Contrato Enterprise de Azure](billing-ea-portal-troubleshoot.md).
