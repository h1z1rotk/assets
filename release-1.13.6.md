# Assets 1.13.6 — passation de publication

Les deux archives de la release **brouillon** `assets-v1.13.6` sont les assets
préparés pour afficher les deux personnages du duo dans le menu, avec l'apparence
du coéquipier. Les manifestes correspondants sont dans cette branche. La mise en
ligne reste à effectuer par l'opérateur ; fusionner le code seul ne l'active pas.

## Changements associés

- [Serveur #541](https://github.com/h1z1rotk/returnoftheking/pull/541), fusionnée : photos Steam de tous les amis du Social, au-delà du duo.
- [Serveur #544](https://github.com/h1z1rotk/returnoftheking/pull/544), fusionnée : personnage du coéquipier et synchronisation de sa tenue.
- [Launcher #57](https://github.com/h1z1rotk/rotk-launcher/pull/57), fusionnée : liste native des deux personnages du menu.
- [Launcher #56](https://github.com/h1z1rotk/rotk-launcher/pull/56), fusionnée : version 2.0.14, qui contient aussi #57. Un brouillon `v2.0.14` existe déjà ; reprendre sa publication plutôt que recréer le tag.
- [Serveur #543](https://github.com/h1z1rotk/returnoftheking/pull/543), encore ouverte au contrôle du 18 septembre 2026 : protocole d'attestation du launcher 2.0.14 et modes `clean` / `patched`. À intégrer et déployer avant de distribuer ce launcher.

## Ordre pour terminer

1. **Préparer le serveur.** Intégrer #543, vérifier que le commit de release contient #541 et #544, puis incrémenter la version serveur avant le nouveau tag. Au contrôle, `main` et le dernier tag publié sont encore en 1.3.133 ; choisir la prochaine version libre et respecter la CI habituelle. Le tag serveur de production déclenche le déploiement de la flotte et les redémarrages prévus par son workflow.

2. **Préparer la politique d'intégrité privée pour les assets 1.13.6.** Utiliser `base-manifest.v1.json`, `feed.json` et `asset-payloads.v1.json` de cette branche, ainsi que les véritables DLL et sidecars de chaque launcher qui doit rester admis. Recalculer les racines de toutes ces versions, dont les modes `clean` et `patched` de 2.0.14 si le patch client est distribué. Le point d'entrée existant est `rotk-web/scripts/publish-attestation-policy.mjs` dans le dépôt serveur ; ses options et les règles d'admission sont documentées dans `docs/LAUNCHER_VERSION_POLICY.md`. Préparer et vérifier les calculs hors ligne avant la bascule, puis valider le contrat publié. Préserver les versions admises, le mode d'enforcement, les autres champs actifs et les clés. Les racines de la migration 0084 ont été calculées pour les assets 1.13.5 : **elles ne conviennent pas aux packs 1.13.6**. Publier une nouvelle politique immuable adaptée après les migrations ; ne pas recopier les anciennes racines ni compter uniquement sur 0084. Garder ce document privé côté serveur.

3. **Déployer le serveur compatible** avec son workflow normal, migrations comprises. Ne pas exposer le launcher 2.0.14 avant le serveur prenant en charge son challenge signé. Si un autre déploiement a déjà eu lieu, vérifier le commit réellement actif avant toute nouvelle action.

4. **Publier les assets et leur politique dans la même fenêtre.** Vérifier qu'aucune publication concurrente n'a remplacé la base 1.13.5, puis publier le brouillon `assets-v1.13.6` comme release stable/latest. Fusionner immédiatement la PR de cette branche pour exposer les deux manifestes sur `main`, et appliquer la politique privée 1.13.6 préparée à l'étape 2 après validation du feed public. Publier la release avant la fusion évite des URLs d'archives encore en 404. Attention : le launcher consulte aussi la dernière release stable ; la publication de la release peut déjà déclencher l'installation des packs, même avant la fusion du feed. Cette étape doit donc être coordonnée avec la politique, pas réalisée isolément.

5. **Admettre et publier le launcher 2.0.14.** Vérifier les artefacts du brouillon existant, sa CI et ses hashes, enregistrer ses racines exactes dans la politique puis l'activer dans **Launcher Logs → Launcher Version Policy**. Publier ensuite la release launcher avec l'installateur, ses fichiers de mise à jour et `latest.yml`. La fusion de #56 ne distribue pas à elle seule cette version.

6. **Activer la scène duo sur les workers de menu.** Mettre `H1Z1_MENU_DUO_SCENE=1` dans la configuration d'environnement réellement chargée par chaque worker de menu concerné, sur EU, NA, SEA et AU. Les unités `rotk-menu@.service` chargent `/etc/rotk/roles/menu-%i.env` après les fichiers communs ; vérifier les éventuelles valeurs qui écraseraient le flag. Faire prendre en compte cette configuration lors du déploiement prévu ou d'un redémarrage contrôlé des menus. Un changement de fichier d'environnement seul ne modifie pas les processus déjà lancés. Le serveur dédié aux parties Duos n'héberge pas ces workers de menu. Aucun flag supplémentaire n'est requis pour les photos Steam des amis de #541.

7. **Vérifier sur deux clients à jour.** Fermer le jeu, mettre le launcher à jour et relancer via celui-ci pour installer les packs. Former un duo avec les deux joueurs en ligne dans le menu : chacun doit voir son personnage et celui du mate avec sa tenue. Contrôler un changement de tenue, le départ/réarrivée du mate, puis les photos d'amis qui ne sont pas dans le duo. Vérifier les logs d'attestation et les versions effectivement chargées. La scène est prévue pour un groupe de deux joueurs dans le menu ; le mate en partie ne reste pas affiché comme un membre de menu actif.

## Archives livrées

| Archive | Taille en octets | SHA-256 |
| --- | ---: | --- |
| `assets_x64_0.payload` | 1529806173 | `709c6c1893e3d20b507e2f4b29d8c7b2851c47f0a634199dd9c63b6917a3f7ff` |
| `rank_menu_ui.payload` | 121148052 | `51f6560f6b7a400ade6ae00d235183390a71f34b7a27a7fc360098ee4d25c3d6` |

Les archives `.payload` sont des ZIP. La première contient `assets_x64_0.pack2` ;
la seconde contient `ui_x64_0.pack2` et `ui_x64_2.pack2`. Installer les deux archives
ensemble : les deux copies de `UIRoot.gfx` doivent être mises à jour. Les hashes
des fichiers installés sont dans `asset-payloads.v1.json`.

## Validation déjà effectuée

Le propriétaire a validé visuellement le GFX corrigé dans les deux clients du
harness. Les packs repartent des assets publiés 1.13.5 et ne remplacent que
`UIRoot.gfx` ; 9 606 autres entrées sont conservées à l'identique. Le service réel
d'installation du launcher 2.0.11 a installé les deux archives, puis confirmé
qu'une deuxième synchronisation ne télécharge rien. Les packs recomposés n'ont
pas fait l'objet d'un nouveau lancement natif en production ; la vérification
finale sur deux clients après publication reste à faire. Les archives envoyées
sont retéléchargées depuis GitHub pour vérifier tailles, SHA-256, contenu ZIP et
hashes des fichiers installés avant remise de cette passation.

Cette livraison ne publie pas de release stable, ne fusionne pas le feed et ne
modifie ni la politique active ni les serveurs.
