# "suchen" -- Ein Prototyp zum Vergleich von Discovery-Diensten

André Hagenbruch  
Universitätsbibliothek Bochum  
44780 Bochum  
andre.hagenbruch@rub.de

Miriam Heise  
Universitätsbibliothek Bochum  
44780 Bochum  
miriam.heise@rub.de

## Zusammenfassung

## Abstract
We describe a prototype web application that integrates two different discovery systems and an OPAC based search index.

### Schlüsselwörter
Discovery-Dienst,

### Keywords
Discovery Service,

## Einleitung
Im Mai 2008 startete die Universitätsbibliothek Bochum das Projekt "Integriertes Bibliotheksportal", in dem eine Anwendung
entstehen sollte, um den Benutzern möglichst viele Datenquellen unter einer einheitlichen Oberfläche zur Recherche
auf Basis moderner Suchmaschinentechnologie anbieten zu können. Darüber hinaus sollte die Anwendung auch den
aktuellen Verfügbarkeitsstatus des jeweiligen Titels anzeigen und Benutzerfunktionalitäten wie z.B. Vormerkungen oder
Kontoverwaltung ermöglichen. Während wir zunächst intensiv OCLC TouchPoint evaluiert haben, zeichnete es sich ab,
dass wir damit nicht alle unsere Ziele erreichen würden. Parallel zu diesem Projekt haben wir eine auf Open Source
Software basierende Such-Plattform für die Hochschulbibliographie der RUB entwickelt, die so konzipiert war, dass 
mit geringen Anpassungen unterschiedliche Datenquellen integriert werden konnten. Im September 2011 testeten wir mit
ProQuest zum ersten Mal die Möglichkeit, einen Discovery-Dienst in diese Plattform zu integrieren. Da ein Demonstrator
recht schnell implementiert werden konnte, entschlossen wir uns, die Produkte Summon von ProQuest und EDS von Ebsco
durch Integration über die angebotenen APIs sowie einen Suchmaschinenindex mit unseren Katalogdaten nebeneinander
zu evaluieren.  
Im Folgenden werden wir die Architektur unseres Protoyps beschreiben, die angebotenen APIs und Metadaten und auch die
Erfahrungen, welche die UB-Mitarbeiter und die regulären Bibliotheksbenutzer mit diesen Diensten gemacht haben.

## Technische Architektur
Da es sich bei den Discovery-Diensten um Anwendungen handelt, die auf aktueller Suchmaschinentechnologie basieren,
werden die Daten des Bochumer OPAC in einem täglich ablaufenden ETL-Prozess in eine Apache Solr-Instanz überführt.
Während Summon ebenfalls Solr als Plattform benutzt, gibt es von Ebsco keine konkreten Informationen über die
eingesetzte Technik. Da bei ProQuest an dieser Stelle Open Source-Software zum Einsatz kommt, kann man als Kunde gut den
zu erwartenden Funktionsumfang der Plattform abschätzen, da sie sowohl im Web als auch in der Literatur ausführlich
dokumentiert ist.  
Sowohl Summon als auch EDS bieten ihren Kunden die Wahl zwischen der Nutzung als SaaS (_Software as a Service_), bei dem
die Webanwendung bei den Firmen gehostet wird und nur in geringem Umfang an die lokalen Bedürfnisse (sowohl in Bezug auf
das Layout als auch die Funktionalitäten) angepasst werden kann, und der Einbindung des Dienstes über eine API
(_Application Programming Interface_). Da auch die Testumgebung möglichst neutral gestaltet werden sollte, wurde die
letztere Möglichkeit gewaählt. Kernstück der Architektur ist ein Django Webapplikationsserver, der sowohl die Discovery
Dienste als auch den lokalen Index anspricht. Für das Oberfächenlayout kommt das CSS-Framework _Bootstrap_[^1] in einer
nur rudimentär an das Corporate Design der Ruhr-Universität Bochum angepassten Form zum Einsatz. Darüber hinaus stellt
ein weiterer Django-Server eine Implementierung der _Document Availability API_ (DAIA)[^2] zur Verfügung, die sowohl
Verfügbarkeitsinformationen aus dem Lokalsystem als auch aus dem _Journals Online & Print_ (JOP)-Dienst der DNB als auch
dem 360 Link-Resolver von ProQuest hinter dieser plattformneutralen Programmierschnittstelle kapselt.  
Ein weiteres Desiderat dieses Tests sah vor, dass die Ergebnisse aus den Suchen in den Discover-Diensten untereinander aber
auch zu unserem OPAC vergleichbar sein sollten. Daher haben wir die Trefferlisten so modelliert, dass eine Anfrage in
eines der drei Systeme die Trefferanzahl der jeweils anderen beiden sowie einen Link mit der Suchanfrage in den
jeweiligen Dienst lieferte. Dabei haben wir uns auf die reine Zeichenkette der Benutzeranfrage beschränkt, da die
Navigatoren der drei Systeme nicht aufeinander abbildbar waren.
![Diagramm der technischen Architektur](./ds_diagramm.jpg)
![Beispiel einer Ergebnisliste](./higgs_boson-full.png)

## APIs und Metadaten

## Benutzererfahrungen

## Fazit
[^1]: http://getbootstrap.com/2.3.2/ "Zuletzt gesehen 2014-07-15"
[^2]: http://gbv.github.io/daiaspec/daia.html §Zuletzt gesehen 2014-07-15"
[1]: http://journal.code4lib.org/articles/7738 "Zuletzt gesehen 2014-07-07"