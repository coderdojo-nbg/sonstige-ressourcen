# Betriebssystem

Es sollte nur die LTS-Version verwendet werden, da bei allen anderen Versionen nur für 6 Monate Updates zur Verfügung stehen. LTS-Versionen werden für 5 Jahre unterstützt und sind in der Regel stabiler, auch wenn dann auf einige aktuelle Feature verzichtet werden muss.

[Ubuntu LTS](https://www.ubuntu.com/download/desktop) runterladen und am besten ein [USB-Startmedium](https://wiki.ubuntuusers.de/Live-USB/) erzeugen. Dann nach Booten von USB Ubuntu installieren, sinnvoll ist eine Internetverbindung.

Der Computername sollte "coderdojo" plus Zahl sein (z.B. "coderdojo6")

## Benutzer

Während der Installation wird ein Nutzer mit Superuser-Rechten angelegt ("ruth"). Nach der Installationen einen Standardnutzer mit dem gleichen Namen wie der Computer anlegen. Dieser Account soll keine Superuser-Rechte haben und ist für die Kinder gedacht. Benutzername und Passwort mittels Labeldrucker auf den Computer kleben.

## Einstellungen

In den Systemeinstellungen unter "Anwendungen & Aktualisierung" -> "Aktualisierungen" bei "Nach Aktualisierungen suchen" die Option "Nie" auswählen. Damit wird verhindert, dass Ubuntu die Kinder mit Benachrichtigungen nervt. Updates müssen sowieso durch die Mentoren eingespielt werden.

## Admintools

```bash
sudo apt-get install aptitude vim curl
```

Danach kann das System aktualisiert werden.

```bash
sudo aptitude update
sudo aptitude safe-upgrade
````

# Programme

## Global

Diese Prgramme werden als Superuser ("ruth") installiert.

### Aus den Standardpaketquellen

Emacs, Java (JDK), Sonic-Pi, Pythontools

```bash
sudo aptitude install emacs openjdk-8-jdk sonic-pi sonic-visualiser python3 idle3 python3-pip git
```

Bei der Konfiguration von `jackd` die Echtzeitpriorisierung ablehnen.

### Als Debianpaket

[Chrome](https://www.google.de/chrome/index.html) und [Visual Studio Code](https://code.visualstudio.com/Download) können als Debianpaket runtergeladen werden.

Für die Installation in das Downloadverzeichnis wechseln und `dpkg` verwenden. Bei der Installation wird ein Eintrag für das Repository erzeugt, Aktualisierungen erfolgen dann beim normalen Update.

```bash
cd <VERZEICHNIS>
sudo dpkg -i <DEBIANPAKET>
sudo apt-get -f install
```
### Atom

Der Editor Atom kann aus einem Repository installiert werden, das zunächst zu den Quellen hinzugefügt werden muss.

```bash
curl -L https://packagecloud.io/AtomEditor/atom/gpgkey | sudo apt-key add -
sudo sh -c 'echo "deb https://packagecloud.io/AtomEditor/atom/any/ any main" > /etc/apt/sources.list.d/atom.list'
sudo aptitude update
sudo aptitude install atom
```
### PyCharm

Als alternative Python-IDE kann die Community-Edition von PyCharm (JetBrains) eingesetzt werden.

```bash
sudo snap install pycharm-community --classic
```

### Pythontools für Minecraft

Die Python-API für Minecraft kann als Modul installiert werden.

```bash
mkdir tmp
cd tmp
git clone https://github.com/py3minepi/py3minepi.git
sudo pip3 install ./py3minepi
```

## Lokal

Alle folgenden Programme werden als Standardbenutzer ("coderdojo") im entsprechenden Home-Verzeichnis installiert.
Zunächst einige Verzeichnisse anlegen:

```bash
mkdir local minecraftMods
```

### IntelliJ IDEA

Aktuelle [Version](https://www.jetbrains.com/idea/download/#section=linux) der Community-Version runterladen  und im Verzeichnis `local` ablegen und entpacken.

```bash
cd bin
./idea.sh
```

Bei der Installation können die Standardeinstellungen übernommen werden.

### Plugins für Atom

```bash
apm install parinfer autoclose-html highlight-line markdown-writer tabs-to-spaces linter linter-eslint
```

### Plugins für Visual Studio

```bash
code --install-extension dbaeumer.vscode-eslint --install-extension dracula-theme.theme-dracula --install-extension eg2.tslint --install-extension formulahendry.code-runner --install-extension msjsdiag.debugger-for-chrome --install-extension robertohuertasm.vscode-icons
```

### Node

Node wird am besten mittel `nvm` installiert.

```
wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash
command -v nvm
nvm install node
```

Wenn `command -v nvm` nichts zurückgibt, das Terminal schließen, neu öffnen und nochmal versuchen.

Damit ist aktuelleste Version von Node installiert. Ein paar Node-Module können installiert werden. Dafür zunächst `npm` initialisieren, es können die Standardwerte übernommen werden.

```bash
npm init
npm install http-server typescript tslint eslint
```

Weitere Infos zu [nvm](https://github.com/creationix/nvm).

### Processing

Aktuelle [Version](https://processing.org/download/) runterladen, in `local` speichern und entpacken. Ins processing-Verzeichnis wechseln und das Skript ausführen

```bash
cd processing-xxxx
./install.sh
```

### Minecraft-Mods

#### Minecraft

Verzeichnis für Minecraft anlegen:

```bash
cd local
mkdir minecraft
```
[Minecraft-Launcher](https://minecraft.net/de-de/download/) runterladen und in `minecraft` ablegen. Es muss nichts weiter installiert werden.

```bash
cd minecraft
java -jar Minecraft.jar
```

#### Spigot

Spigot muss aus den Quellen gebaut werden, die aktuellste Version funktioniert wahrscheinlich, auch wenn die Plugins für ältere Versionen geschrieben sind.

Es die Option `--rev VERSION` gesetzt werden. Sie gibt an welche Version von Spigot installiert werden soll. Ohne diese Option wird automatische die aktuellste ("latest") Version installiert.

```bash
cd minecraftMods
mkdir spigot
cd spigot
git config --global --unset core.autocrlf
wget https://hub.spigotmc.org/jenkins/job/BuildTools/lastSuccessfulBuild/artifact/target/BuildTools.jar
java -jar BuildTools.jar
```
Dieser Prozess dauert eine Weile. Wenn die Installation erfolgreich war, muss der Server konfiguriert.
werden.

```bash
java -jar ./spigot-xxxx.jar
```

Der Server startet jetzt nicht, sondern gibt eine Fehlermeldung aus, weil die EULA-Lizenz noch nicht akzeptiert wurde. Es wurde eine Datei "eula.txt" erstellt. In dieser muss **eula=false** zu **eula=true** geändert werden. Dafür die Datei in einem Texteditor, z.B. `vim` oder `gedit` öffnen und die entsprechende Textzeile ändern.

Nun den Server neu starten, nun sollte es funktionieren. Den Server durch Eingabe von `stop` beenden.

Optional: Spigot speichert für jede Minecraftwelt drei Verzeichnisse ab. Um diese in einem gemeinsamen Ordner abzulegen (dafür einen Ordner anlegen z.B. `mcworlds`), muss in der Datei `bukkit.yml` eine Option ergänzt werden. Im Abschnitt `settings` eine neue Zeile mit `ẁorld-container: mcworlds` einfügen.

#### ScriptCraft

ScriptCraft kann auf der [Webseite](https://scriptcraftjs.org/download/latest/) runtergeladen werden. Es reicht die Jar-Datei im "plugins"-Verzeichnis des Spigot-Servers zu speichern.

Unter Linux muss die Datei ggf. noch ausführbar gemacht werden, damit sie von Spigot geladen wird. Dafür eine Konsole öffnen und zum Verzeichnis mit der Datei wechseln.

```bash
# "scriptcraft.jar" durch tatsächlichen Dateinamen ersetzen
chmod u+x scriptcraft.jar
```
Beim nächsten Serverstart sollte ScriptCraft geladen werden.

#### RaspberryJuice

ScriptCraft kann auf der [Webseite](https://dev.bukkit.org/projects/raspberryjuice) runtergeladen werden. Es reicht die Jar-Datei im "plugins"-Verzeichnis des Spigot-Servers zu speichern.

Unter Linux muss die Datei ggf. noch ausführbar gemacht werden, damit sie von Spigot geladen wird. Dafür eine Konsole öffnen und zum Verzeichnis mit der Datei wechseln.

```bash
# "rasperryjuice.jar" durch tatsächlichen Dateinamen ersetzen
chmod u+x rasperryjuice.jar
```
Beim nächsten Serverstart sollte ScriptCraft geladen werden.

#### Forge

Forge wird nicht systemweit installiert, daher ist es sinnvoll eine eigenes Verzeichnis anzulegen:

```bash
cd minecraftMods
mkdir forge
```

Da von Forge regelmäßig neue Builds bereitgestellt werden, sollte man die benötigte Datei direkt auf der [Webseite](http://files.minecraftforge.net/) runterladen. Auf der linken Seite die gewünschte Version auswählen und für Linux die Jar-Datei runterladen (auf "Installer" klicken). **Achtung!** Beim Runterladen wird auf eine Webseite mit Werbung verlinkt, es empfiehlt sich mit aktivem AdBlocker unterwegs zu sein.

Die Jar-Datei in einem beliebigen Verzeichnis speichern. Eine Konsole in diesem Verzeichnis öffnen und den Installer starten.

```bash
# "forge.jar" durch Dateinamen ersetzen
java -jar forge.jar
```

Es startet ein graphischer Installer. Dort "Install Server" auswählen.
Forge will sich standardmäßig im .minecraft-Verzeichnis installieren. Besser ist es Forge in das oben erstellte Verzeichnis zu installieren. Das geht über den kleinen Button mit drei Punkten. Sollte das Zielverzeichnis nicht leer sein, erscheint eine Warnung, dass kann man aber ignorieren. Auf "Ok" klicken und Installation abwarten.

![Installation von Forge](https://raw.githubusercontent.com/coderdojo-nbg/minecraft/master/wiki-images/forge-install.png)

Auf der Konsole in das Verzeichnis wechseln.

```bash
cd forge
java -jar minecraft-server.1.12.2.jar nogui
```

Der Server startet jetzt nicht, sondern gibt eine Fehlermeldung aus, weil
die EULA-Lizenz noch nicht akzeptiert wurde. Es wurde eine Datei
"eula.txt" erstellt. In dieser muss **eula=false** zu **eula=true**
geändert werden. Dafür die Datei in einem Texteditor, z.B. `vim` oder `gedit`
öffnen und die entsprechende Textzeile ändern.

Nun den Server neu starten, nun sollte es funktionieren. Den Server durch Eingabe von `stop` beenden.

#### MCreator

In der `.bashrc`...

```bash
vim .bashrc
```

.... am Ende folgende Zeile ergänzen:

```
export JAVA_HOME="/usr/lib/openjdk-8-jdk-amd64/bin"
```

Ein neues Verzeichnis anlegen:

```bash
cd minecraftMods
mkdir mcreator
```

Aktuelle [Version](https://mcreator.net/download) runterladen, im Verzeichnis `mcreator` speichern und entpacken. Installer starten.

```bash
cd mcreator
java -jar mcreator.jar
```

### Lokaler Calliope-Editor

Der PXT-Editor kann lokal ausgeführt werden.

```bash
cd local
git clone https://github.com/calliope-mini/pxt-calliope-static
```

Um den Editor zu starten, muss lokal ein Webserver gestartet werden. Dies kann auch in einem Alias hinterlegt werden.

## Aliases

Um sich das Leben etwas leichter zu machen, können Aliases angelegt werden.

```bash
vim .bash_aliases
```

Inhalt der Datei:

```bash
alias la='ls -la'
alias ll='ls -l'
alias processing='./home/coderdojo/local/processing/processing'
alias minecraft='java -jar /home/coderdojo/local/minecraft/Minecraft.jar'
alias http-server='/home/coderdojo/node_modules/.bin/http-server . -c-1'
alias calliope='/home/coderdojo/node_modules/.bin/http-server -c-1 /home/coderdojo/local/pxt-calliope-static/release'
alias spigot='cd /home/coderdojo/minecraftMods/spigot && java -jar /home/coderdojo/minecraftMods/spigot/spigot-1.12.2.jar'
alias forge='cd /home/coderdojo/minecraftMods/forge && java -jar /home/coderdojo/minecraftMods/forge/minecraft_server.1.12.2.jar nogui'
alias mcreator='java -jar /home/coderdojo/local/mcreator/mcreator.jar'
```

Der Pfad zum Homeverzeichnis ("/home/coderdojo") muss ggf. angepasst werden, ebenso Versionen der Minecraftserver (Spigot und Forge).

## Weitere Dateien und Einstellungen

### ESLint

Für das JavaScript-Linting wird im Homeverzeichnis eine Datei mit dem Namen `.eslintrc.json` angelegt. Möglicher Inhalt:

```
{
    "parser": "babel-eslint",
    "parserOptions": {
      "ecmaVersion": 6,
      "sourceType": "module",
      "ecmaFeatures": {
        "jsx": true,
        "experimentalObjectRestSpread": true
        }
      },
    "rules": {
        "quotes": 0,
        "linebreak-style": [2, "unix"],
        "semi": [2, "always"],
        "no-underscore-dangle": 0,
        "no-use-before-define": 0,
        "no-undef": 2,
        "no-unused-vars": 1,
        "eqeqeq": [ 2, "smart" ],
        "curly": [ 2, "all" ],
        "no-trailing-spaces": 1,
        "no-multi-spaces": 1,
        "no-extra-parens": [1, "functions"],
        "default-case": 2,
        "no-extra-strict": 0,
        "no-alert": 1,
        "react/prop-types": 1,
  "no-console": 1
    },
    "env": {
        "es6": true,
        "node": true,
        "browser": true
            },
    "globals": {
      "assert": true,
      "dump": true
    },
    "extends": [
      "eslint:recommended",
      ]
}
```
