# GOOGLE CALENDAR KIOSK 

Laboratorio per la configurazione di un raspberry per la creazione di una lavagna interattiva che mostri il google calendar del giorno

# Configurazione di un Raspberry Pi per Visualizzare Google Calendar

## Apparecchiature Necessarie

- **Raspberry Pi**: Qualsiasi modello compatibile con Raspberry Pi OS (Raspberry Pi 4 raccomandato per le migliori prestazioni).
- **Scheda Micro SD**: Con Raspberry Pi OS installato.
- **Alimentatore per Raspberry Pi**: Assicurati che abbia una potenza adeguata.
- **Cavo HDMI**: Per collegare il Raspberry Pi allo schermo.
- **Schermo**: Monitor o TV con ingresso HDMI.
- **Tastiera e Mouse**: Per le configurazioni iniziali (non necessarie dopo la configurazione).

## Passaggi per la Configurazione

### 1. Controllo del Server Grafico (X11)

- **Verifica se il server grafico in uso è X11**: Apri un terminale e esegui:
  
      echo $XDG_SESSION_TYPE
  
Se l'output è `x11`, sei a posto. Se l'output è `wayland`, dovrai cambiare il server grafico.
Esegui il comando 

      sudo raspi-config

 Vai su Advanced Options → GL Driver e seleziona GL (Fake KMS) o GL (Full KMS). Riavvia il Raspberry Pi.

### 2. Verifica e Installazione di Chromium

    chromium --version

Se Chromium è installato, vedrai la versione. Se non è installato, procedi al passaggio successivo.

Installazione di Chromium: Se Chromium non è installato, esegui:

    sudo apt update
    sudo apt install chromium-browser -y

Se il browser chromium è installato configura il tuo profilo google in modo che il calendar mostrato sia quello della tua utenza.

### 3. Creazione dello Script per Avviare Chromium

Crea uno script: Apri un terminale e crea un nuovo file di script:

    nano /home/pi/start_calendar.sh

Aggiungi il seguente codice:

      #!/bin/bash
      sleep 10  # Attendi 10 secondi
      chromium-browser --kiosk --app=https://calendar.google.com/calendar/r/day --incognito --disable-gpu --disable-software-rasterizer

Salva ed esci: Premi CTRL + O, poi Enter, e CTRL + X per uscire.

Rendi eseguibile lo script: Esegui:

    chmod +x /home/pi/start_calendar.sh

### 4. Modifica del File di Configurazione per Eseguire lo Script all'Avvio

Modifica il file di autostart: Apri il file di autostart:

sudo nano /etc/xdg/lxsession/LXDE-pi/autostart

Aggiungi la seguente riga:


    @/home/pi/start_calendar.sh

Salva ed esci: Premi CTRL + O, poi Enter, e CTRL + X per uscire.

### 5. Riavvia il Raspberry Pi:

    sudo reboot

