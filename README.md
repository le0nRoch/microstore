# Micro Services Shop

## Use Cases

UC01: User möchte sich registrieren und einloggen können um Produkte zu kaufen. \
UC02: User möchte Produkte ansehen um sie möglicherweise zu kaufen. \
UC03: User möchte Produkte in den Warenkorb legen um sie später zu kaufen. \
UC04: User möchte Produkte aus dem Warenkorb kaufen um sie zu besitzen. \
_..._

## Microservices

![Architektur](./docs/architecute.png)

### User Service

Der `User Service` handhabt die User. Er ist für Login, Registrierung und CRUD Operationen der User zuständig.

### Product Catalog Service

Der `Product Catalog Service` stellt dem Client Interface die zu verkaufenden Produkte zur Verfügung. Der `Product Service` ist für die CRUD Operationen der Produkte zuständig.

### Cart Service

Der `Cart Service` ist für die CRUD Operationen des Warenkorbs zuständig. Er validiert den Warenkorb und leitet den Checkout Prozess ein.

### Payment Service

Der `Payment Service` ist für die Bezahlung zuständig.

### Email Service

Der `Email Service` ist für das Versenden von Emails zuständig. Er sendet Emails an den User, wenn er sich registriert oder ein Produkt kauft. Die Aufträge erhält er mittels Kafka.

### Order Service

Der `Order Service` ist für die Bestellungen zuständig. Er speichert die Bestellungen, führt eine Bestellung durch und gibt die versendet die Produkte an den User.

### Storage Service

Der `Storage Service` ist für die Überwachung des Lagerbestands zuständig. Er benachrichtigt den `Buyer Service` mittels Kafka, wenn ein Produkt nicht mehr auf Lager ist.

### Buyer Service

Der `Buyer Service` ist für das Kaufen von Produkten zuständig. Die Bestellung wird mittels Kafka erhalten und durchgeführt.

### Eureka Service

Der `Eureka Service` ist für das Service Discovery zuständig. Er registriert die Services und gibt sie an die Services weiter.

### Kafka

Kafka ist ein Message Broker, der die Services asynchron miteinander kommunizieren lässt.

### Client Interface

Der `Client Frontend Service` ist für die Darstellung der Produkte und des Warenkorbs zuständig. Er zeigt die Produkte an, die in den Warenkorb gelegt werden können. Im Warenkorb kann der User die Produkte sehen, die er kaufen möchte. Beim Checkout wird der User aufgefordert, seine Daten einzugeben um zu bezahlen.

### Gateway

Das `Gateway` dirigiert die Anfragen an die Services.

### Admin Interface

Der `Admin Frontend Service` ist für das Management der Produkte zuständig. Er kann neue Produkte hinzufügen, bestehende Produkte bearbeiten und löschen. Der Admin kann die Bestellungen einsehen und die Bestände überwachen.


## Fälle für Microservices

### User Registrierung

1. Als erstes ruft der User das `Client Interface` auf.
2. Der User klickt dann auf einen Button, um sich zu registrieren.
3. Das `Client Interface` zeigt nun ein Formular an, in dem der User seine Daten eingeben kann.
4. Der User gibt seine Daten ein und klickt auf Registrieren.
5. Das `Client Interface` sendet diese Daten an das `Gateway`.
6. Das `Gateway` leitet die Anfrage an den `User Service` weiter.
7. Der `User Service` speichert die Daten in der Datenbank.
8. Dazu sendet der `User Service` ein Event an den Kafka Broker.
9. Der `Email Service` empfängt das Event vom Kafka Broker und sendet eine Bestätigungsmail an die Useremail.
10. Nun ist der User in der Datenbank registriert und kann einkaufen. 

### Produktkauf

1. Der User ruft das `Client Interface` auf.
2. Das `Client Interface` ruft alle Produkte vom `Product Catalog Service` ab.
3. Der User loggt sich in seinen Account ein.
4. Der `User Service` prüft die Anmeldedaten und gibt eine Bestätigung zurück.
5. Der User sieht nun ein Produkt, welches er kaufen möchte und tut es in den Warenkorb.
6. Der `Cart Service` speichert das Produkt im Warenkorb.
7. Nun möchte der User den Warenkorb kaufen.
8. Der `Cart Service` prüft den Warenkorb und leitet den Checkout Prozess ein.
9. Der `Payment Service` prüft die Zahlungsinformationen und leitet die Bezahlung ein.
10. Das Bezahlen wird nun mit `Stripe` abgewickelt. 
11. Nachdem der Nutzer gezahlt hat, gibt der `Cart Service` die Bestellung an den Kafka Broker weiter. 
12. Die Services `Èmail`, `Storage` und `Order` empfangen die Bestellung und führen sie durch.
13. Der `Storage Service` prüft den Lagerbestand und sendet eine Nachricht an den `Buyer Service`, wenn ein Produkt nicht mehr auf Lager ist.
14. Der `Order Service` speichert die Bestellung in der Datenbank und sendet eine Bestätigungsmail an den User.

### Produkt hinzufügen

1. Ein Admin loggt sich in das `Admin Interface` ein.
2. Der Admin ruft das `Admin Interface` auf.
3. Das `Admin Interface` ruft alle Produkte vom `Product Catalog Service` ab.
4. Der Admin füllt ein Formular aus, um ein neues Produkt hinzuzufügen. 
5. Der Admin klickt auf Hinzufügen.
6. Das `Admin Interface` sendet die Daten an das `Gateway`.
7. Das `Gateway` leitet die Anfrage an den `Product Catalog Service` weiter.
8. Der `Product Catalog Service` speichert das Produkt in der Datenbank.





## Interaktion der Micorservices

### Kafka Broker

Der Kafkabroker kann vom `Cart Service` und vom `User Service` angesprochen werden. Der Kafta Borker hat unter sich den `Email Service`, den `Storage Service`, den `Buyer Service` und den `Order Service` unter sich. 

### Email Service

Der Email Service hört auf Events vom Kafka Broker und versendet jeh nach daten dann ein Email. 


### Product Catalog Service

Der Service kann entweder von Admins mit CRUD operatoren verwedet werden, um die Produkte zu ändern. Die Users können lediglich die Produkte einsehen. 