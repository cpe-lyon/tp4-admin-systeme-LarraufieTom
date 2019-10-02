LARRAUFIE Tom
ICS
TP Droits utilisateurs




## Exercice 1. Gestion des utilisateurs et des groupes

• Commencez par créer deux groupes groupe1 et groupe2

La commande pour créer un groupe est la suivante: **groupadd [nomGroupe]** .

```bash
tom@serveur:~$ sudo groupadd groupe1
```


• Créez ensuite 4 utilisateurs u1, u2, u3, u4 avec la commande useradd, en demandant la création de
leur dossier personnel et avec bash pour shell

La commande pour créer un utilisateur et créer son répertoire personnel est la suivante: **useradd -m [nomUser]** .

```bash
tom@serveur:~$ sudo useradd -m u1
tom@serveur:~$ sudo useradd -m u2
tom@serveur:~$ sudo useradd -m u3
tom@serveur:~$ sudo useradd -m u4
```

• Placez les utilisateurs dans les groupes :

Pour placer un utilisateur existant dans un groupe existant, on utilise la commande suivante: **usermod -a -G [nomGroupe] [nomUser]**
- u1, u2, u4 dans groupe1
```bash
tom@serveur:~$ sudo usermod -a -G groupe1 u1
tom@serveur:~$ sudo usermod -a -G groupe1 u2
tom@serveur:~$ sudo usermod -a -G groupe1 u4
```
- u2, u3, u4 dans groupe2
```bash
tom@serveur:~$ sudo usermod -a -G groupe2 u2
tom@serveur:~$ sudo usermod -a -G groupe2 u3
tom@serveur:~$ sudo usermod -a -G groupe2 u4
```

• Donnez deux moyens d’afficher les membres de groupe2

- **grep "group1" /etc/group** 
- 

• Faites de groupe1 le groupe propriétaire de /home/u1 et /home/u2 et de groupe2 le groupe propriétaire
de /home/u3 et /home/u4

Pour modifer le groupe propriétaire d'un répertoire, on utilise la commande suivante: **sudo chown -R [:NomGroupe] [cible1] [cible2]**

```bash
tom@serveur:~$ sudo chown -R :groupe1 /home/u1 /home/u2
tom@serveur:~$ sudo chown -R :groupe2 /home/u3 /home/u4
```

• Remplacez le groupe primaire des utilisateurs :

Pour modifier le groupe primaire d'un utilisateur, on utilise la commande: **sudo usermod -g [NomGroupe] [NomUser]**
- groupe1 pour u1 et u2
```bash
tom@serveur:~$ sudo usermod -g groupe1 u1
tom@serveur:~$ sudo usermod -g groupe1 u2
```

- groupe2 pour u3 et u4
```bash
tom@serveur:~$ sudo usermod -g groupe2 u3
tom@serveur:~$ sudo usermod -g groupe2 u4
```


• Créez deux répertoires /home/groupe1 et /home/groupe2 pour le contenu commun aux groupes, et
mettez en place les permissions permettant aux membres de chaque groupe d’écrire dans le dossier
associé.

- On commence par créer les deux répertoire avec la commande suivante: **mkdir /home/groupe1**
```bash
tom@serveur:~$ sudo mkdir /home/groupe1
tom@serveur:~$ sudo mkdir /home/groupe2
```
- On definit groupe1 et groupe2 comme propriétaire des répertoires respectif /home/groupe1 et /home/groupe2.
```bash
tom@serveur:/home$ sudo chown -R :groupe1 groupe1
tom@serveur:/home$ sudo chown -R :groupe2 groupe2
```

- On definit les droits des utilisateurs
```bash
tom@serveur:/home$ sudo chmod g+w groupe1
tom@serveur:/home$ sudo chmod g+w groupe2
```

**ll** permet d'avoir toutes les informations sur les répertoires courants (droits etc...)


• Comment faire pour que, dans ces dossiers, seul le propriétaire d’un fichier ait le droit de renommer
ou supprimer ce fichier ?

