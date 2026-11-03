# 1. Einführung und Ziele

In vielen Softwareentwicklungs-Teams sind die individuellen Fähigkeiten und Fachkenntnisse der einzelnen Entwickler nicht transparent erfasst. 
Dieses Defizit führt in der Praxis zu spürbaren Herausforderungen: Es erschwert eine effiziente Ressourcen- und Kapazitätenplanung, behindert die gezielte Identifikation von Wissenslücken (Skill-Gaps) und verzögert die optimale Besetzung von neuen Kundenprojekten.

Um diesen Risiken im Umfeld "XY" entgegenzuwirken, wird eine zentrale **Skillmatrix** eingeführt. 
Die Plattform bietet Führungskräften und dem Ressourcenmanagement eine verlässliche, transparente Übersicht der vorhandenen Hard- und Softskills aller Teammitglieder. Dadurch wird eine datenbasierte Entscheidungsfindung bei der Projektbesetzung und der Personalentwicklung ermöglicht.

## 1.1 Aufgabenstellung

Die Skillmatrix soll die Transparenz innerhalb der IT-Organisation erhöhen und die gezielte Personalentwicklung fördern. Um dies zu erreichen, stellt die Anwendung folgende Kernfunktionalitäten bereit:

*   **Zentrale Erfassung von Entwicklungskenntnissen (Hard Skills):** Entwickler können ihre technologischen Fähigkeiten strukturiert erfassen und selbst bewerten. Dies schafft eine verlässliche Datenbasis und macht informelles Wissen im Team sichtbar.
*   **Transparente Einsicht für Führungskräfte:** Teamleiter und das Ressourcenmanagement erhalten eine konsolidierte Übersicht der vorhandenen Kompetenzen. Dies ermöglicht eine treffsichere Projektbesetzung und eine fundierte Kapazitätenplanung.
*   **Integriertes Wissenscenter:** Bereitstellung von zielgerichteten Schulungs- und Weiterbildungsprogrammen. Das System empfiehlt passende Lerninhalte basierend auf den identifizierten Wissenslücken (Skill-Gaps) der Mitarbeiter.
*   **Fragencenter zur Evaluierung von Soft Skills:** Ein methodischer Fragebogen erfasst und bewertet kommunikative und methodische Kompetenzen in verschiedenen Bereichen der Softwareentwicklung (z. B. Konfliktlösung, Agilität, Code-Reviews).
*   **Zentrale Administrationsansicht:** Eine dedizierte Oberfläche für Administratoren zur Pflege des Skill-Katalogs, zur Verwaltung von Benutzerrollen und Berechtigungen sowie zur langfristigen Instandhaltung der Plattform.

## 1.2 Qualitätsziele

Die folgenden Qualitätsziele sind die primären technischen Treiber für die Architektur des Systems. Jede spätere Technologieentscheidung muss auf diese Ziele einzahlen.

| Priorität | Qualitätsziel | Beschreibung | Wie wir es erreichen (Konzeptuell) |
| :--- | :--- | :--- | :--- |
| **Prio 1** | Datenschutz & Vertraulichkeit | Skill-Daten und Mitarbeiterbewertungen sind hochsensibel. | Strikte serverseitige Rollen- und Rechteprüfung (RBAC). Ein Zugriff auf fremde Profile wird auf Datensatz-Ebene für reguläre Nutzer blockiert. |
| **Prio 2** | Wartbarkeit & Einfachheit | Das System wird von einem kleinen internen Team "nebenbei" betreut. | Wir wählen einen monolithischen Architekturansatz und minimieren die Anzahl verteilter Subsysteme, um den Betriebs- und Deployment-Aufwand gering zu halten. |
| **Prio 3** | Erweiterbarkeit & Offenheit | In Zukunft sollen Daten aus bestehenden Systemen importiert werden können. | Die Kern-Domänenlogik wird über saubere Schnittstellen (APIs) entkoppelt, sodass externe HR-Datenquellen modular angebunden werden können. |

## 1.3 Stakeholder

| Rolle | Erwartung an das System |
| :--- | :--- |
| **Entwickler (User)** | Möchte eine einfache Übersicht der eigenen Fähigkeiten erhalten, um den persönlichen Lernfortschritt zu tracken und sich über das Wissenscenter gezielt weiterzubilden. Erwartet absolute Diskretion (kein öffentlicher "Skill-Pranger"). |
| **Führungskräfte / HR (User)** | Benötigen eine verlässliche Datenbasis zur Erfassung und Analyse von Team-Kompetenzen. Nutzen das System zur effizienten Kapazitätenplanung und zur Identifikation von Skill-Gaps, um gezielte Recruiting-Entscheidungen zu treffen. |
| **Das interne Dev-Team (Betreiber)** | Fordert eine saubere Codebasis (Clean Architecture) und ein leicht skalierbares System, das stabil läuft und eine automatisierte Deployment-Pipeline besitzt. |