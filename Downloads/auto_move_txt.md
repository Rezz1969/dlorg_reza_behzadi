#!/usr/bin/env bash

WATCH_DIR="."

TARGET_DIR="./xxx" 

mkdir -p "$TARGET_DIR"

inotifywait -m -e create -e moved_to -- format "%f" "$WATCH_DIR" | while read -r FILES
do
    if [[ "$FILE" == *.txt || "$FILE" == *.sh]]; then
        if [ -f "$WATCH_DIR/$FILE" ]; then
            echo "Fil: $FILE moved to Directory: $TARGET_DIR/"
            mv "$WATCH_DIR/$FILE" "$TARGET_DIR/"
        fi
    fi
done
