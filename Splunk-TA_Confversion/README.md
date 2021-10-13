# Configuration  Monitoring TA **Splunk-TA_Confversion**
Add-on to monitor and parse Splunk configuration files. This TA is adapted tom use on Splunk Enterprise Servers.
You need to install the **Splunk-App_Confversion** to see the results.

## Installation and configuration

### Splunk Components
- This TA can be installed on all Splunk components including Universal Forwarders.
- This TA should be installed and configured on all components where configuration change tracking is desired.
- This TA must be installed on Indexers and intermediate HFs, as it contains index-time transforms. 
- This TA must be installed on Search Heads, as it comes bundled with important KOs for viewing the indexed configuration data.

### Configuration
> Note: The app in this repo comes pre-configured with files in `local/`. The version on SplunkBase **is not preconfigured**, and requires the following manual steps.  
You can use local/*.conf files in this repo as templates to cofngiure the app in your environment.

Create an index called `splunk_confchange` (local/indexes.conf) if you do not wish to ingest these logs into. (recommended).

- **Optional:** Create **local/macros.conf** and override the `conf_files_index` to point at the index you prefer to chose instead of the one in the step above. 
```conf
...
[conf_files_index]
definition = (index = splunk_confchange)
...
```

- **Optional:** Create **local/inputs.conf** to designate the index you prefer to chose and enable the inputs.
```conf
...
index = splunk_confchange
...
```

#### File change monitor polling interval
By default, Splunk logs changes in the `$SPLUNK_HOME/etc/` directory every 10 minutes.<br>
In some cases, it may be desirable to do this more frequently, to ensure that no filechanges are missed. If this is desired, add the following to local/inputs.conf:
```conf
...
[fschange:$SPLUNK_HOME/etc]
#poll every 30 seconds
pollPeriod = 30
...
```

This change can cause added load on the system on the beguining. Ensure you are comfortable with that before implementing this change. 

### Optional Changes to get more results
In fact wihtin this release you are able to see all changes, even also those in dahsboards (`.xml` files).
If you don't want to see all changes by default, then you need to adapt the **props.conf** and remove the `xml` value on the following extractions:

**From:**

```conf
...
EXTRACT-00userconffile = \/etc\/(?<conf_scope>users)\/(?<conf_user>[^\/]+)\/[^\/]+\/[^\/]+\/[^\/]+?\.(conf|meta|xml)$ in source
EXTRACT-01conffile = \/(?<conf_scope>[^\/]+)\/(?P<conf_app>[^\/]+)\/(?P<conf_context>local|default|metadata|views|nav)/(?P<conf_file>[^\/]+\.(conf|meta|xml))$ in source
...
EXTRACT-z1conffile = path=\".+\/(?P<conf_app>[^\/]+)\/(?P<conf_context>local|default|views|nav)\/(?P<conf_file>[^\/]+\.conf|.xml)\"
...
```

**To:**

```conf
...
EXTRACT-00userconffile = \/etc\/(?<conf_scope>users)\/(?<conf_user>[^\/]+)\/[^\/]+\/[^\/]+\/[^\/]+?\.(conf|meta)$ in source
EXTRACT-01conffile = \/(?<conf_scope>[^\/]+)\/(?P<conf_app>[^\/]+)\/(?P<conf_context>local|default|metadata)/(?P<conf_file>[^\/]+\.(conf|meta))$ in source
...
EXTRACT-z1conffile = path=\".+\/(?P<conf_app>[^\/]+)\/(?P<conf_context>local|default)\/(?P<conf_file>[^\/]+\.conf)\"
...
```


### Security
This TA exposes potentially sensitive information to users. This includes any **passwords/tokens/usernames** contained within conf files on the instance.<br>
It is highly recommended that the index this TA uses be made **accessible soley to administrators** to prevent information disclousre to unauthorised parties. 

## Development
Please track issues here, on GitLab. Merge requests are welcome, but may not be addressed immediately.<br>
- https://github.com/Splunk-App-and-TA-development/Splunk_ConfVersion_App-and-TA/issues 


# Support
Support will be provided by the developers on a best-effort basis. The developers make no commitment to continued development.<br>
The software is provided as is, and the developer accepts no responsibility for any issues with the software, or which may result as a consequence of using the software to the fullest extent permissible by the law.

Please find the license for this software here: https://github.com/Splunk-App-and-TA-development/Splunk_ConfVersion_App-and-TA/blob/main/LICENSE
