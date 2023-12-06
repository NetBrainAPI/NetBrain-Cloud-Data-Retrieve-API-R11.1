# Table of Contents
- [Introduction](#introduction)
- [Supported Devices](#devices)
- [API Definition](#definition)
    - [Input Parameters](#input)
    - [Output](#output)
    - [Exception Raises](#raises)
- [Sample](#sample)
    - [Sample 1 -- Using filter keys](#sample-1) 
    - [Sample 2 -- Using customized filters](#sample-2)
    - [Sample 3 -- Using customized function mapping](#sample-3)
    - [Sample 4 -- Using **kwargs](#sample-4)


# Introduction <a name="introduction"></a>
The `GetResourceData` function is a static method defined in the `NBAWSAPILibrary` class. It is used to retrieve data (tables) of AWS resources.


# Supported Devices <a name="devices"></a>
Below are supported device by built-in mapping function. Please use `customized_func_mapping` for more devices based AWS boto3 document.
* AWS VPC Router
* AWS Transit Gateway
* AWS Network Firewall
* AWS Network Load Balancer
* AWS Application Load Balancer
* AWS Gateway Load Balancer
* AWS DX Router

# API Definition <a name="definition"></a>
The `GetResourceData` function is used to retrieve data from AWS resources using AWS SDK methods. It takes various parameters such as params which contain the standard parameters for the AWS SDK method.
```python
class NBAWSAPILibrary:
    @staticmethod
    def GetResourceData(param, func_name, filter_keys=None, customized_filters=None, customized_func_mapping=None, **kwargs):
        """ Simulate the functionality of NCT and get AWS resource complex data (tables).
 
        Args:
            param (dict): e.g., {'apiServerId': 'b737cc5a-75a4-4663-97d6-eb2c6b576880', 'RegionName': 'ca-central-1', ...}
            func_name (string): This parameter specifies the name of the AWS function that will be called to retrieve the desired resources. E.g., 'describe_transit_gateway_route_tables'
            filter_keys (list): The filters provided in this parameter have the second highest priority. They will be used if there are no filters provided in customized_filters for the same key. E.g., ['transit-gateway-route-table-id']
            customized_filters (list): The filters provided in this parameter have the highest priority. They will override any other filters defined later in the code for the same key. E.g., [{'Name': 'transit-gateway-id', 'Values': ['tgw-0cf091f03edf14349']}]
            customized_func_mapping (dict): Specifies how to fetch resources through the context of a specific device. E.g., {'describe_transit_gateway_route_tables': {'resource_type': 'ec2', 'response_field_name': 'TransitGatewayRouteTables', 'transit-gateway-route-table-id': 'Options.AssociationDefaultRouteTableId', 'transit-gateway-id': 'TransitGatewayId'}}
            **kwargs (keyword argument): Specifies resources that this function needs to fetch. E.g., connectionId='xxxx-xxxx', virtualInterfaceId='xxxx-xxxx' 
 
        Returns:
            (object) list of objects
 
        Exception Raises:
        """
  
        # implementation
        # ...
```

## Input Parameters <a name="input"></a>
 - `param` (dict, required): A dictionary object that contains key-value pairs representing parameters to be passed to the AWS function being called. For example, `{'apiServerId': 'b737cc5a-75a4-4663-97d6-eb2c6b576880', 'RegionName': 'ca-central-1', ...}`.
 - `func_name` (string, required): A string that specifies the name of the AWS function that will be called to retrieve the desired resources. For example, `describe_transit_gateway_route_tables`.
 - `filter_keys` (list, optional, recommended): A list of strings representing keys for filters to be applied to the AWS function call. Filters provided in this parameter have the second highest priority, and will be used if there are no filters provided in `customized_filters` for the same key. [Sample 1 -- Using filter keys](#sample-1) 

    | Resource Type | Device Type | func_name | filter_keys | note |
    | --- | --- | --- | --- | --- |
    | ec2 | VPC router | [describe_route_tables](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/ec2/client/describe_route_tables.html) | vpc-id | The ID of the VPC for the route table. |
    |  |  | [describe_security_groups](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/ec2/client/describe_security_groups.html) | vpc-id | The ID of the VPC specified when the security group was created. |
    |  |  | [describe_network_acls](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/ec2/client/describe_network_acls.html) | vpc-id | The ID of the VPC for the network ACL. |
    |  |  | [describe_vpc_peering_connections](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/ec2/client/describe_vpc_peering_connections.html) | requester-vpc-info.vpc-id | The ID of the requester VPC. |
    |  |  | [describe_vpc_peering_connections](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/ec2/client/describe_vpc_peering_connections.html) | accepter-vpc-info.vpc-id | The ID of the accepter VPC. |
    |  |  | [describe_network_interfaces](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/ec2/client/describe_network_interfaces.html) | vpc-id | The ID of the VPC for the network interface. |
    |  | Transit Gateway | [describe_transit_gateway_route_tables](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/ec2/client/describe_transit_gateway_route_tables.html) | transit-gateway-route-table-id | The ID of the transit gateway route table. |
    |  |  | [describe_transit_gateway_route_tables](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/ec2/client/describe_transit_gateway_route_tables.html) | transit-gateway-id | The ID of the transit gateway. |
    |  |  | [describe_transit_gateway_attachments](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/ec2/client/describe_transit_gateway_attachments.html) | association.transit-gateway-route-table-id | The ID of the transit gateway associated route table. |
    |  |  | [describe_transit_gateway_attachments](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/ec2/client/describe_transit_gateway_attachments.html) | transit-gateway-id | The ID of the transit gateway. |
    | elbv2 | Application Load Balancer | [describe_listeners](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/elbv2/client/describe_listeners.html) | LoadBalancerArn | The Amazon Resource Name (ARN) of the load balancer. |
    |  | Network Load Balancer | [describe_listeners](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/elbv2/client/describe_listeners.html) | LoadBalancerArn | The Amazon Resource Name (ARN) of the load balancer. |
    |  | Gateway Load Balancer | [describe_listeners](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/elbv2/client/describe_listeners.html) | LoadBalancerArn | The Amazon Resource Name (ARN) of the load balancer. |
    | network-firewall | Network Firewall | [describe_firewall_policy](https://boto3.amazonaws.com/v1/documentation/api/1.26.92/reference/services/network-firewall/client/describe_firewall_policy.html) | FirewallPolicyArn | The Amazon Resource Name (ARN) of the firewall policy. |

 - `customized_filters` (list, optional): Customized_filters only supports `EC2` resource. Supposing customer want to create their own customized filters according to AWS boto3 SDK. A list of dictionaries representing customized filters to be applied to the AWS function call. Filters provided in this parameter have the highest priority and will override any other filters defined later in the code for the same key. [Sample 2 -- Using customized filters](#sample-2). Please use EC2 boto3 document as reference: https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/ec2.html

 - `customized_func_mapping` (dict, optional, not recommended): A dictionary object that specifies how to fetch resources through the context of a specific device. Each key represents the name of an AWS function, and its value is another dictionary containing the following fields: `resource_type`, `response_field_name` and so on. [Sample 3 -- Using customized function mapping](#sample-3). Please check below for format and example as well.
 
    ```python
    # Format
    {
        func_name:
        {
            'resource_type': resource_type,
            'response_field_name': response_field_name,
            filter_key1: device_property,
            filter_key2: device_property,
        }
    }
    # Example
    {
      'describe_transit_gateway_route_tables': {
        'resource_type': 'ec2',
        'response_field_name': 'TransitGatewayRouteTables',
        'transit-gateway-route-table-id': 'Options.AssociationDefaultRouteTableId',
        'transit-gateway-id': 'TransitGatewayId'
      }
    }
    ```
   - `Resource Type` (string): Refers to the type of digital asset or service that is being managed or tracked. It could be `ec2`, `elbc2` or `network-firewall`.
   - `func_name` (string): A string that specifies the name of the AWS function that will be called to retrieve the desired resources. For example, `'describe_transit_gateway_route_tables'`.
   - `response_field_name` (string): Refers to the specific attribute or property of the resource that is being accessed or modified..
   - `filter_keys` (string): A list of strings representing keys for filters to be applied to the AWS function call.
   - `device_property` (string): Refers to the specific property keys in device. The value of properties will be used filter values. For example, `'Options.AssociationDefaultRouteTableId'`
  
 - `**kwargs` (keyword argument, optional): keyword arguments used as parameters in Python method that specifies resources that this function needs to fetch through. E.g., `connectionId='xxxx-xxxx'`, `virtualInterfaceId='xxxx-xxxx'`, `DryRun=True`. Please use EC2 boto3 document as reference: https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/directconnect/client/describe_virtual_interfaces.html




## Output <a name="output"></a>
> The JSON response body of the request to the AWS SDK. This is a dictionary with string keys and values.

## Exception Raises <a name="raises"></a>
- If the function name specified in the `func_name` parameter is not found in either `built_in_func_mapping` or `customized_func_mapping`, the code raises an `Exception` with an error message.
- If a filter key specified in either `filter_keys` or `customized_filters` is not defined in the function mapping specified in `config` or `customized_func_mapping`, respectively, the code raises an `Exception` with an error message.
- If a customized filter is not in the correct format (i.e., it does not have a `Name` and `Values` field), the code raises an `Exception` with an error message.
- If the `resource_type` specified in config is not one of the supported types (`ec2`, `elbv2`, or `network-firewall`), the code raises a `Warning` with an error message. However, this warning is treated the same as an exception, and the code immediately raises an `Exception` with the same error message.


# Sample <a name="sample"></a>
## Sample 1 -- Using filter keys <a name="sample-1"></a>
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

import json
  
def BuildParameters(context, device_name, params):
    response = GetDeviceProperties(context, device_name, {'techName': 'Amazon AWS', 'paramType': 'SDN', 'params': ['*']})
    return response
      
def RetrieveData(params):
    nb_node = params['params']
    data = NBAWSAPILibrary.GetResourceData(
                param=nb_node, 
                func_name='describe_transit_gateway_route_tables', 
                filter_keys=['transit-gateway-id']
    )
    return json.dumps(data, indent=4, default=str)
 ```
 
## Sample 2 -- Using customized filters <a name="sample-2"></a>
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

import json
  
def BuildParameters(context, device_name, params):
    response = GetDeviceProperties(context, device_name, {'techName': 'Amazon AWS', 'paramType': 'SDN', 'params': ['*']})
    return response
      
def RetrieveData(params):
    nb_node = params['params']
    # customized_filters is optional   
    customized_filters = [{'Name': 'transit-gateway-id', 'Values': ['tgw-0cf091f03edf14349']}]
    data = NBAWSAPILibrary.GetResourceData(
                param=nb_node, 
                func_name='describe_transit_gateway_route_tables', 
                customized_filters=customized_filters
    )
    return json.dumps(data, indent=4, default=str)
 ```
 
## Sample 3 -- Using customized function mapping <a name="sample-3"></a>
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

import json
  
def BuildParameters(context, device_name, params):
    response = GetDeviceProperties(context, device_name, {'techName': 'Amazon AWS', 'paramType': 'SDN', 'params': ['*']})
    return response
      
def RetrieveData(params):
    nb_node = params['params']
    # customized_func_mapping is optional 
    customized_func_mapping = {
        'describe_transit_gateway_route_tables':
        {
            'resource_type': 'ec2',
            'response_field_name': 'TransitGatewayRouteTables',
            'transit-gateway-route-table-id': 'Options.AssociationDefaultRouteTableId',
            'transit-gateway-id': 'TransitGatewayId'
        }
    }
    data = NBAWSAPILibrary.GetResourceData(
                param=nb_node, 
                func_name='describe_transit_gateway_route_tables', 
                filter_keys=['transit-gateway-id'], 
                customized_func_mapping=customized_func_mapping
    )
    return json.dumps(data, indent=4, default=str)
 ```


## Sample 4 -- Using **kwargs <a name="sample4"></a>
```python
'''
Begin Declare Input Parameters
 [
    {"name": "$intf_name"}
 ]
 End Declare
 '''
import json
import datetime

def BuildParameters(context, device_name, params):
    intf_name = params['intf_name']

    response = GetDeviceProperties(context, device_name, {'techName': 'Amazon AWS', 'paramType': 'SDN', 'params': ['*']})
    dev = response['params']

    res_intf = GetInterfaceProperties(context, device_name, intf_name, {'techName': 'Amazon AWS', 'paramType': 'SDN', 'params': ['*']})
    dev.update(res_intf['params'])
    return response

def RetrieveData(params):
    nb_vif = params['params']
    vif_id = NBAWSAPILibrary.GetResourceIDFromDataModel(nb_vif)
    connection_id = nb_vif['connectionId']

    data = NBAWSAPILibrary.GetResourceData(
                param=nb_vif,
                func_name='describe_virtual_interfaces',
                connectionId=connection_id,
                virtualInterfaceId=vif_id
    )
    return json.dumps(data, indent=4, default=str)
 ```
