---
title: "Mort et vie du MacBook 2011"
author: "bcalvino"
date: 2017-11-07T23:04:12-03:00
keywords: macbook pro, dGPU, arch linux, efi vars, force iGPU, hack of the year, hors Temer
---

Troisième publication : mon courageux _macbook pro 15' early 2011_ est décédé. Jour triste. Je l'ai acheté à l'Apple Store nationale en décembre 2011. Il a déjà enduré beaucoup de choses, y compris une chute qui a entraîné le remplacement de l'écran LCD. En 2013, le fameux problème de la carte graphique AMD (_discrete GPU, dGPU_) a frappé mon MBP pour la première fois. Sans aucune couverture de service après-vente (je n'ai pas souscrit à une extension de garantie, une erreur, et à l'époque, le rappel d'Apple n'avait pas encore commencé), et fuyant les prix exorbitants des nouveaux macbooks avec écran Retina (il était impossible d'en acheter un nouveau à l'époque), je n'avais d'autre choix que de payer l'équivalent de 800 dollars pour une nouvelle carte logique.

J'ai contacté le service après-vente (je les ai toujours choisis depuis des années, je leur fais entièrement confiance), j'ai attendu un délai interminable d'environ un mois pour recevoir la carte, et j'étais rapidement de retour avec mon MBP de combat. Mais pas cette fois. Il y a près d'un mois, le problème de la dGPU est revenu. Quatre ans sans aucun problème, c'était une période bien plus longue que ce que la plupart des gens rapportent après le remplacement de la carte logique. Mais finalement, c'était terminé. Je l'ai emmené chez un centre de services agréé Apple (le même qu'avant), mais la machine n'a même pas passé la porte correctement. On m'a informé que les modèles de 2011 ne sont plus pris en charge car ils sont considérés comme "obsolètes" par Apple. Prochaine tentative : je l'ai emmené dans un centre de services non agréé (recommandé par le personnel de l'agrée). Après quelques jours, j'ai reçu une confirmation par e-mail qu'il s'agissait bien du problème de la dGPU et qu'ils ne pouvaient pas le résoudre car leur fournisseur ne fournissait plus de pièces de rechange pour cette carte. Apple avait-elle trouvé un moyen définitif de forcer l'obsolescence programmée? Enfin, il y a 4 jours, j'ai récupéré mon macbook pro, désormais un poids de papier très coûteux...

Ma prochaine décision a été : puisque personne ne veut résoudre le problème, je vais le faire moi-même ! Je vais acheter une pièce d'occasion et la remplacer moi-même. Même avec l'incertitude de peut-être obtenir une pièce avec une durée de vie réduite et sans savoir si mes compétences de "faites-le vous-même" seront suffisantes, j'étais déterminé. La première étape a été de rechercher sur Google. Comme j'avais modifié le système d'exploitation du MBP et essayé d'installer un système d'exploitation Linux, j'ai cherché des alternatives utilisant une distribution Linux sur le MBP. À ma surprise, je suis tombé sur cette discussion du forum MacRumors. Initialement créée par @AppleMacFinder en mars de cette année, elle montrait une *solution 100% fonctionnelle* pour ressusciter un MBP 2011 avec une carte graphique défectueuse. L'auteur, apparemment basé en Russie, a présenté une méthode simple et efficace pour forcer le MBP à isoler la dGPU et à fonctionner uniquement avec la carte graphique intégrée du processeur Intel (iGPU). Je n'ai pas attendu plus longtemps et j'ai essayé immédiatement.

Le procédé :

1- J'ai créé un disque de démarrage (CD/clé USB) d'Arch Linux, choisi car il n'a pas d'interface graphique. J'ai fait cela sur un ordinateur portable Windows 10.

2- J'ai démarré le MBP avec Arch Linux en maintenant la touche *Option* enfoncée pendant le démarrage et en choisissant le disque (_EFI boot_) à l'écran qui apparaît ensuite. Lorsque le démarrage a commencé, j'ai appuyé sur la touche *E* pour accéder au GRUB et éditer la ligne de commande qui est apparue juste en dessous, en ajoutant `nomodeset` à la fin. Ensuite, j'ai poursuivi le processus en appuyant sur *Entrée*.

3- Le système d'exploitation a démarré et a affiché une interface en ligne de commande Linux. Comme le système de fichiers _efivarfs_ est monté par défaut, il est possible de modifier les variables EFI en remontant d'abord le lecteur pour autoriser l'écriture (cette étape a été créditée à l'utilisateur @totoe_84) :

```bash
# cd /
# umount /sys/firmware/efi/efivars/
# mount -t efivarfs rw /sys/firmware/efi/efivars/
# cd /sys/firmware/efi/efivars/
```

4- Vérifiez le dossier `/sys/firmware/efi/efivars`, s'il y a un fichier `gpu-power-prefs-<UUID>`, il doit être supprimé. Dans mon cas, ce fichier n'existait pas.

5- J'ai sauté certaines recommandations en cas de problèmes d'affichage ou si d'autres fichiers de configuration étaient présents (il est bon de vérifier attentivement la discussion originale) et j'ai continué en créant une nouvelle configuration en utilisant n'importe quelle UUID (l'utilisateur @AppleMacFinder l'a pris à partir d'une carte physique) :

