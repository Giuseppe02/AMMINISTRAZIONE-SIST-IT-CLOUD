# Guida all'installazione di Docker e comandi essenziali
## Installazione di Docker

Per installare Docker nel tuo sistema, segui i passaggi seguenti:
- Set up the repository
	Install the dnf-plugins-core package (which provides the commands to manage your DNF repositories) and set up the repository.

```
sudo dnf -y install dnf-plugins-core
sudo dnf config-manager --add-repo https://download.docker.com/linux/fedora/docker-ce.repo
```

- To install the latest version, run:
```
$ sudo dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

1. AVVIA Docker.
```
$ sudo systemctl start docker
``` 
2. Verifica che l'engine sia installato:
```
$ sudo docker run hello-world
```
## Comandi essenziali di Docker

Ecco alcuni dei comandi più importanti di Docker:

- **`docker version`**: Mostra la versione di Docker installata e la versione del client.
    
- **`docker info`**: Fornisce informazioni dettagliate sullo stato di Docker.
    
- **`docker run [OPTIONS] IMAGE[:TAG] [COMMAND] [ARG...]`**: Esegue un nuovo container da un'immagine Docker specifica.
    
- **`docker ps [OPTIONS]`**: Elenca i container in esecuzione.
    
- **`docker images [OPTIONS]`**: Elenca le immagini Docker disponibili nel sistema.
    
- **`docker pull [OPTIONS] NAME[:TAG|@DIGEST]`**: Scarica un'immagine Docker dal registro.
    
- **`docker build [OPTIONS] PATH | URL | -`**: Crea un'immagine Docker a partire da un Dockerfile.
    
- **`docker push [OPTIONS] NAME[:TAG]`**: Carica un'immagine Docker nel registro.
    
- **`docker stop [OPTIONS] CONTAINER [CONTAINER...]`**: Arresta uno o più container in esecuzione.
    

## Esempio di creazione di un'immagine Docker

Supponiamo di avere una semplice applicazione Node.js che desideriamo incapsulare in un'immagine Docker. Ecco un esempio di Dockerfile:

Dockerfile

```
# Usa un'immagine base con Node.js preinstallato
FROM node:14

# Imposta il percorso di lavoro all'interno del container
WORKDIR /app

# Copia il file package.json nella directory di lavoro
COPY package.json .

# Installa le dipendenze dell'applicazione
RUN npm install

# Copia tutti i file sorgente nella directory di lavoro
COPY . .

# Esponi la porta 3000
EXPOSE 3000

# Definisci il comando da eseguire all'avvio del container
CMD ["npm", "start"]
```

1. Crea un file chiamato `Dockerfile` nella directory del tuo progetto.
    
2. Incolla il codice sopra all'interno del file `Dockerfile`.
    
3. Assicurati che il tuo terminale si trovi nella stessa directory del file `Dockerfile`.
    
4. Esegui il seguente comando per creare l'immagine Docker:
        
    `docker build -t nome_immagine .`
    
    Dove `nome_immagine` è il nome che desideri assegnare all'immagine Docker.
    
    Nota: Assicurati di includere il punto (`.`) alla fine del comando per indicare la directory corrente come contesto di build.
    
Un altro esempio:

```
FROM busybox:latest

COPY start.sh /start.sh

RUN chmod +x /start.sh

ENV \

AUTHOR=Docker

ENTRYPOINT ["/start.sh"]
```

Il file start.sh si trova anch'esso nel path dove si trova Dockerfile.

Una volta completata la creazione dell'immagine, puoi eseguire il tuo container utilizzando il comando `docker run`:
arduino
`docker run -p 3000:3000 nome_immagine`
Dove `nome_immagine` corrisponde al nome dell'immagine Docker che hai creato.

---
## Gestire start/build delle immagini
- **docker-compose.yml** verrà utilizzato per gestire start/build delle due immagini,gestire la variabile TIME_FORMAT, e il mount verso i due servizi in container della directory content:

```
version: '3'
services:
  httpd:
    build:
      context: ./httpd
    ports:
      - 80:80
    volumes:
      - ./content:/var/www/html
    depends_on:
      - content_management

  content_management:
    build:
      context: ./content_management
    volumes:
      - ./content:/content
    environment:
      - TIME_FORMAT=${TIME_FORMAT}
    command: bash -c "while true; do sleep 120; if [ \"$TIME_FORMAT\" == \"MM-YYYY\" ]; then echo \"$(date +'%m-%Y')\" > /content/index.html; elif [ \"$TIME_FORMAT\" == \"MM-DD-YYYY\" ]; then echo \"$(date '+%A %d-%B, %Y')\" > /content/index.html; fi; done"

```


