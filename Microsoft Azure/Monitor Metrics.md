# Table of Contents
- [Introduction](#introduction)
- [API Definition](#api_def)
    - [Input Parameters](#input)
    - [Output](#output)
- [Special Notes](#special_notes)
    - [ExpressRoute Circuit](#circuit)
    - [Virtual Network](#vnet)
- [Samples](#sample)   
    - [Sample 1 -- Get General Resource Metrics](#sample_1)
    - [Sample 2 -- Get Resource Metrics of ExpressRoute Circuit](#sample_2)
    - [Sample 3 -- Get Resource Metrics of Virtual Network](#sample_3)

# Introduction <a name="introduction"></a>
The `GetMonitorMetrics` function is a static method defined in the `NBAzureAPILibrary` class. It leverages the Azure Monitor solution to fetch metrics of Azure resources via the Azure RESTful API.
For a complete list of available metrics for each Azure resource, please reference to Microsoft document: https://learn.microsoft.com/en-us/azure/azure-monitor/essentials/metrics-supported

# API Definition <a name="api_def"></a>
```python
class NBAzureAPILibrary:
    @staticmethod
    def GetMonitorMetrics(
        api_server_id: str,
        azure_resource_uri: str,
        params: object = None,
        api_version: str = '2018-01-01'
    ) -> object:
        # implementation
        # ...
```

## Input Parameters <a name="input"></a>
 - `api_server_id`(str) - The Azure Tenant API Server Instance ID saved in Device.
 - `azure_resource_uri`(str) - The resource identifier for the Azure resource whose metrics are to be fetched.
 - `params`(dic) - A dictionary containing additional URL parameters to use when calling the Azure monitor metrics API. For a complete list of available metrics for each Azure resource, please reference to Microsoft document: https://learn.microsoft.com/en-us/azure/azure-monitor/essentials/metrics-supported .
 - `api_version[optional]`(str) - The API version to use for the Azure monitor metrics API.

## Output <a name="output"></a>
> resp_body_json: The JSON response body of the HTTP request to the Azure monitor metrics API. This is a dictionary with string keys and values.

# Special Notes <a name="special_notes"></a>
## ExpressRoute Circuit <a name="circuit"></a>
 - In NetBrain, we generate two MSEE (Microsoft Enterprise Edge) devices (Primary and Secondary) for each Azure ExpressRoute Circuit. 
 - The Azure ExpressRoute Circuit resource URI is saved in the "circuitId" data field of MSEE.
 - For the usage please check samples below.

## Virtual Network <a name="vnet"></a>
 - In NetBrain, we generate an "Azure VNet Distributed Router" for each Azure Virtual Network to represent its networking entity.
 - The Azure Virtual Network's resource URI is saved in the "vNetId" data field of the Azure VNet Distributed Router.
 - For the usage please check samples below.

# Samples <a name="sample"></a>
## Sample 1 -- Get General Resource Metrics  <a name="sample_1"></a>
```python
'''
Begin Declare Input Parameters
[
]
End Declare

For sample
[
    {"name": "$param1"},
    {"name": "$param2"}
]
'''

metric_name = 'ExpressRouteGatewayCpuUtilization'  # metric name

def BuildParameters(context, device_name, params):
    nb_node = GetDeviceProperties(
        context, 
        device_name,
        {'techName': 'Microsoft Azure', 'paramType': 'SDN', 'params' : ['*']}
    )
    return nb_node

def RetrieveData(params):
    data = NBAzureAPILibrary.GetMonitorMetrics(
        api_server_id=params['apiServerId'],
        azure_resource_uri=params['params']['id'],
        params={'metricnames': metric_name}
    )
    return json.dumps(data, indent=4)
 ```

## Sample 2 -- Get Resource Metrics of ExpressRoute Circuit <a name="sample_2"></a>
```python
'''
Begin Declare Input Parameters
[
]
End Declare

For sample
[
    {"name": "$param1"},
    {"name": "$param2"}
]
'''

metric_name = 'BgpAvailability'  # metric name

def BuildParameters(context, device_name, params):
    nb_node = GetDeviceProperties(
        context, 
        device_name,
        {'techName': 'Microsoft Azure', 'paramType': 'SDN', 'params' : ['*']}
    )
    return nb_node

def RetrieveData(params):
    # Note that MSEE (Microsoft Enterprise Edge) is generated from ExpressRoute Circuit.
    # One ExpressRoute Circuit generates two MSEE (Primary and Secondary).
    # The ExpressRoute Circuit resource URI is saved in the "circuitId" data field of MSEE.
    nb_msee = params['params']
    circuit_id = nb_msee['circuitId']
    
    data = NBAzureAPILibrary.GetMonitorMetrics(
        api_server_id=params['apiServerId'],
        azure_resource_uri=circuit_id,
        params={'metricnames': metric_name}
    )
    return json.dumps(data, indent=4)
```

## Sample 3 -- Get Resource Metrics of Virtual Network  <a name="sample_3"></a>
```python
'''
Begin Declare Input Parameters
[
]
End Declare
'''

metric_name = 'BytesInDDoS'  # metric name

def BuildParameters(context, device_name, params):
    nb_node = GetDeviceProperties(
        context,
        device_name,
        {'techName': 'Microsoft Azure', 'paramType': 'SDN', 'params': ['*']}
    )
    return nb_node
 
 
def RetrieveData(params):   
    # Note that the VNet Router (Virtual Network Distributed Router) is generated from Azure Virtual Network.
    # The purpose is to represent vnet's networking entity.
    # The VNet's resource URI is saved in the "vNetId" data field of the VNet Router.
    nb_vnet_router = params['params']
    vnet_id = nb_vnet_router['vNetId']
    
    data = NBAzureAPILibrary.GetMonitorMetrics(
        api_server_id=params['apiServerId'],
        azure_resource_uri=vnet_id,
        params={'metricnames': metric_name}
    )
    return json.dumps(data, indent=4)
```
