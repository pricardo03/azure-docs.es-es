---
title: Facturas de inscripciones de Azure Enterprise
description: En este artículo se explica cómo administrar y actuar en su factura de Azure Enterprise.
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 01/21/2020
ms.topic: conceptual
ms.service: cost-management-billing
manager: boalcsva
ms.openlocfilehash: 7cb2966c94485d0a05febf3085ab367fcba97434
ms.sourcegitcommit: 38b11501526a7997cfe1c7980d57e772b1f3169b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2020
ms.locfileid: "76514364"
---
# <a name="azure-enterprise-enrollment-invoices"></a>Facturas de inscripciones de Azure Enterprise

En este artículo se explica cómo administrar y actuar en su factura de Azure Enterprise. La factura es únicamente una representación, por lo que debe revisarla para comprobar que sea precisa. También debe familiarizarse con otras tareas que podrían ser necesarias para administrar la factura.

## <a name="change-a-po-number-for-an-overage-invoice"></a>Cambio de un número de pedido de compra para una factura de uso por encima del límite

El portal del Contrato Enterprise de Azure genera automáticamente un número de pedido de compra predeterminado, a menos que el administrador del Contrato Enterprise establezca uno antes de la fecha de la factura. Un administrador del Contrato Enterprise puede actualizar el número de pedido de compra hasta siete días después de recibir un correo electrónico de notificación de factura automática.

### <a name="to-update-the-azure-services-purchase-order-number"></a>Para actualizar el número de pedido de compra de los servicios de Azure:

1. En Azure EA Portal, haga clic en **Report** (Informe) y, a continuación, en **Usage Summary** (Resumen de uso).
1. Seleccione **Edit PO Numbers** (Editar números de pedido de compra) en la esquina superior derecha.
1. Seleccione el botón de radio **Azure Services** (Servicios de Azure).
1. Seleccione un **período de facturación** en el menú desplegable de intervalos de fechas. Los números de pedido de compra pueden editarse hasta siete días después de la notificación de la factura o hasta que se pague la factura, lo que suceda primero.
1. Escriba el nuevo número de pedido de compra en el campo **PO Number** (Número de pedido de compra).
1. Haga clic en **Save** (Guardar) para enviar el cambio.

### <a name="to-update-the-marketplace-purchase-order-number"></a>Para actualizar el número de pedido de compra de Marketplace:

1. En Azure EA Portal, haga clic en **Report** (Informe) y, a continuación, en **Usage Summary** (Resumen de uso).
1. Seleccione **Edit PO Numbers** (Editar números de pedido de compra) en la esquina superior derecha.
1. Seleccione el botón de radio **Marketplace**.
1. Seleccione un **período de facturación** en el menú desplegable de intervalos de fechas. Los números de pedido de compra pueden editarse hasta siete días después de la notificación de la factura o hasta que se pague la factura, lo que suceda primero.
1. Escriba el nuevo número de pedido de compra en el campo **PO Number** (Número de pedido de compra).
1. Haga clic en **Save** (Guardar) para enviar el cambio.

## <a name="cadence-of-azure-ea-billing"></a>Cadencia de facturación de EA de Azure

### <a name="billing-intervals"></a>Intervalos de facturación

Microsoft facturará anualmente en la fecha de entrada en vigor de la inscripción cualquier compra de compromiso de los servicios de Microsoft Azure y a período vencido cualquier uso que exceda los importes del compromiso. Las tarifas de compromiso se ofrecen a razón de una tarifa mensual y se facturan anualmente por adelantado. Las tarifas de uso por encima del límite se calculan cada mes y se facturan a período vencido al final del período de facturación.

En función de cómo decida realizar las compras de compromiso, el compromiso anual coincidirá con la fecha del aniversario de la inscripción o con la fecha de entrada en vigor de la suscripción de enmienda de un año.

Recibirá su factura de uso por encima del límite en función de la fecha de inicio de su inscripción y de su configuración.

**Inscripciones directas con una fecha de inicio anterior al 1 de mayo de 2018**: Los clientes directos de Enterprise Azure (EA) tienen configurado un ciclo de facturación anual para los servicios de Azure (excepto los servicios de Marketplace). El ciclo de facturación se basa en la fecha de aniversario. La fecha de aniversario es la fecha en la que el contrato entró en vigor. Para recibir la primera factura de uso por encima del límite de un servicio de Azure, debe superar el 150 % del umbral de compromiso monetario.  Una vez que el consumo de servicio total supere el 150 % del umbral de compromiso monetario, se convertirá automáticamente en un ciclo de facturación trimestral basado en la fecha de aniversario.  Si no supera el 150% del umbral de compromiso monetario, la inscripción permanecerá en un ciclo de facturación anual y la factura de uso por encima del límite se recibirá al final del año de compromiso.

**Inscripciones directas con una fecha de inicio posterior al 1 de mayo de 2018**: Las facturas de consumo y cargos de Azure del cliente facturados por separado tendrán se realizarán en un ciclo de facturación mensual.  Los cargos que no estén incluidos en el compromiso monetario de Azure se deberán abonar como un pago por uso por encima del límite.  

**Inscripciones indirectas con una inscripción iniciada antes del 1 de mayo de 2018**: Los clientes indirectos de Enterprise Azure (EA) tienen configurado un ciclo de facturación trimestral.  El partner de canal (CP) facturará al cliente directamente.  

**Inscripciones indirectas con una fecha de inicio posterior al 1 de mayo de 2018**: Cualquier contrato indirecto con una fecha de inicio posterior o igual al 1 de mayo de 2018 se facturará mensualmente.  

### <a name="increasing-commitment"></a>Aumento del compromiso

El compromiso se puede aumentar en cualquier momento y se le facturará el número de meses restantes durante el período de compromiso de ese año. Por ejemplo, si realiza una suscripción de enmienda de un año y aumenta su compromiso durante el sexto mes, se le facturarán los seis meses restantes de ese período. Las cantidades de compromiso se actualizarán durante los últimos seis meses del período de compromiso para determinar los cargos por uso por encima del límite.

### <a name="overage"></a>Superávit

Por uso por encima del límite, se le facturará el uso o las reservas que superen su compromiso durante el período de facturación. Para ver un desglose del cálculo de las cantidades de uso por encima del límite de elementos individuales, consulte el informe de resumen de uso o póngase en contacto con su partner de canal.

Para cada elemento de la factura, verá los cargos totales (cantidad extendida), la cantidad de compromiso usada para cubrir los cargos (uso del compromiso) y la cantidad de cargos que superan el compromiso (importe neto).  Los impuestos aplicables se calculan solo sobre el importe neto que supera el compromiso.

La facturación de uso por encima del límite está automatizada.  La temporización de las notificaciones y las facturas depende de la fecha de finalización del período de facturación del cliente.  La notificación de uso por encima del límite se envía normalmente siete días después de la fecha de finalización de facturación del cliente, período durante el cual los clientes pueden entrar en el portal, revisar sus cargos y actualizar los números de pedidos de compra generados por el sistema (que también se pueden actualizar en cualquier momento antes de que se emita la notificación de uso por encima del límite).  Las facturas de uso por encima del límite se enviarán entre 7 y 9 días después.

### <a name="azure-marketplace"></a>Azure Marketplace

A partir del ciclo de facturación de abril de 2019, los clientes comenzarán a recibir una sola factura de Azure, ya que hemos combinado todos los cargos de Azure y Azure Marketplace en una sola factura en lugar de dos facturas independientes. (Este cambio no afecta a los clientes de Australia, Japón o Singapur). Durante la transición a una factura consolidada, recibirá una factura de Marketplace parcial. Esta factura final independiente mostrará los cargos de Marketplace antes de la fecha de la actualización de la facturación. Los cargos de Marketplace después de esa fecha aparecerán en la factura de Azure. Después del período de transición, verá todos los cargos de Azure y Marketplace en la factura consolidada.  

