# Instruktioner till Linux lab

```bash 

 Skriva skript som automatiskt sorterar filer i directory "Downloads" av olika format och flyttar dem till respektive directory:

- textfiler med ändelse .txt och .sh ska hamna i mappen "text files"
- filer av typer .docx ska hamna i mappen "docs"
- filer av typer .jpg, .jpeg, .png och .gif ska hamna i mappen "images"
- pdf-filer ska hamna i mappen "pdfs"
- ljudfiler av typen .mp3 ska hamna i mappen "music"
- videofiler av typen .mp4, .mov, .avi och .wmv ska hamna i mappen "videos"

![Mappar i Downloads](dlorg_reza_behzadi/Screenshots/Skärmbild_1.png)

## Skriptet är som följande:

```bash

#!/usr/bin/env bash

# Mappen som ska övervakas (just nu mappen där skriptet körs)
WATCH_DIR="."

inotifywait -m -e close_write -e moved_to --format "%f" "$WATCH_DIR" | while read -r FILE
do 
    if [[ -f "$WATCH_DIR/$FILE" ]]; then

# Det ska inte ha någon betydelse om suffixet skrivs med stora eller små bokstäver:
    
        EXT="${FILE##*.}"
        EXT_LOWER=$(echo "$EXT" | tr '[:upper:]' '[:lower:]')

# Sätter upp vilka mappar som behövs baserat på olika suffix:

        TARGET_DIR=""
        case "$EXT_LOWER" in
            txt|sh)             TARGET_DIR="textfiles" ;;
            docx)               TARGET_DIR="docs" ;;
            jpeg|jpg|png)       TARGET_DIR="images" ;;
            pdf)                TARGET_DIR="pdfs" ;;
            mp3|wav)            TARGET_DIR="music" ;;
            mp4|mov|avi|wmv     TARGET_DIR="videos" ;;
            *)                  TARGET_DIR="";;   # Hoppar över andra filer
        esac

# Om filen matchar någon av våra filtyper, flytta den till respektive mapp. Om mappen inte finns, skapa den:

        if [[ -n "$TARGET_DIR" ]]; then
            mkdir -p "$TARGET_DIR"
            echo "File: $FILE moved to: $TARGET_DIR/"
            mv "$WATCH_DIR/$FILE" "$TARGET_DIR/"
        fi
    fi
done