```bash
# printf "\x07\x00\x00\x00\x01\x00\x00\x00" > /sys/firmware/efi/efivars/gpu-power-prefs-fa4ce28d-b62f-4c99-9cc3-6815686e30f9
# chattr +i "/sys/firmware/efi/efivars/gpu-power-prefs-fa4ce28d-b62f-4c99-9cc3-6815686e30f9"
# cd /
# umount /sys/firmware/efi/efivars/
# reboot
```

D'autres méthodes ont été introduites dans la discussion, mais j'ai d'abord fait cela. *Comme par magie, mon cher MBP est revenu à la vie !* Comme j'avais effacé le système d'exploitation lors de mes nombreuses tentatives d'autodiagnostic avant de l'envoyer au centre de service agréé, j'ai utilisé le redémarrage en appuyant sur les touches *Option + Commande + R* pour déclencher la récupération sur Internet d'Apple. La procédure a fonctionné parfaitement (auparavant, elle se figeait en cours de route) et a téléchargé le dernier système d'exploitation (High Sierra), qui a été installé et redémarré. Tout était parfait ! Ou presque. Lors du processus de démarrage de High Sierra, lorsque l'ordinateur a redémarré à la fin, il s'est figé à nouveau. Après avoir lu davantage la même discussion sur le forum, j'ai découvert qu'il serait nécessaire d'isoler un fichier de configuration qui tentait d'utiliser le dGPU d'AMD. Pour cela, j'ai utilisé le [guide](https://forums.macrumors.com/threads/force-2011-macbook-pro-8-2-with-failed-amd-gpu-to-always-use-intel-integrated-gpu-efi-variable-fix.2037591/page-35#post-24956091) de l'utilisateur @MikeyN, qui fait quelque chose de très similaire à la méthode que j'ai utilisée au début, mais par un autre chemin :

1- J'ai réinitialisé le SMC et NVRAM en appuyant sur *Option + Contrôle + Majuscule gauche + Power*, puis en les relâchant et en démarrant avec *Option + Commande + P + R* enfoncées.

2- Après avoir entendu le son de démarrage deux fois, j'ai appuyé sur *Commande + R + S* pour démarrer en mode _Recovery_. Ensuite, j'ai désactivé le SIP et la dGPU :

```bash
# csrutil disable
# nvram fa4ce28d-b62f-4c99-9cc3-6815686e30f9:gpu-power-prefs=%01%00%00%00
# nvram boot-args="-v"
# reboot
```

3- J'ai redémarré en mode utilisateur unique en appuyant sur *Commande + S*. Ensuite, j'ai monté la partition _root_ et déplacé le fichier _infracteur_ vers un dossier spécialement créé.

```bash
# /sbin/mount -uw /
# mkdir -p /System/Library/Extensions-off
# mv /System/Library/Extensions/AMDRadeonX3000.kext /System/Library/Extensions-off/
# touch /System/Library/Extensions/
# nvram boot-args=""
# reboot
```

Il y avait quelques étapes supplémentaires pour éviter les problèmes que certains utilisateurs ont déjà rencontrés avec la gestion de l'alimentation et d'autres sous-systèmes. Mais jusqu'à présent, je n'ai pas eu besoin d'ajouter d'autres _hacks_ à mon MBP. Et voilà ! Mon MBP était là, avec un système mis à jour vers High Sierra et prêt à être utilisé ! J'ai déjà installé VirtualBox, Docker, Ruby, Bundler, plusieurs Gems, téléchargé mes référentiels Git, géré mes fichiers _dicom_ qui prennent beaucoup d'espace, et tout va bien jusqu'à présent. Merci à @AppleMacFinder, @totoe_84, @MikeyN et à tous les utilisateurs du forum MacRumors qui ont contribué à créer cette solution ingénieuse qui permet de continuer à utiliser un MBP avec un défaut de carte graphique.

Mise à jour (11/12/17) : J'ai installé la mise à jour 10.13.2 de High Sierra et cela semble avoir annulé les modifications du _hack_. L'installation s'est arrêtée à mi-chemin et le MacBook est entré en boucle de redémarrage. J'ai simplement suivi à nouveau toutes les étapes de la deuxième partie de la description ci-dessus. L'installation a pu se poursuivre et le MacBook a retrouvé un fonctionnement normal. Chaque mise à jour du système comporte le risque de casser à nouveau le _hack_, peut-être de manière irréparable avec la méthode décrite. Ainsi, je ne ferai plus de mises à jour. Cependant, je recommande à tous de mettre à jour vers [High Sierra 10.13.2](https://support.apple.com/fr-fr/HT208331), car elle corrige une grave vulnérabilité de sécurité découverte le 28/11/17, qui permettait un accès root à n'importe qui sans nécessiter de mot de passe (o.O).

Mise à jour (07/02/18) : J'ai installé la mise à jour 10.13.3 de High Sierra, recommandée pour protéger le système contre la vulnérabilité Spectre. Le même problème s'est produit et j'ai dû appliquer à nouveau le _hack_, avec une nouvelle fois succès. Je n'ai pas l'intention de faire d'autres mises à jour, car elles pourraient compromettre définitivement le système.
