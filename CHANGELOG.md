# Journal des versions — AutoBitlocker

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
