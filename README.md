# Report: ICE room Tryhackme

Con questa repository, si vuole reportare il processo di scan di un IP target e l'exploiting di un payload malevolo sul server vulnerabile Icecast. 

**1. CONNESSIONE ALLA VPN**

**_Scaricamento del file di configurazione VPN_**

Il file di configurazione OpenVPN (.ovpn) è stato scaricato dalla sezione Accesso VPN della piattaforma TryHackMe dopo aver effettuato l’accesso con le credenziali personali.

**_Verifica ed Eventuale Installazione di OpenVPN:_**

È stato verificato che OpenVPN fosse installato sul sistema eseguendo il comando:

**2. SCAN DELLA VITTIMA**

Dato un IP target denominato vittima, si effettua uno scan nmap con il comando 

![image](https://github.com/user-attachments/assets/f39622d7-b96d-44c3-beab-23c363a9cf04)

