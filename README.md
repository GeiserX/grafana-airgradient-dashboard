<p align="center">
  <img src="docs/images/banner.svg" alt="Grafana AirGradient Dashboard banner" width="900"/>
</p>

<h1 align="center">Grafana AirGradient Dashboard</h1>

<p align="center">
  Grafana dashboard for AirGradient ONE indoor air quality monitoring with bathroom extractor state tracking.
</p>

---

A comprehensive Grafana dashboard for the [AirGradient ONE](https://www.airgradient.com/indoor/) indoor air quality monitor, designed to visualize all sensor data alongside a bathroom extractor fan state timeline.

## Features

- **CO2 gauge & time series** with color-coded thresholds (green/yellow/orange/red)
- **PM2.5 gauge** following WHO air quality guidelines
- **VOC & NOx Index** gauges and time series
- **Temperature & Humidity** stat panels with sparklines and time series
- **PM1 / PM2.5 / PM10** particulate matter comparison
- **PM0.3 particle count** for ultra-fine particle tracking
- **WiFi signal strength** indicator
- **Extractor fan state timeline** (ON/OFF bands with color mapping)
- **CO2 + Extractor overlay** — correlate ventilation events with CO2 drops

## Prerequisites

- [AirGradient ONE](https://www.airgradient.com/indoor/) sensor
- [Home Assistant](https://www.home-assistant.io/) with the AirGradient integration
- [Prometheus integration](https://www.home-assistant.io/integrations/prometheus/) enabled in Home Assistant
- [Grafana](https://grafana.com/) with a Prometheus datasource scraping Home Assistant

## Installation

### Option 1: Grafana API (recommended)

```bash
curl -s -X POST "http://YOUR_GRAFANA:3000/api/dashboards/db" \
  -u "admin:YOUR_PASSWORD" \
  -H "Content-Type: application/json" \
  -d @dashboard.json
```

### Option 2: Provisioned file

Copy `dashboard.json` to your Grafana provisioned dashboards directory:

```bash
cp dashboard.json /var/lib/grafana/dashboards/
```

## Configuration

The dashboard expects the following Home Assistant entities exposed via Prometheus:

| Metric | Entity | Description |
|--------|--------|-------------|
| `hass_sensor_carbon_dioxide_ppm` | `sensor.*_carbon_dioxide` | CO2 in ppm |
| `hass_sensor_pm25_u0xb5g_per_mu0xb3` | `sensor.*_pm2_5` | PM2.5 in ug/m3 |
| `hass_sensor_pm10_u0xb5g_per_mu0xb3` | `sensor.*_pm10` | PM10 in ug/m3 |
| `hass_sensor_pm1_u0xb5g_per_mu0xb3` | `sensor.*_pm1` | PM1 in ug/m3 |
| `hass_sensor_unit_particles_per_dl` | `sensor.*_pm0_3` | PM0.3 count |
| `hass_sensor_temperature_celsius` | `sensor.*_temperature` | Temperature |
| `hass_sensor_humidity_percent` | `sensor.*_humidity` | Humidity |
| `hass_sensor_state` | `sensor.*_voc_index` | VOC Index |
| `hass_sensor_state` | `sensor.*_nox_index` | NOx Index |
| `hass_sensor_state` | `sensor.*_signal_strength` | WiFi dBm |
| `hass_switch_state` | `switch.*_extractor_*` | Extractor ON/OFF |

> **Note**: Update the `entity` label filters in the dashboard JSON to match your specific entity IDs.

### Datasource

The dashboard uses a Prometheus datasource with UID `ds_prometheus`. Update the UID in `dashboard.json` if yours differs:

```bash
sed -i 's/ds_prometheus/YOUR_DATASOURCE_UID/g' dashboard.json
```

## Maintainers

[@GeiserX](https://github.com/GeiserX)

## Contributing

Feel free to dive in! [Open an issue](https://github.com/GeiserX/grafana-airgradient-dashboard/issues/new) or submit PRs.

This project follows the [Contributor Covenant](http://contributor-covenant.org/version/2/1/) Code of Conduct.
