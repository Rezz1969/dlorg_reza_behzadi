## Här är allmänna skriptet för att för att flytta filerna:

#!/usr/bin/env bash

- Här är directoryn som ska bevakas
WATCH_DIR="."

-  Här är directoryn som filen ska flyttas till
TARGET_DIR="./xxx" 

- Om directoryn som filen ska flyttas till inte finns, skapas den
mkdir -p "$TARGET_DIR"

- skriptet letar efter nya filer och flyttar dem
inotifywait -m -e create -e moved_to -- format "%f" "$WATCH_DIR" | while read -r FILES

-  kontrollerar vad filen har för suffix. Om det är fler filer med olika typer som ska flyttas till samma directory används "|" mellan olika filtyper i if-satsen
if [[ "$FILE" == *.xx1 || "$FILE" == *.xx2 || "$FILE" == *.xx3]]; then
    if [ -f "$WATCH_DIR/$FILE" ]; then
-  meddelar att filen flyttas till $TARGET_DIR:
        echo "Fil: $FILE moved to Directory: $TARGET_DIR/"

- flyttar filen till rätt directory därefter
        mv "$WATCH_DIR/$FILE" "$TARGET_DIR/"
    fi
fi
