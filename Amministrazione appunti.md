## Comandi utili
- **Date** Serve per conoscere la data accetta come argomenti:
```
Date + [%R] //mostra l'ora
Date + [%X] //mostra la data nel formato (gg/mm/aaaa)
```

- **Passwd** Serve per modificare la password
```
passwd nomeutente
```

- **File** Serve per conoscere l'estensione dei file
```
file path 
```
> Es. file /bin/passwd | file /etc/shadow

- **Head/tail** Mostra di default le prime o le utilme righe di un file
```
Head -n(num. righe) path
Tail -n path
```
> Es. Head -**2** */etc/passwd* | tail -**2** /etc/passwd

- **Wc** Conta le linee, caratteri e parole in un file. Usato con **-L**, **-w**, **-c**.
> Es. Wc -l /etc/passwd

- **History** mostra la lista dei comandi usati precedentemente
- **Pwd** mostra la directory corrente
- **Cd** mi muovo tra le directory
- **Touch** creo un file o ne modifico la data di creazione

## Command-line file management
![[Pasted image 20230421122524.png]]
![[Pasted image 20230421122532.png]]

## Matching file names using path name expansion
<u>Pattern matching</u>
![[Pasted image 20230421122655.png]]
![[Pasted image 20230421122703.png]]
> Es. `[student@desktop ~]$mkdir glob; cd glob`
> `[student@desktop ~]$touch alfa bravo charlie delta echo able baker cast dogeasy`
> `[student@desktop glob]$ls` 
> `able alfa baker bravo cast charlie delta dog easy echo`

- **Brace expansion** Usato per generare stringhe di caraatteri
> Es. Creare 12 file nominati "**tv_seasonX_episodeY.ogg**" con X da **1-2** e Y **1-6**
> `touch tv_season{1..2}_episode{1..6}.ogg` 


## Redirecting output to a file
![[Pasted image 20230421121938.png]]

> Es.1 Salva un timestamp per un riferimento 
> `$date > /tmp/saved-timestamp`
> Es.2 Copia le ultime 100 linee da un file log a un'altro
> `$tail -n 100 /var/log/dmesg > /tmp/last-100-boot-messages`

## Constructing pipilines
- Una pipeline è una sequenza di uno o più comandi separati da **|**. Una pipe connette lo standard output del primo comando, allo standard input del comando successivo.
> Es. Questo comando prende lo standard output del comando **ls** e utilizza **less** per visualizzarlo su un terminale alla volta.
> `[student@desktop ~]$ls -l /usr/bin | less`

- comando **tee**, copierà il suo standard input nel suo standard output e reindirizzerà anche il suo standard output ai file nominati come argomenti del comando.
> Es. Questo comando redigerà lo standard output del comando ls al file e lo passerà a less che lo visualizzerà a schermo su un terminale alla volta.
> `[student@desktop ~]$ls -l | tee /tmp/saved-output | less`

## Users and Groups
### Utenti
- comando **id**, mostra le informazioni dell'utente loggato corrente
```
id username
```

- di default il sistema utilizza un "flat file", **/etc/passwd** per memorizzare le informazioni degli utenti locali. Per visualizzarlo digitare:

```
 grep username /etc/passwd
```
> Es. di output
> `student:x:1001:1001:Mario Rossi:/home/student:/bin/bash`

### Gruppi
- Come gli utenti i gruppi hanno un *nome* e un *numero(GID)*. I gruppi loccali sono definiti in **/etc/group**. Essi si dividono in:
	- **Primary**
		- Ogni utente ne ha uno
		- Contiene i propri file creati dall' utente
		- il gruppo primario di un nuovo utente è il nuovo gruppo con lo stesso nome dell'utente
	- **Supplementary**
		- I gruppi aggiuntivi si trovano listati nel file **/etc/group** separati dalla virgola
		- Es. `students:x:1001:teacher,student`


