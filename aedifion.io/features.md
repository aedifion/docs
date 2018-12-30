---
description: This page introduces important key features of aedifion.io
---

# Features

## Advanced data ingress

aedifion.io offers plant-, building- and district-wide data acquisition compatible to many bus communication standards, partially with auto discovery of [datapoints ](https://docs.aedifion.io/docs/glossary#datapoint)and [devices](https://docs.aedifion.io/docs/glossary#device). Moreover, it integrates all IP-available data sources, replicates local data bases and connects to existing data servers. In addition, users or devices can stream their data directly into aedifion.io.

| Type | Example |
| :--- | :--- |
| Bus communication | BACnet, Modbus, KNX, M-Bus,... |
| Data servers | OPC DA, OPC UA |
| Local data bases | MS SQL, mySQL, pgSQL, ... |
| Rest APIs | MS Exchange, weather data, weather forecasts,... |

{% hint style="info" %}
We aspire to integrate &lt;&lt;everything that is IP-based available&gt;&gt;.
{% endhint %}

{% hint style="success" %}
aedifion.io works fully plug-and-play for BACnet system. All it needs is a gateway that operating personnell can easily connect to local networks. Once plugged in, the gateway connects automatically to our servers and we start data integrations.
{% endhint %}

## Metadata acquisition

For each integrated [datapoint ](https://docs.aedifion.io/docs/glossary#datapoint)and for each integrated[ device](https://docs.aedifion.io/docs/glossary#device), aedifion.io is able to acquire comprehensive metadata which originates either from standards like BACnet, from Modbus plat descriptions, from local databases or data servers as OPC. This metadata is automatically processed into aedifion.io's data structuring.  Metadata is added to each datapoint via [tags](https://docs.aedifion.io/docs/glossary#tag). Integrated devices are semantically modelled as [components](https://docs.aedifion.io/docs/glossary#component).  

## Data storage

•Standardmäßig CoV, Threshold 0•Sampelrates zumeist vom Feld her vorgegeben•typischerweise &lt; 5 Sekunden•aktuelle BACnet-SPSen &lt; 1 Sekunde•Kombination aus Zeitreihendatenbanken•Influx DB, self-managed•NoSQL-Datenbanken für Metadaten

## Data provision

•Datenabruf über API \(Matlab, Python, etc.\)•Beta-Frontend über www.aedifion.io•Version 1.0 in Q1 2019•Excel-Plugin•Open-Source Visualisierung „Grafana“•siehe [https://grafana.com/](https://grafana.com/)•Websockets & Widgets für Livedata•CSV-Export \(Plain Data\) beispielsweise für  
 ML-Anwendungen

## Data processing

•Resampling•Statistische Auswertungen••

•Berechnungsvorschriften und Soll-Ist-Vergleiche nach•VDI 6041 technisches Monitoring•AMEV technisches Monitoring•Energiemanagement möglich nach•ISO 50001•Zertifizierung in 2019

### Stream processing

Stream-Prozesse, bspw.:•Berechnung Leistungszahl•Berechnung Wärmestrom•Überprüfung von Betriebsregeln•

### Batch processing

Batch-Prozesse, bspw.:•Berechnung Arbeitszahl•Energetische Berechnungen über längere Zeiträume

### Virtual data points

Virtuelle Datenpunkte•Zurückspielen von Berechnungen

## Data management & structuring

•Konzept der „Tags“•Tag ist ein Tupel:  
 {Key, Value, Source, Timestamp…}•Beispiele•{Messgröße, Vorlauftemperatur, AI, 2019-11-22}•{Einheit, ° C, BACnet, 2019-11-21}••Konzept ermöglicht Datenfeldbezeichung nach gängigen Standards \(VDI 3815, BUDO, ISE-Schlüssel, Bricks-Schema…\)

Renaming

## Project management - rule based access control

•Rollenkonzept•Definition von Rollen•Zuordnen von Rechten für Datenpunkte und Funktionen•Lesen•Schreiben•Nutzen•Zuordnen von Rollen zu Nutzern

## Alarming and notifications

•Standardisierte Alarm-Konzepte per API verfügbar•Throughput•Threshold•Siehe [https://api.aedifion.io/ui/](https://api.aedifion.io/ui/)••Maßgeschneiderte Alarme:•Stream-Prozesse•Virtuelle Datenpunkte•Nutzung Alarm-Konzepte••Viele Ausgabekanäle unterstützt

## Integrations

sum up all integrations done so far. \(Alexa, Microsoft Exchange, Excel tool features\)

### Weather and weather forecast data

sum it up here.

## Controls

kurze beschreibung von Controls

### Controls runtime environment

## Authentification mechanisms

summarize the possibilities.

## Data privacy

show the possibilities.

## Availability and quality assurance

•Kontinuierliche Überwachung des Systemstatus•Messpunkte entlang der gesamten Pipeline•Kombiniert mit ingenieurswissenschaftlichem Know-How••Redundanz & Backups•Komplett gespiegelte Pipeline und Backend•Zusätzliche nächtliche Full-Backups••Alarming•Überwachung der kompletten Pipeline•Intern, per Instant Messenger



