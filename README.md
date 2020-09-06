# VersionOnePointO

## Build Setup

```bash
# install dependencies
$ npm install

# serve with hot reload at localhost:3000
$ npm run dev

# build for production and launch server
$ npm run build
$ npm run start

# generate static project
$ npm run generate
```

For detailed explanation on how things work, check out [Nuxt.js docs](https://nuxtjs.org).


About this Project:

Konzept:
Ich bin nicht so begeistert für Spiele Entwicklung, deswegen habe ich mich dazu entschieden
meinen Shader für eine Webumgebung zu schreiben. Für meine eigene Website brauche ich in vorrausehbarer Zukunft noch
eine Art Slider/Hero-Element. Besonders interessiert hat mich, wie ich verschiedene Bilder ineinander überlenden kann
mithilfe einer Art "Glitch" Effekt. Um diesen Effekt zu erzielen hab ich mich entschieden einfach die UVs meines Models zu animieren.
Erst einmal muss natürlich ein Maus event abgefangen werden und über ein Uniform der Fragment Shader geupdatet werden.
Allgemein eine gewisse Schwierigkeit bei diesem Projekt, war auf die Laufzeit des Projekts zugreifen zu können.
Nach einigen Versuchen habe ich einfach entschieden ein externes JS Bundle zu verwenden (Animejs).

Da das reine Anzeigen und überlenden mir etwas zu langweillig war hab ich mich entschieden nocheine kleine Animation für den Hintergrund zu bauen.
Diese Animation reagiert auch zur Laufzeit. Allerdings habe ich diesen Zeitwert noch etwas abstrahiert.

Ich glaube es ist wenig sinnvoll jede kleine Einzelheit dieses Projekts zu erklären. Der Hauptaspekt ist denke ich einigermaßen klar, aber ich gehe hier vielleicht noch einmal kurz auf Probleme bzw Learnings dieses Projekts ein.
Erst einmal ging es darum das Module überhaupt zum Laufen zu kriegen. In Nuxt gibt es die Möglichkeit externe Bundles als Plugins ein zu binden. Mehr darüber findet man in der Nuxt Dokumentation, um es kurz zu machen, kann man auch kurz einen kleinen Blick in die "Transpile" Propertie der Config werfen.

Da es sich hier um ein SSR Projekt handelt, musste sichergestellt werden, dass die HTML Elemente existieren sobald THREEJS darauf zu greifen möchte. Es gibt bestimmt einfachere Wege aber da es funktioniert, habe ich die gesamte Logik einfach in den "Mounted" Hook gepackt.

Shaderspezifisch habe ich zwar viel gelernt, aber das war großteillig wahrscheinlich noch sehr basic. Ersteinmal die Kommunikation-Pipeline des gesamten Systems via Variyngs und Uniforms.

Der gesamte Ablauf von Localspace zu Worldspace zu Cameraspace war für mich zunächst auch etwas verwirrend, da ich zunächst ein anderes Projekt vor Augen hatte indem ich auf den Z-DEPTH des Vertex-Shaders zu greifen wollte. Auch wenn ich dieses Konzept am Ende nicht verwendet habe, so bin ich doch froh es verstanden zu haben. Ich kann mir gut vorstellen mit diesem Konzept irgendwann nochmal mehr zu machen.

Besonders hilfreich war das "Book of Shaders" und einige Codebeispiele von Codedrops. Ua dieses Beispiel: https://codepen.io/ReGGae/pen/bmyYEj hat mir sehr weiter geholfen. Allg hat das Projekt wirklich Spaß gemacht auch wenn es erst einmal ziemlich schwer war mit den ganzen neuen Konzepten und der 3D Pipeline klar zu kommen.


