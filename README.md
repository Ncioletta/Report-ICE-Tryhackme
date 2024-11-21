# Report: ICE room Tryhackme

Con questa repository, si vuole reportare il processo di scan di un IP target e l'exploiting di un payload malevolo sul server vulnerabile Icecast. 

**1. CONNESSIONE ALLA VPN**

**_Scaricamento del file di configurazione VPN_**

Il file di configurazione OpenVPN (.ovpn) è stato scaricato dalla sezione Accesso VPN della piattaforma TryHackMe dopo aver effettuato l’accesso con le credenziali personali.

**_Installazione di OpenVPN:_**

È stato verificato che OpenVPN fosse installato sul sistema eseguendo il comando:
```
openvpn --version
```

La connessione è stata avviata utilizzando il file di configurazione scaricato, con il seguente comando:
```
sudo openvpn --config <percorso_del_file>.ovpn
```

Per confermare il corretto instradamento del traffico attraverso la VPN, è stato controllato l’indirizzo IP.


**2. SCAN DELLA VITTIMA**

Una volta effettuato il collegamento alla VPN, dato un IP target denominato vittima, si effettua uno scan nmap con il comando:

```
nmap -v -T5 -Pn vittima
```
dove
``` -v ``` indica maggiore verbosità, ```-T5``` indica un ivello di aggressività molto alto per la scansione, ottimizzando la velocità a scapito della precisione e del rischio di rilevamento da parte di sistemi di sicurezza, e ```-Pn``` disabilita il ping.

I risultati sono i seguenti:




