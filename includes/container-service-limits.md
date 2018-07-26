| Recurso | Límite predeterminado |
| --- | :--- |
| Máximo de nodos por clúster | 100 |
| Número máximo de pods por nodo ([red básica con Kubenetes][basic-networking]) | 110 |
| Número máximo de pods por nodo ([red avanzada de Azure CNI][advanced-networking]) | 30<sup>1</sup> |
| Máximo de clústeres por suscripción | 100 |

<sup>1</sup> este valor se puede personalizar a través de la implementación de la plantilla de ARM. Puede consultar ejemplos [aquí][arm-deployment-example].<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/networking-overview.md#basic-networking
[advanced-networking]: ../articles/aks/networking-overview.md#advanced-networking

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
[arm-deployment-example]: https://github.com/Azure/AKS/blob/master/examples/vnet/02-aks-custom-vnet.json#L64-L69
