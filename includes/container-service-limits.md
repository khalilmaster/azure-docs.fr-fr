| Ressource | Limite par défaut |
| --- | :--- |
| Nœuds maximum par cluster | 100 |
| Pods maximum par nœud ([réseau de base avec Kubenet][basic-networking]) | 110 |
| Pods maximum par nœud ([mise en réseau avancée avec Kubenet][advanced-networking]) | 30<sup>1</sup> |
| Clusters maximum par abonnement | 20<sup>2</sup> |

<sup>1</sup> Il est possible de personnaliser cette valeur par le biais d’un déploiement de modèle ARM. Consultez les exemples [ici][arm-deployment-example].<br />
<sup>2</sup> Créer une [demande de support Azure][azure-support] pour demander une augmentation de la limite.<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/networking-overview.md#basic-networking
[advanced-networking]: ../articles/aks/networking-overview.md#advanced-networking

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
[arm-deployment-example]: https://github.com/Azure/AKS/blob/master/examples/vnet/02-aks-custom-vnet.json#L64-L69
