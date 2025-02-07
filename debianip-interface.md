# Configuration d'une adresse IP statique via `/etc/network/interfaces`

## 1. Introduction
Sur les systèmes basés sur Debian/Ubuntu, la configuration des interfaces réseau peut être effectuée via le fichier `/etc/network/interfaces`. Cette documentation explique comment configurer une adresse IP statique sur une interface réseau.

## 2. Vérification des interfaces réseau
Avant toute modification, il est conseillé de lister les interfaces disponibles :
```bash
ip link show
```
Ou avec :
```bash
ifconfig -a
```

## 3. Modifier le fichier `/etc/network/interfaces`
Éditer le fichier avec un éditeur de texte comme `nano` :
```bash
sudo nano /etc/network/interfaces
```
Ajouter ou modifier la configuration pour l'interface concernée (ex: `eth0` ou `ens33`) :
```ini
# Interface Ethernet principale
auto eth0
iface eth0 inet static
    address 192.168.1.100
    netmask 255.255.255.0
    gateway 192.168.1.1
    dns-nameservers 8.8.8.8 8.8.4.4
```

### Explication des paramètres :
- `auto eth0` : Active l'interface au démarrage.
- `iface eth0 inet static` : Configure l'interface avec une IP statique.
- `address` : Adresse IP de l'interface.
- `netmask` : Masque de sous-réseau.
- `gateway` : Passerelle par défaut.
- `dns-nameservers` : Serveurs DNS à utiliser.

## 4. Appliquer les modifications
Après avoir sauvegardé le fichier, redémarrer le service réseau :
```bash
sudo systemctl restart networking
```
Ou redémarrer l'interface spécifique :
```bash
sudo ifdown eth0 && sudo ifup eth0
```

## 5. Vérification de la configuration
Vérifier si la configuration a bien été appliquée :
```bash
ip addr show eth0
```
Ou :
```bash
ifconfig eth0
```
Tester la connectivité :
```bash
ping 8.8.8.8
```

## 6. Conclusion
Configurer une IP statique via `/etc/network/interfaces` est utile pour assurer une connectivité stable, notamment sur les serveurs et équipements nécessitant une adresse fixe.

