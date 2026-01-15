# build_vm
Construction de VM sandbow avec Vagrant dans le cadre scolaire

## Fonctionnement du projet
- télécharger Vagrant ici : https://developer.hashicorp.com/vagrant/install
- Dans le dossier courant taper: vagrant up
- répondre à la question combien de vm voulez-vous ?

Le nombre de vm indiqué sera créée. Un dossier ssh_keys sera créée dans le même dossier que le vagrant_file. Ce sont les clés de connexions ssh à vos VM.

## Spec des VMS
- Virtualiseur: VirtualBox
- OS: Ubuntu/jammy64
- Core: 1
- ram : 2048
- Disksize: 25GB
- Network: 1 réseaux NAT pour l'accès internet et un réseaux en réseau privée hote pour le ssh.
- les utilisateurs sont userx. C'est-à-dire, pour la VM1 le user sera user1 et le password sera user1, ainsi de suite par itération.
