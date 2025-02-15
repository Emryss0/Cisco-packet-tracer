# 📌 Configuration d'un Serveur DHCP sur Debian

## 1️⃣ Installation du Serveur DHCP

Sur Debian, le serveur DHCP est fourni par **isc-dhcp-server**. Pour l'installer, exécutez :

```bash
sudo apt update
sudo apt install isc-dhcp-server -y
```

---

## 2️⃣ Configuration du Serveur DHCP

### 🔹 Définir l'interface réseau à utiliser

Éditez le fichier `/etc/default/isc-dhcp-server` :

```bash
sudo nano /etc/default/isc-dhcp-server
```

Modifiez la ligne suivante en spécifiant votre interface réseau (ex: `ens33`):

```
INTERFACESv4="ens33"
```

Sauvegardez (`CTRL+X`, `Y`, `Entrée`).

---

### 🔹 Configurer la plage d'adresses IP DHCP

Éditez le fichier `/etc/dhcp/dhcpd.conf` :

```bash
sudo nano /etc/dhcp/dhcpd.conf
```

Ajoutez ou modifiez ces lignes selon votre réseau :

```bash
subnet 192.168.50.0 netmask 255.255.255.0 {
range 192.168.50.3 192.168.50.100;
option routers 192.168.50.2;
option domain-name-servers 8.8.8.8, 8.8.4.4;
option broadcast-address 192.168.1.255;
default-lease-time 600;
max-lease-time 7200;

}
```

## 3️⃣ Démarrer et Activer le Service DHCP

```bash
sudo systemctl restart isc-dhcp-server
sudo systemctl enable isc-dhcp-server
```

Vérifiez son statut :

```bash
sudo systemctl status isc-dhcp-server
```

Le service doit être **"active (running)"** ✅.

---

# 📌 Récupérer une Adresse IP depuis le Serveur DHCP

Sur vos **VMs clientes Debian**, configurez l'interface en DHCP :

```bash
sudo nano /etc/network/interfaces
```

Ajoutez/modifiez :

```
auto ens33
iface ens33 inet dhcp
```

Sauvegardez (`CTRL+X`, `Y`, `Entrée`).

Redémarrez l'interface réseau :

```bash
sudo systemctl restart networking
```

Ou utilisez directement **dhclient** pour récupérer une IP :

```bash
sudo dhclient -v ens33
```

Vérifiez l'adresse IP obtenue :

```bash
ip a show eth0
```

Pour voir les baux DHCP attribués (depuis le serveur) :

```bash
cat /var/lib/dhcp/dhcpd.leases
```

✅ **Le serveur DHCP est fonctionnel et les clients obtiennent leurs IPs dynamiquement !** 🎉

