#!/usr/bin/env bash

WATCH_DIR="."

TARGET_DIR="./vidoes" 

mkdir -p "$TARGET_DIR"

inotifywait -m -e create -e moved_to -- format "%f" "$WATCH_DIR" | while read -r FILES
do
    if [[ "$FILE" == *.mp4 || "$FILE" == *.mov || "$FILE" == *.avi || "$FILE" == .wmv ]]; then
        if [ -f "$WATCH_DIR/$FILE" ]; then
            echo "Fil: $FILE moved to Directory: $TARGET_DIR/"
            mv "$WATCH_DIR/$FILE" "$TARGET_DIR/"
        fi
    fi
done
