# Die Welt von Zuul – Vorbereitungen

## Lernziele

Nach Bearbeitung dieses Arbeitsblatts:

- kannst du dich in der "Welt von Zuul" bewegen
- hast du einen Überblick über die Architektur der Anwendung 
- kannst du eine Javadoc-Dokumentation generieren lassen und lesen
- kannst du Git-Grundbefehle anwenden (init, status, add, commit)
- kannst du Versionen mit Git verwalten

---

## Aufgaben

- [ ] **Klone folgendes Projekt:** [https://bitbucket.org/szut_anwend/worldofzuul.git](https://bitbucket.org/szut_anwend/worldofzuul.git)

- [ ] **Überblick über die Klassen:** Um einen Überblick über die Klassen der Anwendung zu bekommen, öffne und studiere das Klassendiagramm im Ordner `doc` des Hauptverzeichnisses.

- [ ] **Javadoc erstellen:** Die Anwendung ist sehr gut dokumentiert. Erstelle im Ordner `doc` eine Javadoc-HTML-Dokumentation. Erkundige dich dafür mit Hilfe des Links [Working with Code Documentation](https://www.jetbrains.com/help/idea/working-with-code-documentation.html).
   
    Erfahre mehr über die Anwendung, indem du die erstellte HTML-Dokumentation in einem Browser öffnest und liest.

- [ ] **Teste die Anwendung:** Führe dazu die `main`-Funktion in der Klasse `ZuulUI` aus. Schaue in einer geeigneten Klasse nach den bisher erlaubten Befehlen.

- [ ] **Studiere die Implementierung:** Schaue dir danach die Implementierung der Klassen an. Kläre dabei folgende Fragen:
    - Was tut diese Anwendung?
    - Welche Befehle akzeptiert das Spiel?
    - Was bewirken die einzelnen Befehle?
    - Wie viele Räume gibt es in der virtuellen Umgebung und wie sind sie angeordnet? Zeichne die virtuelle Umgebung.
    - Erkunde die einzelnen Klassen und schreibe für jede Klasse ihren Einsatzzweck auf.

- [ ] **Git-Repository initialisieren:** Du wirst verschiedene Versionen der Anwendung erstellen. Die verschiedenen Versionen sollen mit der Versionsverwaltung **Git** verwaltet werden.
   
    Git verwaltet die verschiedenen Versionen einer Anwendung in einem **Repository**. Das befindet sich im Ordner `.git`, den du über den Explorer im Hauptverzeichnis des Zuul-Projekts sehen kannst.
   
    Um ein völlig neues Repository zu erstellen, lösche den Ordner `.git` aus dem Verzeichnis. Öffne in IntelliJ das Terminal (Reiter unten links) und setze den Befehl ab:
   
    ```bash
    git init
    ```
   
    Git erzeugt nun im Hauptverzeichnis des Projekts ein leeres Repository.

- [ ] **Dateien tracken:** Die schon angelegten Dateien des Projekts wurden dem Repository noch nicht hinzugefügt. Das kannst du mithilfe eines Git-Befehls in Erfahrung bringen:
   
    ```bash
    git status
    ```
   
    Dieser Befehl zeigt dir alle **ungetrackten** Dateien bzw. Ordner an, das heißt Git hat diese noch nicht im Repository abgespeichert.
   
    Um die Dateien der **Staging Area** hinzuzufügen:
   
    ```bash
    git add -A
    ```
   
    `-A` steht für **All**. Git fügt alle untracked Files der Staging Area hinzu.

- [ ] **Ersten Commit erstellen:** Um deine erste Version zu erzeugen, muss ein Commit erstellt werden:
   
    ```bash
    git commit -m "zuul 0"
    ```
   
    Der `commit`-Befehl verpackt die Dateien in ein **Commit** und speichert sie im Repository. Ein Commit ist ein Schnappschuss des aktuellen Dateisystems. Git verpackt den aktuellen Status und versieht ihn mit einem eindeutigen Hashwert und speichert ihn mit verschiedenen Metadaten ab.

---

## Zusammenfassung

!!! success "Das hast du gelernt"
    - **Git clone** lädt ein Projekt aus einem Remote-Repository
    - **Javadoc** generiert HTML-Dokumentation aus Kommentaren
    - **git init** erstellt ein neues lokales Repository
    - **git add** fügt Dateien zur Staging Area hinzu
    - **git commit** speichert einen Snapshot des Projekts

---

??? question "Selbstkontrolle"
    1. Was ist der Unterschied zwischen `git add` und `git commit`?
    2. Wofür steht das `-m` bei `git commit -m "Nachricht"`?
    3. Welche Klasse enthält die `main`-Methode im Zuul-Projekt?
    
    ??? success "Antworten"
        1. `git add` fügt Dateien zur Staging Area hinzu, `git commit` speichert diese als Version im Repository
        2. `-m` steht für "message" und erlaubt die Commit-Nachricht direkt anzugeben
        3. Die Klasse `ZuulUI`
