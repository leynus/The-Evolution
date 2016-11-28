# **Evolution**

*ein Projekt von Leif Peters und Linus Reck*

Das vorliegende Programm wurde mit Hilfe "Greenfoot's" geschrieben. Beim Bearbeiten orientierten wir und an den Stride-Angaben und an dem Greenfoot-Buch.

## **Anfänge**
Unsere eigentliche Idee war es, mit einem RaspberryPi zu arbeiten. In den ersten Stunden probierten wir dies aus und installierten ein neues Betriebssystem auf einem dieser Computer. Da wir uns diese Arbeit allerdings anders vorgestellt hatten, hörten wir recht schnell mit diesem Projekt auf und ließen uns eine neue Idee einfallen. Unser Ziel war (und ist) es ein Doppelkopfspiel mit Greenfoot zu programmieren. Dafür müssten wir uns aber erstmal mit dem Programm und Programmierung allgemein auseinandersetzen. Wir begannen mit den Greenfoot-Stride Lernaktivitäten. Später haben wir auf Grundlage dieser, eigene Ideen in die Welt gebracht, woraus nach einer Zeit ein Spiel entstanden ist.

## **Sinn des Programms**
Dieses Programm dient der Einarbeitung in das Programm "Greenfoot", weswegen es sowohl Elemente eines Spiels der aktiven Steuerung eines "Actors", als auch Elemente eines sich selbst entwickelnden Programms (evolutionssimulierende Ansätze) enthält. So mag es einem Betrachter erscheinen, als könne man einige Befehlsketten anders ausdrücken.

## **Ablauf des Spiels**
In diesem Spiel steuert man mit den Pfeiltasten einen Wurm. Mit der rechten und linken Pfeiltaste dreht sich der Wurm, mit der oberen und unteren Taste bewegt sich der Wurm vorwärts und rückwärts. In der Spielwelt gibt es noch drei weitere Spielklassen, die vom Computer gesteuert werden. Die Krabben, die Hummer und die Seesterne sind die anderen Klassen in diesem Spiel. Sie fressen sich gegenseitig und vermehren sich, wenn sie jemanden gefressen haben. Wenn sie zu lange nichts fressen, verhungern sie. Der Wurm hat die Fähigkeit, jedes andere Tier zu fressen und kann von keinem anderen Tier getötet werden. Er verhungert aber auch, wenn er zu lange nichts gegessen hat. Wenn der Wurm eine Krabbe frisst, erscheint diese nach einer Zeit als Zombiekrabbe am Todesort wieder. Zombiekrabben sind doppelt so schnell, wie normale Krabben und stecken diese bei Berührung an. Treffen sie auf Hummer oder Seesterne, sterben beide Tiere. 
Ziel des Spiels ist es, so lange wie möglich mit dem Wurm zu überleben.

## **Ablauf der Programmierung**

### **Erste Actor**
Als Grundlage verwenden wir die "Crab-World" Vorlage. Wir haben damit angefangen, dass wir eine einfache Crab.class in die World eingefügt haben. Durch den Befehl "move (5)" bewegt sich diese Crab.class geradeaus. Da die World begrenzt ist, veränderten wir die Bewegungsrichtung beim Auftreffen auf die Wand (Äußeres Ende der Welt). Die Richtung sollte nicht konkret vorgegeben sein, sondern immer zufällig sein. Mit dem Befehl "if (isAtEdge()) {
            turn(Greenfoot.getRandomNumber(90) - 45);
        }" konnten wir dies umsetzen. Wir haben mehrere Objekte der Crab.class in die World hinzugefügt. Außerdem haben wir die starfish.class und die lobster.class mit gleichen Eigenschaften wie die Crab.class eingefügt.

### **Nahrungskette**
Unser vorläufiges Ziel war es nun, eine Art Evolutionssimulation zu erstellen. Die drei verschiedenen Klassen sollten je eine Klasse haben, die sie fressen (aus der Welt entfernen) und von einer Klasse gefressen werden (aus der Welt entfernt werden). Wenn die Crab.class die starfish.class berührt, entfernt sie diese aus der World.

### **Zuschauer als Spieler**
Als nächstes haben wir eine worm.class in das Spiel eingefügt, welche vom Spieler gesteuert werden soll. Mit Befehlen, wie zum Beispiel "{
        if (Greenfoot.isKeyDown("left")) {
            turn(-4);
        }" haben wir die Pfeiltasten belegt. Die obere und untere lässt den Wurm sich vorwärts und rückwärts bewegen. Die rechte und linke Teste lässt den Wurm sich nach rechts oder links drehen. Der Wurm bekam die Eigenschaft, alle anderen Tiere zu fressen. Um das ganze Spiel etwas zufälliger zu machen, stellten wir in der World ein, dass jedes Objekt an einer zufälligen Stelle in der World erscheint.

### **Musik**
Wir haben Töne eingefügt, die immer dann erklingen, wenn zwei "Actor" gleicher Klasse aufeinander trafen. Da wir dies allerdings als relativ störend auf Dauer empfanden, haben wir diesen Programmabschnitt wieder entfernt.

