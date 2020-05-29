
# Ansible pour les experts réseaux.

Je vous propose une série d&#39;articles pour savoir utiliser Ansible, outil devenu incontournable pour automatiser l&#39;administration et l&#39;exploitation de réseaux.

Mon but est de donner les clés pour se lancer sur Ansible et en découvrant une à une les différentes notions plutôt que devoir se confronter à plusieurs en même temps, ce qui m&#39;est arrivé quand j&#39;ai découvert ce logiciel. Au fond, j&#39;essaie d&#39;écrire le guide qui m&#39;a manqué pour apprendre Ansible plus rapidement.

Ma méthode est de partir d&#39;une instruction élémentaire qui fonctionne, de l&#39;analyser et d&#39;en étendre très progressivement la portée.

# Introduction : pourquoi Ansible (et pas un autre)

Ansible est un des nombreux logiciels open source permettant de contrôler à distance un parc de serveurs. Toutefois, dans le domaine de l&#39;administration réseaux, Ansible a pris l&#39;ascendant sur ses concurrents : Chef, Puppet, Salt, …

Pour nous, administrateurs réseaux, la principale force d&#39;Ansible est d&#39;être agentless : Ansible demande seulement aux serveurs distants d&#39;avoir un serveur SSH actif pour pouvoir fonctionner, contrairement à ses concurrents qui installent des relais (agents) sur les distants. Ansible adresse donc parfaitement le domaine des réseaux où les équipements s&#39;administrent en SSH, mais sur lesquels il est bien souvent impossible d&#39;installer un logiciel.

Une autre force d&#39;Ansible est d&#39;avoir choisi Jinja2 comme langage de templating. Nous le verrons plus en détail, mais ce langage est un bon compromis entre puissance et complexité. Son apprentissage est rapide et sa puissance suffisante.

Enfin, comme pour tout développement Open Source qui se démarque, un grand soin a été apporté à la documentation

Par la suite, de nombreux modules ont été développés pour administrer les équipements réseaux et maintenant, Ansible est devenu une option incontournable pour automatiser son administration ou son exploitation.

Cette série d&#39;articles vous propose de découvrir les bases d&#39;Ansible en automatisant des tâches simples sur des routeurs Cisco. Nous allons nous lancer à l&#39;eau dès le premier article en exécutant une commande à distance sur un seul équipement, puis itérer cette commande sur plusieurs équipements, ajouter et organiser des paramètres, et enfin lier des commandes entre elles. 

Le but sera atteint si vous êtes devenus suffisamment autonomes pour poursuivre vous-mêmes l&#39;apprentissage d&#39;Ansible.


<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQ5MDUyNDY4OF19
-->