# blueprint_hilo
Blueprint permettant de configurer la température des thermostats selon le statut du sensor defi hilo. L'intégration HACS Hilo avec Home Assistant est requise pour utiliser ce blueprint. (https://github.com/dvd-dev/hilo)

Options de ce Blueprint:

1- Choisir la liste des thermostats à gérer avant, pendant et après les défis. (Obligatoire) 
2- Choisir, si nécessaire, les entitées de type switch à éteindre durant le défi. Ceci peut inclure par exemple un contrôleur de chauffe-eau ou un échangeur d'air. (Optionnel)
3- Choisir, si nécessaire, les entitées de type light à éteindre durant le défi. (Optionnel)
4- Choisir, si nécessaire, les entitées de type fan à éteindre durant le défi. (Optionnel)
5- Choisir si les entitées de type switch doivent être rallumés après le défi. Si aucune entitée de ce type n'est sélectionné au point 2, cette option n'est pas utilisée.
6- Choisir si les entitées de type light doivent être rallumés après le défi. Si aucune entitée de ce type n'est sélectionné au point 2, cette option n'est pas utilisée.
7- Choisir le les entitées de type fan doivent être rallumés après le défi. Si aucune entitée de ce type n'est sélectionné au point 2, cette option n'est pas utilisée.
8- Choisir les températures à configurer les thermostats selon les différents statuts du sensor.defi_hilo
    a- La période d'ancrage est une période précédent la période de pré-chauffage. (si l'option est choisie dans la configuration de l'intégration Hilo, voir la documentation de l'intégration pour plus de détails). Cette période est utilisée afin de calculer la consommation à éliminer durant le défi.
    b- La période de pré-chauffage est une période de 2 heures précédent le défi. C'est la période durant laquelle on augmente la température des thermostats pour garder une chaleur confortable durant le défi.
    c- La période de réduction et le défi à proprement parlé. C'est la période de 4 heures où on diminue la température des thermostats afin de réduire notre consommation au maximum. C'est aussi durant cette période que les entitées de type switch, light et fan sélectionnées aux étapes 2,3 et 4 sont éteintes.
    d- La période de reprise est la période d'une heure suivant le défi. Il est important, pour ne pas surcharger le réseau suivant le défi de remettre la température normale. La consommation après un défi doit se faire de façon graduelle.
    e- La période de complétion est la période où le défi est complètement terminé. C'est le retour de la température normale de la maison.

Automatisme de secours:

Comme le sensor defi_hilo est dépendant de la connexion Internet avec l'interface Hilo, si un problème survenait à votre connexion durant le processus du défi, il est possible que vous ne soyez pas en mesure de faire fonctionner les automatismes basés sur le statut de ce sensor. Ce Blueprint permet dans ce cas, d'utiliser les attributs du sensor afin de démarrer les différentes phases comme soupape de secours. 

En effet, lorsqu'un défi se prépare bientôt, le sensor se met en mode "scheduled". Durant cette période, le sensor est mis à jour avec des attributs de date/heure des différentes phase du défi à venir. Lorsqu'il est en mode scheduled, un automatisme va prendre ces attributs et les assigner à des triggers de secours. Ainsi, si le sensor n'est pas en mesure, par exemple, de se modifier au mode de pré-chauffage, le trigger de secours qui contient la date/heure du début de la période de pré-chauffage se déclenchera et prendra les températures configurées durant la période de pré-chauffage.