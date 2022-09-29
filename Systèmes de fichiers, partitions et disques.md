# TP 5 - Systèmes de fichiers, partitions et disques 

## Exercice 1. Disques et partitions 

### 2. Vérifiez que ce nouveau disque dur est bien détecté par le système :

Avec la commande 'll /dev/sd*' on oeut voir la liste des disques présent :

![image](https://user-images.githubusercontent.com/80455771/192960348-21ecb93c-ef7a-4cb0-bc22-2aa997be71be.png)

On peut voir que le disque a bien été crée :

![image](https://user-images.githubusercontent.com/80455771/192962303-f06bd49e-711d-4d84-a41e-4e5f7dad6454.png)


### 3.  Partitionnez ce disque en utilisant fdisk : créez une première partition de 2 Go de type Linux (n°83), et une seconde partition de 3 Go en NTFS (n°7) : 

Création de la première partition : 

![image](https://user-images.githubusercontent.com/80455771/192964201-39f984ca-87c7-4266-b291-2fa54ae8ba32.png)

Création de la deuxième partition : 

![image](https://user-images.githubusercontent.com/80455771/192967914-8ee0f632-737c-4731-81bc-91d7c9628369.png)

On peut voir que les deux partitions ont bien été crée :

![image](https://user-images.githubusercontent.com/80455771/192968461-2d9c4169-f89a-4a50-89c9-d7b4e7510e35.png)

### 4. A ce stade, les partitions ont été créées, mais elles n’ont pas été formatées avec leur système de fichiers. A l’aide de la commande mkfs, formatez vos deux partitions :

![image](https://user-images.githubusercontent.com/80455771/192975040-8a5645d8-bc8f-4a00-95f7-7b92f8b5cde2.png)

![image](https://user-images.githubusercontent.com/80455771/192974989-20e179f3-65b3-4c1c-bd6d-b430ff4fcae8.png)

### 5. Pourquoi la commande df -T, qui affiche le type de système de fichier des partitions, ne fonctionne-telle pas sur notre disque ?

La commande `df -T` ne marche pas puisque nos partitions ne sont pas encore montées.

### 6. Faites en sorte que les deux partitions créées soient montées automatiquement au démarrage de la machine, respectivement dans les points de montage /data et /win (vous pourrez vous passer des UUID en raison de l’impossibilité d’effectuer des copier-coller)

Pour que les deux partitions soient montées automatiquement il faut ajouter des lignes dans le dossiers `/etc/fstab` qui sont : `/dev/sdb1 /data ext4 defaults 0 0` et `/dev/sdb2 /win ntfs defaults 0 0` 

![image](https://user-images.githubusercontent.com/80455771/192988313-d9332fd2-9542-4070-a0ec-96499e3f2552.png)

