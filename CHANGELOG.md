# Journal des versions — AutoBitlocker

## 1.1.5 — 2026-10-15 — Libellé Société

- L'interface parle désormais de « Société », comme la documentation : le libellé « Société / client »
  disparaît de la fenêtre principale, de la fiche de récupération, de la fiche PDF, du texte copié
  (« Copier la fiche ») et des messages de saisie manquante. Aucun changement de fonctionnement :
  réglages et historique des sociétés inchangés.
- Scripts annexes (`publish.ps1`, `reserve-securite\Prepare-UsbKey.ps1`) et commentaires des tests en
  vocabulaire neutre (« le poste », « un parc »).

## 1.1.4 — 2026-10-14 — Correctif : mise à jour en attente appliquée au lancement

- Une mise à jour téléchargée lors d'une session précédente (`.new`) pouvait être refusée à tort
  au lancement suivant (« fichier non signé ») puis supprimée — elle était alors retéléchargée et
  installée à la fermeture : jamais de version non vérifiée, mais un lancement de retard. Cause :
  dans la session principale de l'application, le module Security de PowerShell peut refuser de se
  charger (conflits TypeData, travers connu de PowerShell 5.1) et le contrôle de signature devient
  indisponible. La lecture de signature se replie désormais sur un runspace neuf du même processus,
  où elle fonctionne toujours. Découvert lors du test réel du chemin « mise en place au lancement ».

## 1.1.3 — 2026-10-14 — Journal des mises à jour : « déjà à jour » tracé

- `AutoBitlocker\logs\mises-a-jour.log` note désormais aussi les vérifications qui concluent
  « déjà à jour » : on voit que le mécanisme a bien tourné, même quand il n'a rien à faire.

## 1.1.2 — 2026-10-14 — Correctif : la mise à jour automatique aboutit

- Le calcul de l'empreinte SHA-256 du fichier téléchargé échouait dans le runspace d'arrière-plan
  (`Get-FileHash` est une fonction du module Utility, absente des runspaces nus — seules les
  cmdlets compilées y sont garanties) : la mise à jour était refusée à tort. Empreinte désormais
  calculée en .NET pur ; un test interdit la réapparition de ces fonctions dans le module.
  Découvert lors du premier test réel 1.1.0 → 1.1.1.
- À la fermeture, une vérification de mise à jour déjà engagée dispose de quelques secondes pour
  se terminer avant la mise en place : le technicien qui ferme juste après la fiche n'a plus à
  attendre le lancement suivant.

## 1.1.1 — 2026-10-14 — Version et auteur affichés dans l'en-tête

- L'en-tête de la fenêtre principale affiche la version et le copyright
  (« v1.1.1 · © 2026 David Pouderou »), comme PrintRescue : le technicien voit d'un coup d'œil
  quelle version tourne, notamment après une mise à jour automatique.

## 1.1.0 — 2026-10-14 — Distribution GitHub, signature et mise à jour automatique

- **Deux dépôts GitHub**, sur le modèle d'Optimus Cleaner et de PrintRescue : le code source vit
  dans le dépôt privé `AutoBitlocker-src` ; le dépôt public `AutoBitlocker` ne reçoit que la
  documentation, le certificat public et les exécutables signés (page Releases).
- **Exécutable signé** (Authenticode, `CN=David Pouderou`) et horodaté par DigiCert. L'exe porte
  désormais nom, auteur, copyright et version (`ProductVersion = X.Y.Z+commit`).
- **Mise à jour automatique** : au lancement depuis la clé USB, l'outil vérifie en arrière-plan le
  manifeste `latest.json` du dépôt public. Si une version plus récente est publiée, elle est
  téléchargée à côté de l'exe (`.new`), contrôlée trois fois (empreinte SHA-256 annoncée, signature
  du certificat de l'auteur, version inscrite dans le fichier), puis mise en place à la fermeture —
  ou au lancement suivant, avec relance et bandeau « mis à jour ». Jamais de retour en arrière ;
  hors ligne, l'outil démarre normalement et se tait. Débrayage : `-NoUpdate` pour un lancement,
  `misesAJour.automatique: false` dans `AutoBitlocker\settings.json` pour toujours. C'est la seule
  adresse Internet que l'outil contacte ; zéro télémétrie. Journal : `AutoBitlocker\logs\mises-a-jour.log`.
- **Deux scripts à la racine** : `publish.ps1` (compilation, tests bloquants, signature, empreinte)
  et `release.ps1` (contrôles, manifeste, tag signé, dépôt public, Release GitHub, vérification de
  l'adresse « latest »).
- Licence propriétaire (`LICENCE.txt`), © 2026 David Pouderou.
- Documentation : `docs\DISTRIBUTION.md` (certificat, clés, procédure de livraison, que faire si la
  clé de signature est perdue ou volée).
