#!/usr/bin/env bash

WATCH_DIR="."

TARGET_DIR="./images" 

mkdir -p "$TARGET_DIR"

inotifywait -m -e create -e moved_to -- format "%f" "$WATCH_DIR" | while read -r FILES
do
    if [[ "$FILE" == *.jpeg || "$FILE" == *.jpg || "$FILE" == *.png || "$FILE" == *.gif ]]; then
        if [ -f "$WATCH_DIR/$FILE" ]; then
            echo "Fil: $FILE moved to Directory: $TARGET_DIR/"
            mv "$WATCH_DIR/$FILE" "$TARGET_DIR/"
        fi
    fi
done
