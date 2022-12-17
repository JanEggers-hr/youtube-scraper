# youtube-scraper
Einfaches Python-Skript zum Verschriflichen aller Videos eines Youtube-Channels.

## Was es tut
- Nutzt die Library [youtube-dl](https://github.com/ytdl-org/youtube-dl), um eine Liste der Videos des Kanals zu erstellen. 
- Speichert diese Liste mit einigen Infos zu den Videos (Uploaddatum, Views, Länge) als Excel-Datei im Google-Drive des Nutzers ab. 
- Versucht dann mit youtube-dl, diese Videos nach und nach herunterzuladen und gleich in ein MP3 zu wandeln. (Die Videodatei wird aus Platzgründen weggeworfen.)
- Wandelt dann mit der OpenAI-KI-Bibliothek whisper die Audios in Textdateien um.

## Wie man es ausführt

- Braucht: Ein Google-Konto (für die Verbindung mit einem Google Drive und für die Colab-Nutzung)
- Die [Notebook-Seite in diesem Repository](https://github.com/JanEggers-hr/youtube-scraper/blob/main/youtube_scraper.ipynb) ansteuern und auf "Open in Colab" klicken
- In den dritten Code-Zelle die URL des Ziels eintragen, z.B. so: ```channel_url = "www.youtube.com/@channelname"``` (siehe auch unten: Notizen)
- Im Colab-Menü unter "Laufzeit/Laufzeittyp ändern" die GPU einschalten - davon profitiert die Speech-to-text-Umwandlung enorm
- Alle Code-Zellen nacheinander ausführen. Dem Colab auf Anfrage den Zugriff auf das Google-Drive freigeben - es legt dort einen Ordner ```/youtube-scraper/output``` an. 
- Ab und zu nachschauen und ggf. die Download-Zelle nochmal anstoßen - ab und zu wirft Youtube dem Download einen Fehler in den Weg, dann einfach nochmal versuchen. 

## Notizen

- Wer noch nicht mit Colab gearbeitet hat: Nach einer Zeit ohne Nutzereingabe verliert die Benutzeroberfläche die Verbindung zur virtuellen Maschine - die arbeitet aber erst mal weiter. Einfach Verbindung wieder herstellen - und im Zweifelsfall nochmal von vorn: keine Angst, das Skript merkt, welche Videos schon heruntergeladen sind, und macht mit den nächsten weiter. 
- youtube-dl ist sehr großzügig und akzeptiert Youtube-Adressen in allerlei Form: Playlisten, Channel-Namen, URLs von einzelnen Videos. Das Skript kann zwar aus einzelnen Videos keine Liste von Videos erstellen, der Download funktioniert aber trotzdem. 
- Daran denken: Das Skript kann keine Videos finden, die der Ersteller auf "Nicht gelistet" gestellt hat. (Technisch ist das, was als Kanal-Videos angezeigt wird, eine Playlist aller "Öffentlich"-Videos.) Einzelne "Nicht gelistet"-Videos, die man über Links gefunden hat, muss man sich einzeln über diesen Link dazuholen. 
- Das Skript legt im Google Drive der Nutzerin einen Ordner ```/youtube-scraper/output``` an. Dort finden sich die MP3-Dateien, die Transkripte als .txt-Dateien, und die Liste der Videos als .xlsx. Wenn das Skript durch ist: den Ordner am besten einfach umbenennen. 
