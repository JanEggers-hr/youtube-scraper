# youtube-scraper v05

Einfaches Python-Skript zum Verschriftlichen aller Videos eines Youtube-Channels. Wer einen API-Key von Aleph Alpha nutzt, kann sich eine KI-generierte Inhaltsübersicht erstellen lassen. / *A simple Python Notebook that turns all videos in a channel to transcripts. If you have an API key for the Aleph Alpha LLM, you can get an AI-generated summary as well.*

## Wie man es ausführt / *How to run it*

- Braucht: Ein Google-Konto (für die Verbindung mit einem Google Drive und für die Colab-Nutzung) / *User needs: a Google account (for using Google Colab, and a Google Drive as storage)*
- Die [Notebook-Seite in diesem Repository](https://github.com/JanEggers-hr/youtube-scraper/blob/main/youtube_scraper.ipynb) ansteuern und auf "Open in Colab" klicken / *Click "Open in Colab" in the [Notebook](https://github.com/JanEggers-hr/youtube-scraper/blob/main/youtube_scraper.ipynb)*
- In den dritten Code-Zelle die URL des Ziels und Ausgabe-Ordner eintragen / *Change target channel's URL and output folder in the first code cell* ```channel_url = "www.youtube.com/@channelname"```
- Im Colab-Menü unter "Laufzeit/Laufzeittyp ändern" die GPU einschalten - davon profitiert die Speech-to-text-Umwandlung enorm /* **Switch the Runtime type to GPU in the Runtime menu - speeds up speech-to-text conversion immensely** *
- Alle Code-Zellen nacheinander ausführen. Dem Colab auf Anfrage den Zugriff auf das Google-Drive freigeben - es legt dort einen Ordner ```/youtube-scraper/output``` an. 

## Was es tut / *What it does*
- Nutzt die Library [yt-dlp](https://github.com/yt-dlp/yt-dlp), um eine Liste der Videos des Kanals zu erstellen. / *Using the [yt-dlp](https://github.com/yt-dlp/yt-dlp) library - a fork of youtube-dl - to list the videos and get the metadata*
- Speichert diese Liste mit einigen Infos zu den Videos (Uploaddatum, Views, Länge) als Excel-Datei im Google-Drive des Nutzers ab. / *Saves the list containing info like upload date, views, number of comments to the user's GDrive*
- Lädt die Videos in der kurzen M4A-Audio-Variante herunter / *Downloads an M4A audio file of the videos from Youtube*
- Wandelt dann mit der OpenAI-KI-Bibliothek [whisper](https://github.com/openai/whisper) die Audios in Textdateien um. / *Uses OpenAI's [whisper](https://github.com/openai/whisper) library to transcribe the audios*
- Nutzt dann den (kostenpflichtigen) Service von [Aleph Alpha](https://www.aleph-alpha.com/luminous), um den Text automatisiert in Stichpunkte zusammenzufassen / *Uses the (paid) [Aleph Alpha](https://www.aleph-alpha.com/luminous) AI to summarize the text to bullet points*

## Notizen / notes

- yt-dlp ist sehr großzügig und akzeptiert Youtube-Adressen in allerlei Form: Playlisten, Channel-Namen, URLs von einzelnen Videos. Das Skript kann zwar aus einzelnen Videos keine Liste von Videos erstellen, der Download funktioniert aber trotzdem. / *yt-dlp is quite tolerant, so you may try to get single videos, although the script might fail.*
- Daran denken: Das Skript kann keine Videos finden, die der Ersteller auf "Nicht gelistet" gestellt hat. (Technisch ist das, was als Kanal-Videos angezeigt wird, eine Playlist aller "Öffentlich"-Videos.) Einzelne "Nicht gelistet"-Videos, die man über Links gefunden hat, muss man sich einzeln über diesen Link dazuholen. / *Remember that videos can only be found if they are "Public", or listed in a playlist.*
- Das Skript legt im Google Drive der Nutzerin einen Ordner ```MyDrive/youtube-scraper/output``` an. Dort finden sich die M4A-Dateien, die Transkripte als .txt-Dateien, und die Liste der Videos als .xlsx. Wenn das Skript durch ist: den Ordner am besten einfach umbenennen. / *Output files: M4A, TXT, and the XLSX with the metadata and summary, are found in the output folder - ```MyDrive/youtube-scraper/output``` by default*
- Wenn das Skript beim Download eines Videos scheitert - was immer mal wieder vorkommt - versucht es herauszufinden, welche Videos noch offen sind, und startet den Download für diese Videos nochmal. Wenn irgendwas Grundlegendes schief läuft. führt das erst nach einigen hundert Versuchen zum Abbruch. / *Should a download fail, or the Notebook lose its connection, run the cells again - it will get the files it hasn't done yet.*
- *Remember that a Google Colab needs you to stay connected; leave the browser tab open and interact with the page, or it will disconnect after 30 minutes. There are browser plugins to automate that.*