Pour que seul le propriétaire d'un fichier ait le droit de renommer et supprimer, on enleve les droits au groupe et aux autres, avec la commande suivante: **sudo chmod go-w [nomGroupe]**
```bash
tom@serveur:/home$ sudo chmod go-w groupe1
tom@serveur:/home$ sudo chmod go-w groupe2
```

• Pouvez-vous vous connecter en tant que u1 ? Pourquoi ?

Non car nous n'avons pas definit de mot de passe à l'utilisateur u1 , le compte n'est donc pas activé.

**Tout compte créé sans mot de passe est inactif. **

• Activez le compte de l’utilisateur u1 et vérifiez que vous pouvez désormais vous connecter avec son
compte.

Pour activer le compte, il faut definir un mot de passe à l'utilisateur.

```bash
tom@serveur:~$ sudo passwd u1
New password:
Retype new password:
```

• Quels sont l’uid et le gid de u1 ? 

Pour trouver l'uid et le git d'un utilisateur, on entre la commande suivante: **tom@serveur:~$ id u1**.

```bash
tom@serveur:~$ id u1
uid=1001(u1) gid=1002(groupe1) groups=1002(groupe1)
```

• Quel utilisateur a pour uid 1003 ?

Pour connaitre l'utilisateur qui possede l'uid 1003, on tape la commande suivante: **cat /etc/passwd | grep "1003" **
Mais il faut regardé uniquement la 3eme colonne, celle qui indique les id utilisateurs et non les id groupes.

Je ne possede pas d'utilsiateur avec l'uid 1003.

```bash
tom@serveur:~$ cat /etc/passwd | grep "1001"
u1:x:1001:1002::/home/u1:/bin/sh
```


• Quel est l’id du groupe groupe1 ?
Pour connaitre l'id du groupe1, on peut taper la commande suivante : **cat /etc/group | grep "groupe1"**

```bash
tom@serveur:~$ cat /etc/group | grep "groupe1"
groupe1:x:1002:u1,u2,u4
```

• Quel groupe a pour guid 1002 ? ( Rien n’empêche d’avoir un groupe dont le nom serait 1002...)

Pour trouver le groupe qui a le guid 1002, on tape la commande suivante: **cat /etc/group | grep "1002"**
Puis on regarde la 4eme colonne qui correspond au id des groupes.

• Retirez l’utilisateur u3 du groupe groupe2. Que se passe-t-il ? Expliquez.

Pour retirer l'utilisateur u3 du groupe2 on utilise:
**sudo gpasswd -d u3 groupe2**
 
```bash
tom@serveur:~$ sudo gpasswd -d u3 groupe2
[sudo] password for tom:
Removing user u3 from group groupe2
```

>Documentation des options:
Options:
  ``` -d, --lastday LAST_DAY        set date of last password change to LAST_DAY
  -E, --expiredate EXPIRE_DATE  set account expiration date to EXPIRE_DATE
  -h, --help                    display this help message and exit
  -I, --inactive INACTIVE       set password inactive after expiration
                                to INACTIVE
  -l, --list                    show account aging information
  -m, --mindays MIN_DAYS        set minimum number of days before password
                                change to MIN_DAYS
  -M, --maxdays MAX_DAYS        set maximim number of days before password
                                change to MAX_DAYS
  -R, --root CHROOT_DIR         directory to chroot into
  -W, --warndays WARN_DAYS      set expiration warning days to WARN_DAYS
  ```

### Modifiez le compte de u4 de sorte que :

- il expire au 1er juin 2019
```bash
tom@serveur:~$ sudo chage -E 2019/06/01 u4
```

- il faut changer de mot de passe avant 90 jours
```bash
tom@serveur:~$ sudo chage -M 90 u4
```
- il faut attendre 5 jours pour modifier un mot de passe
```bash
tom@serveur:~$ sudo chage -m 5 u4
```
- l’utilisateur est averti 14 jours avant l’expiration de son mot de passe
```bash
tom@serveur:~$ sudo chage -W 14 u4
```

- le compte sera bloqué 30 jours après expiration du mot de passe

```bash
tom@serveur:~$ sudo chage -I 30 u4
```

