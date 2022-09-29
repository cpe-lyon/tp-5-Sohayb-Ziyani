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

Pour que les deux partitions soient montées automatiquement il faut ajouter des lignes dans le dossiers `/etc/fstab` qui sont : `/dev/sdb1 /data ext4 defaults 0 0` et 
`/dev/sdb2 /win ntfs defaults 0 0` 

![image](https://user-images.githubusercontent.com/80455771/192988313-d9332fd2-9542-4070-a0ec-96499e3f2552.png)

### 7. Utilisez la commande mount puis redémarrez votre VM pour valider la configuration

On utilise les commandes : `mount /dev/sdb1` / `mount /dev/sdb2` / `mount -a` (pour forcer la prise en compte des modifications) 

## Exercice 2. Partitionnement LVM

### 1. On va réutiliser le disque de 5 Gio de l’exercice précédent. Commencez par démonter les systèmes de fichiers montés dans /data et /win s’ils sont encore montés, et supprimez les lignes correspondantes du fichier /etc/fstab

Avec la commande umount on peut démonter les systèmes de fichiers : `umount /data` / `umount /win`

![image](https://user-images.githubusercontent.com/80455771/192993114-1023dd5e-757d-44c6-bb44-f6d798deda3e.png)

Et on modifie le fichier /etc/fstab pour supprimer les deux lignes ajouter précedemment : 

![image](https://user-images.githubusercontent.com/80455771/192993335-eecf1bb3-35ef-4144-a4b8-1d728a213ed6.png)

### 2. Supprimez les deux partitions du disque, et créez une partition unique de type LVM

On supprime les deux partitions du disque avec la commande `fdisk` :

![image](https://user-images.githubusercontent.com/80455771/192997143-874b3b39-4b53-4ee7-91f9-f1f70636878e.png)

Et on crée une nouvelle partition : 

![image](https://user-images.githubusercontent.com/80455771/192997786-ad8bc4b5-6c8a-4b30-9eba-c753eedcc913.png)

### 3. A l’aide de la commande pvcreate, créez un volume physique LVM. Validez qu’il est bien créé, en utilisant la commande pvdisplay.

La commande `pvcreate` sert à initialiser des volumes physiques LVM.
On crée donc le nouveau volume :

![image](https://user-images.githubusercontent.com/80455771/193002537-c5ec6b35-3526-4999-8475-e10d608b1ded.png)

### 4. A l’aide de la commande vgcreate, créez un groupe de volumes, qui pour l’instant ne contiendra que le volume physique créé à l’étape précédente. Vérifiez à l’aide de la commande vgdisplay

![image](https://user-images.githubusercontent.com/80455771/193004697-42dea8d4-33be-4cec-8c05-31e3ab1a6142.png)

### 5. Créez un volume logique appelé lvData occupant l’intégralité de l’espace disque disponible.

![image](https://user-images.githubusercontent.com/80455771/193008825-da32a923-ed9c-4d14-b37c-820f5479e4f3.png)

### 6. Dans ce volume logique, créez une partition que vous formaterez en ext4, puis procédez comme dans l’exercice 1 pour qu’elle soit montée automatiquement, au démarrage de la machine, dans /data

Création de la partition :

![image](https://user-images.githubusercontent.com/80455771/193010313-c2218101-5909-496e-803d-6095f6e3763a.png)

Formatez en ext4 :

![image](https://user-images.githubusercontent.com/80455771/193010837-d0218e8b-7133-4a12-838e-be1a01d4147e.png)

Modification dans le fichier : 

![image](https://user-images.githubusercontent.com/80455771/193013652-2262ec22-d354-4202-bb96-4909dd369778.png)

![image](https://user-images.githubusercontent.com/80455771/193016827-fe0e717d-ba4b-4016-94cb-6d03079176b3.png)

### 7. Eteignez la VM pour ajouter un second disque (peu importe la taille pour cet exercice). Redémarrez la VM, vérifiez que le disque est bien présent. Puis, répétez les questions 2 et 3 sur ce nouveau disque.

### 8. Utilisez la commande vgextend <nom_vg> <nom_pv> pour ajouter le nouveau disque au groupe de volumes

### 9.  Utilisez la commande lvresize (ou lvextend) pour agrandir le volume logique. Enfin, il ne faut pas oublier de redimensionner le système de fichiers à l’aide de la commande resize2fs.
