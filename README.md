# Themenübersicht
1. [Die Godot-Engine](#die-godot-engine)
2. Programmiergrundlagen mit GDScript
3. Grundlagen von Git
4. Eigenes Projekt
>[!TIP]
> Bei allen Themen gibt es Übungen und eigenes Programmieren, nicht nur beim eigenen Projekt.

# Die Godot Engine
## 1. Was ist Godot und Installation

<u>Was ist Godot?</u> <br>
Godot ist eine Quelloffene Videospiel Engine. Also ein Werkzeug, welches es leichter macht Videospiele zu entwickeln. Solche Engines sind sinnvoll, damit man nicht für jedes Spiel wichtige Komponenten wie etwa die Physiksimulation oder gar das malen von Pixeln auf den Bildschirm neu implementieren muss.

<u>Installation</u> <br>
Die Engine kann einfach von https://godotengine.org/ heruntergeladen werden. Die hier heruntergeladene Datei ist eine Portable Executable, was heißt sie muss nicht installiert werden und kann direkt ausgeführt werden.

## 2. Grundkonzepte von Godot
<u>Nodes</u> <br>
Nodes sind sozusagen die Bausteine oder Zutaten eines Videospiels in Godot. Sie können verschiedenste Aufgaben erfüllen, wie etwa einen Ton abspielen, ein Bild anzeigen oder die Spielkamera darstellen. Ähnliche wie Dateien auf einem Computer würde aber komplettes Chaos enstehen, wenn all diese Nodes frei herumliegen.

<u>Szenen</u> <br>
Deshalb sortiert man sie in Szenen. Diese funktionieren wie Ordner auf einem Computer. Szenen können dabei wieder alles mögliche sein, von einer einzigen Münze bis zu einem ganzen Level. Deshalb ist ein Spiel in Godot also „ein Baum von Szenen [...] und [...] jede Szene [ist] ein Baum von Nodes“ (Dokumentation Nodes und Szenen).

In der Praxis sieht das dann zum Beispiel so aus: <br>
<img src="res/scene_tree.png" width="200"/> 

weitere Informationen über Nodes und Szenen findest du [hier](https://docs.godotengine.org/de/4.x/getting_started/step_by_step/nodes_and_scenes.html) und wichtige Konzepte in Godot [hier](https://docs.godotengine.org/de/4.x/getting_started/introduction/key_concepts_overview.html#doc-key-concepts-overview).

<u>Signale</u> <br>
Das letzte wichtige Konzept in Godot sind Signale. Mithilfe von Signalen können Nodes untereinander kommunizieren ohne das sie im Code fest miteinander verbunden sind. Mit ihnen kann man beispielweise feststellen, wenn 2 Objekete aufeinandergestoßen sind. Keine Sorge, falls das jetzt unverständlich klang, die Funktion von Signalen wird weit deutlicher, wenn man sie anwendet.

## 3. Anwendungsbeispiel
Um das gerade Besprochene ein wenig anschaulicher zu machen, werden wir jetzt ein kleines Spiel programmieren, in welchem wir (das MPG-Logo) auf dem Bildschirm bewegen können.==Keines Sorge, das Programmierte muss man noch nicht verstehen.== Es geht erst einmal darum die Engine kennenzulernen. Programmiergrundlagen besprechen wir später.

<u>Hauptszene erstellen</u> <br>
Damit wir anfangen können, müssen wir die gerade gelernten Konzepte gleich anwenden. Zu allererst müssen wir eine „Root Node“ erstellen, also eine Wurzel-Node. Diese ist sozusagen der Ursprung unseres Spiels in welches sich alles andere wie etwa der Spieler befindet.

<u>Spieler</u> <br>
Da wir ihn gerade erwähnt haben, wäre es jetzt auch passend ihn zu erstellen. Für den Spieler hat Godot einen passenden Node-Typ, nämlich den `Character Body 2D`. <br>
Wenn wir diesen nur aber erstellen, werden wir zunächst nichts sehen. Das liegt daran, dass der Spieler noch keinen Sprite also noch keine Textur hat. Für den Anfang werden wir dafür einfach das von Godot bereitgestellte Logo nehmen. Später können wir aber auch unsere eigenen Figuren nehmen. <br>
Zudem braucht der Spieler auch einen `CollisionShape 2D,`also ein Objekt welches markiert wie der Spieler physikalisch aussehen sollte. <br>
Diese beiden Nodes `Collision Shape 2D` und `Sprite 2D` sind nun unter dem `Character Body 2D` wie die Äste eines Baums. Somit ist die Player Node jetzt eine Szene. Damit er auch wirklich in unserem Spiel erscheint müssen wir ihn aber noch zur Hauptszene (der „Root Node“ von vorhin) hinzufügen.

<u>Inputmap erstellen</u> <br>
In Godot fragt man bei Benutzerinput nicht nach einem bestimmten Tastendruck (also z.B.: ASCII Codierung) im Code, das übernimmt die Engine, sondern nach einer "Action", welche man in einer Inputmap definiert. Diese Inputmap findet man unter `Project > Project Settings > Input Map`. Hierbei kann man die "Action" selbst nennen wie man will und Bedingungen definieren, welche diese "Action" auslösen. Die "Action" "go-right" kann also z.B.: ausgelöst werden wenn der Spieler die rechte Pfeiltaste oder "D" drückt.

<u>den Spieler bewegen</u> <br>
Godot löst jetzt zwar "Actions" aus, aber es passiert nichts in unserem Spiel. Das liegt daran, dass wir nicht definiert haben was passieren soll, wenn diese "Action" ausgelöst wurde. Dies machen wir mit folgendem  Code. <br>
```GDScript
func _process(delta: float) -> void:
	if Input.is_action_pressed("right") == true:
		position = position + Vector2(speed, 0) * delta
	if Input.is_action_pressed("left"):
		position += Vector2(-speed, 0) * delta
	if Input.is_action_pressed("up"):
		position += Vector2(0, -speed) * delta
	if Input.is_action_pressed("down"):
		position += Vector2(0, speed) * delta
```
`_process` ist dabei eine Godot-interne Funktion, die Ähnlich wie `update` bei einem Arduino ständig aufgerufen wird (hier jeden Frame). Zudem sagen wir das bei einem Funktionsaufruf eine Variable namens `delta` weitergegeben werden muss. Diesen Funktionsaufruf machen dabei nicht wir sondern die Engine. <br>
`delta` steht hierbei für die Zeit die seit dem letzen Frame vergangen ist. Dies für die Bewegung hier wichtig, sodass sich der Spieler nicht bei einer höheren Bildrate schneller bewegt. <br>
Zuletzt bedeutet `void` nur das die Funktion kein "return value" hat. <br>
Mit `if` schauen wir hier, ob eine Bedingung wahr ist. Man kann also auch schreiben `if Input.is_action_pressed("up") == true:`. <br>
`Input` ist nun eine Klasse. Das ist ähnlich wie Ordner auf einem Computer nur ein wenig Funktionen zu sortieren. Innerhalb dieser Klasse gibt es nun die Methode (so nennt man Funktionen, die in Klassen sortiert sind) `is_action_pressed()`. Diese sendet an Godot nun die Anfrage, ob eine Taste gedrückt wurde. <br>
Sollte eine Taste nun gedrückt sein wollen wir den Spieler bewegen. Dies machen wir indem wir die Aktuelle Position des Spielers nehmen und einen Vektor (also eine Verschiebung im Raum) hinzuaddieren.

>[!Tip]
> Wir haben hier jetzt nun einige Funktionen und Variablen gesehen. Diese waren dabei alle von Godot gegeben, weshalb wir sie einfach aufrufen können ohne dem Computer davor sagen zu müssen, was die Funktion eigentlich macht. Nachdem wir Funktionen und Variablen in Kapitel 2 besprochen haben wird das alles klarer.