# Configuration  Monitoring App - **App-Confversion**
App to monitor and parse Splunk configuration files.

To run this App, install the specific **App-Confversion** Add-on to monitor and parse Splunk configuration changes on each Splunk Server.<br>
This App is specific adapted for Splunk Enterprise Servers running in cluster mode.<br>
The best way to get the results is to install this app in the same Search Head you installed the DMC Console.

## Installation and configuration

### Splunk Components
This App should be installed on your DMC console or Splunk DMC server.
It comes bundled with important KOs for viewing the indexed configuration data. 

### Configuration
> Note: The app in this repo comes pre-configured with files in `default/`.  
You can use `local/*.conf` files in this repo as templates to cofngiure the app in your environment.

### How to use the app
Just install the App after the installation and configuration or the _TA-Confversion_.

Ensure that the `index=splunk_confchange` is created and the `macros.conf` in the _TA-Confversion_ are working.<br>
If you change the index name, then you'll maybe have to change it in some dashboards.


### Requirements and Dependencies
There are some points to concern before you start with the app.<br>
Remember to follow the instructions explained below.

#### Install **TA_Confversion** before
to use this App you need first to install the **TA-Confversion**.<br>
You can download the "all in one" package under: - https://github.com/Splunk-App-and-TA-development/Splunk_ConfVersion_App-and-TA

follow the setup instructions once the _TA-Confversion_ is installed and restart then your Splunk instance.

#### Adapt the **Timeline Visualization** XML on Dashboards
To run the App, you need to ensure that the **timeline visualization** is installed.<br>
Download direct from Splunk: https://splunkbase.splunk.com/app/3120/

The best way is to install the **Splunk-TA Common-viz** which contains a big collection of visualizations in one TA.<br>
Download and install it from Github: https://github.com/Splunk-App-and-TA-development/Splunk_TA_common-viz

You need to adapt the path of the visualization in the dashboard named `configuration_change_statistics.xml` if you install the timeline viz from splunkbase.com.

**From:**
```xml
....
...
<option name="Splunk_TA_common-viz.timeline.axisTimeFormat">MINUTES</option>
<option name="Splunk_TA_common-viz.timeline.colorMode">categorical</option>
<option name="Splunk_TA_common-viz.timeline.maxColor">#dc4e41</option>
<option name="Splunk_TA_common-viz.timeline.minColor">#53a051</option>
<option name="Splunk_TA_common-viz.timeline.numOfBins">3</option>
<option name="Splunk_TA_common-viz.timeline.tooltipTimeFormat">MINUTES</option>
<option name="Splunk_TA_common-viz.timeline.useColors">1</option>
....
...
```

**To:**
```xml
....
...
<option name="timeline.timeline.axisTimeFormat">MINUTES</option>
<option name="timeline.timeline.colorMode">categorical</option>
<option name="timeline.timeline.maxColor">#dc4e41</option>
<option name="timeline.timeline.minColor">#53a051</option>
<option name="timeline.timeline.numOfBins">3</option>
<option name="timeline.timeline.tooltipTimeFormat">MINUTES</option>
<option name="timeline.timeline.useColors">1</option>
....
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
-Initial Versionsion


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