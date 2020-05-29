---


---

<h1 id="jour-ii--comprendre-la-commande-exécutée">Jour II : Comprendre la commande exécutée</h1>
<p>Dans l'article précédent, nous avons pu à partir d'une session linux exécuter à distance un show clock sur un routeur : c'est Ansible qui s'est connecté en SSH sur le routeur et a lancé la commande. Il a ensuite fermé la connexion et nous a rendu la main.</p>
<blockquote>
<p>A la base, Ansible est un automate qui exécute des commandes à distance via SSH.</p>
</blockquote>
<p>La commande était la suivante :</p>
<pre><code>ansible all --inventory="csrv1k-230," --module-name=raw --args="show clock" \
 --extra-vars="ansible_host=10.0.0.230 ansible_user=cisco ansible_ssh_pass=cisco" \
 --ssh-extra-args="-o StrictHostKeyChecking=no"
</code></pre>
<p>Et maintenant, nous allons procéder au reverse engineering de cette commande pour bien la comprendre. En d'autres mots, que vient-on de faire ?</p>
<p>En ligne 2, nous retrouvons sans surprise les paramètres identifiés quand nous avons lancé la commande à la main. Ceux-ci sont toutefois encapsulés dans le paramètre extra-vars et sont passés à Ansible via des noms de variable pré-définis :</p>

<table>
<thead>
<tr>
<th></th>
<th></th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>ansible_host</strong></td>
<td>10.0.0.230</td>
</tr>
<tr>
<td><strong>ansible_user</strong></td>
<td>cisco</td>
</tr>
<tr>
<td><strong>ansible_ssh_pass</strong></td>
<td>cisco</td>
</tr>
</tbody>
</table><p>A noter que les noms de variable <em>ansible_ssh_pass</em> et <em>ansible_user</em> ont évolué et dépendent des versions d'Ansible.</p>
<p>Voici la liste des autres paramètres passés :</p>

<table>
<thead>
<tr>
<th><strong>Nom long</strong></th>
<th><strong>Nom court</strong></th>
<th><strong>description</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td><em>–module name</em></td>
<td>-m</td>
<td>fonction (au sens Ansible) à exécuter</td>
</tr>
<tr>
<td><em>–args</em></td>
<td>-a</td>
<td>paramètres du module</td>
</tr>
<tr>
<td><em>–inventory</em></td>
<td>-i</td>
<td>La liste des hosts distants</td>
</tr>
<tr>
<td><em>–extra-vars</em></td>
<td>-e</td>
<td>informations additionnelles</td>
</tr>
<tr>
<td><em>–ssh-extra-args</em></td>
<td></td>
<td>Paramétrage de la connexion ssh</td>
</tr>
<tr>
<td><em>all</em></td>
<td></td>
<td>sélections des sites</td>
</tr>
</tbody>
</table><h2 id="ansible.cfg">Ansible.cfg</h2>
<p>Réglons tout de suite le sort du paramétrage de ssh. Nous avons utilisé :</p>
<p>–ssh-extra-args="-o StrictHostKeyChecking=no"</p>
<p>Si nous enlevons ce paramètre, il est probable qu'il ne se passe rien, mais si vous supprimez le fichier des hosts SSH avec lesquels les clefs publiques ont été échangées (~/.ssh/known_hosts), Ansible refuse d'établir la connexion SSH :</p>
<p><img src="../images/screenshot003.png" alt="screenshot003"></p>
<p>Ce choix se défend pour la gestion de serveurs, ce qui, ne l'oublions pas, est le domaine d'Ansible, mais il est plus discutable pour gérer des équipements réseaux et, incontestablement, il va nous embarrasser dans notre environnement de lab.</p>
<p>Aussi nous allons configurer notre station ansible pour qu'elle accepte de se connecter à des hosts SSH quelconques.</p>
<p>Ansible récupère sa configuration dans le fichier</p>
<pre><code>/etc/ansible/ansible.cfg
</code></pre>
<p>et la retouche avec le fichier du répertoire courant, s'il existe :</p>
<pre><code>ansible.cfg
</code></pre>
<p>Ces fichiers sont organisés avec la syntaxe INI :</p>
<pre><code>[section]
parameter=value
</code></pre>
<p>Plutôt que de modifier le fichier partagé par tous les utilisateurs, nous allons créer le fichier ansible.cfg, pour désactiver le contrôle des clefs publiques :</p>
<pre><code>$ echo -e "[defaults]\nhost_key_checking = False\n" &gt; ansible.cfg
</code></pre>
<p>De plus, ceux qui utilisent un terminal de fond sombre apprécieront de pouvoir remplacer la colorisation bleu foncé des logs par une couleur plus lisible avec la configuration suivante :</p>
<pre><code>$ echo -e "[colors]\nverbose = bright magenta\n" &gt;&gt; ansible.cfg
</code></pre>
<p><img src="../images/screenshot004.png" alt="screenshot004"></p>
<p>Nous avons retrouvé l'environnement de lab qui nous convient et sommes enfin prêts à poursuivre notre tour d'Ansible. J'entends bien les plus râleurs dire « C'est pas trop tôt », mais ils conviendront aussi qu'on apprend mieux quand on est bien installé !</p>
<h2 id="la-notion-de-module">La notion de module</h2>
<p>Ansible est développé de façon modulaire. Le moteur délègue les tâches à des modules spécialisés. A ce jour il existe plus de 3000 modules supportés par Ansible (<a href="https://docs.ansible.com/ansible/latest/modules/list_of_all_modules.html">https://docs.ansible.com/ansible/latest/modules/list_of_all_modules.html</a>).</p>
<p>Dans notre exemple, nous avons utilisé le module le plus simple <em>raw</em> (<a href="https://docs.ansible.com/ansible/latest/modules/raw_module.html">cf https://docs.ansible.com/ansible/latest/modules/raw_module.html</a>), qui prend un paramètre [show clock] et l'envoie à travers une session SSH.</p>
<p>Ou en prenant le problème à l'envers, la commande à exécuter est passée via l'argument –-arg [-a] qui sera passé par Ansible au module raw précisé par l'argument –module-name [-m].</p>
<p>Soit, avec les notations courtes :</p>
<p><img src="../images/module001.png" alt="module001"></p>
<p>Notre module raw a donc :</p>
<ul>
<li>établi une connexion SSH avec le routeur et envoyé la chaîne « show clock », que le routeur a interprétée</li>
<li>lu la sortie renvoyée par le routeur</li>
<li>fermé la connexion</li>
<li>retourné la sortie du routeur à Ansible</li>
</ul>
<p>Même si d'autres modules sont plus pertinents pour l'administration réseau, on voit facilement qu'avec ce module élémentaire, on peut lancer bon nombre de commandes sans quitter son terminal linux.</p>
<p>Les données (nom du routeur, adresse IP, compte, …) sont passées au module directement par Ansible, car Ansible considère à raison que ces paramètres appartiennent au moteur, non au module, établissant de ce fait un cloisonnement efficace entre les données et les actions (ce qui fait ma transition vers le prochain article).</p>
<p><a href="part_03.md">next</a></p>

