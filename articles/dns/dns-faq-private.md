---
title: Preguntas frecuentes sobre Azure DNS privado
description: Preguntas frecuentes sobre Azure DNS privado
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 6/12/2019
ms.author: victorh
ms.openlocfilehash: 480cf22491dbbfcb9fe1961b5c9a7aa6fe12a0cb
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67274036"
---
# <a name="azure-private-dns-faq"></a>Preguntas frecuentes sobre Azure DNS privado

> [!IMPORTANT]
> Azure DNS privado se encuentra actualmente en su versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="does-azure-dns-support-private-domains"></a>¿Azure DNS admite dominios privados?

La compatibilidad con dominios privados es posible mediante la característica Azure DNS Private Zones. Las zonas DNS privadas se administran con las mismas herramientas que las zonas de Azure DNS accesibles desde Internet. Se pueden resolver solo desde dentro de las redes virtuales especificadas. Para más información, consulte la [introducción](private-dns-overview.md).

Para sobre otras opciones de DNS internas de Azure, consulte [Resolución de nombres para las máquinas virtuales e instancias de rol](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

## <a name="will-azure-dns-private-zones-work-across-azure-regions"></a>¿Funcionará Azure DNS Private Zones entre regiones de Azure?

Sí. Las zonas privadas se admiten para la resolución DNS entre redes virtuales de distintas regiones de Azure. Las zonas privadas funcionan incluso sin emparejamiento explícito de las redes virtuales. Todas las redes virtuales se deben especificar como redes virtuales de resolución para la zona privada. Los clientes pueden necesitar que las redes virtuales se emparejen para que el tráfico TCP/HTTP fluya de una región a otra.

## <a name="is-connectivity-to-the-internet-from-virtual-networks-required-for-private-zones"></a>Para las zonas privadas, ¿se requiere que las redes virtuales tengan conectividad con Internet?

No. Las zonas privadas funcionan junto con las redes virtuales. Los clientes las usan para administrar los dominios de las máquinas virtuales u otros recursos dentro y entre redes virtuales. No se requiere conectividad con Internet para la resolución de nombres.

## <a name="can-the-same-private-zone-be-used-for-several-virtual-networks-for-resolution"></a>¿Se puede usar la misma zona privada para varias redes virtuales para la resolución?

Sí. Puede asociar hasta 1000 redes virtuales con una única zona privada.

## <a name="can-a-virtual-network-that-belongs-to-a-different-subscription-be-added-as-a-linked-virtual-network-to-a-private-zone"></a>¿Se puede agregar una red virtual que pertenezca a otra suscripción como red virtual vinculada a una zona privada?

Sí. Debe tener permiso para la operación de escritura en las redes virtuales y en la zona DNS privada. El permiso de escritura se puede conceder a varios roles de RBAC. Por ejemplo, el rol de RBAC Colaborador de la red virtual clásica tiene permisos de escritura en las redes virtuales. Para más información sobre los roles de RBAC, consulte el artículo sobre [el control de acceso basado en rol](../role-based-access-control/overview.md).

## <a name="will-the-automatically-registered-virtual-machine-dns-records-in-a-private-zone-be-automatically-deleted-when-you-delete-the-virtual-machine"></a>¿Se eliminarán automáticamente los registros de DNS de una máquina virtual registrada en una zona privada cuando se elimine la máquina virtual?

Sí. Si elimina una máquina virtual que se encuentra dentro de una red virtual vinculada que tiene habilitado el registro automático, se eliminan automáticamente los registros anotados.

## <a name="can-an-automatically-registered-virtual-machine-record-in-a-private-zone-from-a-linked-virtual-network-be-deleted-manually"></a>¿Se puede eliminar manualmente un registro de máquina virtual registrado automáticamente en una zona privada de una red virtual vinculada?

Sí. Puede sobrescribir los registros de DNS que se registraron automáticamente con un registro de DNS que se creó de manera manual en la zona. La siguiente pregunta y su respuesta abordan este tema.

## <a name="what-happens-when-i-try-to-manually-create-a-new-dns-record-into-a-private-zone-that-has-the-same-hostname-as-an-automatically-registered-existing-virtual-machine-in-a-linked-virtual-network"></a>¿Qué ocurre cuando se intenta crear manualmente un registro de DNS en una zona privada que tiene el mismo nombre de host que una máquina virtual existente registrada de manera automática en una red virtual vinculada?

Intenta crear manualmente un registro de DNS en una zona privada con el mismo nombre de host que una máquina virtual existente registrada de manera automática en una red virtual vinculada. Al hacerlo, el nuevo registro de DNS sobrescribe el registro de máquina virtual que se registró automáticamente. Si intenta eliminar nuevamente este registro de DNS que se creó de manera manual de la zona, la eliminación se realizará correctamente. El registro automático vuelve a ocurrir siempre que la máquina virtual siga existiendo y tenga adjunta una dirección IP privada. El registro de DNS se vuelve a crear de manera automática en la zona.

## <a name="what-happens-when-we-unlink-a-linked-virtual-network-from-a-private-zone-will-the-automatically-registered-virtual-machine-records-from-the-virtual-network-be-removed-from-the-zone-too"></a>¿Qué ocurre si desvinculamos una red virtual vinculada de una zona privada? ¿Los registros de máquina virtual registrados automáticamente de la red virtual también se quitarán de la zona?

Sí. Para desvincular una red virtual vinculada de una zona privada, debe actualizar la zona DNS para quitar el vínculo de la red virtual asociada. En este proceso, los registros de máquina virtual que se registraron automáticamente se quitan de la zona.

## <a name="what-happens-when-we-delete-a-linked-virtual-network-thats-linked-to-a-private-zone-do-we-have-to-manually-update-the-private-zone-to-unlink-the-virtual-network-as-a-linked-virtual-network-from-the-zone"></a>¿Qué ocurre cuando se elimina una red virtual vinculada a una zona privada? ¿Es necesario actualizar manualmente la zona privada para desvincular la red virtual vinculada de la zona?

Sí. Cuando se elimina una red virtual vinculada sin desvincularla primero de una zona privada, la operación de eliminación se realiza correctamente, pero la red virtual no se desvinculará de manera automática de la zona privada, si la hubiera. Debe desvincular manualmente la red virtual de la zona privada. Es por este motivo que debe desvincular la red virtual de la zona privada antes de eliminarla.

## <a name="will-dns-resolution-by-using-the-default-fqdn-internalcloudappnet-still-work-even-when-a-private-zone-for-example-privatecontosocom-is-linked-to-a-virtual-network"></a>¿Funcionará la resolución de DNS con el valor de FQDN predeterminado (internal.cloudapp.net) aunque una zona privada (por ejemplo, private.contoso.com) esté vinculada a una red virtual?

Sí. Las zonas privadas no reemplazan las resoluciones DNS predeterminadas mediante el uso de la zona internal.cloudapp.net proporcionada por Azure. Se ofrece como característica o mejora adicional. Ya sea si confía en la zona internal.cloudapp.net proporcionada por Azure o en su propia zona privada, use el nombre de dominio completo de la zona en la que quiere la resolución.

## <a name="will-the-dns-suffix-on-virtual-machines-within-a-linked-virtual-network-be-changed-to-that-of-the-private-zone"></a>¿Se cambiará el sufijo DNS en las máquinas virtuales de una red virtual vinculada por el de la zona privada?

No. El sufijo DNS de las máquinas virtuales en la red virtual vinculada permanecerá como el sufijo predeterminado proporcionado por Azure ("*.internal.cloudapp.net"). En las máquinas virtuales, dicho sufijo DNS se puede cambiar manualmente por el de la zona privada.

## <a name="what-are-the-usage-limits-for-azure-private-dns"></a>¿Cuáles son los límites de uso de Azure DNS privado?

Cuando se usa Azure DNS privado, se aplican los siguientes límites predeterminados.

| Recurso | Límite predeterminado |
| --- | --- |
|Zonas DNS privadas por suscripción|1000|
|Conjuntos de registros por zona DNS privada|25 000|
|Registros por conjunto de registros|20|
|Vínculos de red virtual por zona DNS privada|1000|
|Vínculos de red virtual por zonas DNS privadas con el registro automático habilitado|100|
|Número de zonas DNS privadas a la que se puede vincular una red virtual con el registro automático habilitado|1|
|Número de zonas DNS privadas a la que se puede vincular una red virtual|1000|

## <a name="is-there-portal-support-for-private-zones"></a>¿Admite Azure Portal las zonas privadas?

Sí, y las zonas privadas ya creadas mediante las API, PowerShell, la CLI y los SDK son visibles en Azure Portal.

## <a name="why-dont-my-existing-private-dns-zones-show-up-in-new-portal-experience"></a>¿Por qué no aparecen las zonas DNS privadas existentes en la nueva experiencia del portal?

Como parte de la versión de actualización de vista previa, se incluye un nuevo modelo de recursos para las zonas DNS privadas. Las zonas DNS privadas existentes deben migrarse al nuevo modelo de recursos para que se muestren en la nueva experiencia del portal. Vea a continuación cómo migrar al nuevo modelo de recursos.

## <a name="how-do-i-migrate-my-existing-private-dns-zones-to-the-new-model"></a>¿Cómo se migran las zonas DNS privadas existentes al nuevo modelo?
Se recomienda realizar la migración al nuevo modelo tan pronto como sea posible. Aunque se admitirá el modelo de recursos heredado, no se desarrollarán más características basadas en él. En el futuro, está previsto dejar de usarlo en favor del nuevo modelo de recursos. Para instrucciones sobre cómo migrar las zonas DNS privadas al nuevo modelo de recursos, consulte la [guía de migración de las zonas privadas de Azure DNS](private-dns-migration-guide.md).

## <a name="next-steps"></a>Pasos siguientes

- [Más información sobre Azure DNS privado](private-dns-overview.md)