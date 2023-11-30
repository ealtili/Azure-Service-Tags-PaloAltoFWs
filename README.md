![Support:Community](https://img.shields.io/badge/Support-Community-blue)
![License:MIT](https://img.shields.io/badge/License-MIT-green)

# Option 1 Azure Service Tags as Palo Alto Networks External Dynamic Lists (EDL)

## Overview

This repository contains a simple Docker container that downloads the Microsoft Azure Service Tags and hosts them on a Webserver for ingestion into a Palo Alto Networks VM-Series firewall as an External Dynamic List.

## Deployment Option 1: Azure Container Instance

Create A Resource Group:
```
az group create --name azure-servicetags --location westeurope
```
Deploy the Container Azure Container Instance with a public IP:
```
az container create --name azure-servicetags --image salsop/azure-servicetags:1.0 --resource-group azure-servicetags --ip-address public --cpu 1 --memory 1 --restart-policy always
```

Deploy the Container in Azure Container Instance within an existing virtual network subnet. *(Note this needs to be an empty subnet)*
```
az container create --name azure-servicetags --image salsop/azure-servicetags:1.0 --resource-group azure-servicetags --vnet existing-vnet --subnet empty-subnet --cpu 1 --memory 1 --restart-policy always
```

## Deployment Option 2: Linux Host running Docker

### Running the Docker Container
First clone this repository to the Linux instance that you are intending to run this docker container on.
```
git clone https://www.github.com/ealtili/Azure-Service-Tags-PaloAltoFWs
```
Then build and execute the docker image:
```
docker build . -i servicetags
```
```
docker exec -d -p 80:80 servicetags
```

This will now be running on your host system. To confirm open a web browser and visit the IP address of the host computer. You should now have access to a webpage hosting all of the text files containing the IPs for ingestion as External Dynamic Lists.

### Configuring the External Dynamic Lists on Palo Alto Networks VM-Series firewalls.

Visit the webpage and select the relevant text file containing the service that you require then copy the URL. 

For **Storage** in the **UK South** region the URL will look similar to this:
```
http://linux/Storage.UKSouth.txt
```

## Support Policy
This solution is released under an as-is, best effort, support policy. These scripts should be seen as community supported. We do not provide technical support or help in using or troubleshooting the components of the project through our normal support options.

## Source
Steve Alsop

## Option 2 

# Azure IP Ranges Script

This script is a utility for retrieving and formatting the IP ranges of Microsoft Azure services. 
The original JSON file is downloaded from: https://www.microsoft.com/en-us/download/details.aspx?id=56519

## Last update
This repository contains the IPs from "ServiceTags_Public_20230710.json"

## Description

The script begins by downloading a JSON file from Microsoft's download page that contains the latest IP ranges for all Azure services. It then parses the JSON file to extract the unique system services and their associated IP addresses.

The IP addresses for each service are written to separate text files named after the service. These files are stored in the `ranges-services-pa` directory within the `output` directory. The format of these files is compatible with Palo Alto Networks (PA) devices.

## Disclaimer

This repository and its contents are provided "AS IS" without warranty of any kind, either express or implied, including, but not limited to, the implied warranties of merchantability, fitness for a particular purpose, or non-infringement. The creator and contributors to this repository are not responsible for any damages, including any general, special, incidental, or consequential damages, that may occur out of the use or inability to use these scripts or associated files.

## Recommendations

For organizations and individuals wishing to use the script in this repository, it is highly recommended to fork the repository. This is to ensure that you have control over any updates and can modify the script according to your specific needs.

The script parses a JSON file that Microsoft updates on a weekly basis. To ensure that you have the most recent IP address ranges, it is recommended that you download the new JSON file every week and perform the necessary changes at your site to correctly identify services running in Azure.

These service tags can also be used to simplify the Network Security Group rules for your Azure deployments though some service tags might not be available in all clouds and regions. For more information, please visit the [Azure Service Tags](http://aka.ms/servicetags) page.