• Quel est l’interpréteur de commandes (Shell) de l’utilisateur root ?`

```bash
tom@serveur:~$ eecho $SHELL
/bin/bash
```

• à quoi correspond l’utilisateur nobody ?

nobody (informatique) Dans la plupart des variantes d'Unix, nobody (personne en anglais) est le nom conventionnel d'un compte d'utilisateur à qui aucun fichier n'appartient, qui n'est dans aucun groupe qui a des privilèges et dont les seules possibilités sont celles que tous les "autres utilisateurs" ont.

• Par défaut, combien de temps la commande sudo conserve-t-elle votre mot de passe en mémoire ?

La commande sudo conserve environ 15 minutes le mot de passe en mémoire.

Quelle commande permet de forcer sudo à oublier votre mot de passe ?
```bash
exit or Ctrl+d
```

## Exercice 2. Gestion des permissions

• Dans votre $HOME, créez un dossier test, et dans ce dossier un fichier fichier contenant quelques
lignes de texte. Quels sont les droits sur test et fichier ?
```bash
tom@serveur:~$ mkdir test
tom@serveur:~$ cd test
tom@serveur:~/test$ touch fichier
```

Voici les droits de test et fichier avec la commande **ls -l**
```bash
drwxrwxr-x  2 tom  tom  4096 oct.   2 10:52 test/
-rw-rw-r--  1 tom tom   17 oct.   2 10:52 fichier
```


• Retirez tous les droits sur ce fichier (même pour vous), puis essayez de le modifier et de l’afficher en
tant que root. Conclusion ?

On retire tout les droits sur le fichier
```bash
tom@serveur:~/test$ chmod 000 fichier
```

```bash
tom@serveur:~/test$ sudo su
root@serveur:/home/tom/test# cat fichier
CECI EST UN TEST
```

En tant que root, nous pouvons modifier et afficher.
Le root passe outre les permissions.



• Redonnez vous les droits en écriture et exécution sur fichier puis exécutez la commande echo "echo
Hello" > fichier. On a vu lors des TP précédents que cette commande remplace le contenu d’un
fichier s’il existe déjà. Que peut-on dire au sujet des droits ?

```bash
tom@serveur:~/test$ chmod u+wx fichier

```
On test d'ecrire dans le fichier.
```bash
tom@serveur:~/test$ echo "echo Hello" > fichier
```
Cela fonctionne.

Le droit d'ecriture fonctionne bien.

• Essayez d’exécuter le fichier. Est-ce que cela fonctionne ? Et avec sudo ? Expliquez.

>nous ne pouvons pas executer le fichier . Le droit d'execution ne fonctionne pas sur le fichier.

>On test la commande avec sudo et cela fonctionne bien.

```bash
tom@serveur:~/test$ ./fichier
bash: ./fichier: Permission denied
tom@serveur:~/test$ sudo ./fichier
Hello
```

• Placez-vous dans le répertoire test, et retirez-vous le droit en lecture pour ce répertoire. Listez le
contenu du répertoire, puis exécutez ou affichez le contenu du fichier fichier. Qu’en déduisez-vous ?

```bash
tom@serveur:~/test$ chmod u-r ../test
tom@serveur:~/test$ ls
ls: cannot open directory '.': Permission denied
```

Ce n'est pas possible de lister les fichiers.

Rétablissez le droit en lecture sur test
```bash 
tom@serveur:~/test$ chmod u+r ../test
```


• Créez dans test un fichier nouveau ainsi qu’un répertoire sstest. Retirez au fichier nouveau et au
répertoire test le droit en écriture. Tentez de modifier le fichier nouveau. Rétablissez ensuite le droit
en écriture au répertoire test. Tentez de modifier le fichier nouveau, puis de le supprimer. Que pouvezvous déduire de toutes ces manipulations ?

```bash
tom@serveur:~/test$ touch nouveau
tom@serveur:~/test$ mkdir sstest

