# GCP

## Installation

1. Create a directory for the install. Suggested:: `mkdir -p /opt/circonus/cloud-agent`
1. Download the [latest release](https://github.com/circonus-labs/circonus-cloud-agent/releases/latest)
1. Unpack the release in the directory created in first step
1. In this directory, create a config folder. Suggested: `mkdir /opt/circonus/cloud-agent/etc/gcp.d`
1. Auto-create a service specific configuration template in the desired format (yaml, toml, or json).  Suggested: `sbin/circonus-cloud-agentd --enable-gcp --gcp-example-conf=yaml > etc/gcp.d/gcp-config.yaml`
    * Note, the `id` in the template is defaulted to the filename.  This should be changed to a name that will be unique across all cloud-agents used in Circonus
    * Follow [configuration](#configuration) instructions to finish config settings.
1. Setup as a system service or run in foreground ensuring that `--enable-gcp` is specified

## Options

```sh
$  sbin/circonus-cloud-agentd -h
The Circonus Cloud Agent collects metrics from cloud infrastructures and fowards them to Circonus.

Usage:
  circonus-cloud-agent [flags]

Flags:
      --gcp-conf-dir string         GCP configuration directory (default "/opt/circonus/cloud-agent/etc/gcp.d")
      --gcp-example-conf string     Show GCP config (json|toml|yaml) and exit
  -c, --config string               config file (default: circonus-cloud-agent.yaml|.json|.toml)
  -d, --debug                       [ENV: CCA_DEBUG] Enable debug messages
      --enable-gcp                  Enable GCP metric collection client
  -h, --help                        help for circonus-cloud-agent
      --log-level string            [ENV: CCA_LOG_LEVEL] Log level [(panic|fatal|error|warn|info|debug|disabled)] (default "info")
      --log-pretty                  [ENV: CCA_LOG_PRETTY] Output formatted/colored log lines [ignored on windows]
  -V, --version                     Show version and exit
```

## Configuration

### Google

1. Find project to monitor in GCP UI
1. Add service account to project
   * Set service account name, e.g. 'circonus-cloud-agent'
   * Add description
   * Click create
   * Add roles: project>viewer, monitoring>monitoring viewer, compute engine>compute viewer
   * Create key, type JSON (note where the key is downloaded)
       * Place the JSON file downloaded in GCP setup somewhere and ensure that the user running `circonus-cloud-agentd` will be able to read the file (e.g. `nobody` on linux when run as a systemd service).
       * Ensure that the configuration file setting `gcp.credentials_file` is set to the full filespec of where the service account JSON file was placed.
1. The following APIs must be enabled for the project
   * [Cloud Resource Manager API](https://console.cloud.google.com/apis/library/cloudresourcemanager.googleapis.com) - role _project>viewer_ is used to obtain the project meta data: ensure it is an active project, project name is used to create check bundle, project labels are added as a base set of stream tags along with project id
   * [Stackdriver Monitoring API](https://console.cloud.google.com/apis/library/monitoring.googleapis.com) - role _monitoring>monitoring viewer_ is used to retrieve available metrics and metric data
   * [Compute Engine API](https://console.cloud.google.com/apis/library/compute.googleapis.com) - role _compute engine>compute viewer_ is used to obtain a list of instances, obtain state/status of an instance, name, labels (for stream tags), etc.
1. Add these to the `gcp` section of the configuration file.

### Circonus

1. Use Circonus UI to create or identify an API Token to use
1. Add the `key` to the config file under the `circonus` section

### Example configuration

Minimum configuration:

```yaml
---
id: ...
gcp:
  credentials_file: ...
circonus:
  key: ...
```
