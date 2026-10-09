#!/usr/bin/env bash

WATCH_DIR="."

inotifywait -m -e close_write -e moved_to --format "%f" "$WATCH_DIR" | while read -r FILE
do
    if [[ -f "$WATCH_DIR/$FILE" ]]; then

        EXT="${FILE##*.}"
        EXT_LOWER=$(echo "$EXT" | tr '[:upper:]' '[:lower:]')

        TARGET_DIR=""
        case "$EXT_LOWER" in
            txt|sh)             TARGET_DIR="textfiles" ;;
            docx)               TARGET_DIR="docs" ;;
            jpeg|jpg|png)       TARGET_DIR="images" ;;
            pdf)                TARGET_DIR="pdfs" ;;
            mp3|wav)            TARGET_DIR="music" ;;
            mp4|mov|avi|wmv)    TARGET_DIR="videos" ;;
            *)                  TARGET_DIR="" ;;
        esac

        if [[ -n "$TARGET_DIR" ]]; then
            mkdir -p "$TARGET_DIR"
            echo "File: $FILE moved to: $TARGET_DIR/"
            mv "$WATCH_DIR/$FILE" "$TARGET_DIR/"
        fi
    fi
done

