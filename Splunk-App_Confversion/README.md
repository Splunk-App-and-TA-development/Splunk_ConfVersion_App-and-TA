# Configuration  Monitoring App - **Splunk-TA_Confversion**
App to monitor and parse Splunk configuration files.

To run this App, install the specific **Splunk-TA_Confversion** Add-on to monitor and parse Splunk configuration changes on each Splunk Server.
This App is specific adapted for Splunk Enterprise Servers running in cluster mode. 
The best way to get the results is to install this app in the same Search Head you installed the DMC Console.

## Installation and configuration

### Splunk Components
This App should be installed on your DMC console or Splunk DMC server.
It comes bundled with important KOs for viewing the indexed configuration data. 

### Configuration
> Note: The app in this repo comes pre-configured with files in `default/`.  
You can use local/*.conf files in this repo as templates to cofngiure the app in your environment.


#### How to use the app
Just install the App and ensure that the `index=splunk_confchange` is created and the macros.conf in the TA are working.
If you change the index name, then you'll maybe have to change it in some dashboards.


#### Dependencies
To run the App, you need to ensure that all visualizations are installed.

This change can cause added load on the system. Ensure you are comfortable with that before implementing this change. 

### Security
This TA exposes potentially sensitive information to users. This includes any passwords/tokens/usernames contained within conf files on the instance. 

It is highly recommended that the index this TA uses be made accessible soley to administrators to prevent information disclousre to unauthorised parties. 

## Development
Please track issues here, on GitLab. Merge requests are welcome, but may not be addressed immediately. 

# Support
Support will be provided by the developers on a best-effort basis. The developers make no commitment to continued development. The software is provided as is, and the developer accepts no responsibility for any issues with the software, or which may result as a consequence of using the software to the fullest extent permissible by the law.

Please find the license for this software here: https://github.com/d3/d3/blob/master/LICENSE
