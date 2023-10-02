# Table of Contents
- [Introduction](#introduction)
- [API Definition](#api_def)
    - [Input Parameters](#input)
    - [Output](#output)
- [Special Notes](#special-notes)
  - [Resource Support Status](#resource-support-status)
- [Samples](#sample)
    - [Sample 1: Get Resource API Data of Google General Resource](#sample1)
    - [Sample 2: Get Resource API Data of Google Cloud NAT](#sample2)
    - [Sample 3: Get Resource API Data of Google Firewall](#sample3)
    - [Sample 4: Get Resource API Data of Google Load Balancer](#sample4)

# Introduction <a id="introduction"></a>
The `GetResourceDataByAPI` function is a static method of the `NBGCPAPILibrary` class that retrieves Google resource data via the Google REST API.


# API Definition <a id="api_def"></a>
```python
class NBGCPAPILibrary:
    @staticmethod
    def GetResourceDataByAPI(
            api_server_id: str,
            resource_uri: str,
            http_method: str = 'GET',
            url_params: dict = None,
            json_body: object = None,
    ) -> object:
        # implementation
        # ...
```

## Input Parameters <a id="input"></a>
The function takes in several arguments, including:
 - `api_server_id` (str) The external API Server ID of this technology instance. User should be able to get it in API Script context. Check Sample Google API Parser in NetBrain Parser Library for usage reference.
 - `resource_uri` (str) The resource identifier.
 - `http_method[optional]` (str) GET or POST. The default method is "GET". 
 - `url_params[optional]` (dict) API request params for GET request.
 - `json_body[optional]` (object) API request body.

## Output <a id="output"></a>
> The JSON response body of the HTTP request to the Google RESTful API, which can be found in Azure API Document (e.g. https://cloud.google.com/compute/docs/reference/rest/v1)
> This is a dictionary with string keys and values.

# Special Notes <a id="special-notes"></a>
## Resource Support Status
- Supported Resources: 
  - Google Cloud NAT (Special case)
  - Google Cloud Router
  - Google Firewall (Special case)
  - Google Load Balancer (Special case)
  - Google Private Service Connect Endpoint
  - Google Partner Interconnect
  - Google Dedicated Interconnect
  - Google VPC Router
  - Google VPN Gateway
  - Google Virtual Machine
- Unsupported Resources: 
  - Google Internet Gateway (Virtual Node)
  - Google Global Internet Gateway  (Virtual Node)



# Samples <a id="sample"></a>
## Sample 1: Get Resource API Data of Google General Resource <a id="sample1"></a>


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
from datetime import datetime, timezone, timedelta
import json

"""
Start - Define User Parameters 
"""
# The field "key" of the corresponding data field of GCP resources in NetBrain data model
GCP_RESOURCE_ID_KEY = 'id'
GCP_RESOURCE_NAME_KEY = 'gcp_name'
GCP_RESOURCE_SELF_LINK_KEY = 'selfLink'
"""
End - Define User Parameters 
"""

def BuildParameters(context, device_name, params):
    response = GetDeviceProperties(
        context, device_name,
        {
            'techName': 'Google Cloud',
            'paramType': 'SDN',
            'params': ['*']
        }
    )
    return response



def GetResourceInfoFromDataModel(params: dict) -> dict:
    """
    Get Resource Info from NetBrain Data Model
    Args:
        params(dict): dictionary of netbrain data model
    Returns:
        (dict) the Resource Info that contains
            - API Server ID
            - Project ID
            - GCP Resource ID
            - GCP Resource Name
            - GCP Resource Self Link
    """
    # Common used variables: GCP related Resource id, name, self link uri
    nb_node = params['params']

    # Setup api server id
    api_server_id = params['apiServerId']

    # Get proj_id
    proj_id = nb_node['projectId'] if 'projectId' in nb_node else None

    gcp_resource_id = nb_node[GCP_RESOURCE_ID_KEY] if GCP_RESOURCE_ID_KEY in nb_node else None
    gcp_resource_name = nb_node[GCP_RESOURCE_NAME_KEY] if GCP_RESOURCE_NAME_KEY in nb_node else None
    gcp_resource_self_link = nb_node[GCP_RESOURCE_SELF_LINK_KEY] if GCP_RESOURCE_SELF_LINK_KEY in nb_node else None

    # Unsupported Devices
    if gcp_resource_self_link == None:
        raise Exception('Error: Not support device "{}"'.format(gcp_resource_name))


    resource_info = {
        'apiServerId': api_server_id,
        'projId': proj_id,
        'id': gcp_resource_id,
        'name': gcp_resource_name,
        'selfLink': gcp_resource_self_link
    }
    return resource_info



def RetrieveData(params):
    # Get resource info that is used to send RestAPI to the GCP Monitor Service
    resource_info = GetResourceInfoFromDataModel(params)

    # Get Live Data
    data = NBGCPAPILibrary.GetResourceDataByAPI(
        api_server_id=resource_info['apiServerId'],
        resource_uri=resource_info['selfLink']
    )
    return json.dumps(data, indent=4, default=str)
```


## Sample 2: Get Resource API Data of Google Cloud NAT <a id="sample2"></a>

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
from datetime import datetime, timezone, timedelta
import json

"""
Start - Define User Parameters 
"""
# The field "key" of the corresponding data field of GCP resources in NetBrain data model
GCP_RESOURCE_ID_KEY = 'id'
GCP_RESOURCE_NAME_KEY = 'gcp_name'
GCP_RESOURCE_SELF_LINK_KEY = 'cloudRouterLink'
"""
End - Define User Parameters 
"""


def BuildParameters(context, device_name, params):
    response = GetDeviceProperties(
        context, device_name,
        {
            'techName': 'Google Cloud',
            'paramType': 'SDN',
            'params': ['*']
        }
    )
    return response



def GetResourceInfoFromDataModel(params: dict) -> dict:
    """
    Get Resource Info from NetBrain Data Model
    Args:
        params(dict): dictionary of NetBrain data model
    Returns:
        (dict) the Resource Info that contains
            - API Server ID
            - Project ID
            - GCP Resource ID
            - GCP Resource Name
            - GCP Resource Self Link
    """
    # Common used variables: GCP related Resource id, name, self link uri
    nb_node = params['params']

    # Setup api server id
    api_server_id = params['apiServerId']

    # Get proj_id
    proj_id = nb_node['projectId'] if 'projectId' in nb_node else None

    gcp_resource_id = nb_node[GCP_RESOURCE_ID_KEY] if GCP_RESOURCE_ID_KEY in nb_node else None
    gcp_resource_name = nb_node[GCP_RESOURCE_NAME_KEY] if GCP_RESOURCE_NAME_KEY in nb_node else None
    gcp_resource_self_link = nb_node[GCP_RESOURCE_SELF_LINK_KEY] if GCP_RESOURCE_SELF_LINK_KEY in nb_node else None

    # Unsupported Devices
    if gcp_resource_self_link == None:
        raise Exception('Error: Not support device "{}"'.format(gcp_resource_name))


    resource_info = {
        'apiServerId': api_server_id,
        'projId': proj_id,
        'id': gcp_resource_id,
        'name': gcp_resource_name,
        'selfLink': gcp_resource_self_link
    }
    return resource_info



def RetrieveData(params):
    # Get resource info that is used to send RestAPI
    resource_info = GetResourceInfoFromDataModel(params)

    # Get Live Data
    data = NBGCPAPILibrary.GetResourceDataByAPI(
        api_server_id=resource_info['apiServerId'],
        resource_uri=resource_info['selfLink']
    )
    return json.dumps(data, indent=4, default=str)

```

## Sample 3: Get Resource API Data of Google Firewall <a id="sample3"></a>

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
from datetime import datetime, timezone, timedelta
import json

"""
Start - Define User Parameters 
"""
# The field "key" of the corresponding data field of GCP resources in NetBrain data model
GCP_RESOURCE_ID_KEY = 'id'
GCP_RESOURCE_NAME_KEY = 'gcp_name'
GCP_RESOURCE_SELF_LINK_KEY = 'selfLink'
"""
End - Define User Parameters 
"""


def BuildParameters(context, device_name, params):
    response = GetDeviceProperties(
        context, device_name,
        {
            'techName': 'Google Cloud',
            'paramType': 'SDN',
            'params': ['*']
        }
    )
    return response



def GetResourceInfoFromDataModel(params: dict) -> dict:
    """
    Get Resource Info from NetBrain Data Model
    Args:
        params(dict): dictionary of netbrain data model
    Returns:
        (dict) the Resource Info that contains
            - API Server ID
            - Project ID
            - GCP Resource ID
            - GCP Resource Name
            - GCP Resource Self Link
    """
    # Common used variables: GCP related Resource id, name, self link uri
    nb_node = params['params']

    # Setup api server id
    api_server_id = params['apiServerId']

    # Get proj_id
    proj_id = nb_node['projectId'] if 'projectId' in nb_node else None

    gcp_resource_id = nb_node[GCP_RESOURCE_ID_KEY] if GCP_RESOURCE_ID_KEY in nb_node else None
    gcp_resource_name = nb_node[GCP_RESOURCE_NAME_KEY] if GCP_RESOURCE_NAME_KEY in nb_node else None
    gcp_resource_self_link = nb_node[GCP_RESOURCE_SELF_LINK_KEY] if GCP_RESOURCE_SELF_LINK_KEY in nb_node else None

    resource_info = {
        'apiServerId': api_server_id,
        'projId': proj_id,
        'id': gcp_resource_id,
        'name': gcp_resource_name,
        'selfLink': gcp_resource_self_link,
        'networkName': nb_node['networkName']
    }
    return resource_info



def RetrieveData(params):
    # Get resource info that is used to send RestAPI to the GCP Monitor Service
    resource_info = GetResourceInfoFromDataModel(params)


    # Get Uri of Firewall
    project_id = resource_info['projId']
    network_name = resource_info['networkName']
    url = f'https://compute.googleapis.com/compute/v1/projects/{project_id}/global/firewalls'
    url_params = {
        # Doc: https://cloud.google.com/compute/docs/reference/rest/v1/firewalls/list
        #   e.g.filter = 'network="https://www.googleapis.com/compute/v1/projects/norse-fragment-296509/global/networks/asia-vpc-spoke4"'
        'filter': f'(network="https://www.googleapis.com/compute/v1/projects/{project_id}/global/networks/{network_name}")'
    }


    # Get Live Data
    data = NBGCPAPILibrary.GetResourceDataByAPI(
        api_server_id=resource_info['apiServerId'],
        resource_uri=url,
        url_params=url_params
    )
    return json.dumps(data, indent=4, default=str)
```

## Sample 4: Get Resource API Data of Google Load Balancer <a id="sample4"></a>

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
from datetime import datetime, timezone, timedelta
import json

"""
Start - Define User Parameters 
"""
# The field "key" of the corresponding data field of GCP resources in NetBrain data model
GCP_RESOURCE_ID_KEY = 'id'
GCP_RESOURCE_NAME_KEY = 'gcp_name'
GCP_RESOURCE_SELF_LINK_KEY = 'selfLink'
"""
End - Define User Parameters 
"""


def BuildParameters(context, device_name, params):
    response = GetDeviceProperties(
        context, device_name,
        {
            'techName': 'Google Cloud',
            'paramType': 'SDN',
            'params': ['*']
        }
    )
    return response



def GetResourceInfoFromDataModel(params: dict) -> dict:
    """
    Get Resource Info from NetBrain Data Model
    Args:
        params(dict): dictionary of NetBrain data model
    Returns:
        (dict) the Resource Info that contains
            - API Server ID
            - Project ID
            - GCP Resource ID
            - GCP Resource Name
            - GCP Resource Self Link
    """
    # Common used variables: GCP related Resource id, name, self link uri
    nb_node = params['params']

    # Setup api server id
    api_server_id = params['apiServerId']

    # Get proj_id
    proj_id = nb_node['projectId'] if 'projectId' in nb_node else None

    gcp_resource_id = nb_node[GCP_RESOURCE_ID_KEY] if GCP_RESOURCE_ID_KEY in nb_node else None
    gcp_resource_name = nb_node[GCP_RESOURCE_NAME_KEY] if GCP_RESOURCE_NAME_KEY in nb_node else None
    gcp_resource_self_link = nb_node[GCP_RESOURCE_SELF_LINK_KEY] if GCP_RESOURCE_SELF_LINK_KEY in nb_node else None

    resource_info = {
        'apiServerId': api_server_id,
        'projId': proj_id,
        'id': gcp_resource_id,
        'name': gcp_resource_name,
        'selfLink': gcp_resource_self_link
    }
    return resource_info



def RetrieveData(params):
    # Get resource info that is used to send RestAPI
    resource_info = GetResourceInfoFromDataModel(params)

    # Get resource_uri of ldb
    resource_uri = params['params']['nbProperties']['forwardingRules'][0]

    # Get Live Data
    data = NBGCPAPILibrary.GetResourceDataByAPI(
        api_server_id=resource_info['apiServerId'],
        resource_uri=resource_uri
    )

    # Add details of backendService for internal-tcp-ldb
    if 'backendService' in data:
        backend_url = data['backendService']
        backendService = NBGCPAPILibrary.GetResourceDataByAPI(
            api_server_id=resource_info['apiServerId'],
            resource_uri=backend_url
        )
        data['backendService'] = backendService

    return json.dumps(data, indent=4, default=str)
```