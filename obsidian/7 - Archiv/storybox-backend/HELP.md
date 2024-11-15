
# Getting Started

### Reference Documentation

For further reference, please consider the following sections:

* [Official Gradle documentation](https://docs.gradle.org)
* [Spring Boot Gradle Plugin Reference Guide](https://docs.spring.io/spring-boot/docs/3.3.0/gradle-plugin/reference/html/)
* [Create an OCI image](https://docs.spring.io/spring-boot/docs/3.3.0/gradle-plugin/reference/html/#build-image)
* [Spring Boot Actuator](https://docs.spring.io/spring-boot/docs/3.3.0/reference/htmlsingle/index.html#actuator)
* [Spring Security](https://docs.spring.io/spring-boot/docs/3.3.0/reference/htmlsingle/index.html#web.security)
* [Spring Reactive Web](https://docs.spring.io/spring-boot/docs/3.3.0/reference/htmlsingle/index.html#web.reactive)

### Guides

The following guides illustrate how to use some features concretely:

* [Building a RESTful Web Service with Spring Boot Actuator](https://spring.io/guides/gs/actuator-service/)
* [Securing a Web Application](https://spring.io/guides/gs/securing-web/)
* [Spring Boot and OAuth2](https://spring.io/guides/tutorials/spring-boot-oauth2/)
* [Authenticating a User with LDAP](https://spring.io/guides/gs/authenticating-ldap/)
* [Building a Reactive RESTful Web Service](https://spring.io/guides/gs/reactive-rest-service/)

### Additional Links

These additional references should also help you:

* [Gradle Build Scans – insights for your project's build](https://scans.gradle.com#gradle)

---
## Lokales Hosten

### Docker 

#### Änderungen um das Backend im eigenen Container zu hosten

Einige Werte der `application.yaml` Konfigurationen werden von der `docker-compose.yaml` bereits geliefert, wodurch manche Schritte übersprungen werden können. Darunter z.B. auch die Datasource Konfiguration.  

Die Postgresql Datenbank sollte trotzdem vorher gefüllt werden. Das geht im Docker Container mit dem `psql` command und der anschließened SQL Query.

### Anleitung

#### Vorbereitungen

###### Step 1: Git repo klonen
Für dieses Beispiel befinden wir uns in einem Ordner `/projects/`.
`git clone git@gitlab.com:software-development-and-data/backend-services/sdd-storybox-next-backend.git`  
Öffne den projects ordner in deinem Editor oder IDE.  
Ich nutze im weitern Verlauf VSCode.
###### Step 2: Java Version
Das Backend nutzt Java 21 Wenn diese nicht auf deinem Computer ist, lade sie herunter.  
Öffne die Settings.json des Projects und suche nach `java.configuration.runtimes`.
```JSON
  "java.configuration.runtimes": [
      {
          "name": "JavaSE-21",
          "path": "/Library/Java/JavaVirtualMachines/jdk-21.jdk/Contents/Home",
          "default": true
      }
  ] 
```
Nachdem das getan ist, nutzt VSCode die Java 21 Runtime für dein Projekt.

#### Application.yaml
Die `application.yaml` befindet sich in `src/main/resources/application.yaml`.  

###### Step 1: Datenbankeinstellungen

Erstelle eine lokale postgresql Datenbank ([Anleitung](https://www.codecademy.com/article/installing-and-using-postgresql-locally)) oder mit `Homebrew`. Das Projekt benutzt standardmäßig postgresql@16, 17 funktioniert aber auch. Als Verwaltungstool kann ich pgAdmin 4 empfehlen.  
```YAML
  # Datasource configuration
  datasource:
    url: ${STORYBOX_DB_URL}
    username: ${STORYBOX_DB_USER}
    password: ${STORYBOX_DB_PASSWORD}
```
<span style="color: red; font-weight: bold;">Achtung</span> die URL muss in die `DataSourceConfiguration.java`, Zeile 20, manuell eingefügt werden, da dort die Valueinjection den Pfad nicht richtig auflöst.
Beispiel: `@Value("jdbc:postgresql://localhost:5432/postgres")`

Um die anderen Werte zuzuweisen, füge den Wert nach dem `:` ein.
Beispiel: 
```YAML
  # Datasource configuration
  datasource:
    url: ${STORYBOX_DB_URL}
    username: postgres
    password: 123
```

Nun muss die `region` Tabelle gefüllt werden. Ich schlage vor: (beliebig wählbar, aber Schema beachten)
```SQL
INSERT INTO region (version, created, created_by, last_modified, last_modified_by, deleted, region_name, cities)
VALUES
(1, '2024-11-06 13:07:18.732252+01', 'system', '2024-11-06 13:07:18.732252+01', 'system', FALSE, 'Bodenseekreis', 'Lindau, Wangen, Friedrichshafen');

```

###### Step 2: Mailkonfiguration

Deaktiviere den Mailservice, in dem du den Mailhost zu `<?>` setzt, dadurch verursacht er zwar eine Fehlermeldung, sorgt aber nicht für dem Abbruch der Anwendung.
Setzte den Port auch auf irgendein Wert z.b. `1000`.
```YAML
  # Spring Mail configuration
  mail:
    host: <?>
    port: 1000
    properties:
      mail:
        smtp:
          connectiontimeout: 5000
          timeout: 3000
          writetimeout: 10000
    username: ${STORYBOX_MAIL_USER:}
    password: ${STORYBOX_MAIL_PASSWORD:}

  freemarker:
    check-template-location: false
```

###### Step 3: Actuatorkonfiguration
Der Actuator ist ein nützliches Tool um den Zustand der Spring Boot Anwedung zu überprüfen. Im weiteren Verlauf aktivieren wir den Zugriff darauf für alle Anfragen.
```YAML
# Actuator configuration
management:
  actuator-password: ABC
  endpoints:
    web:
      base-path: /admin
      exposure:
        include: loggers,flyway,health,metrics,heapdump
  endpoint:
    health:
      probes:
        enabled: true
      show-details: always
```
Gib dem `actuator-password` irgendein Wert, wir werden es nicht mehr brauchen.  
Bei `web.exposure.include` füge `metrics` und `heapdump` hinzu.
Das sind unsere Actuator Endpukte, die uns dann z.B. über `/admin/metrics` Werte liefern.

###### Step 4: Storyboxeinstellungen

```YAML
storybox:
  env: dev # dev, stage or prod
  base-url: http://localhost:8080
  api-key: SXFP-CHYK-ONI6-S89U
  brand: nordkurier
  gcp-bucket: bkt-service-p-cloudrun-storybox-next-storage
  legal-terms: LEGAL TERMS
  gtm-id: ${STORYBOX_GTM_ID:}
  export:
    url: https://sve-stg-newsfeed.forward-publishing.com/feeds/storybox
    max-retries: ${STORYBOX_EXPORT_MAXRETRIES:3}
    cron: ${STORYBOX_EXPORT_CRON:0 */5 * * * *} # Alle 5 Minuten
  authentication:
    auth-provider-url: https://uat.mppglobal.com
    login-url: https://account-stage.nordkurier.de/login
    redirect-url: https://www-stage.nordkurier.de/storybox
    client-id: 741
    client-secret: d7ESp42Gwa3Y9D
  mail:
    from: storybox@nordkurier.de
    max-retries: ${STORYBOX_MAIL_MAXRETRIES:3}
  test:
    sessionId: 1bd1edcc03b94c03b2c1b68112a8d5df
    email: fhertsch235@gmail.com
```
Setze das `env` auf dev, dadurch wird das lokal_db Profil aktiviert.  
Die `base-url` sollte den Wert haben, auf dem dasd Backend läuft.  
Der Rest kann kopiert werden. Füge am Ende noch den Punkt `test` hinzu. Damit können wir eine gültige ESuite Session vorgaukeln. Um die `sessionId` zu erhalten gehe in den SDD Workspace in Postman und schicke eine eSuite Anfrage nach der Vorlage `athenticate live`. Alternativ kann man sich auch bei der Storybox anmelden und denk cookie herauskopieren.  

Unter `export` werden Werte gegeben, wohin die Artikel exportiert werden sollen. Ein Cron-job führt diesen Export dann alle 5min aus.  
Der Punkt `authentication` zeigt auf unsere Esuite und ermöglicht es dem Backend, Nutzerdaten abzugreifen. 


###### Step 5: Logging (Optional)
```YAML
logging:
  level:
    org:
      springframework:
        security: TRACE
        web: DEBUG
```
Unter den `Storybox` Einstellungen findest du die `logging` Konfiguration. Aktiviere sie und füge `web: DEBUG` hinzu.

###### Step 6: Berechtigungen erteilen
In der `SecurityConfiguration.java` setzte in Zeile 106 `.requestMatchers("/admin/health/**").access(authorization)` zu `.requestMatchers("/admin/**").permitAll()`. Jetzt kann der Endpunkt `http://localhost:8080/admin/metrics` und alle anderen `/admin/` Endpunkte aufgerufen werden. Das brauchen wir später noch.

###### Step 6.5:
Setze unter dem `/admin/` Endpunkt in Zeile 251 den requestMatcher `.requestMatchers("/**").authenticated()` auf .permitAll().

###### Step 7: Mock Daten

Erstelle im Ordner `services` eine Java Data `UserInfo.java` und fülle sie mit folgenden Daten: 
```Java
package de.svl.storybox.service;

public class UserInfo {
    private final String email;
    private final String firstName;
    private final String lastName;

    public UserInfo(String email, String firstName, String lastName) {
        this.email = email;
        this.firstName = firstName;
        this.lastName = lastName;
    }

    public String email() {
        return email;
    }

    public String firstName() {
        return firstName;
    }

    public String lastName() {
        return lastName;
    }
}
```
In der `StoryboxService.java` gestalte den Block `// Obtain user information and update authentication:` in Zeile 130 wie folgt:
```Java
// Obtain user information and update authentication:
LOG.info("Obtaining user information for article '{}'.", article.getHeadline());
//final ESuiteAuthenticationToken token = (ESuiteAuthenticationToken) SecurityContextHolder.getContext().getAuthentication();
//final String sessionId = sessionId;
//final ESuiteService.UserInfo userInfo = eSuiteService.getUserInfo(sessionId);
UserInfo userInfo = new UserInfo("beispiel@example.com", "Max", "Mustermann");
final String email = userInfo.email();
//token.setPrincipal(email);
article.setAuthorFirstname(userInfo.firstName());
article.setAuthorLastname(userInfo.lastName());
```
Bei `UserInfo userInfo = new UserInfo("beispiel@example.com", "Max", "Mustermann");
` können beliebige Mockdaten eingegeben werden.
In der `StoryboxController.java` müssen wir auch die Email injecten.  
Füge `private final String email;`, `@Value("${storybox.test.email}") final String email,`, `this.email = email;` hinzu und baue den `/profile` Endpunkt um:
```Java
    @GetMapping("/profile")
    public ProfileDto getProfile() {
        //final ESuiteAuthenticationToken token = (ESuiteAuthenticationToken) SecurityContextHolder.getContext().getAuthentication();
        //final String sessionId = token.getSessionId();
        //final String userInfo = sessionId; //eSuiteService.getUserInfo(sessionId);
        //final String email = email;
        final Optional<Profile> profileOptional = storyboxService.getProfile(email);

        if (profileOptional.isEmpty()) {
            return new ProfileDto(email, "SV Gruppe", "Test", "0123456789"); // Profildaten
        } else {
            final Profile profile = profileOptional.get();

            return new ProfileDto(
                profile.getEmail(),
                profile.getOrganization(),
                profile.getRole(),
                profile.getPhone()
            );
        }
    }
```

---

Jetzt kann das Backend mit dem `bootRun` Gradle task gestartet werden.  
Als Test ob es geklappt hat kann `http://localhost:8080/api/storybox/configuration` aufgerufen werden. Dieser Endpunkt sollte `json` Daten liefern.
Edit: Ja, `bootTestRun` funktioniert an dieser Stelle auch.

---
 #### Frontend

 Geh zurück in den `Projects` Ordner, klone das Storybox frontend repo mit `git c lone git@gitlab.com:software-development-and-data/web-frontend/sdd-storybox-next-frontend.git` benne das geklonte repo zu `frontend` um, sonst klappt nachher die Verbindung nicht. `mv sdd-storybox-next-frontend frontend`, wenn jetzt `ls` ausgeführt wird sollte folgende Ausgabe erscheinen:
 ```YAML
 frontend
 sdd-storybox-next-backend
 ```
 Gehe zur `tailwind.config.js` im root Verzeichnis. Hier muss ein import angepasst werden. Füge `import tailwindcssPrimeui from 'tailwindcss-primeui'` als oberste Zeile hinzu und ändere Zeile 81 zu `plugins: [tailwindcssPrimeui]`.  

 Das wars schon, mehr muss im Frontend nicht geändert werden.
   
---

 Installiere die packages mit `npm install` und starte das Frontend mit `npm run dev`. Besuche deine eigene Storybox auf `http://localhost:5173`. Du kannst dem Artikelprozess folgen und anschließend die Daten aus dem Body der POST-Anfrage an das Backend kopieren und in Postman verwenden.  

 Die abgeschickten Artikel landen im `bkt-service-p-cloudrun-storybox-next-storage` Bucket im Dev Ordner.

 ##### Info
 Die GCP Credentials müssen bei einem Admin besorgt werden und sollten in Stammverzeichnis als `auth.json` abgelegt werden.
 
 ---

#### Idee

In Step 4 kann die `redirect-url` auf `http://localhost:5173/text-erfassung` gesetzt werden. Dadurch kann Step 6.5 eigentlich weggeslassen werden, da die Login-Api von stage.nordkurier.de die erforderlichen cookies und token an den localhost schicken sollte...  
Man wird aber sofort wieder an die Login-Api zurückgeleitet?

---

Bei Fragen kann sich an f.hertsch@sv-gruppe.de gewendet werden.

