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

Modifiez la ligne suivante en spécifiant votre interface réseau (ex: `eth0`):

```
INTERFACESv4="eth0"
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
subnet 192.168.1.0 netmask 255.255.255.0 {
    range 192.168.1.100 192.168.1.200;
    option routers 192.168.1.1;
    option domain-name-servers 8.8.8.8, 8.8.4.4;
    option broadcast-address 192.168.1.255;
    default-lease-time 600;
    max-lease-time 7200;
}
```

Pour attribuer une IP fixe à une machine spécifique :

```bash
host PC_Fixe {
    hardware ethernet AA:BB:CC:DD:EE:FF;
    fixed-address 192.168.1.50;
}
```

Sauvegardez (`CTRL+X`, `Y`, `Entrée`).

---

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
auto eth0
iface eth0 inet dhcp
```

Sauvegardez (`CTRL+X`, `Y`, `Entrée`).

Redémarrez l'interface réseau :

```bash
sudo systemctl restart networking
```

Ou utilisez directement **dhclient** pour récupérer une IP :

```bash
sudo dhclient -v eth0
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

