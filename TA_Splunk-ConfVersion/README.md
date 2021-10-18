# Configuration  Monitoring TA **TA_Splunk-ConfVersion**
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
In some cases, it may be desirable to do this more frequently, to ensure that no filechanges are missed.<br>
If this is desired, add the following to `local/inputs.conf`:
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
**This TA exposes potentially sensitive information to users.**<br>
This includes any **passwords/tokens/usernames** contained within conf files on the instance.<br>
We highly recommended to allow the **access soley to administrators** to index this TA uses to prevent information disclousre to unauthorised parties. 

The installation of this Addon can cause added load on the Splunk systems. Ensure you are comfortable with that before implementing this change. 

## Development
Please track issues here, on GitLab. Merge requests are welcome, but may not be addressed immediately.<br>
- https://github.com/Splunk-App-and-TA-development/Splunk_ConfVersion_App-and-TA/issues 


## **Release Notes**

#### v 1.1.0
- Adapt some extractions and inputs to be able to see more changes.
- Divide TA and App in two different apps.
- Document TA and App installaion

#### v 1.0.0
- Initial Versionsion


## **Support**
Support will be provided by the developers on a best-effort basis. The developers make no commitment to continued development.<br>
The software is provided as is, and the developer accepts no responsibility for any issues with the software, or which may result as a consequence of using the software to the fullest extent permissible by the law.<br>
Please use Github to open issues or feature requests:
- **https://github.com/Splunk-App-and-TA-development/Splunk_TA_common-viz/issues**


#### Splunk Version Support

| Supported Splunk Versions  | Unsupported or Deprecated  |
| --- | --- |
|  8.2.x, 8.1.x, 8.0.x, 7.3.9, 7.3.6 | 7.3.5 and lower, 6.6.x, 6.5.x, 6.4, 6.3, 6.2, older  |


#### Credits
This app is supported by Patrick Vanreck (SwissTXT). Contact us under: **[yoyonet-info@gmx.net](mailto:yoyonet-info@gmx.net)**.

- Find us under **[SECLAB Splunk App & TA Development](https://github.com/Splunk-App-and-TA-development "SECLAB Splunk App & TA Development")**
- Send questions to ***[yoyonet-info@gmx.net](mailto:yoyonet-info@gmx.net)***
- Developped by **Patrick Vanreck**


#### Software License
Please find the license for this software here: https://github.com/Splunk-App-and-TA-development/Splunk_ConfVersion_App-and-TA/blob/main/LICENSE

#### Copyrights
Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"),<br>
to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense,<br>
and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:
	
The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.


<div class="footer">
    Copyright &copy; 2017-2021 by Patrick Vanreck (STXT Security)
</div>
