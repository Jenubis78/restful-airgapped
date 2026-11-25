# Miroir local du provider `magodo/restful` v0.26.0

Ce dossier contient les binaires officiels du provider (release GitHub v0.26.0),
rangés selon le format « packed » attendu par Terraform, pour permettre un
`terraform init` sans accès au registry public.

```
terraform-mirror/
├── registry.terraform.io/magodo/restful/
│   ├── index.json                 # index au format "network mirror"
│   ├── 0.26.0.json                # hashes h1 par plateforme
│   └── terraform-provider-restful_0.26.0_<os>_<arch>.zip
├── terraform.rc.example           # config CLI à copier
├── example.terraform.lock.hcl     # lock file prêt à l'emploi
├── SHA256SUMS / SHA256SUMS.sig    # sommes officielles de la release
└── README.md
```

Plateformes incluses : `windows_amd64`, `windows_arm64`, `linux_amd64`,
`linux_arm64`, `darwin_amd64`, `darwin_arm64`.

Les archives ont été vérifiées contre le `SHA256SUMS` officiel de la release.

## Utilisation

1. Copier `terraform.rc.example` vers :
   - Windows : `%APPDATA%\terraform.rc`
   - Linux / macOS : `~/.terraformrc`

   ou définir `TF_CLI_CONFIG_FILE` vers ce fichier.
2. Remplacer `path` par le chemin absolu de ce dossier `terraform-mirror`.
3. Dans le projet Terraform, épingler la version :

   ```hcl
   terraform {
     required_providers {
       restful = {
         source  = "magodo/restful"
         version = "0.26.0"
       }
     }
   }
   ```

4. (Optionnel) copier `example.terraform.lock.hcl` en `.terraform.lock.hcl`
   dans le projet, puis lancer `terraform init`.

## Ajouter une plateforme ou une version

Avec un accès Internet :

```bash
terraform providers mirror -platform=windows_amd64 -platform=linux_amd64 ./terraform-mirror
```

ou télécharger les zips depuis
https://github.com/magodo/terraform-provider-restful/releases et les déposer
dans `registry.terraform.io/magodo/restful/`.

Pour régénérer le lock file à partir du miroir :

```bash
terraform providers lock -fs-mirror=./terraform-mirror \
  -platform=windows_amd64 -platform=linux_amd64 -platform=darwin_arm64
```
