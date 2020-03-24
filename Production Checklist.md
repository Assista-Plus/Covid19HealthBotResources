
# COVID-19 Healthcare Bot Production Check List

Hi! This is my personal check list with information gathered from working with many peers on several **COVID-19 Health Bot** projects at Microsoft. 

## Important considerations

### 1. Front-end UI changes:
Front-end or UI changes requested by many bot users are collated here to help new users with rapid deployment; comments added for understanding and maintainability. 

### 2. Create a second bot instance for production environment:
Once testing is done in the W1-Free instance in development (DEV) environment, create another instance for production (PROD) environment. *Note: Decision to create different instances, # of instances is at user discretion. Guideline is to have a minimum of 2 instances – PROD and Non-PROD. There are 2 ways to clone the health bot instance/scenarios based on your need:

**(1) Backup – Restore the entire configuration**
		Click on the Setting icon on top-right corner and in the drop-down menu, click on Backup. A .hbs file will be generated and downloaded to your local computer/VM. 	After configuring the newly created health bot instance, switch to the new instance by clicking on My Healthcare bots.

In the new bot portal, click on Setting icon in top-right corner and click Restore and browse the previously downloaded .hbs file and type “Restore”. You will have the exact configuration as the first bot with respect to scenarios, language models etc but with message count = 0.

**(2) Export-Import each custom scenario**
If you want to move only specific scenarios to Prod instance, you can export each scenario individually or multiple scenarios at once by selecting the required scenario and clicking Export in Native format. You will get a .json file (single scenario export) or .zip file (multiple scenarios export) with .json files.


Switch to your new bot instance and navigate to Scenarios -> Manage and click on Import. Import accepts single .json files and not .zip files. *Note: Do not change the name during export/import of scenarios – this affects default settings and might lead to inaccurate scenarios in target bot instance. Also, importing scenarios with same scenario_id will prompt you for overwrite.

### Enable custom telemetry in health bot

In the Production bot instance, Navigate to Integration -> Secrets. In Custom Telemetry section, add App Insights Instrumentation key. This instrumentation key can be obtained from Overview blade of an Application Insights resource. Note: You can create a new App Insights resource or use a common App Insights for this step and for web chat hosting App Service.

With this feature, you can pipe data from the Healthcare Bot into Application Insights and then Product Analytics/Business Analyst/Performance Monitoring team can query with Log Analytics or export data as .csv or build dashboards with Power BI (M query). For more information on using custom telemetry feature, please refer to technical documentation [link](https://docs.microsoft.com/en-us/HealthBot/custom_telemetry). Sample queries and tutorials for writing App Insights queries can be found [here](https://docs.microsoft.com/en-us/azure/azure-monitor/log-query/get-started-portal).

Note: You also get built-in analytics with health bot by navigating to Analytics -> Reports
 
### Production environment App Service
**(1) Scale up**	
Deploy the health bot to Azure as seen in Creating the WebChat channel section in [blog 1](https://techcommunity.microsoft.com/t5/healthcare-and-life-sciences/updated-quick-start-setting-up-your-covid-19-health-bot/ba-p/1230537). For the production instance, App Service has to scale up to support production workloads. In your Azure portal, navigate to production App Service -> Settings -> Scale up (App Service plan). Choose one of the production types (S1, P1V2, P2V2 or P3V2) based on your requirements using guidance [here](https://azure.microsoft.com/en-us/pricing/details/app-service/).

**(2) Scale out**
Also, setup Scale out (App Service plan) either to Manual Scale or Custom autoscale based on your metrics. Learn more about Autoscale [here](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/autoscale-get-started)

**(3) Set HTTPS only On**
Navigate to App Service Settings -> TLS/SSL settings -> Bindings and turn HTTPS Only: to ON

**(4) Always On**
Navigate to App Service Configuration -> General Settings and turn the Always On variable to ON

### Enable Application Insights and diagnostic logging for all App Service instances
If you have used GitHub sample to click and deploy to Azure, the health bot is hosted on an Azure App Service. Navigate to App Service -> Application Insights and click on Turn on Application Insights

### High availability
To maintain high availability of health bot and underlying App Service, make sure to consider adding two instances of App Service in an Active/Active or Active/Stand-by region configuration with an Azure Front Door or Traffic Manager. For more information on this feature, please refer to [Azure Front Door](https://docs.microsoft.com/en-us/azure/frontdoor/front-door-overview) and [Azure Traffic Manager](https://docs.microsoft.com/en-us/azure/traffic-manager/traffic-manager-overview) links.

	

**More resources to be updated regularly.**