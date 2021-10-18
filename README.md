# Splunk Configuration Change Monitoring - App and TA
App and Add-on to monitor and parse Splunk configuration files.

## Installation and configuration
Underhalf the Git repo you'll find two main folders that you need to upload to `/opt/splunk/etc/apps/`.
After the installation of booth folders in `/opt/splunk/etc/apps/` restart the Splunk daemon to make it run.

1. Create an new index in your environment: `index = splunk_confchange`

2. Upload the App **App-ConfVersion** to `/opt/splunk/etc/apps/`.
_App with the views of the Configuration Changes on your Splunk Servers._

3. Upload the TA  **TA-ConfVersion** `/opt/splunk/etc/apps/`.
_TA with the extractions and parsings of the Configuration Changes on your Splunk Servers._

4. Restart your Splunk Server: `/opt/splunk/bin/splunk restart`


### Requirements and Dependencies

1. The **TA-ConfVersion** can be installed on all Splunk components including Universal Forwarders. 
	- We recommended to install/configure the TA on all components where configuration change tracking is desired.

2. The **TA-ConfVersion** must be installed on Indexers and intermediate HFs, as it contains index-time transforms. 

3. The **TA-ConfVersion** must be installed on Search Heads, as it comes bundled with important KOs for viewing the indexed configuration data. 

4. You need to install and configura the **TA-ConfVersion** berfore to run the **App-ConfVersion**.

5. We recommended to install the **App-ConfVersion** on your **[Distributed Monitoring Console](https://docs.splunk.com/Documentation/Splunk/8.2.2/DMC/DMCoverview)** to be able to see all Changes on all your servers.
	- You can enable the _Monitoring Console_ on one of your Splunk Search Head if you don't have a dedicated _DMC Server_. 


## Configuration of the **TA-Confversion**

### Configuration Files Contained Within the Add-on
- `app.conf`
- `indexes.conf`
- `inputs.conf`
- `props.conf`
- `transforms.conf`

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
By default, Splunk logs changes in the `$SPLUNK_HOME/etc/` directory every 10 minutes. In some cases, it may be desirable to do this more frequently, to ensure that no filechanges are missed. If this is desired, add the following to local/inputs.conf:
```conf
...
[fschange:$SPLUNK_HOME/etc]
#poll every 30 seconds
pollPeriod = 30
...
```

#### File change monitor polling interval

By default, Splunk logs changes in the `$SPLUNK_HOME/etc/` directory every 10 minutes. In some cases, it may be desirable to do this more frequently, to ensure that no filechanges are missed. If this is desired, add the following to local/inputs.conf:
```conf
[fschange:$SPLUNK_HOME/etc]
#poll every 30 seconds
pollPeriod = 30
```

This change can cause added load on the system. Ensure you are comfortable with that before implementing this change. 

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

## Configuration of the **App-Confversion**
Depending on the version of some Visualizations you installed before, you may need to adapt some dashboards afterwards.

#### Adapt the **Timeline Visualization** XML on Dashboards
To run the App, you need to ensure that the **timeline visualization** is installed.
Download direct from Splunk: https://splunkbase.splunk.com/app/3120/

The best way is to install the **Splunk-TA Common-viz** which contains a big collection of visualizations in one TA.
Download and install it from Github: https://github.com/Splunk-App-and-TA-development/Splunk_TA_common-viz

You need to adapt the path of the visualization in the dashboard named `configuration_change_statistics.xml` if you install the timeline viz from splunkbase.com.

**From:**
...
```xml
<option name="Splunk_TA_common-viz.timeline.axisTimeFormat">MINUTES</option>
<option name="Splunk_TA_common-viz.timeline.colorMode">categorical</option>
<option name="Splunk_TA_common-viz.timeline.maxColor">#dc4e41</option>
<option name="Splunk_TA_common-viz.timeline.minColor">#53a051</option>
<option name="Splunk_TA_common-viz.timeline.numOfBins">3</option>
<option name="Splunk_TA_common-viz.timeline.tooltipTimeFormat">MINUTES</option>
<option name="Splunk_TA_common-viz.timeline.useColors">1</option>
...
```

**To:**
```xml
<option name="timeline.timeline.axisTimeFormat">MINUTES</option>
<option name="timeline.timeline.colorMode">categorical</option>
<option name="timeline.timeline.maxColor">#dc4e41</option>
<option name="timeline.timeline.minColor">#53a051</option>
<option name="timeline.timeline.numOfBins">3</option>
<option name="timeline.timeline.tooltipTimeFormat">MINUTES</option>
<option name="timeline.timeline.useColors">1</option>
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
