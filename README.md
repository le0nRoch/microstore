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