tom@serveur:~/test$ chmod u-w nouveau
tom@serveur:~/test$ chmod u-w test
```
>Quand on tente de modifier le fichier nouveau avec la commande **nano nouveau**, un message s'affiche
[ File 'nouveau' is unwritable ] 
Nous ne pouvons pas ecrire dedans.

>Nous ne pouvons pas non plus supprimer le fichier nouveau car nous n'avons pas les droits en écriture sur ce fichier.
En en déduit que même avec les droits en écriture sur le répertoire test, nous ne pouvons tout de même pas supprimer le fichier nouveau si nous n'avons pas les droits en écriture sur ce fichier.

• Positionnez vous dans votre répertoire personnel, puis retirez le droit en exécution du répertoire test.
Tentez de créer, supprimer, ou modifier un fichier dans le répertoire test, de vous y déplacer, d’en
lister le contenu, etc…Qu’en déduisez vous quant au sens du droit en exécution pour les répertoires ?

On retire le droit en execution du repertoire test.

```bash
tom@serveur:~$ chmod a-x test


tom@serveur:~$ cd test
-bash: cd: test: Permission denied
tom@serveur:~$ touch fichier2 /test
touch: cannot touch '/test': Permission denied

tom@serveur:~$ ls test
ls: cannot access 'test/nouveau': Permission denied
ls: cannot access 'test/sstest': Permission denied
ls: cannot access 'test/fichier': Permission denied
fichier  nouveau  sstest
```

>Nous avons retiré le droit d'execution du repertoire **test**.
Nous ne pouvons donc plus nous y deplacer, créer, supprimer ou modifier des fichier à l'interieur.

>En conclusion, le  droit d'execution pour les repertoires bloque l'entrée dans ce repertoire, et de manipuler des données dedans également.


• Rétablissez le droit en exécution du répertoire test. Positionnez vous dans ce répertoire et retirez lui
à nouveau le droit d’exécution. Essayez de créer, supprimer et modifier un fichier dans le répertoire
test, de vous déplacer dans ssrep, de lister son contenu. Qu’en concluez-vous quant à l’influence des
droits que l’on possède sur le répertoire courant ? Peut-on retourner dans le répertoire parent avec ”cd
..” ? Pouvez-vous donner une explication ?

```bash
tom@serveur:~$ chmod a+x test
tom@serveur:~$ cd test
tom@serveur:~/test$ chmod a-x ../test

tom@serveur:~/test$ touch fichier2
touch: cannot touch 'fichier2': Permission denied
tom@serveur:~/test$ cd sstest
-bash: cd: sstest: Permission denied
tom@serveur:~/test$ ls
ls: cannot open directory '.': Permission denied
```

>Comme à la question précedente, nous ne pouvons pas ajouter, modifier ,supprimer, nous deplacer et lister le contenu du répertoire test.

>Le droit d'éxecution nous empeche totalement toutes modifications et navigations dans ce répertoire courant ET également dans ces fichiers et répertoires **FILS**.

>La commande cd .. est possible car elle nous permet de sortir de ce répertoire vers un répertoire **PERE** donc ce n'est pas impacté par les droits du répertoire test. En revanche nous serons interdit de retourner à l'interieur de ce répertoire car nous n'avons pas les droits d'execution donc pas de navigation possible.


• Rétablissez le droit en exécution du répertoire test. Attribuez au fichier fichier les droits suffisants
pour qu’une autre personne de votre groupe puisse y accéder en lecture, mais pas en écriture.

```bash
tom@serveur:~$ chmod a+x test

tom@serveur:~/test$ chmod 750 fichier
```
Les droits **750** permettent de :
- donner tout les droits à l'utilisateur
- Seulement la lecture et l'execution au groupe
- aucun droits au autres.


• Définissez un umask très restrictif qui interdit à quiconque à part vous l’accès en lecture ou en écriture,
ainsi que la traversée de vos répertoires. Testez sur un nouveau fichier et un nouveau répertoire.
```bash
tom@serveur:~/test$ umask 077
```

Cette commande: 
- enlève tout les droits aux membres du groupe et aux autres.
- laisse tout les droits à l'utilisateur propriétaire.

```bash
tom@serveur:~/test$ mkdir Umask
drwx------  2 tom tom 4096 oct.   2 11:51 Umask/

