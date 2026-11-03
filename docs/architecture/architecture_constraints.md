# 2. Randbedingungen

Randbedingungen sind feste Vorgaben und Einschränkungen, die den architektonischen Spielraum eingrenzen und bei jeder technischen Entscheidung berücksichtigt werden müssen.

## 2.1 Technische Randbedingungen

| Randbedingung | Erläuterung / Auswirkung auf die Architektur |
| :--- | :--- |
| **Kein eigener Server** | Es steht kein klassischer Linux-Server (VPS/Root-Server) für die Anwendung zur Verfügung. Externe Dienste (wie die Datenbank) müssen als Docker Container auf dem Laptop des Entwicklers bereitgestellt werden. |
| **Technologiestandard der Firma** | Der offizielle Technologiestandard für dieses System im Firmenkontext ist eine Kombination aus **Java** (Backend) und **Next.js** (Frontend). |
| **Authentifizierungs-Infrastruktur** | Externe Identitätsdienste (SaaS wie Clerk/Auth0) sind ausgeschlossen. Vorgabe ist ein eigenes Authentifizierungssystem oder eine selbstgehostete Instanz (z. B. **Keycloak**), die mit Next.js integriert werden muss. |

## 2.2 Organisatorische Randbedingungen

| Randbedingung | Erläuterung / Auswirkung auf die Architektur |
| :--- | :--- |
| **Privates Übungsprojekt** | Das System wird als privates Projekt zur Weiterbildung im Bereich Software-Architektur entwickelt. |
| **Flexible Frontend-Conventions** | Die offiziellen Frontend-Richtlinien der Firma dienen als Orientierung, werden für dieses private Projekt jedoch bewusst als flexibel und anpassbar definiert. |
| **Ein-Personen-Team** | Entwicklung, Architektur und Betrieb liegen bei einer einzigen Person. Die Architektur muss so gewählt werden, dass sie von einer Person beherrscht werden kann. |

## 2.3 Rechtliche Randbedingungen

| Randbedingung | Erläuterung / Auswirkung auf die Architektur |
| :--- | :--- |
| **Fiktive DSGVO-Konformität** | Da die App im Firmenkontext sensible Leistungsdaten verarbeiten würde, wird architektonisch so geplant, als müsste sie der DSGVO entsprechen (Verschlüsselung, strikte Mandantentrennung). |
