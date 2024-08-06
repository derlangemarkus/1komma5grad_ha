# How to integrate 1komma5grad Heartbeat into Home Assistant

This guide will show you how to integrate 1komma5grad Heartbeat into Home Assistant. It is not a full integration, but a configuration workaround to get the data into Home Assistant. Just follow the steps to create a RESTful sensor in Home Assistant that reads the data from the 1komma5grad Heartbeat API.

Please note that this guide is not officially supported by 1komma5grad. It is a workaround that might break if the API changes.

## No guarantee
**I accept absolutely no liability for damage to your Home Assistant or Heartbeat. Everything you do, you do at your own risk!**

## Step 1: Find out your system ID

You will need a browser that can display requests in its developer console. I recommend using Google Chrome. Right click and select "Inspect" to open the developer console. Go to the [1komma5grad Heartbeat website](https://my.1komma5.io/) and log in. Open the developer console and go to the "Network" tab. Reload the page and look for a request to a request similar to `https://api.gridx.de/systems/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/live` or  `https://api.gridx.de/systems/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/weather`. 

The `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` is your system ID. Please remember it for the next steps.

## Step 2: Create RESTful sensors in Home Assistant

### Create an integration directory (if you don't have one already)
Please open a text editor (e.g. [Visual Studio Code](https://community.home-assistant.io/t/home-assistant-community-add-on-visual-studio-code/107863)) and create a new directory in your Home Assistant configuration directory. You can name it `integrations` or whatever you like.

### Copy the 1komma5grad config file
Put the  1komma5grad config file ([1komma5grad.yaml](1komma5grad.yaml)) into the directory you just created.

### Adjust your credentials
In 1komma5grad.yaml you will find lines for your username and password. Fill in these values for `PUT_IN_YOUR_USERNAME_HERE` and `PUT_IN_YOUR_PASSWORD_HERE`. 

Unfortunately I did not find a way to include these values from an outsourced secret.yaml. Just let me knew if you know a solution.

### Adjust your system ID 
In the 1komma5grad.yaml there is an API request in the RESTful sensor. Please insert your system ID from the step above at `PUT_IN_YOUR_SYSTEM_ID_HERE`.

### Add the 1komma5grad sensor to your configuration.yaml
Add the following lines to your `configuration.yaml`:

```yaml
homeassistant:
  packages: !include_dir_named integrations

```

## Step 3: Restart Home Assistant

Restart your Home Assistant. It is strongly recommended to check the YAML configuration in the Developer Tool section first.

## Step 4: Check the new entities

First of all there should be an entity called "1komma5grad API Token". This is just an entity for internal use. The value of the entity should be some cryptic characters and numbers. Please keep this value secret!

Furthermore, you will find some entities which names start with "1komma5grad". Their names are in German. You can rename them in the 1komma5grad.yaml or you can give them another name in Home Assistant. 

I you like you can add some helpers e.g. to convert the power measured in Watt in energy measured in kWh by using an [integral helper](https://www.home-assistant.io/integrations/integration/).

## Further things to mention
- As said there is neither a guarantee nor liability!
- There is even no support. Good luck!
- Since I do not have a wallbox or heat pump I will not invest time to add further sensors.
- Please feel free to improve my tiny little workaround hack. Maybe by writing a HACS integration (I would be happy if I can assist) or a pull request.
- **Have fun!**