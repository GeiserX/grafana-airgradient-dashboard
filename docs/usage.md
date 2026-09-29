# Usage

## What you need

- An [AirGradient ONE](https://www.airgradient.com/indoor/) sensor
- [Home Assistant](https://www.home-assistant.io/) with the AirGradient integration
- The [Prometheus integration](https://www.home-assistant.io/integrations/prometheus/) enabled in Home Assistant
- [Grafana](https://grafana.com/) with a Prometheus datasource scraping Home Assistant

## Entities

The dashboard expects these Home Assistant entities, exposed through Prometheus:

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
| `hass_switch_state` | `switch.*_extractor_*` | Extractor on or off |
| `hass_binary_sensor_state` | `binary_sensor.*_input_*` | The extractor's physical switch input |

The `entity` label filters in `dashboard.json` name the author's entities (`sensor.i_9psl_*`, `switch.extractor_bano_extractor_bano`, `binary_sensor.extractor_bano_input_0`). Replace them with your own entity IDs before importing.

## Datasource

The dashboard uses a Prometheus datasource with UID `ds_prometheus`. Change the UID in `dashboard.json` if yours differs:

```bash
sed -i 's/ds_prometheus/YOUR_DATASOURCE_UID/g' dashboard.json
```

## Provisioning from a file

`dashboard.json` is wrapped for the HTTP API (`{"dashboard": ..., "overwrite": true}`). Grafana's file provisioner wants the bare dashboard object, so unwrap it into your provisioned dashboards directory:

```bash
jq .dashboard dashboard.json > /var/lib/grafana/dashboards/airgradient.json
```
