# v0.3.0

* add: custom collector to walk through ec2 list of ebs volumes and collect from each one individually
* upd: dimensions `map[string]string`
* upd: dump _only_ on when `trace_metrics` setting true
* fix: lint warnings

# v0.2.1

* add: AWS/DX

# v0.2.0

* doc: update azure readme
* upd: clarify time delta calculation
* add: more aggressive ticker to start collections
* add: log collection duration
* upd: remove dead code
* fix: dynamic dimensions from call param
* fix: catch zero instances for a given region
* add: cloud vendor specific check submodule
* upd: add region debug message
* upd: reduce ticker drift to 2sec
* upd: promote duration message to Info
* upd: only print submission in debug mode
* upd: print response body as raw json
* fix: verify returned check bundle has a submission url
* upd: dependencies
* add: use service specific check submodules
* upd: add interval logging on client start
* upd: add additional aws colleciton logging
* upd: adjust aws collection interval delta calculation
* upd: stricter linting
* doc: update install instructions (correct repository for releases)

# v0.1.1

* fix: release to circonus-labs repository

# v0.1.0

* Initial release
