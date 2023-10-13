# Table of Contents
- [Introduction](#introduction)
- [API Definition](#api-definition)
  - [Input Parameters](#input-parameters)
  - [Output](#output)
  - [Sample](#sample)
- [Supported Devices](#supported-devices)
  - [Azure Virtual Network Gateway](#azure-virtual-network-gateway)    
  - [Azure Virtual Machine](#azure-virtual-machine)    
  - [Azure MSEE](#azure-msee)    
  - [Azure Firewall](#azure-firewall)    
  - [More Coming Soon...](#more-coming-soon)
  
# Introduction
The `GetResourceData` function is a static method defined in the `NBAzureAPILibrary` class. It is used to retrieve various types of data of Azure resources.<br />
For the available data types of each Azure resource, please check [Supported Devices](#supported-devices).

# API Definition
```python
class NBAzureAPILibrary:
    @staticmethod
    def GetResourceData(api_server_id: str,
                        nb_resource_data: object,
                        data_type: str
                        ) -> object:
    # implementation
        # ...
```

## Input Parameters
 - `api_server_id` (str) The external API Server ID of this technology instance. User should be able to get it in API Script context. For details please check [Sample](#sample).
 - `nb_resource_data` (object) The entire resource data model in NetBrain. It is retrieved by calling the `GetDeviceProperties` API method, passing in the device name and ['*'] for the parameter. For details please check [Sample](#sample).
 - `data_type` (str) - The specific data type of the current Azure resource. For the available data types of each Azure resource, please check [Supported Devices](#supported-devices).

## Output
> The JSON response body of the requested API. For the response data structure of each data type, please check [Supported Devices](#supported-devices).

## Sample

```python
'''
Begin Declare Input Parameters
[
]
End Declare
'''
  
def BuildParameters(context, device_name, params):
    response = GetDeviceProperties(
        context,
        device_name,
        {'techName': 'Microsoft Azure', 'paramType': 'SDN', 'params' : ['*'] }
    )
    return response
      
def RetrieveData(params):   
    nb_node = params['params']  
    data = NBAzureAPILibrary.GetResourceData(
        api_server_id=params['apiServerId'],
        nb_resource_data=nb_node,
        data_type='connection'
    )
      
    return json.dumps(data, indent=4)
 ```

<br />

# Supported Devices
- [Azure Virtual Network Gateway](#azure-virtual-network-gateway)  
- [Azure Virtual Machine](#azure-virtual-machine)  
- [Azure MSEE](#azure-msee)  
- [Azure Firewall](#azure-firewall)  
- [More Coming Soon...](#more-coming-soon)

## Azure Virtual Network Gateway
- [Type "connection"](#type-connection)
  - [Introduction](#introduction-1)
  - [Content](#content)
  - [Sample](#sample-1)
### Type "connection"
#### Introduction
Passing-in the keyword "**connection**" for the param `data_type`, the API returns the Azure's original API response of "Virtual Network Gateway - Connections", with the details of some associated resources, e.g. Local Network Gateway.


#### Content
Below are the Azure APIs used to generate this configuration.
|**Resource/Action**|**Relationship**|**Azure API Version**|**Azure API document**|
|------|------|------|------|
| Virtual Network Gateways - List Connections | self | 2022-09-01 | https://learn.microsoft.com/en-us/rest/api/network-gateway/virtual-network-gateways/list-connections?tabs=HTTP | 
| Virtual Network Gateway Connections - Get | self | 2022-09-01 | https://learn.microsoft.com/en-us/rest/api/network-gateway/virtual-network-gateway-connections/get?tabs=HTTP | 
| Local Network Groups - Get | properties.localNetworkGateway | 2022-09-01 | https://learn.microsoft.com/en-us/rest/api/network-gateway/local-network-gateways/get?tabs=HTTP | 

#### Sample
<details>
<summary>API Response</summary>

```json
[
    {
        "name": "connS2S",
        "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/connections/connS2S",
        "etag": "W/\"00000000-0000-0000-0000-000000000000\"",
        "type": "Microsoft.Network/connections",
        "location": "centralus",
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "00000000-0000-0000-0000-000000000000",
            "virtualNetworkGateway1": {
                "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/virtualNetworkGateways/vpngw",
                "properties": {}
            },
            "localNetworkGateway2": {
                "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/localNetworkGateways/localgw",
                "properties": {}
            },
            // Local Network Gateway: https://learn.microsoft.com/en-us/rest/api/network-gateway/local-network-gateways/get?tabs=HTTP
            "localNetworkGateway": {
                "name": "localgw",
                "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/localNetworkGateways/localgw",
                "etag": "W/\"00000000-0000-0000-0000-000000000000\"",
                "type": "Microsoft.Network/localNetworkGateways",
                "location": "centralus",
                "properties": {
                    "provisioningState": "Succeeded",
                    "resourceGuid": "00000000-0000-0000-0000-000000000000",
                    "localNetworkAddressSpace": {
                        "addressPrefixes": [
                            "10.1.0.0/16"
                        ]
                    },
                    "gatewayIpAddress": "x.x.x.x"
                }
            },
            "ingressNatRules": [
                {
                    "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/virtualNetworkGateways/vpngw/natRules/natRule1"
                }
            ],
            "egressNatRules": [
                {
                    "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/virtualNetworkGateways/vpngw/natRules/natRule2"
                }
            ],
            "connectionType": "IPsec",
            "connectionProtocol": "IKEv2",
            "routingWeight": 0,
            "dpdTimeoutSeconds": 30,
            "sharedKey": "Abc123",
            "enableBgp": false,
            "gatewayCustomBgpIpAddresses": [
                {
                    "ipConfigurationId": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/virtualNetworkGateways/vpngw/ipConfigurations/default",
                    "customBgpIpAddress": "169.254.21.1"
                },
                {
                    "ipConfigurationId": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/virtualNetworkGateways/vpngw/ipConfigurations/ActiveActive",
                    "customBgpIpAddress": "169.254.21.3"
                }
            ],
            "useLocalAzureIpAddress": false,
            "usePolicyBasedTrafficSelectors": false,
            "ipsecPolicies": [],
            "trafficSelectorPolicies": [],
            "connectionStatus": "Connecting",
            "ingressBytesTransferred": 0,
            "egressBytesTransferred": 0,
            "connectionMode": "Default"
        }
    }
]
```
</details>
<br />


## Azure Virtual Machine
- [Type "configuration"](#type-configuration)
  - [Introduction](#introduction-2)
  - [Content](#content-1)
  - [Sample](#sample-2)
- [Type "run_time_state"](#type-run_time_state)
  - [Introduction](#introduction-3)
  - [Content](#content-2)
  - [Sample](#sample-3)
### Type "configuration"
#### Introduction
Passing-in the keyword "**configuration**" for the param `data_type`, the API returns the Azure's original API response of Virtual Machine, with the details of some associated resources, including Network Interfaces, Network Security Groups.


#### Content
Below are the Azure APIs used to generate this configuration.
|**Resource/Action**|**Relationship**|**Azure API Version**|**Azure API document**|
|------|------|------|------|
| Virtual Machines - Get | self | 2022-11-01 | https://learn.microsoft.com/en-us/rest/api/compute/virtual-machines/get | 
| Network Interfaces - Get | properties.networkProfile.networkInterfaces | 2022-09-01 | https://learn.microsoft.com/en-us/rest/api/virtualnetwork/network-interfaces/get | 
| Network Security Groups - Get | properties.networkProfile.networkInterfaces.properties.networkSecurityGroup | 2022-09-01 | https://learn.microsoft.com/en-us/rest/api/virtualnetwork/network-security-groups/get | 


#### Sample
<details>
<summary>API Response</summary>

```json
{
    "name": "myVM",
    "id": "/subscriptions/{subscription-id}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
    "type": "Microsoft.Compute/virtualMachines",
    "location": "West US",
    "tags": {
        "myTag1": "tagValue1"
    },
    "properties": {
        "vmId": "0f47b100-583c-48e3-a4c0-aefc2c9bbcc1",
        "hostGroup": {
            "id": "/subscriptions/{subscription-id}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/hostGroups/myHostGroup"
        },
        "hardwareProfile": {
            "vmSize": "Standard_D2s_v3"
        },
        "storageProfile": {
            "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "2016-Datacenter",
                "version": "latest"
            },
            "osDisk": {
                "osType": "Windows",
                "name": "myOsDisk",
                "createOption": "FromImage",
                "caching": "ReadWrite",
                "managedDisk": {
                    "storageAccountType": "Premium_LRS",
                    "id": "/subscriptions/{subscription-id}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/disks/myOsDisk"
                },
                "diskSizeGB": 30
            },
            "dataDisks": []
        },
        "osProfile": {
            "computerName": "myVM",
            "adminUsername": "admin",
            "windowsConfiguration": {
                "provisionVMAgent": true,
                "enableAutomaticUpdates": false
            },
            "secrets": []
        },
        "networkProfile": {       
            // Network Interface: https://learn.microsoft.com/en-us/rest/api/virtualnetwork/network-interfaces/get?tabs=HTTP     
            "networkInterfaces": [                
                {
                    "name": "test-nic",
                    "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/networkInterfaces/test-nic",
                    "location": "eastus",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "ipConfigurations": [
                            {
                                "name": "ipconfig1",
                                "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/networkInterfaces/test-nic/ipConfigurations/ipconfig1",
                                "properties": {
                                    "provisioningState": "Succeeded",
                                    "privateIPAddress": "172.20.2.4",
                                    "privateIPAllocationMethod": "Dynamic",
                                    "publicIPAddress": {
                                        "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/publicIPAddresses/test-ip"
                                    },
                                    "subnet": {
                                        "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/virtualNetworks/rg1-vnet/subnets/default"
                                    },
                                    "primary": true,
                                    "privateIPAddressVersion": "IPv4"
                                }
                            }
                        ],
                        "dnsSettings": {
                            "dnsServers": [],
                            "appliedDnsServers": [],
                            "internalDomainNameSuffix": "test.bx.internal.cloudapp.net"
                        },
                        "macAddress": "00-0D-3A-1B-C7-21",
                        "enableAcceleratedNetworking": true,
                        "disableTcpStateTracking": true,
                        "enableIPForwarding": false,
                        // Network Security Groups: https://learn.microsoft.com/en-us/rest/api/virtualnetwork/network-security-groups/get?tabs=HTTP
                        "networkSecurityGroup": {
                            "name": "testnsg",
                            "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/networkSecurityGroups/testnsg",
                            "type": "Microsoft.Network/networkSecurityGroups",
                            "location": "westus",
                            "properties": {
                                "provisioningState": "Succeeded",
                                "securityRules": [
                                    {
                                        "name": "rule1",
                                        "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/networkSecurityGroups/testnsg/securityRules/rule1",
                                        "properties": {
                                            "provisioningState": "Succeeded",
                                            "protocol": "*",
                                            "sourcePortRange": "*",
                                            "destinationPortRange": "80",
                                            "sourceAddressPrefix": "*",
                                            "destinationAddressPrefix": "*",
                                            "access": "Allow",
                                            "priority": 130,
                                            "direction": "Inbound"
                                        }
                                    }
                                ],
                                "defaultSecurityRules": [
                                    {
                                        "name": "AllowVnetInBound",
                                        "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/networkSecurityGroups/testnsg/defaultSecurityRules/AllowVnetInBound",
                                        "properties": {
                                            "provisioningState": "Succeeded",
                                            "description": "Allow inbound traffic from all VMs in VNET",
                                            "protocol": "*",
                                            "sourcePortRange": "*",
                                            "destinationPortRange": "*",
                                            "sourceAddressPrefix": "VirtualNetwork",
                                            "destinationAddressPrefix": "VirtualNetwork",
                                            "access": "Allow",
                                            "priority": 65000,
                                            "direction": "Inbound"
                                        }
                                    },
                                    {
                                        "name": "AllowAzureLoadBalancerInBound",
                                        "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/networkSecurityGroups/testnsg/defaultSecurityRules/AllowAzureLoadBalancerInBound",
                                        "properties": {
                                            "provisioningState": "Succeeded",
                                            "description": "Allow inbound traffic from azure load balancer",
                                            "protocol": "*",
                                            "sourcePortRange": "*",
                                            "destinationPortRange": "*",
                                            "sourceAddressPrefix": "AzureLoadBalancer",
                                            "destinationAddressPrefix": "*",
                                            "access": "Allow",
                                            "priority": 65001,
                                            "direction": "Inbound"
                                        }
                                    },
                                    {
                                        "name": "DenyAllInBound",
                                        "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/networkSecurityGroups/testnsg/defaultSecurityRules/DenyAllInBound",
                                        "properties": {
                                            "provisioningState": "Succeeded",
                                            "description": "Deny all inbound traffic",
                                            "protocol": "*",
                                            "sourcePortRange": "*",
                                            "destinationPortRange": "*",
                                            "sourceAddressPrefix": "*",
                                            "destinationAddressPrefix": "*",
                                            "access": "Deny",
                                            "priority": 65500,
                                            "direction": "Inbound"
                                        }
                                    },
                                    {
                                        "name": "AllowVnetOutBound",
                                        "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/networkSecurityGroups/testnsg/defaultSecurityRules/AllowVnetOutBound",
                                        "properties": {
                                            "provisioningState": "Succeeded",
                                            "description": "Allow outbound traffic from all VMs to all VMs in VNET",
                                            "protocol": "*",
                                            "sourcePortRange": "*",
                                            "destinationPortRange": "*",
                                            "sourceAddressPrefix": "VirtualNetwork",
                                            "destinationAddressPrefix": "VirtualNetwork",
                                            "access": "Allow",
                                            "priority": 65000,
                                            "direction": "Outbound"
                                        }
                                    },
                                    {
                                        "name": "AllowInternetOutBound",
                                        "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/networkSecurityGroups/testnsg/defaultSecurityRules/AllowInternetOutBound",
                                        "properties": {
                                            "provisioningState": "Succeeded",
                                            "description": "Allow outbound traffic from all VMs to Internet",
                                            "protocol": "*",
                                            "sourcePortRange": "*",
                                            "destinationPortRange": "*",
                                            "sourceAddressPrefix": "*",
                                            "destinationAddressPrefix": "Internet",
                                            "access": "Allow",
                                            "priority": 65001,
                                            "direction": "Outbound"
                                        }
                                    },
                                    {
                                        "name": "DenyAllOutBound",
                                        "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/networkSecurityGroups/testnsg/defaultSecurityRules/DenyAllOutBound",
                                        "properties": {
                                            "provisioningState": "Succeeded",
                                            "description": "Deny all outbound traffic",
                                            "protocol": "*",
                                            "sourcePortRange": "*",
                                            "destinationPortRange": "*",
                                            "sourceAddressPrefix": "*",
                                            "destinationAddressPrefix": "*",
                                            "access": "Deny",
                                            "priority": 65500,
                                            "direction": "Outbound"
                                        }
                                    }
                                ]
                            }
                        },
                        "primary": true,
                        "virtualMachine": {
                            "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachines/vm1"
                        },
                        "dscpConfiguration": {
                            "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Compute/dscpConfiguration/mydscpconfiguration"
                        },
                        "vnetEncryptionSupported": false
                    },
                    "type": "Microsoft.Network/networkInterfaces"
                }
            ]
        },
        "provisioningState": "Succeeded"
    }
}
```

</details>
<br />

### Type "run_time_state"
#### Introduction
Passing-in the keyword "**run_time_state**" for the param `data_type`, the API returns the Azure's API response of "Virtual Machine - Instance View", which describes the run-time state of a virtual machine. The data structure of "statuses" field is modified from original response for easier usage.


#### Content
Below are the Azure APIs used to generate this configuration.
|**Resource/Action**|**Relationship**|**Azure API Version**|**Azure API document**|
|------|------|------|------|
| Virtual Machines - Get | self | 2022-11-01 | https://learn.microsoft.com/en-us/rest/api/compute/virtual-machines/instance-view?tabs=HTTP | 


#### Sample
<details>
<summary>API Response</summary>

```json
{
    "computerName": "myVM",
    "osName": "Windows Server 2016 Datacenter",
    "osVersion": "Microsoft Windows NT 10.0.14393.0",
    "vmAgent": {
        "vmAgentVersion": "2.7.41491.949",
        "statuses": {
            "ProvisioningState": {
                "code": "ProvisioningState/succeeded",
                "level": "Info",
                "displayStatus": "Ready",
                "message": "GuestAgent is running and accepting new configurations.",
                "time": "2023-07-01T23:11:22+00:00"
            }
        }
    },
    "disks": [
        {
            "name": "myOsDisk",
            "statuses": {
                "ProvisioningState": {
                    "code": "ProvisioningState/succeeded",
                    "level": "Info",
                    "displayStatus": "Provisioning succeeded",
                    "time": "2023-07-01T21:29:47.477089+00:00"
                }
            }
        }
    ],
    "hyperVGeneration": "V1",
    "assignedHost": "/subscriptions/{subscription-id}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/hostGroups/myHostGroup/hosts/myHost",
    "statuses": {
        "ProvisioningState": {
            "code": "ProvisioningState/succeeded",
            "level": "Info",
            "displayStatus": "Provisioning succeeded",
            "time": "2023-07-01T21:30:12.8051917+00:00"
        },
        "PowerState": {
            "code": "PowerState/running",
            "level": "Info",
            "displayStatus": "VM running"
        }
    }
}
```

</details>
<br />

## Azure MSEE
In NetBrain, we generate two MSEE (Microsoft Enterprise Edge) devices (Primary and Secondary) for each Azure ExpressRoute Circuit.
- [Type "route_table"](#type-route_table)
  - [Introduction](#introduction-4)
  - [Content](#content-3)
  - [Sample](#sample-4)
  
### Type "route_table"
#### Introduction
Passing-in the keyword "**route_table**" for the param `data_type`, the API returns the the route tables of each circuit peering, which describes the currently advertised routes table associated with the express route circuit.


#### Content
Below are the Azure APIs used to generate this configuration.
|**Resource/Action**|**Relationship**|**Azure API Version**|**Azure API document**|
|------|------|------|------|
| Express Route Circuits - List Routes Table | self | 2022-09-01 | https://learn.microsoft.com/en-us/rest/api/expressroute/express-route-circuits/list-routes-table | 
| Express Route Circuits - Get | To get ExpressRoute Circuit peerings.  | 2022-09-01 | https://learn.microsoft.com/en-us/rest/api/expressroute/express-route-circuits/get | 

#### Sample
<details>
<summary>API Response</summary>

```json
[
    {
        "peeringName": "AzurePrivatePeering",
        "peeringId": "/subscriptions/subId/resourceGroups/rg/providers/Microsoft.Network/expressRouteCircuits/curcuitName/peerings/AzurePrivatePeering",
        "routeTable": [
            {
                "network": "",
                "nextHop": "",
                "locPrf": "",
                "weight": 0,
                "path": ""
            }
        ]
    },
    {
        "peeringName": "MicrosoftPeering",
        "peeringId": "/subscriptions/subId/resourceGroups/rg/providers/Microsoft.Network/expressRouteCircuits/curcuitName/peerings/MicrosoftPeering",
        "routeTable": [
            {
                "network": "",
                "nextHop": "",
                "locPrf": "",
                "weight": 0,
                "path": ""
            }
        ]
    }
]
```

</details>
<br />

## Azure Firewall
- [Type "firewall_rule"](#type-firewall_rule)
  - [Introduction](#introduction-5)
  - [Content](#content-4)
  - [Sample](#sample-5)

### Type "firewall_rule"
#### Introduction
Passing-in the keyword "**firewall_rule**" for the param `data_type`, the API returns the NAT rules, network rules, and applications rules of the Azure Firewall. <br />
There are two types of firewall rules -- classic rules and firewall policy (ref: https://learn.microsoft.com/en-us/azure/firewall/rule-processing).<br />
The classic firewall rules can be found directly in the firewall's Azure API data. The other type of firewall rule can be fetched via firewall policy and its base policy's Azure API data.


#### Content
Below are the Azure APIs used to generate this configuration.
|**Resource/Action**|**Relationship**|**Azure API Version**|**Azure API document**|
|------|------|------|------|
| Azure Firewalls - Get | self | 2022-09-01 | https://learn.microsoft.com/en-us/rest/api/firewall/azure-firewalls/get | 
| Firewall Policies - Get | self | 2022-09-01 | https://learn.microsoft.com/en-us/rest/api/virtualnetwork/firewall-policies/get | 

#### Sample
<details>
<summary>API Response - Classic Firewall Rules</summary>

```json
[
  {
    "name": "networkRuleCollections",
    "properties": {
      "ruleCollections": [
        {
          "name": "netrulecoll",
          "priority": 112,
          "action": {
            "type": "Deny"
          },
          "rules": [
            {
              "name": "L4-traffic",
              "description": "Block traffic based on source IPs and ports",
              "sourceAddresses": [
                "192.168.1.1-192.168.1.12",
                "10.1.4.12-10.1.4.255"
              ],
              "destinationPorts": [
                "443-444",
                "8443"
              ],
              "destinationAddresses": [
                "*"
              ],
              "protocols": [
                "TCP"
              ]
            },
            {
              "name": "L4-traffic-with-FQDN",
              "description": "Block traffic based on source IPs and ports to amazon",
              "sourceAddresses": [
                "10.2.4.12-10.2.4.255"
              ],
              "destinationPorts": [
                "443-444",
                "8443"
              ],
              "destinationFqdns": [
                "www.amazon.com"
              ],
              "protocols": [
                "TCP"
              ]
            }
          ]
        }
      ]
    }
  },
  {
    "name": "natRuleCollections",
    "properties": {
      "ruleCollections": [
        {
          "name": "natrulecoll",
          "priority": 112,
          "action": {
            "type": "Dnat"
          },
          "rules": [
            {
              "name": "DNAT-HTTPS-traffic",
              "description": "D-NAT all outbound web traffic for inspection",
              "sourceAddresses": [
                "*"
              ],
              "destinationAddresses": [
                "1.2.3.4"
              ],
              "destinationPorts": [
                "443"
              ],
              "protocols": [
                "TCP"
              ],
              "translatedAddress": "1.2.3.5",
              "translatedPort": "8443"
            },
            {
              "name": "DNAT-HTTP-traffic-With-FQDN",
              "description": "D-NAT all inbound web traffic for inspection",
              "sourceAddresses": [
                "*"
              ],
              "destinationAddresses": [
                "1.2.3.4"
              ],
              "destinationPorts": [
                "80"
              ],
              "protocols": [
                "TCP"
              ],
              "translatedFqdn": "internalhttpserver",
              "translatedPort": "880"
            }
          ]
        }
      ]
    }
  },
  {
    "name": "applicationRuleCollections",
    "properties": {
      "ruleCollections": [
        {
          "name": "apprulecoll",
          "priority": 110,
          "action": {
            "type": "Deny"
          },
          "rules": [
            {
              "name": "rule1",
              "description": "Deny inbound rule",
              "protocols": [
                {
                  "protocolType": "Https",
                  "port": 443
                }
              ],
              "targetFqdns": [
                "www.test.com"
              ],
              "sourceAddresses": [
                "216.58.216.164",
                "10.0.0.0/24"
              ]
            }
          ]
        }
      ]
    }
  }
]
```
</details>

<details>
<summary>API Response - Firewall Rules of Firewall Policies</summary>

```json
{
    "name": "firewallPolicy",
    "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/firewallPolicies/firewallPolicy",
    "type": "Microsoft.Network/firewallPolicies",
    "etag": "w/\\00000000-0000-0000-0000-000000000000\\",
    "location": "West US",
    "tags": {
        "key1": "value1"
    },
    "properties": {
        "size": "0.5MB",
        "provisioningState": "Succeeded",
        "threatIntelMode": "Alert",
        "threatIntelWhitelist": {
            "ipAddresses": [
                "20.3.4.5"
            ],
            "fqdns": [
                "*.microsoft.com"
            ]
        },
        "ruleCollectionGroups": [
            {
                "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/firewallPolicies/firewallPolicy/ruleCollectionGroups/ruleCollectionGroup1"
            }
        ],
        "insights": {
            "isEnabled": true,
            "retentionDays": 100,
            "logAnalyticsResources": {
                "workspaces": [
                    {
                        "region": "westus",
                        "workspaceId": {
                            "id": "/subscriptions/subid/resourcegroups/rg1/providers/microsoft.operationalinsights/workspaces/workspace1"
                        }
                    },
                    {
                        "region": "eastus",
                        "workspaceId": {
                            "id": "/subscriptions/subid/resourcegroups/rg1/providers/microsoft.operationalinsights/workspaces/workspace2"
                        }
                    }
                ],
                "defaultWorkspaceId": {
                    "id": "/subscriptions/subid/resourcegroups/rg1/providers/microsoft.operationalinsights/workspaces/defaultWorkspace"
                }
            }
        },
        "firewalls": [],
        "snat": {
            "privateRanges": [
                "IANAPrivateRanges"
            ]
        },
        "sql": {
            "allowSqlRedirect": true
        },
        "dnsSettings": {
            "servers": [
                "30.3.4.5"
            ],
            "enableProxy": true,
            "requireProxyForNetworkRules": false
        },
        "explicitProxy": {
            "enableExplicitProxy": true,
            "httpPort": 8087,
            "httpsPort": 8087,
            "enablePacFile": true,
            "pacFilePort": 8087,
            "pacFile": "https://tinawstorage.file.core.windows.net/?sv=2020-02-10&ss=bfqt&srt=sco&sp=rwdlacuptfx&se=2021-06-04T07:01:12Z&st=2021-06-03T23:01:12Z&sip=68.65.171.11&spr=https&sig=Plsa0RRVpGbY0IETZZOT6znOHcSro71LLTTbzquYPgs%3D"
        },
        "sku": {
            "tier": "Premium"
        },
        "intrusionDetection": {
            "mode": "Alert",
            "configuration": {
                "signatureOverrides": [
                    {
                        "id": "2525004",
                        "mode": "Deny"
                    }
                ],
                "bypassTrafficSettings": [
                    {
                        "name": "bypassRule1",
                        "description": "Rule 1",
                        "protocol": "TCP",
                        "sourceAddresses": [
                            "1.2.3.4"
                        ],
                        "destinationAddresses": [
                            "5.6.7.8"
                        ],
                        "destinationPorts": [
                            "*"
                        ]
                    }
                ]
            }
        },
        "transportSecurity": {
            "certificateAuthority": {
                "name": "clientcert",
                "keyVaultSecretId": "https://kv/secret"
            }
        }
    }
}
```
</details>
<br />


## More Coming Soon