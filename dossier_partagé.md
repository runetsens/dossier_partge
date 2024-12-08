# Configuration d'un serveur de fichiers sur Windows Server 2022

## Préparation de l'environnement
1. Installer une VM avec Windows Server 2022.
2. Configurer un domaine Active Directory.
3. Installer une VM Windows 10 et la joindre au domaine.

## Installation du rôle Serveur de fichiers
1. Ouvrir le Gestionnaire de serveur.
2. Ajouter le rôle **Serveur de fichiers**.

## Création des dossiers
- Commande PowerShell :
  ```powershell
  New-Item -Path "C:\Documents_Entreprise" -ItemType Directory
  New-Item -Path "C:\Documents_Entreprise\RH" -ItemType Directory
  New-Item -Path "C:\Documents_Entreprise\Comptabilité" -ItemType Directory
  New-Item -Path "C:\Documents_Entreprise\Direction" -ItemType Directory

## Configuration du partage
New-SmbShare -Name "Docs" -Path "C:\Documents_Entreprise" -FullAccess "Administrators" -ReadAccess "Domain Users"

## Configuration des permissions NTFS
icacls "C:\Documents_Entreprise\RH" /grant "Entreprise\RH:(OI)(CI)F"
icacls "C:\Documents_Entreprise\Comptabilité" /grant "Entreprise\Comptabilité:(OI)(CI)F"
icacls "C:\Documents_Entreprise\Direction" /grant "Entreprise\Direction:(OI)(CI)F"
icacls "C:\Documents_Entreprise" /grant "Entreprise\Domain Users:(OI)(CI)R"

## Get-SmbShare
Get-SmbShare

## Configuration du lecteur réseau sur le client
New-PSDrive -Name "Z" -PSProvider FileSystem -Root "\\<NomDuServeur>\Docs" -Persist

## Tests des accès
Get-ChildItem -Path "Z:\"


