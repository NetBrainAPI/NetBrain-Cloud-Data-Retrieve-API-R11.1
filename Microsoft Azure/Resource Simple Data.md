# Table of Contents
- [Introduction](#introduction)
- [API Definition](#api_def)
    - [Input Parameters](#input)
    - [Output](#output)    
- [Special Notes](#special_notes)
    - [ExpressRoute Circuit](#circuit)
    - [Virtual Network](#vnet)
- [Samples](#sample)
    - [Sample 1 -- Get Resource API Data with HTTP GET Method](#sample_1) 
    - [Sample 2 -- Get Virtual Network Gateway BGP Peer Status with HTTP POST Method](#sample_2) 
    - [Sample 3 -- Get ExpressRoute Circuit API Data](#sample_3)
    - [Sample 4 -- Get ExpressRoute Circuit Route Table](#sample_4)
    - [Sample 5 -- Get Virtual Network API Data](#sample_5)
- [Default Azure API Versions](#default_api_version)


# Introduction <a name="introduction"></a>
The `GetResourceDataByAPI` function is a static method of the `NBAzureAPILibrary` class that retrieves Azure resource data via the Azure Management REST API. It supports both GET and POST methods to download resource data, where GET is used to download the resource data, and POST is used to download large or complex data sets asynchronously.

# API Definition <a name="api_def"></a>
```python
class NBAzureAPILibrary:
    @staticmethod
    def GetResourceDataByAPI(
        api_server_id: str,
        azure_resource_uri: str,
        action: str = None,
        http_method: str = 'GET',
        json_body: object = None,
        api_version: str = ''
    ) -> object:
    # implementation
    # ...
```

## Input Parameters <a name="input"></a>
The function takes in several arguments, including:
 - `api_server_id` (str) The external API Server ID of this technology instance. User should be able to get it in API Script context. Check Sample Azure API Parser in NetBrain Parser Library for usage reference.
 - `azure_resource_uri` (str) e.g. The resource identifier, e.g. /{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/virtualNetworks/{virtualNetworkName}
 - `action[optional]` (str) in case that you want to download some specific data from the resource. e.g. pass in "getLearnedRoutes" when you want to download vnet gateway learned routes. Ref: https://learn.microsoft.com/en-us/rest/api/network-gateway/virtual-network-gateways/get-learned-routes?tabs=HTTP
 - `http_method[optional]` (str) GET or POST. The default method is "GET". Note that async methods like downloading large data set would need a POST method. (e.g. download vnet gateway learned routes, vnic effective routes, etc.) please check Microsoft Azure API document for reference.
 - `json_body[optional]` (object) API request body
 - `api_version[optional]` (str) API Version of the Azure Rest API, e.g. '2022-09-01', which can be found in Azure API document. If not provided, then a default API version is assigned for each resource provider. The default mapping can be found at the end of this document [#here](#default_api_version).

## Output <a name="output"></a>
> The JSON response body of the HTTP request to the Azure RESTful API, which can be found in Azure API Document (e.g. https://learn.microsoft.com/en-us/rest/api/network-gateway/virtual-network-gateways/get-learned-routes?tabs=HTTP)
> This is a dictionary with string keys and values.

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
## Sample 1 -- Get Resource API Data with HTTP GET Method <a name="sample_1"></a>
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

def BuildParameters(context, device_name, params):
    nb_node = GetDeviceProperties(
        context,
        device_name,
        {'techName': 'Microsoft Azure', 'paramType': 'SDN', 'params': ['*']}
    )
    return nb_node

def RetrieveData(params):
    nb_node = params['params']
    data = NBAzureAPILibrary.GetResourceDataByAPI(
        api_server_id=params['apiServerId'],
        azure_resource_uri=nb_node['id']
    )
    return json.dumps(data, indent=4)
```

## Sample 2 -- Get Virtual Network Gateway BGP Peer Status with HTTP POST Method <a name="sample_2"></a>
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

def BuildParameters(context, device_name, params):
    nb_node = GetDeviceProperties(
        context,
        device_name,
        {'techName': 'Microsoft Azure', 'paramType': 'SDN', 'params': ['*']}
    )
    return nb_node

def RetrieveData(params):
    nb_node = params['params']
    data = NBAzureAPILibrary.GetResourceDataByAPI(
        api_server_id=params['apiServerId'],
        azure_resource_uri=nb_node['id'],
        action='getBgpPeerStatus',
        http_method='POST'
    )
    return json.dumps(data, indent=4)
```

## Sample 3 -- Get ExpressRoute Circuit API Data  <a name="sample_3"></a>
```python
'''
Begin Declare Input Parameters
[
]
End Declare
'''
 
def BuildParameters(context, device_name, params):
    nb_node = GetDeviceProperties(
        context,
        device_name,
        {'techName': 'Microsoft Azure', 'paramType': 'SDN', 'params': ['*']}
    )
    return nb_node
 
 
def RetrieveData(params):   
    # Note that MSEE (Microsoft Enterprise Edge) is generated from ExpressRoute Circuit.
    # One ExpressRoute Circuit generates two MSEE (Primary and Secondary).
    # The ExpressRoute Circuit resource URI is saved in the "circuitId" data field of MSEE.
    nb_msee = params['params']
    circuit_id = nb_msee['circuitId']
    
    data = NBAzureAPILibrary.GetResourceDataByAPI(
        api_server_id=params['apiServerId'],
        azure_resource_uri=circuit_id
    )
    return json.dumps(data, indent=4)
```

## Sample 4 -- Get ExpressRoute Circuit Route Table  <a name="sample_4"></a>
```python
'''
Begin Declare Input Parameters
[
]
End Declare
'''
 
def BuildParameters(context, device_name, params):
    nb_node = GetDeviceProperties(
        context,
        device_name,
        {'techName': 'Microsoft Azure', 'paramType': 'SDN', 'params': ['*']}
    )
    return nb_node
 
 
def RetrieveData(params):   
    # Note that MSEE (Microsoft Enterprise Edge) is generated from ExpressRoute Circuit.
    # One ExpressRoute Circuit generates two MSEE (Primary and Secondary).
    # The ExpressRoute Circuit resource URI is saved in the "circuitId" data field of MSEE.
    nb_msee = params['params']
    circuit_id = nb_msee['circuitId']
    
    # get Expressroute Circuit data
    circuit_api_data = NBAzureAPILibrary.GetResourceDataByAPI(
        api_server_id=params['apiServerId'],
        azure_resource_uri=circuit_id
    )
    
    results = []
    
    # formulate route table id
    # noted that route table is for each peering
    # ref: https://learn.microsoft.com/en-us/rest/api/expressroute/express-route-circuits/list-routes-table?tabs=HTTP
    device_path = 'Primary'
    if 'properties' in circuit_api_data and 'peerings' in circuit_api_data['properties'] and circuit_api_data['properties']['peerings']:
        for peering in circuit_api_data['properties']['peerings']:
            peering_name = peering['name']
            peering_id = f"{circuit_id}/peerings/{peering_name}"
            table_id = f"{peering_id}/routeTables/{device_path}"
            peering_api_data = NBAzureAPILibrary.GetResourceDataByAPI(
                api_server_id=params['apiServerId'],
                azure_resource_uri=table_id,
                http_method='POST'
            )
            if peering_api_data:
                results.append({
                    'peeringName': peering_name,
                    'peeringId': peering['id'],
                    'routeTable': peering_api_data
                }) 
    
    return json.dumps(results, indent=4)
```

## Sample 5 -- Get Virtual Network API Data  <a name="sample_5"></a>
```python
'''
Begin Declare Input Parameters
[
]
End Declare
'''
 
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
    
    data = NBAzureAPILibrary.GetResourceDataByAPI(
        api_server_id=params['apiServerId'],
        azure_resource_uri=vnet_id
    )
    return json.dumps(data, indent=4)
```

# Default Azure API Versions <a name="default_api_version"></a>
Below are the default API versions for different Azure provider.
|**Azure Provider**|**Default API Version**|
|------|------|
| Microsoft.Network | 2022-09-01 |
| Microsoft.Compute | 2022-11-01 |
| Microsoft.Sql | 2021-11-01 |
| Microsoft.DBforMySQL | 2017-12-01 |
| Microsoft.DBforPostgreSQL | 2017-12-01 |
| Microsoft.DBforMariaDB | 2018-06-01 |
| Microsoft.Storage | 2022-09-01 |
| Microsoft.DocumentDB | 2022-05-01 |