### **Fortpflanzung**
Neue Objekte zu erzeugen stand weiterhin im Fokus unserer Anstrengungen. Wir versuchten eine Art Fortpflanzung einzuführen, indem wir immer ein neues Objekt in die World einfügten, wenn zwei gleiche Objekte aufeinandertrafen. Dies führte allerdings zu einem unkontrollierten, exponentiellen Wachstum (auch Crab-Bomb genannt). Wir haben dieses Problem gelöst, indem wir immer ein neues Objekt eingeführt haben, wenn ein Objekt ein anderes frisst (mit Ausnahme von der worm.class). Dazu gaben wir den Befehl an die World, dass immer wenn ein Objekt ein anderes entfernt, ein neuer Objekt mit der gleichen Klasse wie die des Entfernenden einzufügen. Dies haben wir mit folgendem Programmabschnitt gemacht:
if (isTouching(starfish.class)) {
            World myworld;
            myworld = getWorld();
            myworld.addObject( new Crab(), Greenfoot.getRandomNumber(600), Greenfoot.getRandomNumber(400));
            removeTouching(starfish.class);
         
### **Einführung von Hunger**
Mit der Einführung von Variablen eröffneten sich uns neue Möglichkeiten um Zähler einzubauen. Wir konnten jetzt den Faktor Zeit in unser Spiel einbauen. Wir haben einen Hungerwert eingeführt, der ein Objekt aus der World entfernt, wenn es zu lang kein anderes Objekt mehr gefressen (aus der World entfernt) hat. Dazu stellten wir für jedes Objekt einen Zähler (eine Variable, die mit jeder Schleife +1 hochzählt) ein, der zurückgesetzt wird, wenn es ein anderes Objekt aus der World entfernt. Wenn dieser Zähler einen bestimmten Wert erreicht, gibt das Objekt der World den Befehl, dieses Objekt zu entfernen.
private int TageSeitMahlzeit
TageSeitMahlzeit = TageSeitMahlzeit + 1
if (TageSeitMahlzeit == 300) {
            getWorld().removeObject(this);
        }
if (isTouching(starfish.class)) {
            removeTouching(starfish.class);
            TageSeitMahlzeit = 0;
        }

### **Die Schlange**
Um unser Programm mehr mit einem Spiel und der damit verbundenen Leistung in Verbindung zu bringen, haben wir eine Verlust-Simulation eingeführt. Eine Schlange frisst den Wurm, wenn dieser einige Zeit lang keine Nahrung mehr zu sich genommen hat. Um dies zu erreichen, haben wir der worm.class befohlen, dass sie sich nur bewegen kann, wenn ihre Hungervariable ("tageSeitMahlzeit") unter dem Wert 300 liegt. Wenn dieser Wert erreicht ist, kann sie sich nur noch drehen und sie gibt ein Signal an die World, welche daraufhin eine Snake.class auf die Position X=0 und der selben Position Y, wie die der worm.class eingeführt. Diese haben wir durch eine Variable bestimmt, welche die Y-Koordinate beim Erreichen des Hungerlevels (tageSeitMahlzeit=300) misst.
private int tageSeitMahlzeit;
    private int Todesort
if (Greenfoot.isKeyDown("up") && tageSeitMahlzeit < 300) {
            move(5);
        }
if (tageSeitMahlzeit == 300) {
            Todesort = getY();
            getWorld().addObject( new Snake(), 0, Todesort);
        }
    }
}
Die Schlange bewegt sich parallel zur X-Achse auf den Wurm zu, frißt diesen und bewegt sich in der gleichen Richtung zurück, in der sie hergekommen ist.

### **Zombiekrabben**
In einer weiteren Programmergänzung behandelten wir das Problem, was mit toten Objekten passiert. Als weitere Spielherausforderung erschufen wir "Zombiekrabben". Crab-Objekte die von der Worm.class gefressen wurden sollten nach einer gewissen Zeit "wiederauferstehen". Dies haben wir mit dem Zusammenspiel von mehreren Variablen und if-Schleifen gemacht. Wenn der Wurm eine Krabbe frisst, wird durch zwei Variablen ("TodesortCrabX" und "TodesortCrabY") festgehalten. Außerdem wird die Variable "Zombiecrab" um 1 erhöht. Wenn diese Variable größer als 0 ist, beginnt eine zweite Variable ("ToteCrab") hochzuzählen. Wenn diese bei dem Wert 200 angelangt ist, wird der World ein Befehl gegeben, eine Crab2.class an dem vorher bestimmten Todesort einzufügen. Die Variable "Zombiecrab" wird um 1 subtrahiert und "ToteCrab" wird auf 0 zurückgesetzt.
private int Zombiecrab;
    private int TodesortCrabX;
    private int TodesortCrabY;
    private int ToteCrab
removeTouching(Crab.class);
            Zombiecrab = Zombiecrab + 1;
            TodesortCrabX = getX();
            TodesortCrabY = getY();
        }
        if (Zombiecrab > 0) {
            ToteCrab = ToteCrab + 1;
        }
        if (ToteCrab == 200) {
            getWorld().addObject( new Crab2(), TodesortCrabX, TodesortCrabY);
            Zombiecrab = Zombiecrab - 1;
            ToteCrab = 0;
        }
Gäbe es nur ein Objekt der Crab.class, hätte man auch mit einer booleanschen Variable arbeiten könne (True, wenn die worm.class die Crab.class berührt hätte). Da es aber mehrere Krabben gibt, haben wir dieses Problem wie oben beschrieben gelöst. Die Crab2.class ist doppelt so schnell, wie die Crab.class. Wenn die Crab2.class auf die lobster.class oder die starfish.class trifft, werden beide Objekte aus der World entfernt.

### **Game Over**
Zum Schluss haben wir ein "Game Over" Schriftzug in der World entstehen lassen. Die Grafik dazu haben wir selbst erstellt. Dieser tritt in der Mitte des Spielfeldes auf, wenn die Schlange das linke Ende der Karte erreicht hab.

## **Pläne für die Zukunft**
Wie schon vorher gesagt, ist dieses Programm dazu da, sich in Greenfoot herein zu arbeiten. Unser finales Ziel ist immernoch ein Doppelkopfspiel zu entwickeln. Wann genau wir damit beginnen, ist jedoch noch unklar.
