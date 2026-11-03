# 4. Lösungsstrategie

Die Lösungsstrategie beschreibt die grundlegenden technologischen und konzeptionellen Entscheidungen, die getroffen wurden, um die Qualitätsziele (Kapitel 1) unter den gegebenen Randbedingungen (Kapitel 2) zu erfüllen.

## 4.1 Technologische Kernentscheidungen & Begründungen

### 1. Architektur-Muster: Abgekoppeltes Frontend/Backend (Decoupled Architecture)
* **Entscheidung:** Strikte Trennung von Benutzeroberfläche (Next.js) und Geschäftslogik (Spring Boot).
* **Begründung:** Dies spiegelt exakt den geforderten Technologiestandard der Organisation wider (Kapitel 2.1). Es ermöglicht eine unabhängige Entwicklung und stellt sicher, dass das Backend als "Single Source of Truth" für alle Daten agiert.

### 2. Absicherung der sensiblen Daten (Erfüllung von Qualitätsziel Prio 1: Datenschutz)
* **Entscheidung:** Einsatz von **Spring Security** im Backend in Kombination mit **Keycloak** als zentralem OAuth2/OIDC Identity Provider.
* **Konzept:** 
  1. Das Next.js-Frontend wickelt den Login über Keycloak ab und erhält ein kryptografisch signiertes **JWT (JSON Web Token)**.
  2. Bei jedem API-Request schickt Next.js dieses Token als `Bearer`-Token im HTTP-Header mit.
  3. Das Spring Boot Backend prüft das Token bei *jedem einzelnen Aufruf* eigenständig. 
  4. Über Annotationen (z. B. `@PreAuthorize("hasRole('ROLE_MANAGER')")`) blockiert das Backend unberechtigte Zugriffe direkt auf Methodenebene. Ein Datenabfluss über manipulierte Frontend-Anfragen wird damit technisch unmöglich gemacht.

### 3. Datenhaltung & Validierung
* **Entscheidung:** Relationale Datenhaltung in **PostgreSQL** mittels **Spring Data JPA (Hibernate)**.
* **Begründung:** JPA bietet eine Typsicherheit, die perfekt zu den strukturierten Skill-Daten passt. Die Geschäftslogik validiert die eingehenden Daten (z. B. Skill-Level strikt zwischen 1 und 5) im Backend, bevor sie persistiert werden.

### 4. Erfüllung von Qualitätsziel Prio 2 (Wartbarkeit & Local-First)
* **Entscheidung:** Bereitstellung einer zentralen `docker-compose.yml` für die lokale Infrastruktur.
* **Begründung:** Da kein externer Server zur Verfügung steht (Kapitel 2.1), erlaubt Docker Compose es, die PostgreSQL-Datenbank und Keycloak mit einem einzigen Befehl (`docker compose up`) lokal in exakt der gleichen Konfiguration zu starten. Das minimiert den Einrichtungsaufwand für die lokale Entwicklungsumgebung drastisch.

## 4.2 Übergreifende Architekturprinzipien

* **Stateless Backend:** Das Spring Boot Backend speichert keine Sitzungsdaten (Sessions) im Speicher. Jede Anfrage ist durch das JWT selbstbeschreibend. Dies hält das Backend extrem schlank und theoretisch jederzeit horizontal skalierbar.
* **Backend-driven Validation:** Das Frontend validiert Daten nur für eine bessere User Experience (z. B. Fehlermeldung direkt im Formular). Die *echte, bindende Validierung* findet aus Sicherheitsgründen ausschließlich im Java-Backend statt.

### 5. Caching-Strategie & Performance-Absicherung
* **Entscheidung:** Für den Start (MVP-Phase und ca. 100 Nutzer) wird auf eine separate In-Memory-Datenbank (wie Redis) verzichtet. Stattdessen nutzen wir das integrierte Caching von **Spring Boot Cache / Hibernate Second-Level-Cache**.
* **Begründung:** PostgreSQL ist bei korrekter Indizierung weit mehr als ausreichend für die Last von 100 simultanen Nutzern. Der Verzicht auf Redis hält die Infrastruktur schlank und erfüllt das Qualitätsziel **Prio 2 (Einfachheit & Wartbarkeit)**. 
* **Zukunftssicherheit:** Die Service-Schicht im Spring Boot Backend wird von Anfang an mit abstrakten Spring-Caching-Annotationen (z. B. `@Cacheable`) versehen. Sollte die Nutzerlast in Zukunft drastisch steigen, kann Redis als dedizierter Cache-Provider über die `application.properties` nachgerüstet werden, ohne den bestehenden Java-Code ändern zu müssen.

> 💡 **Architektur-Einsicht (YAGNI-Prinzip):**  
> Hier greift das Prinzip *„You Aren't Gonna Need It“* (Du wirst es erst einmal nicht brauchen). Als Architekt verhindern wir damit künstliche Komplexität am Anfang, halten das System aber durch die Verwendung von Spring-Abstraktionen maximal flexibel für eine spätere Skalierung.
