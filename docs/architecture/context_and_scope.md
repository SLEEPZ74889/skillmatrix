# 3. Kontextabgrenzung

Die Kontextabgrenzung definiert die Außengrenzen der Skillmatrix. Sie beschreibt, welche Akteure (Benutzer) und Nachbarsysteme mit der Anwendung interagieren und über welche Kanäle der Datenaustausch stattfindet.

## 3.1 Fachlicher und technischer Kontext

Da die Anwendung vollständig lokal betrieben wird, befinden sich das Hauptsystem sowie die gesamte Daten- und Identitätsinfrastruktur innerhalb derselben lokalen Umgebung (Entwickler-Laptop):

```mermaid
graph TD
    %% User-Rollen (Fachlich)
    Mitarbeiter[Entwickler / Mitarbeiter] -->|HTTP / port 3000| NextJSFrontend
    Manager[Führungskraft / HR] -->|HTTP / port 3000| NextJSFrontend

    %% Die lokale Systemumgebung
    subgraph Entwickler_Laptop [Lokale Laufzeitumgebung]
        NextJSFrontend(Next.js Frontend)
        SpringBootBackend(Java Core Backend - Spring Boot)
        Database[(PostgreSQL Datenbank - Docker)]
        Keycloak[Keycloak Identity Provider - Docker]
    end

    %% Technische Verbindungen (Lokal)
    NextJSFrontend -->|REST API / HTTP| SpringBootBackend
    SpringBootBackend -->|JPA / JDBC port 5432| Database
    SpringBootBackend -->|OAuth2 / Token Validation port 8080| Keycloak
    NextJSFrontend -.->|Login Redirect / OIDC| Keycloak

    %% Styles
    classDef system fill:#1F4E79,stroke:#112E4A,stroke-width:2px,color:#FFF;
    classDef infra fill:#4A5568,stroke:#2D3748,stroke-width:2px,color:#FFF;
    
    class NextJSFrontend,SpringBootBackend system;
    class Keycloak,Database infra;
```

## 3.2 Schnittstellenbeschreibung

In der folgenden Tabelle werden die Schnittstellen nach außen sowie zwischen den Hauptkomponenten im Detail definiert:

| Nachbarsystem / Akteur / Komponente | Richtung | Protokoll / Medium | Beschreibung |
| :--- | :--- | :--- | :--- |
| **Mitarbeiter & Führungskräfte** | Eingehend | HTTP / Localhost (Port 3000) | Greifen über den Webbrowser auf das Next.js-Frontend zu, um Daten einzusehen oder zu pflegen. |
| **Next.js Frontend ↔ Spring Boot** | Bidirektional | HTTP REST / JSON | Das Frontend ruft die REST-Endpunkte des Backends auf. Anfragen werden durch das Mitschicken des von Keycloak ausgestellten Bearer-Tokens (JWT) im HTTP-Header autorisiert. |
| **Keycloak (Identity Provider)** | Ausgehend | OpenID Connect (OIDC) / OAuth2 (Port 8080) | Verwaltet Benutzer und Rollen. Das Frontend leitet für den Login dorthin weiter. Das Spring Boot Backend validiert eingehende Token kryptografisch gegen Keycloak, um die Rechte zu prüfen. |
| **PostgreSQL Datenbank** | Ausgehend | PostgreSQL-Protokoll (Port 5432) | Die primäre Datenquelle für Profile, Skills und Fragen. **Ausschließlich das Spring Boot Backend** greift via JPA/Hibernate direkt auf die Datenbank zu. Das Frontend hat keinen Direktzugriff. |
