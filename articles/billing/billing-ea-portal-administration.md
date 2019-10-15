---
title: Administración del portal del Contrato Enterprise de Azure
description: En este artículo se explican las tareas comunes que un administrador lleva a cabo en el portal del Contrato Enterprise de Azure.
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 10/07/2019
ms.topic: conceptual
ms.service: billing
manager: boalcsva
ms.openlocfilehash: 21e9d4af783ed5d9eb3ace1c8b5189163b89f8b0
ms.sourcegitcommit: f9e81b39693206b824e40d7657d0466246aadd6e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2019
ms.locfileid: "72035210"
---
# <a name="azure-ea-portal-administration"></a>Administración del portal del Contrato Enterprise de Azure

En este artículo se explican las tareas comunes que un administrador lleva a cabo en el portal del Contrato Enterprise de Azure (https://ea.azure.com). El portal del Contrato Enterprise de Azure es un portal de administración en línea que ayuda a los clientes a administrar el costo de los servicios del Contrato Enterprise de Azure. Para obtener información introductoria acerca del portal del Contrato Enterprise de Azure, consulte el artículo [Introducción al portal del Contrato Enterprise de Azure](billing-ea-portal-get-started.md).

## <a name="add-a-new-enterprise-administrator"></a>Adición de un nuevo administrador de empresa

Los administradores de empresa tienen los máximos privilegios al administrar una inscripción del Contrato Enterprise de Azure. Se creó el administrador del Contrato Enterprise de Azure inicial cuando se configuró el acuerdo del Contrato Enterprise. Sin embargo, puede agregar o quitar nuevos administradores en cualquier momento. Solo los administradores existentes agregan nuevos administradores. Para más información sobre cómo agregar administradores de empresa adicionales, consulte [Creación de otro administrador de empresa](billing-ea-portal-get-started.md#create-another-enterprise-admin). Para más información acerca de los roles del perfil de facturación, consulte [Tareas y roles del perfil de facturación](billing-understand-mca-roles.md#billing-profile-roles-and-tasks).

## <a name="update-user-state-from-pending-to-active"></a>Actualización del estado de usuario de pendiente a activo

Cuando se agregan nuevos propietarios de la cuenta a una inscripción del Contrato Enterprise de Azure por primera vez, su estado aparece como _pendiente_. Cuando un nuevo propietario de la cuenta recibe el correo electrónico de bienvenida de activación, puede iniciar sesión para activar su cuenta. Al activar su cuenta, el estado de la cuenta se actualiza de _pendiente_ a _activo_. Es posible que se pida a los nuevos usuarios que escriban su nombre y apellidos para crear una cuenta comercial. Si es así, deben agregar la información necesaria para continuar y, después, la cuenta se activará.

## <a name="add-a-department-admin"></a>Adición de un administrador de departamento

Cuando un administrador de Contrato Enterprise de Azure crea un departamento, el administrador puede agregar administradores de departamento y asociar cada uno de ellos a un departamento. Un administrador de departamento puede crear nuevas cuentas. Se necesitan nuevas cuentas para que se creen las suscripciones del Contrato Enterprise de Azure.

Para más información sobre cómo agregar un administrador de departamento, consulte [Creación de un administrador de departamento del Contrato Enterprise de Azure](billing-ea-portal-get-started.md#add-a-department-admin).

## <a name="enterprise-user-roles"></a>Roles de los usuarios de empresa

El portal del Contrato Enterprise de Azure le ayuda a administrar los costos y el uso del Contrato Enterprise de Azure. Hay tres roles principales en el portal del Contrato Enterprise de Azure:

- Administrador del Contrato Enterprise
- Administrador de departamentos
- Propietario de cuenta

Cada rol tiene un nivel de acceso y autoridad diferente.

Para más información sobre los roles de usuario, consulte Roles de usuario de empresa.

## <a name="add-an-azure-ea-account"></a>Adición de una cuenta del Contrato Enterprise de Azure

La cuenta del Contrato Enterprise de Azure es una unidad organizativa del portal del Contrato Enterprise de Azure que se usa para administrar las suscripciones y también se usa para la creación de informes. Para acceder a los servicios de Azure y usarlos, debe crear una cuenta o tener una ya creada.

Para más información sobre las cuentas de Azure, consulte la sección sobre cómo agregar una cuenta.

## <a name="transfer-an-enterprise-account-to-a-new-enrollment"></a>Transferencia de una cuenta empresarial a una nueva inscripción

Tenga en cuenta los puntos siguientes cuando transfiera una cuenta de empresa a una nueva inscripción:

- Solo se transfieren las cuentas especificadas en la solicitud. Si se eligen todas las cuentas, se transfieren todas.
- La inscripción de origen conserva su estado como activo o extendido. Puede seguir usando la inscripción hasta que expire.

### <a name="effective-transfer-date"></a>Fecha de transferencia efectiva

La fecha de transferencia efectiva puede ser la fecha de inicio o una fecha posterior de la inscripción a la que desea realizar la transferencia. La inscripción a la que está transfiriendo es la _inscripción de destino_. Después de la transferencia de la cuenta, toda la información de uso de la cuenta antes de la fecha de transferencia efectiva permanece en la inscripción desde la que se está realizando la transferencia. La inscripción desde la que está transfiriendo es la _inscripción de origen_.  El uso de la inscripción de origen se cobra en el compromiso monetario o como uso por encima del límite. El uso que se produce después de la fecha de transferencia efectiva se transfiere a la nueva inscripción y se cobra en consecuencia.

Puede atrasar la transferencia de una cuenta a la fecha de inicio de la inscripción de destino. También puede atrasarla a la fecha de inicio efectiva de la inscripción de origen.

### <a name="monetary-commitment"></a>Compromiso monetario

El compromiso monetario no se transfiere entre inscripciones. Los saldos de compromiso monetario se asocian contractualmente a la inscripción en la que se solicitaron. El compromiso monetario no se transfiere como parte del proceso de transferencia de la cuenta o inscripción.

### <a name="services-affected"></a>Servicios afectados

No hay ningún tiempo de inactividad durante la transferencia de la cuenta. Se puede completar el mismo día de la solicitud si se proporciona toda la información necesaria.

### <a name="prerequisites"></a>Requisitos previos

Al solicitar una transferencia de cuentas, proporcione la siguiente información:


- Nombre de cuenta e identificador del propietario de la cuenta que se va a transferir
- Para la inscripción de origen, el número de inscripción y la cuenta que se va a transferir
- Para la inscripción de destino, el número de inscripción al que se va a transferir
- Para la fecha de vigencia de la transferencia de cuentas, puede ser la fecha de inicio de la inscripción de destino o después de ella.

Otros puntos que hay que tener en cuenta antes de transferir una cuenta:

- Es necesaria la aprobación de un administrador del Contrato Enterprise para la inscripción de origen y destino.
  - En algunos casos, es posible que Microsoft solicite una aprobación adicional por parte de un administrador del Contrato Enterprise de la inscripción de origen.
- Si la transferencia de una cuenta no cumple los requisitos, considere la posibilidad de realizar una transferencia de inscripción.
- La transferencia de la cuenta transfiere todos los servicios y suscripciones relacionados con las cuentas específicas.
- Una vez completada la operación, la cuenta transferida aparece como inactiva en la inscripción de origen.
- Las transferencias de cuentas se pueden retroceder a cualquier fecha dentro de la fecha de inicio de la inscripción de destino.
- La cuenta muestra la fecha de finalización correspondiente a la fecha de transferencia efectiva en la inscripción de origen y la fecha de inicio en la inscripción de destino.
- Cualquier uso que se produzca en la cuenta antes de la fecha de transferencia efectiva permanece bajo la inscripción de origen.


## <a name="transfer-enterprise-enrollment-to-a-new-one"></a>Transferencia de la inscripción empresarial a una nueva

Cuando se solicita la transferencia de una inscripción empresarial completa a una inscripción, se producen las siguientes acciones:

- Se transfieren todos los servicios, suscripciones, cuentas, departamentos y toda la estructura de inscripción de Azure, incluidos todos los administradores de departamento del Contrato Enterprise.
- El estado de inscripción se establece en _Transferido_. La inscripción transferida solo está disponible con fines de informe de historial de uso.
- No se pueden agregar roles o suscripciones a una inscripción transferida. El estado transferido evita el uso adicional en la inscripción.
- Se pierde cualquier saldo de compromiso monetario restante del contrato, incluidos los términos futuros.

### <a name="effective-transfer-date"></a>Fecha de transferencia efectiva

La fecha de transferencia efectiva puede ser la fecha de inicio de la inscripción o una fecha posterior a la que desea realizar la transferencia a la suscripción de destino.

El uso de la inscripción de origen se cobra en el compromiso monetario o como uso por encima del límite. El uso que se produce después de la fecha de transferencia efectiva se transfiere a la nueva inscripción y se cobra en consecuencia.

### <a name="effective-transfer-date-in-the-past"></a>Fecha de transferencia efectiva en el pasado

Puede atrasar la transferencia de una cuenta a la fecha de inicio de la inscripción de destino. También puede atrasarla a la fecha de inicio efectiva de la inscripción de origen.

### <a name="monetary-commitment"></a>Compromiso monetario

El compromiso monetario no se transfiere entre inscripciones. Los saldos de compromiso monetario se asocian contractualmente a la inscripción en la que se solicitaron. El compromiso monetario no se transfiere como parte del proceso de transferencia de la cuenta o inscripción.

### <a name="services-affected"></a>Servicios afectados

No hay ningún tiempo de inactividad durante la transferencia de la cuenta. Se puede completar el mismo día de la solicitud si se proporciona toda la información necesaria.

### <a name="prerequisites"></a>Requisitos previos

Al solicitar una transferencia de inscripciones, proporcione la siguiente información:

- Para la inscripción de origen, el número de inscripción y la cuenta que se va a transferir.
- Para la inscripción de destino, el número de inscripción al que se va a transferir.
- Para la fecha de vigencia de la transferencia de inscripción, puede ser la fecha de inicio de la inscripción de destino o después de ella. La fecha elegida no puede afectar al uso de ninguna factura por uso por encima del límite ya emitida.

Otros puntos que hay que tener en cuenta antes de una transferencia de inscripciones:

- Es necesaria la aprobación de un administrador del Contrato Enterprise para la inscripción de origen y destino.
  - En algunos casos, es posible que Microsoft solicite una aprobación adicional por parte de un administrador del Contrato Enterprise de la inscripción de origen.
- Si una transferencia de inscripción no cumple sus requisitos, considere la posibilidad de transferir una cuenta.
- Solo se transfieren las cuentas que especifique. Puede solicitar la transferencia de todas las cuentas.
- La inscripción de origen conserva su estado como activo o extendido. Puede seguir usando la inscripción hasta que expire.

## <a name="change-account-owner"></a>Cambiar el propietario de la cuenta

El portal del Contrato Enterprise de Azure puede transferir suscripciones de un propietario de cuenta a otro. Para más información, consulte [Cambio del propietario de la cuenta](billing-ea-portal-get-started.md#change-account-owner).

## <a name="subscription-transfer-effects"></a>Efectos de la transferencia de la suscripción

Si se transfiere una suscripción de Azure a una cuenta en el mismo inquilino de Azure Active Directory, todos los usuarios, grupos y entidades de servicio que tenían [control de acceso basado en rol (RBAC)](../role-based-access-control/overview.md) para administrar recursos conservarán el acceso.

Para ver los usuarios con acceso RBAC a la suscripción:

1. En Azure Portal, abra **Suscripciones**.
2. Seleccione la suscripción que desee ver y, después, seleccione **Control de acceso (IAM)** .
3. Seleccione **Asignaciones de roles**. La página de asignación de roles enumera todos los usuarios que tienen acceso RBAC a la suscripción.

Si transfiere la suscripción a una cuenta en otro inquilino de Azure AD, todos los usuarios, grupos y entidades de servicio que tenían [RBAC](../role-based-access-control/overview.md) para administrar recursos _perderán_ el acceso. Aunque el acceso RBAC no está presente, el acceso a la suscripción puede estar disponible a través de mecanismos de seguridad, entre los que se incluyen:

- Certificados de administración que conceden al usuario derechos administrativos a los recursos de la suscripción. Para más información, consulte [Create and Upload a Management Certificate for Azure](../cloud-services/cloud-services-certs-create.md) (Creación y actualización de un certificado de administración para Azure).
- Claves de acceso para servicios como Almacenamiento. Para más información, vea [Introducción a las cuentas de Azure Storage](../storage/common/storage-account-overview.md).
- Credenciales de acceso remoto para servicios como Azure Virtual Machines.

El destinatario debe restringir el acceso a los recursos de Azure y considerar la actualización de todos los secretos asociados al servicio. La mayoría de los recursos se pueden actualizar mediante el uso de los siguientes pasos:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).
2. En el menú Concentrador, seleccione **Todos los recursos**.
3. Seleccione el recurso.
4. En la página de recursos, haga clic en **Settings** (Configuración) para ver y actualizar los secretos existentes.



## <a name="close-an-azure-enterprise-enrollment"></a>Cierre de una inscripción de Azure Enterprise

Si desea cerrar la inscripción del Contrato Enterprise de Azure, puede:

- Cancelar todas las suscripciones asociadas a la instancia del Contrato Enterprise de Azure en Azure Portal.
- Ponerse en contacto con el asociado o asesor de software de clientes o pedirle que cierre el Contrato Enterprise de Azure.

## <a name="update-notification-settings"></a>Actualización de la configuración de notificación

Los administradores de empresa se inscriben automáticamente para recibir notificaciones de uso asociadas a la inscripción. Cada administrador de empresa puede cambiar el intervalo de las notificaciones individuales o puede desactivarlas completamente.

Los contactos de notificación se muestran en el portal del Contrato Enterprise de Azure en la sección **Notification Contact** (Contacto para notificaciones). La administración de los contactos de notificaciones garantiza que las personas adecuadas de la organización obtienen notificaciones del Contrato Enterprise de Azure.

Para ver la configuración actual de notificaciones:

1. En el portal del Contrato Enterprise de Azure, vaya a **Manage** > **Notification Contact** (Administrar > Contacto para notificaciones).
2. Dirección de correo electrónico: la dirección de correo electrónico asociada a la cuenta de Microsoft del administrador de empresa, la cuenta profesional o educativa que recibe las notificaciones.
3. Unbilled Balance Notification Frequency (Frecuencia de notificación de saldo sin facturar): frecuencia con la que las notificaciones están configuradas para enviarse a cada administrador de empresa individual.

Para agregar un contacto:

1. Haga clic en **+Add Contact** (+Agregar contacto).
2. Especifique la dirección de correo electrónico y confírmela.
3. Haga clic en **Save**(Guardar).

El nuevo contacto de notificación se muestra en la sección **Notification Contact** (Contacto para notificaciones). Para cambiar la frecuencia de notificación, seleccione el contacto de notificación y haga clic en el símbolo de lápiz situado a la derecha de la fila seleccionada. Establezca la frecuencia en **diaria**, **semanal**, **mensual** o **ninguna**.

Puede suprimir _que se aproxima a la fecha de finalización del período de cobertura_ y _que se aproxima a la fecha de deshabilitación y desaprovisionamiento_ de las notificaciones del ciclo de vida. Al deshabilitar las notificaciones del ciclo de vida, se suprimen las notificaciones sobre el período de cobertura y la fecha de finalización del contrato.

## <a name="manage-partner-notifications"></a>Administración de notificaciones de asociados

Los administradores de asociados pueden administrar la frecuencia con la que reciben las notificaciones de uso para las inscripciones. Reciben automáticamente notificaciones semanales del saldo no facturado. Pueden cambiar la frecuencia de las notificaciones individuales a mensual, semanal, diaria o desactivarlas completamente.

Si un usuario no recibe una notificación, compruebe que la configuración de notificaciones del usuario es correcta con los pasos siguientes.

1. Inicie sesión en el portal del Contrato Enterprise de Azure como administrador de usuarios.
2. Haga clic en **Manage** (Administrar) y, a continuación, en la pestaña **Partner** (Asociado).
3. Vea la lista de administradores en la sección **Administrator** (Administrador).
4. Para modificar las preferencias de notificación, mantenga el cursor sobre el administrador adecuado y haga clic en el símbolo de lápiz.
5. Configure la frecuencia de notificación y las notificaciones del ciclo de vida según sea necesario.
6. Agregue un contacto, si es necesario, y haga clic en **Add** (Agregar).
7. Haga clic en **Save**(Guardar).

![Ejemplo que muestra el cuadro Agregar contacto ](./media/billing-ea-portal-administration/create-ea-manage-partner-notification.png)

## <a name="azure-sponsorship-offer"></a>Oferta Patrocinio de Azure
La oferta Patrocinio de Azure es una cuenta de Microsoft Azure patrocinada limitada. Está disponible solo mediante invitación por correo electrónico a clientes limitados seleccionados por Microsoft. Si es apto para la oferta Patrocinio de Microsoft Azure, recibirá una invitación por correo electrónico a su identificador de cuenta.
Para más información, consulte:

- Introducción de la oferta de patrocinio: https://azure.microsoft.com/en-us/offers/ms-azr-0143p/
- Portal de saldo de patrocinio: https://www.microsoftazuresponsorships.com/balance  
- Preguntas más frecuentes sobre patrocinio: https://azuresponsorships-staging.azurewebsites.net/faq
- Solicitud de soporte técnico para la activación de patrocinio: http://aka.ms/azrsponsorship

## <a name="next-steps"></a>Pasos siguientes
- Obtenga información acerca de cómo las [reservas de máquinas virtuales](billing-ea-portal-vm-reservations.md) pueden ayudarle a ahorrar dinero.
- Si necesita ayuda para solucionar problemas del portal del Contrato Enterprise de Azure, consulte [Solución de problemas de acceso al portal del Contrato Enterprise de Azure](billing-ea-portal-troubleshoot.md).
