# Block 7 - Observability und Resilience

Dieser Integrationsbaustein macht Anwendung und Cluster gemeinsam beobachtbar. Prometheus sammelt Anwendungs-, RabbitMQ-, Kubernetes- und PostgreSQL-Metriken; Grafana und die Systemansicht zeigen denselben realen Zustand aus unterschiedlichen Perspektiven.

## Verwendung im Kurs

- Privates Integrationspaket fuer das bestehende Studierenden-Repository
- Kein neues Abgabe-Repository: Die Aenderungen werden in den fortlaufenden Projektstand uebernommen
- Fuer einen reproduzierbaren Stand ist der Release `v1.0.0` zu verwenden
- Die enthaltenen Zugangsdaten sind ausschliesslich fuer das lokale Kurs-Lab bestimmt

## Enthalten

- `cluster-observer` mit namespace-begrenztem Read-only-RBAC
- ServiceMonitors, PodMonitor und HPA
- vorkonfiguriertes Grafana-Dashboard
- RabbitMQ-Prometheus-Plugin und Metrik-Patches
- Smoke-, Reset- und Failure-Demo-Skripte
- `scale-city.sh` fuer native Kubernetes-Skalierungsdemos

## Arbeitsauftrag

1. Monitoring-Stack installieren und den Baustein in Block 6 integrieren.
2. Golden Signals den vorhandenen Metriken und Panels zuordnen.
3. Restaurant-Pod entfernen und Self-Healing im Systemdashboard verfolgen.
4. Last erzeugen und HPA, Queue-Tiefe, Fehlerrate und Latenz korrelieren.
5. Einen fachlichen und einen technischen Alarm mit konkreter Schwelle formulieren.
6. Kuriere, Kunden und ein Restaurant skalieren und `desired -> ready -> Stadtfigur/Kuechenmodul` verfolgen.

```bash
CONTEXT=k3d-delivery-lab ./platform/monitoring/install.sh
kubectl --context k3d-delivery-lab apply -k deploy/overlays/block-07-observability
CONTEXT=k3d-delivery-lab ./scripts/failure-demo.sh
CONTEXT=k3d-delivery-lab ./scripts/smoke-test.sh
CONTEXT=k3d-delivery-lab ./scripts/scale-city.sh couriers 6
CONTEXT=k3d-delivery-lab ./scripts/scale-city.sh customers 5
CONTEXT=k3d-delivery-lab ./scripts/scale-city.sh restaurant-pizza 3
```

Abnahme: Alle Prometheus-Targets sind aktiv, das Grafana-Dashboard hat Daten, HPA erhaelt Metriken, Entity-Zahlen folgen echten Replica-Zahlen und die Anwendung erholt sich sichtbar von der Fehlerdemo.
