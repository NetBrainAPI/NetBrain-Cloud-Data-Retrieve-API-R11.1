# Table of Contents
- [Introduction](#introduction)
- [API Definition](#api_def)
    - [Input Parameters](#input)
    - [Output](#output)
- [Special Notes](#special-notes)
    - [Google Virtual Private Cloud](#vpc)
    - [Unsupported Virtual Node](#unsupported-virtual-node)
- [Sample](#sample)
    - [Sample 1: Get Resource Metrics of Google VPC Network Instances Per Peering Group Usage](#sample1)
    - [Sample 2: Get Resource Metrics of Google VPN Gateway Connections](#sample2)
    - [Sample 3: Get Resource Metrics of Google Cloud Router Sent Routes Count](#sample3)
    - [Sample 4: Get Resource Metrics of Google HTTP Load Balance  Total Latencies](#sample4)
    - [Sample 5: Get Resource Metrics of Google Cloud NAT New Connections Count](#sample5)
    - [Sample 6: Get Resource Metrics of Google Partner Interconnect Network Attachment Capacity](#sample6)
    - [Sample 7: Get Resource Metrics of Google Private Service Connect Endpoint](#sample7)
    - [Sample 8: Get Resource Metrics of Google Firewall Subnet Firewall Hit Count](#sample8)
    - [Sample 9: Get Get Resource Multiple Metrics of Google VPC Network Instances Per Peering Group Usage and Limit](#sample9)


# Introduction <a id="introduction"></a>
The `GetMonitorMetrics` function is a static method defined in the `NBGCPAPILibrary` class. It leverages the Google Monitor solution to fetch metrics of Google resources via the Google RESTful API.
For a complete list of available metrics for each Google resource, please reference to Google document: https://cloud.google.com/monitoring/api/metrics

# API Definition <a id="api_def"></a>
```python
class NBGCPAPILibrary:
    @staticmethod
    def GetMonitorMetrics(
            api_server_id: str,
            proj_id: str,
            url_params: dict,
            api_version: str = 'v3'
    ) -> object:
        # implementation
        # ...
```

## Input Parameters <a id="input"></a>
 - `api_server_id`(str) - The external API server used to discover this GCP resource.
 - `proj_id`(str) - The project ID of the GCP resource belonging to. 
 - `url_params`(dic) - A dictionary, containing additional URL parameters like filter and interval, used when calling the Google monitor metrics API. For a complete list of available metrics for each Google resource, please reference to the document: https://cloud.google.com/monitoring/api/ref_v3/rest/v3/projects.timeSeries/list
     - <details><summary>e.g.:</summary>

        ```json5
        {
            'filter':  {
                'metric.type' : 'router.googleapis.com/bgp/received_routes_count',
                'metric.labels.bgp_peer_name' : 'auto-ia-bgp-gcptonetbonda-xxxx'
            },
            'interval.startTime': '2023-08-16T05:40:54Z',
            'interval.endTime': '2023-08-16T07:40:54Z'
        }
        ```
        </details>
 - `api_version[optional]`(str) - API Version of the Google Rest API. default 'v3'.
## Output <a id="output"></a>
> resp_body_json: The JSON response body of the HTTP request to the Google monitor metrics API. This is a dictionary with string keys and values.


# Special Notes <a id="special-notes"></a>

## Google Virtual Private Cloud <a id="vpc"></a>
- In NetBrain, we generate "VPC Router" for each Google Virtual Network to represent its networking entity.
- The VPC's resource id is saved in the "networkId" in the nb_node data from `RetrieveData` method.
- For the usage please check samples below.

## Unsupported Virtual Nodes <a id="unsupported-virtual-node"></a>
- There are some virual nodes building in NetBrain structure, which might not have metrics data:
  - Google Internet Gateway
  - Google Global Internet Gateway

# Samples <a id="sample"></a>
**Filtering Metrics**

To filter metrics, you will need to modify the following variables:

- `FILTER_METRIC_TYPE_PREFIX`
- `FILTER_METRIC_TYPE`

These variables control the metric types that are included in your API requests. You can find a comprehensive list of available metrics for various Google Cloud Platform (GCP) resources by referring to the [Metrics list](https://cloud.google.com/monitoring/api/metrics). This list will provide you with the necessary metric types to use for your filtering requirements.

**Filtering by Resource**

To retrieve specific resource metrics, you can use label-based filtering. This involves utilizing labels to narrow down your search for metrics associated with a particular resource. For more information on the available monitored resource types and their labels, please consult the [Monitored resource types](https://cloud.google.com/monitoring/api/resources) documentation.

By effectively using these labels, you can tailor your API requests to obtain the precise metric data you require for your GCP resources.

## Sample 1: Get Resource Metrics of Google VPC Network Instances Per Peering Group Usage <a id="sample1"></a>
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
GCP_RESOURCE_ID_KEY = 'networkId'
GCP_RESOURCE_NAME_KEY = 'gcp_name'
GCP_RESOURCE_SELF_LINK_KEY = 'selfLink'

# Time Range for the GCP Monitor statistics
END_TIME = datetime.now(timezone.utc)
START_TIME = END_TIME - timedelta(hours=24)

# Metrics
#   to get a complete list, please ref to: https://cloud.google.com/monitoring/api/metrics_gcp
FILTER_METRIC_TYPE_PREFIX = 'compute.googleapis.com/'
FILTER_METRIC_TYPE = 'quota/instances_per_peering_group/usage'
"""
End - Define User Parameters 
"""


def BuildParameters(context, device_name, params):
    response = GetDeviceProperties(
        context,
        device_name,
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
    proj_id = NBGCPAPILibrary.GetProjectIdByResource(nb_node)

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
    # Get resource info that is used to send RestAPI to the GCP Monitor Service
    resource_info = GetResourceInfoFromDataModel(params)

    # Setup url_params
    url_params = {
        'filter': {
            'metric.type': FILTER_METRIC_TYPE_PREFIX + FILTER_METRIC_TYPE,
            # Returning labels to filter specific resource
            #    See full return type list, please ref: https://cloud.google.com/monitoring/api/resources
            'resource.labels.network_id': resource_info['id']
        },
        'interval.startTime': START_TIME.strftime('%Y-%m-%dT%H:%M:%SZ'),
        'interval.endTime': END_TIME.strftime('%Y-%m-%dT%H:%M:%SZ')
    }

    # Get Live Data
    data = NBGCPAPILibrary.GetMonitorMetrics(
        api_server_id=resource_info['apiServerId'],
        proj_id=resource_info['projId'],
        url_params=url_params
    )
    return json.dumps(data, indent=4, default=str)

```

## Sample 2: Get Resource Metrics of Google VPN Gateway Connections <a id="sample2"></a>
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
# The field 'key' of the corresponding data field of GCP resources in NetBrain data model
GCP_RESOURCE_ID_KEY = 'id'
GCP_RESOURCE_NAME_KEY = 'gcp_name'
GCP_RESOURCE_SELF_LINK_KEY = 'selfLink'

# Time Range for the GCP Monitor statistics
END_TIME = datetime.now(timezone.utc)
START_TIME = END_TIME - timedelta(seconds=330)

# Metrics
#   to get a complete list, please ref to: https://cloud.google.com/monitoring/api/metrics_gcp
FILTER_METRIC_TYPE_PREFIX = 'vpn.googleapis.com/'
FILTER_METRIC_TYPE = 'gateway/connections'
"""
End - Define User Parameters 
"""


def BuildParameters(context, device_name, params):
    response = GetDeviceProperties(
        context,
        device_name,
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
    proj_id = NBGCPAPILibrary.GetProjectIdByResource(nb_node)

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
    # Get resource info that is used to send RestAPI to the GCP Monitor Service
    resource_info = GetResourceInfoFromDataModel(params)

    # Setup url_params
    url_params = {
        'filter': {
            'metric.type': FILTER_METRIC_TYPE_PREFIX + FILTER_METRIC_TYPE,
            # Returning labels to filter specific resource
            #    See full return type list, please ref: https://cloud.google.com/monitoring/api/resources
            'resource.labels.gateway_id': resource_info['id']
        },
        'interval.startTime': START_TIME.strftime('%Y-%m-%dT%H:%M:%SZ'),
        'interval.endTime': END_TIME.strftime('%Y-%m-%dT%H:%M:%SZ')
    }

    # Get Live Data
    data = NBGCPAPILibrary.GetMonitorMetrics(
        api_server_id=resource_info['apiServerId'],
        proj_id=resource_info['projId'],
        url_params=url_params
    )
    return json.dumps(data, indent=4, default=str)

```
## Sample 3: Get Resource Metrics of Google Cloud Router Sent Routes Count<a id="sample3"></a>
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

# Time Range for the GCP Monitor statistics
END_TIME = datetime.now(timezone.utc)
START_TIME = END_TIME - timedelta(seconds=330)

# Metrics
#   to get a complete list, please ref to: https://cloud.google.com/monitoring/api/metrics_gcp
FILTER_METRIC_TYPE_PREFIX = 'router.googleapis.com/'
FILTER_METRIC_TYPE = 'sent_routes_count'
"""
End - Define User Parameters 
"""


def BuildParameters(context, device_name, params):
    response = GetDeviceProperties(
        context,
        device_name,
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
    proj_id = NBGCPAPILibrary.GetProjectIdByResource(nb_node)

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
    # Get resource info that is used to send RestAPI to the GCP Monitor Service
    resource_info = GetResourceInfoFromDataModel(params)

    # Setup url_params
    url_params = {
        'filter': {
            'metric.type': FILTER_METRIC_TYPE_PREFIX + FILTER_METRIC_TYPE,
            # Returning labels to filter specific resource
            #    See full return type list, please ref: https://cloud.google.com/monitoring/api/resources
            'resource.labels.router_id': resource_info['id']
        },
        'interval.startTime': START_TIME.strftime('%Y-%m-%dT%H:%M:%SZ'),
        'interval.endTime': END_TIME.strftime('%Y-%m-%dT%H:%M:%SZ')
    }

    # Get Live Data
    data = NBGCPAPILibrary.GetMonitorMetrics(
        api_server_id=resource_info['apiServerId'],
        proj_id=resource_info['projId'],
        url_params=url_params
    )
    return json.dumps(data, indent=4, default=str)

```

## Sample 4: Get Resource Metrics of Google HTTP Load Balancing Total Latencies<a id="sample4"></a>
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

# Time Range for the GCP Monitor statistics
END_TIME = datetime.now(timezone.utc)
START_TIME = END_TIME - timedelta(hours=4)

# Metrics
#   to get a complete list, please ref to: https://cloud.google.com/monitoring/api/metrics_gcp
FILTER_METRIC_TYPE_PREFIX = 'loadbalancing.googleapis.com/'
FILTER_METRIC_TYPE = 'https/total_latencies'
"""
End - Define User Parameters 
"""


def BuildParameters(context, device_name, params):
    response = GetDeviceProperties(
        context,
        device_name,
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
    proj_id = NBGCPAPILibrary.GetProjectIdByResource(nb_node)

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
    # Get resource info that is used to send RestAPI to the GCP Monitor Service
    resource_info = GetResourceInfoFromDataModel(params)

    # Get forwarding rule name
    forwarding_rule = params['params']['nbProperties']['forwardingRules'][0].split("/")[-1]

    # Setup url_params
    url_params = {
        'filter': {
            'metric.type': FILTER_METRIC_TYPE_PREFIX + FILTER_METRIC_TYPE,
            # Returning labels to filter specific resource
            #    See full return type list, please ref: https://cloud.google.com/monitoring/api/resources
            'resource.labels.forwarding_rule_name': forwarding_rule
        },
        'interval.startTime': START_TIME.strftime('%Y-%m-%dT%H:%M:%SZ'),
        'interval.endTime': END_TIME.strftime('%Y-%m-%dT%H:%M:%SZ')
    }

    # Get Live Data
    data = NBGCPAPILibrary.GetMonitorMetrics(
        api_server_id=resource_info['apiServerId'],
        proj_id=resource_info['projId'],
        url_params=url_params
    )
    return json.dumps(data, indent=4, default=str)
```


## Sample 5: Get Resource Metrics of Google Cloud NAT New Connections Count<a id="sample5"></a>
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

# Time Range for the GCP Monitor statistics
END_TIME = datetime.now(timezone.utc)
START_TIME = END_TIME - timedelta(seconds=330)

# Metrics
#   to get a complete list, please ref to: https://cloud.google.com/monitoring/api/metrics_gcp
FILTER_METRIC_TYPE_PREFIX = 'compute.googleapis.com/'
FILTER_METRIC_TYPE = 'nat/new_connections_count'
"""
End - Define User Parameters 
"""


def BuildParameters(context, device_name, params):
    response = GetDeviceProperties(
        context,
        device_name,
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
    proj_id = NBGCPAPILibrary.GetProjectIdByResource(nb_node)

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
    # Get resource info that is used to send RestAPI to the GCP Monitor Service
    resource_info = GetResourceInfoFromDataModel(params)

    # Setup url_params
    url_params = {
        'filter': {
            'metric.type': FILTER_METRIC_TYPE_PREFIX + FILTER_METRIC_TYPE,
            # Returning labels to filter specific resource
            #    See full return type list, please ref: https://cloud.google.com/monitoring/api/resources
            'metric.labels.nat_gateway_name': resource_info['name']
        },
        'interval.startTime': START_TIME.strftime('%Y-%m-%dT%H:%M:%SZ'),
        'interval.endTime': END_TIME.strftime('%Y-%m-%dT%H:%M:%SZ')
    }

    # Get Live Data
    data = NBGCPAPILibrary.GetMonitorMetrics(
        api_server_id=resource_info['apiServerId'],
        proj_id=resource_info['projId'],
        url_params=url_params
    )
    return json.dumps(data, indent=4, default=str)

```


## Sample 6: Get Resource Metrics of Google Partner Interconnect Network Attachment Capacity<a id="sample6"></a>
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

# Time Range for the GCP Monitor statistics
END_TIME = datetime.now(timezone.utc)
START_TIME = END_TIME - timedelta(seconds=330)

# Metrics
#   to get a complete list, please ref to: https://cloud.google.com/monitoring/api/metrics_gcp
FILTER_METRIC_TYPE_PREFIX = 'interconnect.googleapis.com/'
FILTER_METRIC_TYPE = 'network/attachment/capacity'
"""
End - Define User Parameters 
"""


def BuildParameters(context, device_name, params):
    response = GetDeviceProperties(
        context,
        device_name,
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
    proj_id = NBGCPAPILibrary.GetProjectIdByResource(nb_node)

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
    # Get resource info that is used to send RestAPI to the GCP Monitor Service
    resource_info = GetResourceInfoFromDataModel(params)

    # Setup url_params
    url_params = {
        'filter': {
            'metric.type': FILTER_METRIC_TYPE_PREFIX + FILTER_METRIC_TYPE,
            # Returning labels to filter specific resource
            #    See full return type list, please ref: https://cloud.google.com/monitoring/api/resources
            'resource.labels.interconnect': resource_info['name']
        },
        'interval.startTime': START_TIME.strftime('%Y-%m-%dT%H:%M:%SZ'),
        'interval.endTime': END_TIME.strftime('%Y-%m-%dT%H:%M:%SZ')
    }

    # Get Live Data
    data = NBGCPAPILibrary.GetMonitorMetrics(
        api_server_id=resource_info['apiServerId'],
        proj_id=resource_info['projId'],
        url_params=url_params
    )
    return json.dumps(data, indent=4, default=str)

```



## Sample 7: Get Resource Metrics of Google Private Service Connect Endpoint<a id="sample7"></a>
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
GCP_RESOURCE_ID_KEY = 'pscConnectionId'
GCP_RESOURCE_NAME_KEY = 'gcp_name'
GCP_RESOURCE_SELF_LINK_KEY = 'selfLink'

# Time Range for the GCP Monitor statistics
END_TIME = datetime.now(timezone.utc)
START_TIME = END_TIME - timedelta(hours=4)

# Metrics
#   to get a complete list, please ref to: https://cloud.google.com/monitoring/api/metrics_gcp
FILTER_METRIC_TYPE_PREFIX = 'compute.googleapis.com/'
FILTER_METRIC_TYPE = 'private_service_connect/consumer/closed_connections_count'
"""
End - Define User Parameters 
"""


def BuildParameters(context, device_name, params):
    response = GetDeviceProperties(
        context,
        device_name,
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
    proj_id = NBGCPAPILibrary.GetProjectIdByResource(nb_node)

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
    # Get resource info that is used to send RestAPI to the GCP Monitor Service
    resource_info = GetResourceInfoFromDataModel(params)

    # Setup url_params
    url_params = {
        'filter': {
            'metric.type': FILTER_METRIC_TYPE_PREFIX + FILTER_METRIC_TYPE,
            # Returning labels to filter specific resource
            #    See full return type list, please ref: https://cloud.google.com/monitoring/api/resources
            'resource.labels.psc_connection_id': resource_info['id']
        },
        'interval.startTime': START_TIME.strftime('%Y-%m-%dT%H:%M:%SZ'),
        'interval.endTime': END_TIME.strftime('%Y-%m-%dT%H:%M:%SZ')
    }

    # Get Live Data
    data = NBGCPAPILibrary.GetMonitorMetrics(
        api_server_id=resource_info['apiServerId'],
        proj_id=resource_info['projId'],
        url_params=url_params
    )
    return json.dumps(data, indent=4, default=str)
```


## Sample 8: Get Resource Metrics of Google Firewall Subnet Firewall Hit Count<a id="sample8"></a>
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
GCP_RESOURCE_NAME_KEY = 'networkName'
GCP_RESOURCE_SELF_LINK_KEY = 'selfLink'

# Time Range for the GCP Monitor statistics
END_TIME = datetime.now(timezone.utc)
START_TIME = END_TIME - timedelta(seconds=330)

# Metrics
#   to get a complete list, please ref to: https://cloud.google.com/monitoring/api/metrics_gcp
FILTER_METRIC_TYPE_PREFIX = 'firewallinsights.googleapis.com/'
FILTER_METRIC_TYPE = 'subnet/firewall_hit_count'
"""
End - Define User Parameters 
"""


def BuildParameters(context, device_name, params):
    response = GetDeviceProperties(
        context,
        device_name,
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
    proj_id = NBGCPAPILibrary.GetProjectIdByResource(nb_node)

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
    # Get resource info that is used to send RestAPI to the GCP Monitor Service
    resource_info = GetResourceInfoFromDataModel(params)

    # Debug Example
    # raise Exception(resource_info)

    # Setup url_params
    url_params = {
        'filter': {
            'metric.type': FILTER_METRIC_TYPE_PREFIX + FILTER_METRIC_TYPE,
            # Returning labels to filter specific resource
            #    See full return type list, please ref: https://cloud.google.com/monitoring/api/resources
            'metric.labels.network_name': resource_info['name']
        },
        'interval.startTime': START_TIME.strftime('%Y-%m-%dT%H:%M:%SZ'),
        'interval.endTime': END_TIME.strftime('%Y-%m-%dT%H:%M:%SZ')
    }

    # Get Live Data
    data = NBGCPAPILibrary.GetMonitorMetrics(
        api_server_id=resource_info['apiServerId'],
        proj_id=resource_info['projId'],
        url_params=url_params
    )
    return json.dumps(data, indent=4, default=str)

```


## Sample 9: Get Get Resource Multiple Metrics of Google VPC Network Instances Per Peering Group Usage and Limit<a id="sample9"></a>
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
GCP_RESOURCE_ID_KEY = 'networkId'
GCP_RESOURCE_NAME_KEY = 'gcp_name'
GCP_RESOURCE_SELF_LINK_KEY = 'selfLink'

# Time Range for the GCP Monitor statistics
END_TIME = datetime.now(timezone.utc)
START_TIME = END_TIME - timedelta(hours=24)

# Multi-Metrics please refer to
#   to get a complete list, please ref to: https://cloud.google.com/monitoring/api/metrics_gcp
METRICS = [
    {
        'FILTER_METRIC_TYPE_PREFIX': 'compute.googleapis.com/',
        'FILTER_METRIC_TYPE': 'quota/instances_per_peering_group/usage'
    },
    {
        'FILTER_METRIC_TYPE_PREFIX': 'compute.googleapis.com/',
        'FILTER_METRIC_TYPE': 'quota/instances_per_peering_group/limit'

    }
]
"""
End - Define User Parameters 
"""


def BuildParameters(context, device_name, params):
    response = GetDeviceProperties(
        context,
        device_name,
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
    proj_id = NBGCPAPILibrary.GetProjectIdByResource(nb_node)

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
    # Get resource info that is used to send RestAPI to the GCP Monitor Service
    resource_info = GetResourceInfoFromDataModel(params)

    result = {}
    for mtx in METRICS:
        prefix = mtx['FILTER_METRIC_TYPE_PREFIX']
        type = mtx['FILTER_METRIC_TYPE']
        url_params = {
            'filter': {
                'metric.type': prefix + type,
                # Returning labels to filter specific resource
                #    See full return type list, please ref: https://cloud.google.com/monitoring/api/resources
                'resource.labels.network_id': resource_info['id']
            },
            'interval.startTime': START_TIME.strftime('%Y-%m-%dT%H:%M:%SZ'),
            'interval.endTime': END_TIME.strftime('%Y-%m-%dT%H:%M:%SZ')
        }

        result_key = prefix + type

        result_value = NBGCPAPILibrary.GetMonitorMetrics(
            api_server_id=resource_info['apiServerId'],
            proj_id=resource_info['projId'],
            url_params=url_params
        )
        result[result_key] = result_value

    return json.dumps(result, indent=4, default=str)
```
