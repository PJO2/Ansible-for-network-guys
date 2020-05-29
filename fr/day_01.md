---


---

<h1 id="jour-1--faire-un-show-clock-sur-un-routeurswitch">Jour 1 : Faire un show clock sur un routeur/switch</h1>
<p>Comme annoncé, nous allons commencer tout petit : nous sommes sur une session Unix et notre but est d'exécuter la commande « <em>show clock</em> » en pilotant à distance un équipement Cisco. Si vous disposez d'un autre équipement, vous pouvez évidemment substituer le show clock par toute autre commande brève.</p>
<p>En automatisation, une bonne habitude est de faire le travail au moins une fois à la main. Aussi, nous nous plaçons sur notre station linux et exécutons les commandes suivantes :</p>
<pre><code>$ ssh -l cisco 10.0.0.230
Warning: Permanently added '10.0.0.230' (RSA) to the list of known hosts.
Password:

csrv1k-230#show clock
*15:29:59.820 UTC Tue May 26 2020
csrv1k-230#
csrv1k-230#exit
Connection to 10.0.0.230 closed.
$
</code></pre>
<p>Arrêtons-nous quelques instants sur cette requête de routine pour tout expert réseau. Nous avons eu à fournir les données suivantes :</p>

<table>
<thead>
<tr>
<th></th>
<th></th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>10.0.0.230</strong></td>
<td>L'adresse IP de mon routeur</td>
</tr>
<tr>
<td><strong>show clock</strong></td>
<td>La commande à exécuter (bon là je ne vous apprends rien)</td>
</tr>
<tr>
<td><strong>cisco</strong></td>
<td>Le compte utilisé</td>
</tr>
<tr>
<td><strong>cisco</strong></td>
<td>Le password</td>
</tr>
</tbody>
</table><p>Nous avons aussi reçu une notification du système sur l'échange des clefs publiques via la session SSH.</p>
<p>Evidemment, toutes ces informations devront être passées en paramètre à Ansible (et non, il ne les devine pas tout seul !).</p>
<h2 id="installer-ansible">Installer ansible</h2>
<p>Je vais passer rapidement sur l'installation d'Ansible, car cette partie est très bien documentée par ailleurs, et, à part peut-être montrer comment installer une version spécifique, ma valeur ajoutée devrait être faible.</p>
<h3 id="linstallation">L'installation</h3>
<p>Il faut disposer a minima d'un linux en mode serveur. Si vous ne pouvez pas installer une VM sous vmWare, une excellente alternative est d'installer VirtualBox sur un PC Windows ou sur un MacBook et de créer sa VM sous VirtualBox. Pour des tests, un système avec 2gb de RAM et 16Gb de disque suffit amplement.</p>
<p>Par <s>fainéantise</s> habitude, j'utilise la distribution debian, mais ubuntu convient également et CentOS est probablement le meilleur choix, puisque Ansible, comme CentOS sont maintenus par la même société.</p>
<p>Une fois connecté sur la VM, on lance l'installation :</p>
<pre><code>$ apt-get install ansible
</code></pre>
<p>Ou</p>
<pre><code>$ yum install ansible
</code></pre>
<p>Cette procédure prend en charge l'installation de plusieurs applications :</p>

<table>
<thead>
<tr>
<th></th>
<th></th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>python</strong></td>
<td>langage de programmation de Ansible</td>
</tr>
<tr>
<td><strong>pip</strong></td>
<td>utilitaire de mise à jour des librairies python</td>
</tr>
<tr>
<td><strong>jinja2</strong></td>
<td>un langage de templating codé en python</td>
</tr>
<tr>
<td><strong>ansible</strong></td>
<td>ça on sait</td>
</tr>
<tr>
<td><strong>modules standard d'ansible</strong></td>
<td>Une collection de modules</td>
</tr>
</tbody>
</table><h3 id="la-mise-à-jour">La mise à jour</h3>
<p>La version installée d'Ansible dépend de la distribution linux installée. On le vérifie en tapant :</p>
<pre><code>$ ansible --version
</code></pre>
<p>L'utilisation du module <em>cli_command</em> nécessite la version 2.7. Si le package installé est inférieur, on procède à un upgrade via pip :</p>
<pre><code>$ pip install –upgrade "ansible==2.7.8"
</code></pre>
<h2 id="ma-toute-première-commande">Ma toute première commande</h2>
<p>La commande à exécuter est la suivante :</p>
<pre><code>$ ansible all --inventory="csrv1k-230," --module-name=raw --args="show clock" \
 --extra-vars="ansible_host=10.0.0.230 ansible_user=cisco ansible_ssh_pass=cisco" \
 --ssh-extra-args="-o StrictHostKeyChecking=no"
</code></pre>
<p>Bien entendu, l'adresse IP du routeur doit être changée, ainsi qu'éventuellement l'authentification SSH.</p>
<p>On fera aussi bien attention à la virgule qui termine le paramètre inventory.</p>
<p><img src="../images/screenshot001.png" alt="screenshot001"></p>
<p>Nous pouvons également lancer cette même commande en mode verbose, en ajoutant l'argument –vvv :</p>
<p><img src="../images/screenshot002.png" alt="screenshot002"></p>
<p>Nous voyons bien notre commande exécutée correctement même si la mise en forme de la réponse nous déçoit un peu.</p>
<p><a href="part_02.md">next</a></p>

