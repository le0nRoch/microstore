# Micro Services Shop

## Use Cases

UC01: User möchte sich registrieren und einloggen können um Produkte zu kaufen. \
UC02: User möchte Produkte ansehen um sie möglicherweise zu kaufen. \
UC03: User möchte Produkte in den Warenkorb legen um sie später zu kaufen. \
UC04: User möchte Produkte aus dem Warenkorb kaufen um sie zu besitzen. \
_..._

## Microservices

_Grafik in bearbeitung..._

### User Service


### Product Service

Der `Product Service` stellt die Produkte zur Verfügung. Er gibt die Produkte zurück, die der User sehen kann. Der `Product Service` ist für die CRUD Operationen der Produkte zuständig. 


### Cart Service

Der `Cart Service` ist für die CRUD Operationen des Warenkorbs zuständig. Er speichert die Produkte, die der User in den Warenkorb gelegt hat. Der `Cart Service` gibt die Produkte zurück, die der User in den Warenkorb gelegt hat. Der `Cart Service` wird später für den Checkout benötigt.

### Email Service

Der `Email Service` ist für das Versenden von Emails zuständig. Er sendet Emails an den User, wenn er sich registriert oder ein Produkt kauft.


### Order Service

Der `Order Service` ist für die Bestellungen zuständig. Er speichert die Bestellungen, führt eine Bestellung durch und gibt die versendet die Produkte an den User. 


### Lagerbestand Service

Der `Lagerbestand Service` ist für die Überwachung des Lagerbestands zuständig. Er benachrichtigt den `Buyer Service`, wenn ein Produkt nicht mehr auf Lager ist. 

### Buyer Service

Der `Buyer Service` ist für die Überwachung der Bestände und Käufe zuständig. Er überwacht die Bestände und Käufe und benachrichtigt den User, wenn ein Produkt nicht mehr auf Lager ist. Wenn ein Produkt nicht mehr auf Lager ist, wird werden die Bestände aktualisiert und aufgestockt.


### Monitoring Service

Der `Monitoring Service` ist für die Überwachung der Bestände und Käufe zuständig. Er gibt die Bestände und Käufe zurück und macht logging. 


### Client Frontend Service

Der `Client Frontend Service` ist für die Darstellung der Produkte und des Warenkorbs zuständig. Er zeigt die Produkte an, die in den Warenkorb gelegt werden können. Im Warenkorb kann der User die Produkte sehen, die er kaufen möchte. Beim Checkout wird der User aufgefordert, seine Daten einzugeben um zu bezahlen.

### Admin Frontend Service

Der `Admin Frontend Service` ist für das Management der Produkte zuständig. Er kann neue Produkte hinzufügen, bestehende Produkte bearbeiten und löschen. Der Admin kann die Bestellungen einsehen und die Bestände überwachen.