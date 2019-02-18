# Snips On Premise Infrastructure

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fchepeftw%2Fazure-quickstart-templates%2Fmaster%2F007-snips-on-premise-template%2FmainTemplate.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>
<a href="http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2Fchepeftw%2Fazure-quickstart-templates%2Fmaster%2F007-snips-on-premise-template%2FmainTemplate.json" target="_blank">
    <img src="http://armviz.io/visualizebutton.png"/>
</a>

## Prerequisites

- Snips Token, you can get it from https://console.snips.ai/. This will be asked as the input for this deployment. You can add it later but it is better if you have it before hand.

- The SSH key for the admin user.

- The initial type and capacity of the Virtual Machine Scale Set, this can be changed later and adapted to your needs.

## Description

This template creates different resources to support the Snips On-Premise solution.

- Storage Account
- Image
- Network Security Group
- Virtual Network
- Public IP Address
- Load Balancer
- Virtual Machine Scale Set 
- Autoscale Settings
- Vault

#### Storage Account and Image

The storage account is used as temporary space for the VMs startup logs.
The Image is the template used by the Virtual Machines to start the proper OS and services.
This comes from a generalized VHD which runs all the required software to run the Snips On-Premise solution. 

#### Load Balancer, Public IP Address, Virtual Network and Network Security Group

These are the network resources required for our solution. 
We use a [Standard Load Balancer](https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-standard-overview) which allows scalable, robust and solid load balancing.
This relies on a Fixed Public IP Address to access the Load Balancer.
On the internal side, the [load balancer](https://docs.microsoft.com/en-us/azure/templates/microsoft.network/2018-08-01/loadbalancers) relies on the Virtual Network, which provides an internal subnet with a network prefix of 10.0.0.0/24 where all instances will be connected.
As well, this Virtual Network contains the proper Network Security Group which allows traffic for SSH (port 22) and for Snips traffic (port 9000) for protocol TCP.
**Important:** The load balancer will not work if it does not have the proper security group rules.

#### Virtual Machine Scale Set, Autoscale Settings and Vault

These are the compute resources required for our solution.
We use a Virtual Machine Scale Set (VMSS), which is a set of Virtual Machines (or instances) that use a pre-defined VHD assembled with the necessary services to run the Snips On-Premise Solution.
With this VMSS, we also include a set of autoscaling settings to provide autoscalability.
These set of rules are to increase pr decrease the number of instances as the load varies.
Both rules rely on the average CPU utilization percentage, with 1 minute samples.
The first rule is applied when the CPU utilization is greater than 40% for 5 consecutive minutes.
The second rule is applied when the CPU utilization is less than 30% for 5 consecutive minutes.
These rules when triggered will take a scale action of 1 instance appropriately.
Meaning that, if the overall load of all the instances in the VMSS is greater than 40% for 5 consecutive minutes, then it will spawn a new instance to increase performance.
If this repeats, it will continue spawning new instances until it reaches a predefined maximum number of instances.
The same applies if the overall load of the VMSS decreases under 30%, then it will terminate 1 instance.
The VMSS has a predefined number of 2 instances as base line.

To allow the instances to pull the proper Snips assistant, it relies on a token.
This token can be generated from the [Snips Console](https://console.snips.ai/).
Then you can enter the token when creating the deployment in Azure or later through the Azure Vault.
To properly add you token after the deployment, keep in mind that you first have to give your user access to the vault itself.

## Useful References

- [Application Offer](https://docs.microsoft.com/en-gb/azure/marketplace/cloud-partner-portal/azure-applications/cpp-azure-app-offer)
- [Template Tutorials](https://docs.microsoft.com/en-us/azure/azure-resource-manager/)
- [Template Functions](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-template-functions)
- [Template errors](https://kvaes.wordpress.com/2016/05/02/azure-resource-manager-marketplace-image-requires-plan-information-in-the-request/)