tom@serveur:~/test$ touch fichierUmask
-rw-------  1 tom tom    0 oct.   2 11:53 fichierUmask
```

Le repertoire Umask permet seulement les droits à l'utilisateur.
Le fichier fichierUmask permet la lecture et l'ecriture à l'utilisateur mais pas l'execution.

• Définissez un umask très permissif qui autorise tout le monde à lire vos fichiers et traverser vos répertoires, mais n’autorise que vous à écrire. Testez sur un nouveau fichier et un nouveau répertoire.

>umask 022

```bash
tom@serveur:~/test$ umask 022
tom@serveur:~/test$ mkdir TESTrep
tom@serveur:~/test$ touch TESTfile
tom@serveur:~/test$ ll
total 20
drwxrwxr-x  4 tom tom 4096 oct.   2 12:05 ./
drwxr-xr-x 10 tom tom 4096 oct.   2 11:25 ../
-rwxr-x---  1 tom tom   11 oct.   2 10:59 fichier*
-r--rw-r--  1 tom tom    0 oct.   2 11:09 nouveau
drwxrwxr-x  2 tom tom 4096 oct.   2 11:13 sstest/
-rw-r--r--  1 tom tom    0 oct.   2 12:05 TESTfile
drwxr-xr-x  2 tom tom 4096 oct.   2 12:05 TESTrep/
```

>On autorise la lecture et la navigation aux membres du groupes et aux autres, mais pas l'ecriture.

>On remarque que les droits d'execution ne sont pas présent même avec un mask qui les ajoutent.


• Définissez un umask équilibré qui vous autorise un accès complet et autorise un accès en lecture aux
membres de votre groupe. Testez sur un nouveau fichier et un nouveau répertoire.

>Commande: umask 037

```bash

tom@serveur:~/test$ umask 037
tom@serveur:~/test$
tom@serveur:~/test$ mkdir TESTrep
tom@serveur:~/test$ touch TESTfile
tom@serveur:~/test$ ll
total 20
drwxrwxr-x  4 tom tom 4096 oct.   2 12:10 ./
drwxr-xr-x 10 tom tom 4096 oct.   2 11:25 ../
-rwxr-x---  1 tom tom   11 oct.   2 10:59 fichier*
-r--rw-r--  1 tom tom    0 oct.   2 11:09 nouveau
drwxrwxr-x  2 tom tom 4096 oct.   2 11:13 sstest/
-rw-r-----  1 tom tom    0 oct.   2 12:10 TESTfile
drwxr-----  2 tom tom 4096 oct.   2 12:10 TESTrep/

```



• Transcrivez les commandes suivantes de la notation classique à la notation octale ou vice-versa (vous
pourrez vous aider de la commande stat pour valider vos réponses) :
- chmod u=rx,g=wx,o=r fic
```bash
chmod 534 fic
```
- chmod uo+w,g-rx fic en sachant que les droits initiaux de fic sont r--r-x---
```bash
chmod 602 fic
```
- chmod 653 fic en sachant que les droits initiaux de fic sont 711
```bash
chmod u-x,g+r,o+w
```
- chmod u+x,g=w,o-r fic en sachant que les droits initiaux de fic sont r---r-x---
```bash
chmod 520 fic
```


• Affichez les droits sur le programme passwd. Que remarquez-vous ? En affichant les droits du fichier
/etc/passwd, pouvez-vous justifier les permissions sur le programme passwd ?

```bash
tom@serveur:/etc$ ls -l /bin/passwd
-rwsr-xr-x 1 root root 63736 mars  22  2019 /bin/passwd
tom@serveur:/etc$ ls -l /etc/passwd
-rw-r--r-- 1 root root 1775 sept. 30 14:58 /etc/passwd

```

On remarque sur les droits que tout les droits sont actifs sauf le droit d'ecriture pour le groupe de l'utilisateur et les autres utilisateurs.

Les droits sont tel car les autres utilisateurs autres que le user propriétaire (root) ne doivent pas pouvoir écrire dans ce programme.

Et ils ont les droits de lecture et écriture pour pouvoir naviguer dans les fichiers et pour lister le contenu du programme.



• Access Control Lists (ACL) : suivez le tutoriel de cette page : https://doc.ubuntu-fr.org/acl.


• Quotas disques : suivez le tutoriel de cette page : https://doc.ubuntu-fr.org/quota.
