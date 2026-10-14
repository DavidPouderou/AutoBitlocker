# AutoBitlocker

Outil portable (clé USB) pour **déployer BitLocker en série** sur des postes Windows 10 / 11
Professionnel et Entreprise en groupe de travail : activation du chiffrement, point de
restauration préalable, enregistrement de la clé de récupération (fiche PDF photographiable,
journal CSV pour import), le tout en trois saisies. Un seul fichier `.exe`, sans installation.

Outil interne de technicien : il n'est pas destiné au grand public. Le dépôt ne contient pas le
code source (voir la licence), seulement la documentation, le certificat public et les versions
publiées.

## Téléchargement

Dernière version : [page Releases](https://github.com/DavidPouderou/AutoBitlocker/releases/latest).

| Fichier | Pour |
|---|---|
| `AutoBitlocker.exe` | Windows 10 et Windows 11 Professionnel / Entreprise (un seul fichier, ~400 Ko) |

Aucune installation : copier le fichier à la racine de la clé USB du technicien et le lancer.
L'outil demande les droits administrateur au lancement ; sans eux, il ne démarre pas. Windows
PowerShell 5.1 et WPF, déjà présents sur tout Windows 10 / 11, suffisent.

Au premier lancement d'un fichier téléchargé, Windows peut afficher « Windows a protégé votre
ordinateur » : cliquer sur « Informations complémentaires », puis « Exécuter quand même ». Le
certificat de signature est auto-signé (voir plus bas) et Windows ne le connaît pas.

## Mise à jour automatique

À chaque lancement depuis la clé USB, l'outil vérifie en arrière-plan si une version plus
récente est publiée ici. Si c'est le cas, il la télécharge à côté de l'exécutable, contrôle son
empreinte, sa signature et sa version, puis la met en place à la fermeture : la nouvelle version
est utilisée au lancement suivant, et le bandeau du pied de page l'annonce. Rien n'est jamais
interrompu, et jamais de retour à une version plus ancienne.

Sans connexion, il démarre normalement et ne dit rien. `-NoUpdate` désactive la vérification
pour un lancement ; `"misesAJour": { "automatique": false }` dans `AutoBitlocker\settings.json`
la désactive définitivement. C'est la seule adresse Internet que l'outil contacte ; aucune
télémétrie, aucune donnée d'intervention ne quitte la clé USB.

## Vérifier un téléchargement

L'exécutable est signé (Authenticode, `CN=David Pouderou`) et horodaté par DigiCert. Le
certificat public est dans ce dépôt : `certificat-signature.cer`, empreinte SHA-1
`8F2D1E7BADDEE288B503D464997FD73DCAD6764A`. L'empreinte SHA-256 du fichier est dans
`SHA256SUMS.txt`, joint à chaque version.

```powershell
Get-AuthenticodeSignature .\AutoBitlocker.exe | Format-List Status, SignerCertificate, TimeStamperCertificate
Get-FileHash .\AutoBitlocker.exe -Algorithm SHA256
```

`UnknownError` signifie que la chaîne n'est pas approuvée (certificat auto-signé) et que le fichier
est intact ; `HashMismatch` signifie que le fichier a été modifié. L'outil applique les mêmes
contrôles, plus stricts encore, avant chaque mise à jour automatique.

## Signaler un problème

Par les [Issues](https://github.com/DavidPouderou/AutoBitlocker/issues) de ce dépôt, avec la
version (visible dans les propriétés de l'exe, onglet « Détails ») et le dossier
`AutoBitlocker\logs\` de la clé USB. **Attention : ne jamais joindre le journal
`AutoBitlocker\journal\` ni les PDF — ils contiennent les clés de récupération.** Les fichiers de
`logs\` masquent les clés, mais les relire avant de les joindre.

## Licence

Logiciel propriétaire, © 2026 David Pouderou, tous droits réservés : voir [LICENCE.txt](LICENCE.txt).
Le code source n'est pas distribué. Historique des versions : [CHANGELOG.md](CHANGELOG.md).