### Superutenti
-  Per cambiare utente utilizzare il comando: 
```
su [-] <username>
```
- Il comando **sudo**, permette di usare un comando come root o un altro utente basato sulle impostazioni nel file **/etc/sudoers**

## Managing local user accounts
- **useradd** crea un utente
- **usermod** modifica un utente esistente
	- ![[Pasted image 20230421144158.png]]
	- ![[Pasted image 20230421144213.png]]
	- per cambiare il gruppo **primario** di un utente con il comando usermod:
		- `usermod -g <nomegruppo> <user>`
	- per aggiungere un gruppo supplementare:
		- `usermod -aG <nomegruppo2> <user>`

- **userdel** elimina utente
- **id**, mostra informazioni dell'utente
- **passwd**, setta la password

## Managing local group accounts
- **groupadd** crea gruppi
> Es. di creazione specificando un **GID** diverso da quello di default
> `groupadd -g 5000 ateam`
- **groupmod** modifica un gruppo esistente
> Es. di modifica per specificare un nuovo **GID**
> `groupmod -g 6000 ateam`
- **groupdel**, elimina un gruppo
```
groupdel <nome del gruppo>
```

## Managing user Password
- **chage**, comando per definire le impostazioni delle password per l'utente a seconda di certi criteri quali sono:
```
chage -d 0 <username> /forzerà un aggiornamento della pasword al prossimo login
chage -l <username> /lista le impostazioni dell utente
chage -E YY-MM-DD /expire di un account su un giorno specifico
```
![[Pasted image 20230428145449.png]]
- **chage -m 0 -M 90 -W 7 -I 14 username**
- **date**, può essere usato per calcolare una data futura
> - Es. `date -d "+45 days"`



## Linux file permissions
### Permessi sui file
-  Si usa il comando **chmod** che include:
```
- chmod [###][file o dir]
	- [x1|x2|x3]: r=4, w=2, x=1 (la somma, 7, vuol dire tutti i permessi)
	- x1: permessi su utente
	- x2: permessi su gruppo
	- x3: permessi su altro
```
![[Pasted image 20230428152422.png]]

>Es. di permessi su file con tutti i permessi sull'utente e solo lettura per i gruppi
>	**chmod 740 thomasFile**

> Es. su una dir (metodo simbolico)
>	**chmod g+w /home/disney-team/**

### Permessi proprietario del file o gruppo
- Si usa il comando **chown** che include
```
chown [user][file]
chown :[gruppo][dir]
opzione -R cambia ricorsivamente all'intero albero proprietario
```
> Es. viene cambiato il proprietaario del gruppo foodir agli admin 
> 	**chown -R :admins foodir**


### Permessi file di default
- Il comando **umask** usato con un singolo argomento numerico, cambia l'umask della shell corrente. I valori per le shell degli utenti vengono salvate nei file **/etc/profile** e **/etc/bashrc**.
> <u>Es. creando file e dir senza modificare umask</u>
> **touch newfile
> ls -la newfile
> -rw-r--r--. 1 student students 0 Sep 4 17:49 newfile
> mkdir newdir
> ls -lad newdir/
> drwxr-xr-x. 2 student students 6 Sep 4 17:49 newdir/**
> <u>Es. settando umask 0 questa impostazione non nasconderà nessuno dei permessi dei nuovi file</u>
> **umask 0
> touch newfile2
> ls -la newfile2
> -rw-rw-rw-. 1 student students 0 Sep 4 17:50 newfile2
> mkdir newdir2
> ls -lad newdir2
> drwxrwxrwx. 2 student students 6 Sep 4 17:51 newdir2**
> <u>Se setto il valore umask a 007 questa impostazione maschererà tutte le "altre" autorizzazioni dei nuovi file</u>
**> -rw-rw----. 1 student students 0 Sep 4 17:52 newfile3
> drwxr-xr-x. 2 student students 6 Sep 4 17:49 newdir/**