### <a name="purchase-order-numbers"></a>Números de pedido de compra

El valor predeterminado de un pedido de compra es un número generado por el sistema. Para actualizar el número de pedido de compra, los usuarios deben iniciar sesión en Enterprise Portal como administrador de empresa e ir a la sección Reports (Informes). En la esquina superior derecha de la ventana, hay un cuadro de número de pedido de compra que permite al administrador de empresa editar dicho número haciendo clic en el icono de lápiz.

## <a name="adjust-billing-frequency"></a>Ajuste de la frecuencia de facturación

La frecuencia de facturación de un cliente es anual, trimestral o mensual. El período de facturación se determina cuando un cliente firma su contrato. La facturación mensual es el intervalo de facturación más pequeño.

Es necesaria la aprobación del administrador de empresa para cambiar un período de facturación de anual a trimestral para las inscripciones directas. Es necesaria la aprobación de un administrador de asociados para las inscripciones indirectas. El cambio se hace efectivo al final del trimestre de facturación actual.

Para cambiar un período de facturación anual o trimestral a mensualmente, se requiere una enmienda del contrato.  Cualquier cambio en el período de facturación de inscripción existente requiere la aprobación de un administrador de empresa o del usuario identificado como _contacto de facturación_ en el contrato. Puede enviar la aprobación en el [soporte técnico del portal del Contrato Enterprise de Azure](https://support.microsoft.com/supportrequestform/cf791efa-485b-95a3-6fad-3daf9cd4027c) y, después, seleccionar la categoría de problema de **facturación**.  El cambio se hace efectivo al final del trimestre de facturación actual.

Si se firma una enmienda M503, puede pasar cualquier contrato de cualquier frecuencia a la facturación mensual. 

## <a name="credits-and-adjustments"></a>Créditos y ajustes

Todos los créditos o ajustes que se aplican a la inscripción están visibles en [https://ea.azure.com](https://ea.azure.com) en la sección **Informes**.

Para ver los créditos, siga los pasos siguientes:

1. Seleccione la sección **Reports** (Informes).
1. Haga clic en **Usage Summary** (Resumen de uso).
1. En la esquina superior derecha, cambie de la vista _M_ a la _C_.
1. Amplíe el campo de ajustes de la tabla de compromisos de servicio de Azure.
1. En esta línea verá los créditos aplicados a su inscripción y una breve explicación, por ejemplo: Crédito de Acuerdo de Nivel de Servicio.

## <a name="request-an-invoice-copy"></a>Solicitud de una copia de la factura

Para solicitar una copia de la factura, póngase en contacto con su asociado.

## <a name="overage-offset-by-customers"></a>Desplazamiento del uso por encima del límite por los clientes

Si el cliente tiene usos por encima del límite que desea usar con compromiso monetario, se deben cumplir los siguientes criterios:

- El cliente debe tener cargos por uso por encima del límite que se han incurrido pero que no se han pagado y están en el plazo de un año de la fecha de finalización del servicio facturado.
- El importe de compromiso monetario disponible debe cubrir el número total de cargos incurridos, incluidas todas las facturas de Azure no pagadas.
- El término de facturación que se solicita completar debe cerrarse completamente. La facturación se cierra por completo después del quinto día de cada mes.
- El período de facturación que se solicita para el desplazamiento debe estar totalmente cerrado.
- El descuento de ACD se basa en el nuevo compromiso real menos los fondos planeados para el consumo anterior. Este requisito solo se aplica a los cargos incurridos por uso por encima del límite. Esto solo funciona para los servicios que consumen el compromiso monetario, por lo que no puede cubrir los cargos de Marketplace. Los cargos de Marketplace se facturan por separado.
- Si un cliente desea completar un desplazamiento del uso por encima del límite, puede abrir una solicitud de soporte técnico. O bien, el equipo de cuentas puede abrir la solicitud de soporte técnico. Para completar el proceso, se requiere la aprobación por correo electrónico del administrador del Contrato Enterprise del cliente o el contacto de facturación.

## <a name="view-price-sheet-information"></a>Visualización de la información de la hoja de precios

Los administradores de empresa pueden ver la lista de precios asociada a su inscripción para los servicios de Azure.

Para ver la hoja de precios actual:

1. En el portal del Contrato Enterprise de Azure, haga clic en **Reports** (Informes) y después haga clic en **Price Sheet** (Hoja de precios).
2. Vea la hoja de precios o haga clic en **Descargar**.

![Ejemplo que muestra la información de la hoja de precios](./media/ea-portal-enrollment-invoices/ea-create-view-price-sheet-information.png)

Para descargar una lista de precios histórica:

1. En el portal del Contrato Enterprise de Azure, haga clic en **Reports** (Informes) y luego en **Download usage** (Descargar uso).
2. Descargue la hoja de precios.

![Ejemplo que muestra dónde descargar una hoja de precios anterior](./media/ea-portal-enrollment-invoices/create-ea-view-price-sheet-download-historical-price-list.png)

Si tiene alguna pregunta sobre por qué hay una discrepancia en los precios, algunas de las razones incluyen:

Los precios pueden haber cambiado entre la inscripción anterior y la nueva inscripción. Los cambios de precio se producen porque el precio es contractual para la inscripción específica desde la fecha de inicio hasta la fecha de finalización de un contrato. Cuando se transfiere una inscripción a una nueva, sigue los precios de un nuevo contrato. Los precios se definen en la hoja de precios del cliente. Por lo tanto, es posible que los precios sean más altos en la nueva inscripción.

Si una inscripción entra en un período extendido, los precios también cambian. Los precios cambian a las tarifas de pago por uso.

## <a name="request-detailed-usage-information"></a>Solicitud de información de uso detallada

Los administradores de empresa pueden ver un resumen de los datos de uso, el compromiso monetario consumido y los cargos asociados con el uso adicional en el portal del Contrato Enterprise de Azure. Los cargos se presentan en el nivel de resumen en todas las cuentas y suscripciones.

Para ver el uso detallado en cuentas específicas, puede descargar el informe de detalles de uso yendo a **Informes** > **Descargar uso**. El informe no incluye ningún impuesto aplicable. Puede haber una latencia de hasta ocho horas desde el momento en que se incurrió en el uso hasta el momento en que se refleja en el informe.

En el caso de inscripciones indirectas, el asociado debe habilitar la función de incremento antes de que pueda ver cualquier información relacionada con el costo.

## <a name="reports"></a>Informes

Los administradores de empresa pueden ver un resumen de sus datos de uso, el compromiso monetario consumido y los cargos asociados con el uso adicional en Enterprise Portal. Los cargos se presentarán en el nivel de resumen en todas las cuentas y suscripciones.

**Informes de EA**

- Resumen de uso y gráficos
- Informe de uso de servicio
- Informe de saldo y cargos
- Informe de detalles de uso
- Informe de cargos de Marketplace
- Hoja de precios
- Descarga de informes avanzada
- Formato de informe CSV

**Para ver los informes y gráficos de resumen de uso**

1. En Azure EA Portal, en el panel de navegación izquierdo, haga clic en **Reports** (Informes) y vea la pestaña **Usage Summary** (Resumen de uso).
1. Seleccione el período de compromiso deseado en el menú desplegable de intervalos de fechas en la parte superior izquierda.
1. Seleccione el período o mes deseado en el gráfico para ver detalles adicionales.
1. El gráfico muestra el uso mes a mes con un desglose del uso realizado, el cargo extra por los servicios, los cargos facturados por separado y cargos por Marketplace.
1. En el mes seleccionado, filtre por departamentos, cuentas y suscripciones debajo del gráfico.
1. Cambie entre los desgloses Charge by Services (Cargo por servicios) y Charge by Hierarchy (Cargo por jerarquía).
1. Vea los servicios de Azure, los cargos facturados por separado y los cargos de Azure Marketplace en detalle.

## <a name="service-usage-report"></a>Informe de uso de servicio

La página de informe de uso de servicio permite a los administradores de empresa ver un resumen de sus datos de uso de servicio. Aunque el uso se presentará en el nivel de resumen de todas las cuentas y suscripciones, también puede filtrar el informe por cuentas o suscripciones para ver el uso detallado.

Tenga en cuenta que también puede haber una latencia de hasta cinco días entre la fecha en que se realizó el uso y el momento en que el uso se muestra en este informe.

### <a name="to-view-the-report"></a>Para ver el informe:

1. Inicie sesión en Enterprise Portal.
1. En el panel de navegación izquierdo, haga clic en **Reports** (Informes).
1. Haga clic en la pestaña **Usage Summary** (Resumen de uso).
1. Haga clic en el intervalo de fechas que desee.
1. Elija qué cuentas o suscripciones desea ver.
1. Cambie la vista de Charge by Services (Cargo por servicios) o Charge by Hierarchy (Cargo por jerarquía) para mostrar distintos desgloses.
1. Vea los detalles, incluidos el nombre del servicio, la unidad de medida, las unidades consumidas, la tasa efectiva y el costo extendido.

## <a name="download-csv-reports"></a>Descarga de informes CSV

La página de descarga de informes mensuales permite a los administradores de empresa descargar varios informes como archivos. csv, incluido un informe de saldo y cargos, un informe de detalles de uso, un informe de cargos de Marketplace y una hoja de precios.

## <a name="to-download-reports"></a>Para descargar informes:

1. En Azure EA Portal, haga clic en **Report** (Informe).
1. Seleccione **Usage Download** (Descarga de uso) en la cinta de opciones superior.
1. Seleccione **Download** (Descargar) junto al informe del mes correspondiente.



## <a name="csv-report-formatting"></a>Formato de informe CSV

Los clientes que ven los informes CSV de Azure EA Portal en euros pueden tener problemas de formato relacionados con las comas y los puntos.

Por ejemplo, es posible que vea lo siguiente:

| **ServiceResource** | **ResourceQtyConsumed** | **ResourceRate** | **ExtendedCost** |
| --- | --- | --- | --- |
| Horas | 24 | 0,0535960591133005 | 12,863,054,187,192,100,000,000 |

Debería ver lo siguiente:

| ServiceResource | ResourceQtyConsumed | ResourceRate | ExtendedCost |
| --- | --- | --- | --- |
| Horas | 24 | 0,0535960591133005 | 1,2863054187192120000000 |

### <a name="root-cause"></a>Causa principal

Excel importa todos los campos como texto "general" y supone que un número está separado con el estándar matemático: "1,000.00".  Por lo tanto, una moneda europea que use un punto (.) como separador de miles y una coma (,) como separador de decimales —"1.000, 00"— se mostrará incorrectamente. Esto puede variar en función de la configuración regional del idioma.

### <a name="when-importing-the-csv-use-the-following-steps"></a>Al importar el archivo CSV, siga estos pasos:

1.    Abra Excel y vaya a Archivo > Abrir.
1.    Aparecerá el Asistente para importar texto.
1.    En Tipo de los datos originales, elija _Delimitados_.  El valor predeterminado es _De ancho fijo_.
1.    Presione **Siguiente**.
1.    En Delimitadores, active la casilla de Coma. El valor predeterminado es "Tabulación" (desactive esta casilla).
1.    Presione **Siguiente**.
1.    Desplácese a las columnas "ResourceRate" y "ExtendedCost".
1.    Seleccione la columna "ResourceRate" (aparecerá resaltada en negro).
9.    En la sección Formato de los datos en columnas, seleccione "Texto" en lugar de "General".  Verá que el encabezado de columna cambia de "General" a "Texto".
10. Repita los pasos 8 y 9 en la columna "Extended Cost". Seleccione **Finalizar**.

Si tiene Excel asociado para abrir automáticamente archivos \*.csv, debe usar la función "Abrir" en Excel en su lugar. Abra Excel y vaya a Archivo > Abrir.

## <a name="balance-and-charge-report"></a>Informe de saldo y cargos

El informe de saldo y cargos ofrece un resumen mensual de información sobre saldos, nuevas compras, gastos de servicios en Azure Marketplace, ajustes y gastos por de uso por encima del límite. Todas las filas de la tabla de resumen del compromiso de servicio de Azure permanecerán estáticas mes a mes, mientras que los detalles de todas las compras y ajustes se mostrarán en las secciones New Purchase Details (Detalles de nuevas compras) y Adjustment Details (Detalles de ajustes).

### <a name="download-the-balance-and-charge-report"></a>Descarga del informe de saldo y cargos

1. Inicie sesión en Azure EA Portal como administrador de empresa.
1. En el panel de navegación izquierdo, haga clic en **Reports** (Informes).
1. Haga clic en la pestaña **Report Download** (Descarga de informes).
1. Seleccione el mes correspondiente en la columna _Balance and Charge_ (Saldo y cargos) y haga clic para descargar el informe.

## <a name="usage-detail-report"></a>Informe de detalles de uso

El informe de detalles de uso ofrece un resumen mensual de los servicios y cantidades consumidos por una inscripción, incluida información sobre los nombres de los medidores, los tipos de medidores y las cantidades consumidas.

### <a name="download-the-usage-detail-report"></a>Descarga del informe de detalles de uso.

1. Inicie sesión en Azure EA Portal como administrador de empresa.
1. En el panel de navegación izquierdo, haga clic en **Reports** (Informes).
1. Haga clic en la pestaña **Download Usage** (Descarga de uso).
1. Seleccione el mes correspondiente en la columna _Usage Detail_ (Detalles de uso) y haga clic para descargar el informe.

## <a name="marketplace-charges-in-azure-ea-portal-reports"></a>Informes de cargos de Marketplace en Azure EA Portal

Puede encontrar información adicional sobre cargos de Marketplace [aquí](https://azure.microsoft.com/marketplace/faq/).

Existen dos tipos de cargos de Marketplace:

- **Basado en uso:** productos medidos en unidades transaccionales.  Por ejemplo, las máquinas virtuales se cobran por hora, mientras que las búsquedas de Bing API se cobran según el número de búsquedas.
- **Precio mensual:** se factura mensualmente según el uso y el acceso.

Para ver los distintos cargos en Enterprise Portal:

1. **Informe de resumen de uso:** muestra **tanto** los cargos de Marketplace de tarifa mensual como los basados en el uso.
1. **Informe de cargos de Marketplace**: muestra **solo** los cargos de Marketplace basados en el uso.  No se muestran las tarifas puntuales.

Tenga en cuenta que Azure Marketplace no está disponible para inscripciones de MPSA.

## <a name="advanced-report-download"></a>Descarga de informes avanzada

Para los informes de cuentas o intervalos de fechas específicos, se puede usar la descarga de informes avanzados. A partir del 30 de agosto de 2016, el formato del archivo de salida cambia de .xlsx a .csv para alojar conjuntos de registros mayores.

1. Seleccione **Advanced Report Download** (Descarga de informes avanzada).
1. Seleccione **Appropriate Date Range** (Intervalo de fechas adecuado).
1. Seleccione **Appropriate Accounts** (Cuentas adecuadas).
1. Seleccione **Request Usage Data** (Solicitar datos de uso).
1. Seleccione el botón **Refresh** (Actualizar) hasta que el estado del informe se actualice a "Download" (Descargar).
1. Descargue el informe.

## <a name="reporting-for-non-enterprise-administrators"></a>Informes para administradores que no son de empresa

Los administradores de empresa pueden habilitar el acceso para ver los costos de los administradores de departamento y los propietarios de cuenta de una inscripción. Una vez habilitado, un propietario de cuenta puede descargar informes .csv específicos de su cuenta y sus suscripciones, y ver la información visualmente en la sección de informes de Enterprise Portal.

### <a name="to-enable-access"></a>Para habilitar el acceso:

 1. Inicie sesión como administrador de empresa.
 1. Haga clic en **Manage** (Administrar) en el panel de navegación izquierdo.
 1. Haga clic en la pestaña **Inscripción**.
 1. En la sección Enrollment Detail (Detalles de inscripción), seleccione el icono de lápiz situado junto a:
    - DA View Charges (El administrador del departamento ve los cargos)
    - AO View Charges (El propietario de la cuenta ve los cargos)
 1. Seleccione **Habilitado**.
 1. Haga clic en **Save**(Guardar).

### <a name="to-view-reports"></a>Para ver informes:

1. Inicie sesión en Azure EA Portal como administrador de departamento o como propietario de cuenta.
1. En el panel de navegación izquierdo, haga clic en **Reports** (Informes).
1. Haga clic en la pestaña **Usage Summary** (Resumen de uso) para ver la información de la cuenta y la suscripción visualmente.
1. Haga clic en **Usage Download** (Descarga de uso) para ver los informes .csv.

Los administradores de departamento y los propietarios de cuenta no pueden ver los cargos al usar la función _Advanced Report Download_ (Descarga de informes avanzada). Los costos se muestran como $0.

AO View Charges (El propietario de la cuenta ve los cargos) se extiende a los propietarios de cuenta y a todos los usuarios que tienen permisos en las suscripciones asociadas. Si es un cliente indirecto, el partner de canal debe habilitar las características de costos.

## <a name="move-usage-data-to-another-enrollment"></a>Traslado de los datos de uso a otra inscripción

Los datos de uso solo se trasladan cuando se atrasa una transferencia. Hay dos opciones para trasladar los datos de uso de una inscripción a otra:

- Transferencia de cuentas desde una inscripción a otra
- Transferencia de inscripciones de una inscripción a otra

Para cualquiera de las opciones, debe enviar [una solicitud de soporte técnico](https://support.microsoft.com/supportrequestform/cf791efa-485b-95a3-6fad-3daf9cd4027c) al equipo de soporte técnico del Contrato Enterprise para obtener ayuda. 

## <a name="azure-ea-pricing-overview"></a>Introducción a los precios de Azure EA

En este documento se proporcionan detalles sobre cómo se calcula el uso y se responden muchas de las preguntas más frecuentes sobre los precios de los distintos servicios de Azure en los programas empresariales, donde los cálculos son más complejos.

### <a name="price-protection"></a>Protección de precios

Se garantiza que los clientes y los partners de canal (en el caso de los canales indirectos) recibirán precios iguales o inferiores a los indicados en su hoja de precios del cliente o el precio en vigor en la fecha efectiva de su compra de Azure. Este precio se denomina precio base de referencia. En el caso de los servicios introducidos después de esa compra, se trata del precio en vigor en el descuento de nivel aplicable cuando se introdujo el servicio por primera vez. Esta protección de precios es aplicable durante la vigencia del período de compromiso, que puede ser de uno o de tres años, en función del programa empresarial.

### <a name="price-changes"></a>Cambios en los precios

Microsoft puede reducir el precio del programa empresarial actual para servicios individuales de Azure durante el período de una inscripción.  Estas tarifas reducidas se extenderán a los clientes existentes, mientras el precio reducido sigue en vigor.  Si posteriormente se aumentan estas tarifas, el precio de la inscripción de un servicio no aumentará más allá del límite superior de la protección de precios del cliente, tal y como se definió anteriormente, pero puede aumentar con respecto al precio reducido anterior.  En cualquier caso, Microsoft informará a los clientes y a los partners de canal indirectos de los próximos cambios en los precios mediante un correo electrónico a los administradores empresariales y de asociados relacionados con la inscripción.

### <a name="current-effective-pricing"></a>Precios vigentes actuales

Para ver los precios actuales de una inscripción, los clientes y asociados de canal pueden iniciar sesión en [Azure Enterprise Portal](https://ea.azure.com/) y navegar a la página de la hoja de precios de esa inscripción.  Si adquiere Azure indirectamente a través de uno de nuestros partners de canal, deberá obtener las actualizaciones de precios de su partner de canal si este no ha habilitado el marcado de precios para su inscripción.

### <a name="introduction-of-new-azure-services"></a>Introducción de nuevos servicios de Azure

Estamos mejorando continuamente Azure y agregamos periódicamente nuevos servicios que tienen un precio independiente de los servicios existentes.  Algunos servicios en versión preliminar están disponibles automáticamente, mientras que otros requieren una acción del cliente en el [Portal de cuentas de Azure](https://account.windowsazure.com/PreviewFeatures). Tenga en cuenta también que al pasar los servicios de la versión preliminar a disponibilidad general, los precios pueden aumentar a medida que se aplican acuerdos de nivel de servicio de rendimiento y confiabilidad completos. Por último, algunos servicios se proporcionan con precios promocionales en vigor cuando se introducen por primera vez, que pueden aumentar con posterioridad. Los incrementos de precios promocionales o de versión preliminar a precios de disponibilidad general no están restringidos por la protección de precios base de referencia normal y, en adelante, se aplicarán al uso de esos servicios. Los clientes pueden evitar los cargos relacionados con estos servicios si eligen no usar el nuevo servicio.

### <a name="introduction-of-regional-pricing"></a>Introducción de precios regionales

Además de la introducción de nuevos servicios, los servicios también cambian periódicamente de un único modelo global a otro más regional, a medida que la compatibilidad regional para esos servicios aumenta. Cuando se introduce la regionalización de un servicio por primera vez, se aplica la protección de precios para esas nuevas regiones en comparación con el anterior precio global vigente para la inscripción. Sin embargo, las regiones adicionales introducidas con posterioridad para ese mismo servicio se consideran nuevos servicios y se ofrecen con sus propias tarifas individuales independientes de la protección de precios base de referencia.

### <a name="enterprise-msdn-devtest"></a>Enterprise MSDN para desarrollo y pruebas

Los administradores de empresa pueden permitir que los propietarios de cuenta creen suscripciones basadas en la oferta de EA MSDN para desarrollo y pruebas. Para que esto funcione correctamente, el propietario de la cuenta tendrá que configurar las suscripciones de EA MSDN para desarrollo y pruebas necesarias para los suscriptores de MSDN subyacentes. Esto permite a los suscriptores activos de MSDN ejecutar cargas de trabajo de desarrollo y pruebas en Azure a tarifas especiales de desarrollo y pruebas. Para más información, consulte [EA MSDN para desarrollo y pruebas](https://azure.microsoft.com/offers/ms-azr-0148p/).

### <a name="enterprise-program-usage-calculation-guidelines"></a>Directrices de cálculo de uso de programas empresariales

Consulte [Servicios de Azure](https://azure.microsoft.com/services/) y [Precios de Azure](https://azure.microsoft.com/pricing/) para ver información pública básica, unidades de medida, preguntas frecuentes y directrices de informes de uso para cada servicio individual. Además, al calcular el uso del servicio, deben usarse las siguientes directrices de programas empresariales.

### <a name="enterprise-program-units-of-measure"></a>Unidades de medida de programas empresariales

Las unidades de medida de los programas empresariales suelen ser diferentes de las que se muestran en nuestros otros programas, como el programa del contrato de Microsoft Online Services (MOSA). Esto significa que, para una serie de servicios, se agrega la unidad de medida para proporcionar los precios normalizados. La unidad de medida que se muestra en la vista del resumen de uso de Enterprise Portal siempre es la medida empresarial. En [Nombres descriptivos de servicios](https://azurepricing.blob.core.windows.net/supplemental/Friendly_Service_Names.xlsx), se proporciona una lista completa de las unidades de medida y conversiones actuales de cada servicio.

### <a name="rounding-rules"></a>Reglas de redondeo

El redondeo en Enterprise Portal utiliza la lógica de redondeo bancario o redondeo gaussiano del estándar IEEE. Esta lógica redondea al dígito par más cercano para los valores de medio dígito, donde las funciones de redondeo hacia arriba típicas siempre redondean los valores medios al siguiente dígito más alto. Este método proporciona una suma total más precisa sobre el grupo en comparación con la lógica de Excel estándar.

Cuando el primer dígito que se va a quitar es 5 y después no hay más dígitos o los dígitos siguientes son ceros, el dígito que le precede se convierte en par (es decir, se redondea al dígito par más cercano). Por ejemplo, tanto 2,315 como 2,325 darán 2,32 al redondearse a la centésima más cercana.

Como referencia, al usar Excel para modelar las reglas de redondeo y conversión utilizadas en Enterprise Portal, se deben usar las fórmulas de REDOND.MULT, tal como se muestra a continuación.

| Escenario | Fórmula de lógica bancaria |
| --- | --- |
| Redondeo de uso | =REDOND.MULT({_source_}, 0.0002) |
| Redondeo de precios (2 decimales) | =REDOND.MULT({_source_}, 0.02) |
| Redondeo de precios (0 decimales) | =REDOND.MULT({_source_}, 2) |

**Tabla** **2** **: Conversión de horas de servicios en la nube y máquina virtual**

### <a name="conversion-between-download-usage-file-and-usage-summary-portal-view"></a>Conversión entre la vista del archivo de uso de descarga y el portal de resumen de uso

Los datos de uso de recursos sin procesar se detallan hasta un máximo de seis posiciones decimales, tal como se puede observar en el informe de datos de uso de descarga. Sin embargo, Azure Enterprise Portal redondea el uso a cuatro posiciones decimales para las unidades de compromiso y trunca a cero decimales para las unidades de uso por encima del límite. El uso sin procesar se redondea primero a cuatro dígitos antes de la conversión a unidad de medida empresarial y las unidades empresariales resultantes se redondean de nuevo a cuatro dígitos. El consumo de horas real antes de la conversión solo se muestra en el informe de uso de descarga y no en la propia interfaz de usuario.

Por ejemplo, supongamos que se notifican 694,533404 horas de SQL Server reales en el informe de uso de descarga. Estas unidades se convierten a continuación en 6,94533404 unidades de 100 horas de proceso que se redondean a 6,9453 para que se muestren en Enterprise Portal.

A continuación, estas unidades simplemente se multiplican por el precio de la unidad de compromiso y el resultado se trunca a dos decimales para determinar el importe de facturación extendido. En el caso del yen japonés (JPY) y el won coreano (KRW), el importe extendido se redondea a cero decimales.

Para este mismo ejemplo de uso por encima del límite, las unidades facturables se truncarían a seis y, a continuación, se multiplicarían por el precio unitario de uso por encima del límite para determinar el importe de facturación extendido. Para la facturación de proveedores de servicios administrados (MSP), todo el uso asociado a un departamento marcado como MSP se trunca a cero decimales después de la conversión a la unidad de medida de EA, al igual que para los informes de uso por encima del límite. Como resultado, la suma de este uso podría ser menor que la suma total de todo el uso indicado en la interfaz de usuario en función de si el MSP todavía se encuentra dentro de su saldo de compromiso monetario o si ya está en uso por encima del límite.

### <a name="graduated-pricing"></a>Precios escalonados

Los precios de los programas empresariales no incluyen precios escalonados (cuando el precio por unidad disminuye a medida que aumenta el uso). Al pasar de un programa MOSA con precios escalonados a un programa empresarial, los precios se establecen de forma acorde con el nivel de base para ese servicio, después de ajustarse a los descuentos del programa empresarial, si procede.

### <a name="partial-hour-billing"></a>Facturación por horas parciales

Todo el uso se factura basándose en minutos convertidos a horas parciales, en lugar de incrementos de horas completas.  Las tarifas empresariales que se muestran como por horas se aplican simplemente al total de horas, incluidas todas las horas parciales.

### <a name="average-daily-consumption"></a>Consumo diario medio

Cuando un servicio tiene un precio mensual, pero el uso se registra diariamente, este uso se evalúa una vez al día, dividiéndose entre 31 y sumándose al número de días del mes de facturación. Esto da como resultado tarifas que nunca son mayores de lo previsto para ningún mes y que son ligeramente inferiores para los meses con menos de 31 días.

### <a name="compute-hours-conversion"></a>Conversión de horas de proceso

Antes, todo el uso de máquinas virtuales y servicios en la nube básicos y estándar A0, A2, A3 y A4 se emitía en fracciones (para A0) o múltiplos (para A2, A3 y A4) de los minutos del medidor para las máquinas virtuales A1. Esto producía cierta confusión entre nuestros clientes, por lo que se está implementando un cambio para asignar el uso por minuto a medidores dedicados para A0, A2, A3 y A4.

La nueva medición entrará en vigor entre el 27 de enero y el 28 de enero de 2016. En el archivo .csv descargado para la implementación durante este período de transición, observará dos elementos de línea: Uno para el uso emitido en el medidor A1 y otro para el uso emitido en el nuevo medidor dedicado correspondiente al tamaño de la implementación.

| **Tamaño de la implementación** | **Uso emitido como múltiplo de A1 hasta el 26 de enero de 2016** | **Uso emitido conforme a una medida dedicada a partir del 27 de enero de 2016** |
| --- | --- | --- |
| A0 | 0,25 horas de A1 | 1 hora de A0 |
| A2 | 2 horas de A1 | 1 hora de A2 |
| A3 | 4 horas de A1 | 1 hora de A3 |
| A4 | 8 horas de A1 | 1 hora de A4 |

### <a name="regions"></a>Regions

En el caso de los servicios en los que la zona y la región afectan a los precios, en la tabla siguiente se muestra la asignación de regiones y zonas geográficas:

| Geoárea | Regiones de transferencia de datos | Regiones de CDN |
| --- | --- | --- |
| Zona 1 | Norte de Europa <br> Oeste de Europa <br> Oeste de EE. UU. <br> Este de EE. UU. <br> Centro y norte de EE. UU. <br> Centro y Sur de EE. UU. <br> Este de EE. UU. - 2 <br> Centro de EE. UU. | Norteamérica <br> Europa |
| Zona 2 | Este de Asia Pacífico <br> Sudeste de Asia Pacífico <br> Este de Japón <br> Oeste de Japón <br> Este de Australia <br> Sudeste de Australia | Asia Pacífico <br> Japón <br> América Latina <br> Oriente Medio y África <br> Este de Australia <br> Sudeste de Australia |
| Zona 3 | Sur de Brasil |   |

**Tabla** **4**  **: Regiones de transferencia de datos**

No hay cargos por la salida de datos entre servicios (por ejemplo, O365 y Azure) hospedados en el mismo centro de datos.

### <a name="monetary-commitment-and-unbilled-usage"></a>Compromiso monetario y uso no facturado

El compromiso monetario de Azure es un importe pagado por adelantado para los servicios de Azure. El compromiso monetario se consume a medida que se usan los servicios. Los servicios propios de Azure utilizan el compromiso monetario. Sin embargo, hay excepciones para los cargos facturados por separado y los servicios de Marketplace.

### <a name="charges-billed-separately"></a>Gastos facturados por separado

Algunos productos y servicios proporcionados por orígenes de terceros no consumen compromiso monetario de Azure. En su lugar, estos elementos se facturarán por separado como parte de la factura de uso por encima del límite del ciclo de facturación estándar.

Hemos combinado todos los cargos de Azure y Marketplace en una sola factura alineada con el ciclo de facturación de la inscripción. (Esto no afecta a los clientes de Australia, Japón o Singapur).

La factura consolidada mostrará en primer lugar el uso de Azure, seguido de todos los cargos de Marketplace. Los clientes de Australia, Japón o Singapur seguirán viendo los cargos de Marketplace en una factura independiente.

Si no hay uso por encima del límite al final del ciclo de facturación estándar, los cargos independientes se facturarán por separado. (Aplicable a clientes de Australia, Japón y Singapur)

**Entre los servicios facturados por separado se incluyen los siguientes:**

- Canonical
- Citrix XenApp Essentials
- Usuario registrado de Citrix XenDesktop
- Openlogic
- Usuario registrado de Remote Access Rights XenApp Essentials
- Ubuntu Advantage
- Visual Studio Enterprise (mensual)
- Visual Studio Enterprise (anual)
- Visual Studio Professional (mensual)
- Visual Studio Professional (anual)

## <a name="azure-marketplace-for-ea-customers"></a>Azure Marketplace para clientes de EA

Para los clientes directos, los cargos de Marketplace están visibles en Azure EA Portal. Las compras y el consumo de Marketplace se facturarán fuera del compromiso monetario y trimestralmente o mensualmente a pago vencido.

Los clientes indirectos pueden encontrar sus suscripciones de Azure Marketplace en la página "Manage Subscriptions" (Administrar suscripciones) de Azure EA Portal, pero no se muestran los precios. Los clientes deben ponerse en contacto con su LSP para obtener información sobre los gastos en Marketplace.

Por las nuevas compras de Marketplace periódicas, mensuales o anuales, se facturará la cantidad total del período en el que se compraron los artículos de Marketplace. Estos artículos se renovarán automáticamente en el siguiente período el mismo día de la compra original.

Esto no afectará a los cargos periódicos actuales de Marketplace. Los cargos periódicos mensuales continuarán renovándose el primer día de cada mes natural y los cargos anuales se renovarán en el aniversario de la fecha de compra.

Algunos servicios de terceros revendedores disponibles en Azure Marketplace ahora consumen el saldo del compromiso monetario del Contrato Enterprise. Antes, estos servicios se facturaban por separado, fuera del compromiso monetario de EA. El compromiso monetario de EA para estos servicios de Marketplace simplifica las compras para los clientes y la administración de los pagos. Para ver una lista completa de los servicios que actualmente consumen compromiso monetario, consulte el [sitio web de Azure](https://azure.microsoft.com/updates/azure-marketplace-third-party-reseller-services-now-use-azure-monetary-commitment/?WT.mc_id=azurebg_email_Trans_33771_1695_Release_Direct_Tier2_March14).

### <a name="partners"></a>Asociados

Los LSP pueden descargar una lista de precios específica de Marketplace. Para ello, vaya a la hoja de precios en Azure EA Portal y haga clic en el vínculo **Marketplace Price list** (Lista de precios de Marketplace) en la esquina superior derecha. La lista de precios de Marketplace reflejará todos los servicios disponibles y sus precios.

**Para descargar la lista de precios, siga estos pasos:**

1. Vaya a Reports > Price Sheet (Informes > Hoja de precios).
1. En la esquina superior derecha, busque el vínculo a la lista de precios de Azure Marketplace debajo de su nombre de usuario.
1. Haga clic con el botón derecho en el vínculo y, después, en **Guardar destino como**.
1. En la ventana Guardar, cambie el título del documento a _AzureMarketplacePricelist.zip_, que cambiará el archivo de xlsx a un archivo zip.
1. Una vez completada la descarga, tendrá un archivo zip con listas de precios específicas del país.
1. Los LSP deben hacer referencia al archivo individual del país para obtener los precios específicos del país. Los LSP deben usar la pestaña "Notifications" (Notificaciones) para ver las SKU nuevas netas de Marketplace, así como las SKU que se han quitado.
1. Los cambios en los precios no serán frecuentes pero, cuando se produzcan, se notificará a los LSP los aumentos en los precios y los cambios en las características con 30 días de antelación por correo electrónico.
1. Los LSP recibirán una factura por inscripción, por ISV y por trimestre.

### <a name="enabling-marketplace-purchases"></a>Habilitar compras en Marketplace

Los administradores de empresas tienen la capacidad de "deshabilitar" o "habilitar" las compras de Marketplace para todas las suscripciones de Azure de la inscripción. Si el administrador de empresa deshabilita las compras y hay suscripciones de Azure que ya tienen suscripciones a Marketplace, esas suscripciones a Marketplace no se cancelarán ni se verán afectadas.

Aunque los clientes pueden convertir sus suscripciones de Azure directas a EA asociándolas a su inscripción en Azure EA Portal, esta acción no convierte automáticamente las suscripciones de Marketplace secundarias.

**Para habilitar compras en Marketplace:**

1. Inicie sesión en Azure EA Portal como administrador de empresa.
1. Vaya a _Manage_ (Administrar).
1. En _Enrollment Detail_ (Detalles de la inscripción), haga clic en el icono de lápiz situado junto al elemento de línea _Azure Marketplace_
1. Cambie entre _Enabled/Disabled_ (Habilitado/deshabilitado) o _BYOL Free SKUs Only\*_ (Solo SKU BYOL o libres), según corresponda.
1. Haga clic en **Save**(Guardar).

### <a name="marketplace-charges-in-azure-ea-portal-reports"></a>Informes de cargos de Marketplace en Azure EA Portal

Puede encontrar información adicional sobre cargos de Marketplace [aquí](https://azure.microsoft.com/marketplace/faq/).

Existen dos tipos de cargos de Marketplace:

- **Basados en el uso:** los productos se miden en unidades transaccionales.  Por ejemplo, las máquinas virtuales se cobran por hora, mientras que las búsquedas de Bing API se cobran según el número de búsquedas.
- **No basados en el uso:** cargo único o tarifa mensual periódica independiente del uso.

Tanto los cargos basados en el uso como los no basados en el uso se recogerán en el informe de cargos de Marketplace.

Tenga en cuenta que Azure Marketplace no está disponible para inscripciones de MPSA.

\*Las opciones BYOL (traiga su propia licencia) y solo gratis limitarían la compra y adquisición de SKU de Azure Marketplace a BYOL y solo SKU gratuitas.

### <a name="services-billed-hourly-for-ea"></a>Servicios facturados por horas para EA

La red de entrega de aplicaciones y el firewall de aplicaciones web se facturan por horas para EA, frente a la tarifa mensual en MOSP.

### <a name="remote-app"></a>Aplicación remota

Los clientes de EA pagan por la aplicación remota en función de su nivel de precios de EA y no se les cobra adicionalmente. El precio estándar incluye 40 horas iniciales, mientras que el precio ilimitado cubre 80 horas iniciales. La aplicación remota deja de emitir uso más allá de 80 horas.

## <a name="azure-marketplace-faq"></a>Preguntas frecuentes sobre Azure Marketplace

En este documento de preguntas frecuentes se revisan las actualizaciones de la aplicación del compromiso monetario de Azure a algunos servicios de terceros publicados en Azure Marketplace.

### <a name="what-are-we-changing-with-respect-to-marketplace-services-and-azure-monetary-commitment"></a>¿Qué se está cambiando con respecto a los servicios de Marketplace y al compromiso monetario de Azure?

Además de las instancias reservadas de máquina virtual de Azure, los clientes reciben una factura independiente por todos los servicios que compran en Azure Marketplace. Estamos expandiendo el uso del compromiso monetario de Azure para incluir ahora algunos de los servicios de Azure Marketplace publicados por terceros que los clientes compran con más frecuencia.

### <a name="why-are-we-making-this-change"></a>¿Por qué se realiza este cambio?

Los clientes buscan continuamente nuevas maneras de aprovechar el pago por adelantado que han realizado en forma de compromiso monetario de Azure.  Abordamos una solicitud frecuente de los clientes, que atañe a una gran parte de nuestros clientes de Azure Marketplace al expandir el compromiso monetario a estos servicios.

### <a name="what-is-the-customer-benefit"></a>¿Cuál es la ventaja para el cliente?

Los clientes obtienen una experiencia de facturación más sencilla y la capacidad de asegurarse de que emplean su compromiso monetario de Azure.  Mediante la adición de esta ventaja para el compromiso monetario prepagado y las instancias reservadas de máquina virtual que usan el compromiso monetario, el compromiso monetario de Azure cobra aún más valor.

### <a name="what-services-will-deduct-from-azure-monetary-commitment-and-how-will-my-customer-know"></a>¿Qué servicios se deducirán del compromiso monetario de Azure y cómo lo sabrá mi cliente?

Durante la experiencia de compra de Azure Marketplace, se distinguirá cada servicio que utilizará compromiso monetario con un aviso. Entre los editores admitidos actualmente se incluyen determinados servicios publicados por Red Hat, SUSE, Autodesk y Oracle. Los servicios que tienen convenciones de nomenclatura similares pero que publican otras partes no identificadas anteriormente no se deducirán del compromiso monetario. Al final de esta sección de preguntas frecuentes encontrará una lista completa.

### <a name="what-if-my-customer-runs-out-of-monetary-commitment"></a>¿Qué ocurre si mi cliente se queda sin compromiso monetario?

En el caso de los clientes que hayan consumido todo su compromiso monetario y ahora estén en uso por encima del límite, los cargos relacionados con estos servicios aparecerán en la próxima factura de uso por encima del límite junto con cualquier otro servicio de consumo.  Esto supone un cambio, ya que anteriormente se habrían facturado estos cargos en su propia factura con otras ofertas de Azure Marketplace.

### <a name="why-are-we-not-enabling-azure-monetary-commitment-for-all-marketplace-purchases"></a>¿Por qué no se habilita el compromiso monetario de Azure para todas las compras de Marketplace?

Trabajamos con frecuencia para ofrecer la mejor experiencia de cliente relacionada con el compromiso monetario de Azure. Este cambio afectará a un gran número de clientes y a una parte importante del gasto total en Azure Marketplace. Es posible que se agreguen otros servicios en el futuro.

### <a name="how-does-this-impact-indirect-enrollment-and-partners"></a>¿Cómo afecta esto a la inscripción indirecta y los asociados?

Nuestros asociados o clientes de inscripciones indirectas no se verán afectados. Estos servicios están sujetos a las mismas funcionalidades de marcado de asociados que otros servicios de consumo. El único cambio será la factura en la que aparecen y el pago de los cargos con compromiso monetario.

### <a name="list-of-services-that-will-deduct-from-azure-monetary-commitment"></a>Lista de servicios que se deducirán del compromiso monetario de Azure

Algunas ofertas específicas de Azure Marketplace pueden usar fondos de un compromiso monetario. Vea el [compromiso monetario de Azure](https://azure.microsoft.com/updates/azure-marketplace-third-party-reseller-services-now-use-azure-monetary-commitment) para obtener una lista completa de productos que participan en este programa.

## <a name="additional-information"></a>Información adicional

Para más información, consulte estas fuentes adicionales. Estos archivos se actualizan dos veces al mes, los días 6 y 20. Los detalles de cada archivo son los siguientes:

| Título del apéndice | Descripción | Convención de nomenclatura de URL |
| --- | --- | --- |
| [**Nombres descriptivos de servicios**](https://azurepricing.blob.core.windows.net/supplemental/Friendly_Service_Names.xlsx) | Proporciona una lista de todos los servicios activos con la categoría de servicio, el nombre descriptivo del servicio, el nombre del compromiso y el número de artículo, el nombre de consumo y el número de artículo, las unidades de medida y los factores de conversión entre el uso indicado y el uso mostrado en Enterprise Portal. | Friendly\_Service\_Names.xlsx |
| [**Campos de descarga de servicio**](https://azurepricing.blob.core.windows.net/supplemental/Service_Download_Fields.xlsx) | Esta hoja de cálculo proporciona una lista de todas las posibles combinaciones de campos relacionados con el servicio del informe de descarga de uso. | Service\_Download\_Fields.xlsx |

**Table** **5** **: Fuentes de información adicionales**

## <a name="power-bi-reporting"></a>Informes de Power BI

### <a name="power-bi-pro"></a>Power BI Pro

Power BI Pro ahora está disponible para los clientes de EA. Con Power BI Pro, puede generar y compartir informes para administrar de forma eficaz datos de costos, con características adicionales de colaboración y actualización de datos. Power BI Pro ofrece mayor capacidad de datos y límites de streaming de datos. Próximamente se incluirán nuevas características de administración de costos para los clientes de Azure Enterprise.

Los actuales usuarios de Power BI Free que tengan el paquete de contenido de Microsoft Azure Consumption Insights pueden usar la versión de evaluación gratuita de 60 días de Power BI Pro. Si desea seguir usando Power BI Pro después de la evaluación gratuita, puede agregar una licencia para ello.

Para suscribirse a la evaluación gratuita, vaya al icono de engranaje y seleccione **Administrar almacenamiento personal**. A continuación, seleccione **Probar Power BI Pro gratis** a la derecha. Consulte [Registro de autoservicio en Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/#power-bi-pro-60-day-trial) para obtener más información sobre la evaluación gratuita de Power BI Pro.

### <a name="microsoft-azure-ea-power-bi-pro-trial-terms"></a>Términos y condiciones de la prueba de Power BI Pro de Microsoft Azure EA

- **Uso general**: la versión extendida de Power BI Pro para la oferta de evaluación del paquete de contenido "Microsoft Azure Enterprise" (la "Oferta") está disponible para los usuarios cualificados existentes durante el período de la Oferta, para permitirles acceder a cierta información relacionada con el consumo de Microsoft Azure mediante el uso de un paquete de contenido de Power BI específico.
- **Requisitos**: los usuarios con un Contrato Enterprise (EA) pueden participar en la Oferta si tienen una función relacionada con la facturación, el servicio o la administración de costos de Microsoft Azure de su organización.
- **Exclusiones**:
  - los usuarios que ya participan en la evaluación de Power BI Pro extendida seguirán incluyéndose en la oferta existente anteriormente y no podrán optar a la oferta de evaluación de Power BI Pro de Azure EA.
  - Los usuarios que participan en la oferta solo pueden usar Power BI Pro con el paquete de contenido de Microsoft Azure Enterprise. Se prohíbe cualquier otro uso de Power BI Pro.
  - Vigencia: la Oferta comenzará el 1 de junio de 2017 y terminará el 31 de mayo de 2018.  La aceptación puede producirse en cualquier momento durante el período de 12 meses, aunque la oferta terminará el 31 de mayo de 2018 para todos los usuarios, independientemente de en qué momento hayan aceptado la Oferta.

### <a name="to-access-the-microsoft-azure-consumption-insights-content-pack"></a>Para acceder al paquete de contenido de Microsoft Azure Consumption Insights:

1. Vaya a [Microsoft Azure Consumption Insights](https://app.powerbi.com/getdata/services/azureconsumption?cpcode=MicrosoftAzureConsumptionInsights&amp;getDataForceConnect=true&amp;WT.mc_id=azurebg_email_Trans_33675_1378_Service_Notice_EA_Customer_Power_BI_EA_Content_Pack_Apr26). Haga clic en **Obtenerlo ahora**.
1. Proporcione el número de inscripción y el número de meses. Haga clic en **Next**.
1. Proporcione la clave de acceso de la API para conectarse. Puede encontrar la clave de la inscripción en [Enterprise Portal](https://ea.azure.com/?WT.mc_id=azurebg_email_Trans_33675_1378_Service_Notice_EA_Customer_Power_BI_EA_Content_Pack_Apr26). Haga clic en **Iniciar sesión**.
1. El proceso de importación se iniciará automáticamente. Cuando haya finalizado, aparecerá un nuevo panel, informe y modelo en el panel de navegación. Haga clic en el panel para ver los datos importados.

Para más información sobre cómo generar la clave de API para su inscripción, visite el archivo de ayuda de API Reports en [Enterprise Portal](https://ea.azure.com/?WT.mc_id=azurebg_email_Trans_33675_1378_Service_Notice_EA_Customer_Power_BI_EA_Content_Pack_Apr26). Para más información sobre el nuevo paquete de contenido, consulte [Microsoft Azure Consumption Insights](/power-bi/desktop-connect-azure-cost-management).

### <a name="to-access-the-legacy-power-bi-ea-content-pack"></a>Para acceder al paquete de contenido de EA de Power BI heredado:

 1. Vaya al [sitio web de Power BI](https://app.powerbi.com/getdata/services/azure-enterprise).
 1. Inicie sesión con una cuenta profesional o educativa válida.
    - La cuenta profesional o educativa puede ser la misma o distinta de la que se usa para acceder a la inscripción mediante Azure EA Portal.
 1. En el panel de servicios, seleccione **Microsoft Azure Enterprise** y haga clic en **Conectar**.
 1. En la pantalla "Conectar con Azure Enterprise", elija:
    - Dirección URL del entorno de Azure: [https://ea.azure.com](https://ea.azure.com/).
    - Número de meses: elija entre 1 y 36.
    - Número de inscripción: escriba el número de inscripción.
    - Haga clic en **Next**.
 1. Escriba la clave de API en el cuadro Clave de autenticación. Puede obtener la clave de API aquí en Azure Enterprise Portal en la pestaña "Download Usage" (Descargar uso) de arriba; para ello, haga clic en "API Access Key" (Clave de acceso de la API).
    - Copie y pegue la clave en el cuadro "Clave de cuenta".
 1. Los datos tardarán aproximadamente de 5 a 30 minutos en cargarse en Power BI, según el tamaño de los conjuntos de datos.

Los informes de Power BI están disponibles para clientes directos, asociados e indirectos de Contrato Enterprise que tengan acceso a la información de facturación.

## <a name="report-faq"></a>Preguntas frecuentes sobre informes

En esta sección del artículo se responden preguntas frecuentes relacionadas con la interpretación de informes.

### <a name="why-is-my-cost-showing-as-0"></a>¿Por qué se muestra el costo como $0?

**Inscripción directa** Si es propietario de cuenta o administrador de departamento, póngase en contacto con su administrador de EA para habilitar la característica de precios:

1. Haga clic en **Manage** (Administrar) en el panel de navegación izquierdo.
1. Haga clic en el lápiz azul que hay junto a DA View Charges (El administrador del departamento ve los cargos).
1. Seleccione **Enabled** (Habilitado) y guarde.
1. Haga clic en el lápiz azul que hay junto a AO View Charges (El propietario de la cuenta ve los cargos).
1. Seleccione **Enabled** (Habilitado) y guarde.

Esta acción proporcionará a los propietarios de cuenta y administradores de departamento acceso a la información de costos y precios en los informes de uso.

**Inscripción indirecta** Consulte con su asociado que haya habilitado la característica de precios. Esto solo puede hacerlo el asociado y, una vez que haya activado la característica, podrá ver el costo y los precios de la inscripción como administrador de EA.

Si desea habilitar la característica de visualización de cargos para sus propietarios de cuenta y administradores de departamento, siga los pasos anteriores que se indican en **Inscripción directa**.

### <a name="there-is-no-sku-information-on-the-usage-detail-report"></a>No hay información de SKU en el informe de detalles de uso

El informe de detalles de uso no tiene información de SKU; sin embargo, podrá ver la información de servicio utilizada en el informe. Después, puede descargar el informe de la hoja de precios para obtener la información de SKU.

### <a name="the-total-amount-on-marketplace-does-not-match-on-usage-summary-and-csv-report"></a>El importe total en Marketplace no coincide en el resumen de uso y en el informe de CSV

El informe de cargos de Marketplace muestra solo los cargos de Marketplace basados en el uso. No se muestran las tarifas puntuales. Puede consultar la página de resumen de uso para ver los usos más actualizados de los cargos basados en el uso y las tarifas puntuales.

### <a name="there-is-no-information-on-my-api-report"></a>El informe de API no tiene información

Las claves de API expiran cada seis meses. Genere una nueva clave de API si está experimentando un problema. También es importante solicitar al administrador de EA que genere las nuevas claves de API y siga los pasos que se describen en las preguntas frecuentes sobre informes de API.

### <a name="my-power-bi-report-isnt-working"></a>Mi informe de Power BI no funciona

Si tiene algún problema con Power BI, registre un vale de soporte técnico con el equipo de Power BI en [https://support.powerbi.com](https://support.powerbi.com)para que el equipo pueda ayudarle.

### <a name="my-resource-tags-arent-showing-up-on-my-reports"></a>Mis etiquetas de recursos no aparecen en mis informes

Las etiquetas de recursos se administran en Azure Portal. Puede ponerse en contacto con el equipo de suscripciones de Azure en [https://portal.azure.com](https://portal.azure.com). Siga los pasos descritos en [este vínculo](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request) sobre cómo generar una solicitud de soporte técnico.

### <a name="why-does-my-resource-rate-change-every-day"></a>¿Por qué cambia la tasa de recursos cada día?

La tasa de recursos del informe de uso detallado es un valor calculado y representa la tasa media mensual que se cobra por un servicio. Esta tarifa se calcula usando el promedio del compromiso mensual y los cargos mensuales por uso por encima del límite de una unidad de servicio. La parte del uso que se aplica a las tarifas de uso por encima del límite y al compromiso cambiará hasta el día en que se cierre el mes. Por este motivo, la tasa de recursos también cambiará durante el mes. La tasa de recursos se bloquea al quinto día después del final del mes.

### <a name="glossary-of-processes-for-calculating-the-resource-rate"></a>Glosario de procesos para calcular la tasa de recursos

**Total de unidades sin procesar:** cantidad consumida en el informe de uso detallado.
**Recurso de MOCP por unidad:** sistema de uso ascendente que emite los usos de cada servicio en distintas unidades. (Valor predeterminado) **Consumo por unidad:** unidad de medida de EA. (Valor predeterminado) **Precio:** precio unitario de Azure EA Portal.
**Costo total:** costo total del informe de uso detallado o del uso del compromiso + uso por encima del límite de Azure EA Portal.


### <a name="charges-calculation"></a>Cálculo de cargos

**Conversión en recurso de MOCP por unidad** = ROUND(total de unidades sin procesar * recurso de MOCP por unidad, 4) **Conversión en unidades** = unidades después de conversión en recurso de MOCP por unidad/consumo por unidad **Costo total** = unidades * precio

### <a name="download-usage-calculation-logic"></a>Lógica de cálculo de uso de descarga

**Tasa de recursos** = costo total/(total de unidades sin procesar/recurso de MOCP por unidad)

La tasa de recursos se deriva de los cargos y, a menudo, no coincide con el precio unitario real de la hoja de precios.

Para el cálculo de los cargos de uso por encima del límite, los datos de uso de recursos sin procesar se detallan hasta un máximo de seis posiciones decimales, tal como se puede observar en el informe de datos de uso de descarga. Sin embargo, Azure EA Portal redondea el uso a cuatro posiciones decimales para las unidades de compromiso y trunca a cero decimales para las unidades de uso por encima del límite. Esto significa que, en Azure EA Portal, para todo el uso que se cobra como uso por encima del límite, solo se cobran las unidades completas. Habrá una gran diferencia entre el precio unitario y la tasa de recursos para el uso que se cobra como uso por encima del límite o en meses mixtos.

## <a name="next-steps"></a>Pasos siguientes
- Para más información sobre la facturación y los cargos, consulte [Descripción de la factura de Azure Enterprise](../understand/review-enterprise-agreement-bill.md).
- Para empezar a usar el portal del Contrato Enterprise de Azure, consulte [Introducción al portal del Contrato Enterprise de Azure](ea-portal-get-started.md).
