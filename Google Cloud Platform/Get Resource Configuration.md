# Usage
To retrieve the configuration data for a resource, you can utilize NetBrain's built-in configuration file function, which does not require coding. This function enables you to quickly obtain the resource configuration.

# Table of Contents
  - [Google VPC Router](#google-vpc-router)
  - [Google VPN Gateway](#google-vpn-gateway)
  - [Google Cloud Router](#google-cloud-router)
  - [Google Load Balancer](#google-load-balancer)
  - [Google Firewall](#google-firewall)
  - [Google Cloud NAT](#google-cloud-nat)
  - [Google Virtual Machine](#google-virtual-machine)
  - [Google Internet Gateway](#google-internet-gateway)
  - [Google Global Internet Gateway](#google-global-internet-gateway)
  - [Google Partner Interconnect](#partner-google-interconnect)
  - [Google Dedicated Interconnect](#dedicated-google-interconnect)
  - [Google Private Service Connect Endpoint](#google-private-service-connect-endpoint)

## Google VPC Router

### Introduction
The configuration of the Google VPC Router relies solely on the corresponding Google API of the VPC Network. The Google API provides detailed information regarding the configuration of the virtual network, including its subnetworks, peerings, etc.

### Content
Below are the Google APIs used to generate this configuration.
|Resource/Action|Relationship|Google API Version|Google API document|
|------|------|------|------|
| VPC Router - Get | self | v1 | https://cloud.google.com/compute/docs/reference/rest/v1/networks/get | 
| Subnetwork - Get | subnetworks | v1 | https://cloud.google.com/compute/docs/reference/rest/v1/networks/get | 

### Sample
<details><summary>Configuration File</summary>
    
```json
{
    "netbrainNotes": "This config file is generated via API",
    "netbrainHostName": "central-vpc-hub-router(<vpc_id>-router)",
    "kind": "compute#network",
    "id": "xxxxxxxxxxxxxxxxxxx",
    "creationTimestamp": "2021-04-21T14:17:20.628-07:00",
    "name": "central-vpc-hub",
    "description": "Second Host Project",
    "selfLink": "https://www.googleapis.com/compute/v1/projects/<project-id>/global/networks/central-vpc-hub",
    "selfLinkWithId": "https://www.googleapis.com/compute/v1/projects/<project-id>/global/networks/xxxxxxxxxxxxxxxxxxx",
    "autoCreateSubnetworks": false,
    # Subnetworks: https://cloud.google.com/compute/docs/reference/rest/v1/networks/get
    "subnetworks": [
{
            "kind": "compute#subnetwork",
            "id": "<subnetwork-id>",
            "creationTimestamp": "2021-11-29T07:11:15.160-08:00",
            "name": "australia-southeast-subnet1",
            "description": "australia-southeast-subnet1",
            "network": "https://www.googleapis.com/compute/v1/projects/<project-id>/global/networks/australia-vpc-spoke6",
            "ipCidrRange": "xx.xx.xx.xx/28",
            "gatewayAddress": "xx.xx.xx.xx",
            "region": "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/australia-southeast1",
            "selfLink": "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/australia-southeast1/subnetworks/australia-southeast-subnet1",
            "privateIpGoogleAccess": true,
            "secondaryIpRanges": [
                {
                    "rangeName": "range-2",
                    "ipCidrRange": "xx.xx.xx.xx/28"
                }
            ],
            "fingerprint": "xxxxxxxxxx",
            "enableFlowLogs": false,
            "privateIpv6GoogleAccess": "DISABLE_GOOGLE_ACCESS",
            "purpose": "PRIVATE",
            "logConfig": {
                "enable": false,
                "aggregationInterval": "INTERVAL_5_SEC",
                "flowSampling": 0.5,
                "metadata": "INCLUDE_ALL_METADATA"
            },
            "stackType": "IPV4_ONLY"
        }
    ],
    "peerings": [
        {
            "name": "host-proj2-vpc-to-host-proj1-vpc",
            "network": "https://www.googleapis.com/compute/v1/projects/<project-id>/global/networks/central-vpc-hub",
            "state": "INACTIVE",
            "stateDetails": "[2022-04-22T17:42:28.882-07:00]: Peer network tried to connect and failed.",
            "autoCreateRoutes": true,
            "exportCustomRoutes": true,
            "importCustomRoutes": true,
            "exchangeSubnetRoutes": true,
            "exportSubnetRoutesWithPublicIp": true,
            "importSubnetRoutesWithPublicIp": true,
            "stackType": "IPV4_ONLY"
        }
    ],
    "routingConfig": {
        "routingMode": "GLOBAL"
    },
    "mtu": 1460,
    "networkFirewallPolicyEnforcementOrder": "AFTER_CLASSIC_FIREWALL"
}
```

</details>
<br />

## Google VPN Gateway

### Introduction
The configuration of the Google VPN Gateway is dependent on the Google API response of the Google VPN Gateway as the primary response. The full resource configuration consists of some associated resources' API data, including tunnels.

### Content

Below are the Google APIs used to generate this configuration.

|Resource/Action|Relationship|Google API Version|Google API document|
|------|------|------|------|
| VPN Gateway - Get | self | v1 | https://cloud.google.com/compute/docs/reference/rest/v1/vpnGateways/get
| Tunnels - Get | tunnels | v1 | https://cloud.google.com/compute/docs/reference/rest/v1/vpnTunnels/get https://cloud.google.com/compute/docs/reference/rest/v1/vpnTunnels/list

### Sample
<details><summary>Configuration File</summary>

```json
{
    "netbrainNotes": "This config file is generated via API",
    "netbrainHostName": "tyler-vpn-000(<vpn_gateway_id>)",
    "kind": "compute#vpnGateway",
    "id": "<vpn_gateway_id>",
    "creationTimestamp": "2021-08-19T12:02:24.423-07:00",
    "name": "tyler-vpn-000",
    "description": "",
    "region": "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/us-east1",
    "network": "https://www.googleapis.com/compute/v1/projects/<project-id>/global/networks/default",
    "selfLink": "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/us-east1/vpnGateways/tyler-vpn-000",
    "labelFingerprint": "xxxxxxxxxxxx",
    "vpnInterfaces": [
        {
            "id": 0,
            "ipAddress": "xx.xx.xx.xx"
        }
    ],
    # vpn tunnels: 
    #   https://cloud.google.com/compute/docs/reference/rest/v1/vpnTunnels/get
    #   https://cloud.google.com/compute/docs/reference/rest/v1/vpnTunnels/list
    "tunnels": [
        {
            "kind": "compute#vpnTunnel",
            "id": "<tunnel-id>",
            "creationTimestamp": "2021-06-14T13:44:43.411-07:00",
            "name": "to-hostproj1-tunnel-1",
            "description": "to-hostproj1-tunnel-1",
            "region": "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/us-west4",
            "vpnGateway": "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/us-west4/vpnGateways/serv-proj2-us-west4-cloudvpn",
            "vpnGatewayInterface": 0,
            "peerGcpGateway": "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/us-west4/vpnGateways/host-proj1-vpc-spk3-cloudvpn",
            "router": "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/us-west4/routers/serv-proj2-us-west4-cloudvpn",
            "peerIp": "xx.xx.xx.xx",
            "sharedSecret": "*",
            "sharedSecretHash": "xxxxxxxxxxxxxxxxxxxxxx",
            "status": "ESTABLISHED",
            "selfLink": "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/us-west4/vpnTunnels/to-hostproj1-tunnel-1",
            "ikeVersion": 2,
            "detailedStatus": "Tunnel is up and running.",
            "localTrafficSelector": [
                "0.0.0.0/0"
            ],
            "remoteTrafficSelector": [
                "0.0.0.0/0"
            ],
            "labelFingerprint": "xxxxxxxxxxx"
        }
    ]
}


```

</details>
<br />

## Google Cloud Router

### Introduction
The configuration of the Google Cloud Router is dependent on the Google API response of the Google Cloud Router as the primary response. The Google API provides detailed information regarding the configuration of the routers, including its name, region, network and etc.

### Content
Below are the Google APIs used to generate this configuration.
|Resource/Action|Relationship|Google API Version|Google API document|
|------|------|------|------|
| Cloud Routers - Get | self | v1 | https://cloud.google.com/compute/docs/reference/rest/v1/routers/get

### Sample
<details><summary>Configuration File</summary>

```json
{
    "netbrainNotes": "This config file is generated via API",
    "netbrainHostName": "central-cloud-router-2(<router_id>)",
    "kind": "compute#router",
    "id": "<router_id>",
    "creationTimestamp": "2021-02-09T09:02:01.340-08:00",
    "name": "central-cloud-router-2",
    "description": "",
    "region": "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/us-central1",
    "network": "https://www.googleapis.com/compute/v1/projects/<project-id>/global/networks/central-vpc-hub",
    "interfaces": [
        {
            "name": "if-to-servproj3-bgpsession-1",
            "linkedVpnTunnel": "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/us-central1/vpnTunnels/to-servproj3-tunnel-1",
            "ipRange": "xx.xx.xx.xx/30"
        }
    ],
    "bgpPeers": [
        {
            "name": "to-servproj3-bgpsession-1",
            "interfaceName": "if-to-servproj3-bgpsession-1",
            "ipAddress": "xx.xx.xx.xx",
            "peerIpAddress": "xx.xx.xx.xx",
            "peerAsn": 65103,
            "advertiseMode": "CUSTOM",
            "advertisedGroups": [
                "ALL_SUBNETS"
            ],
            "advertisedIpRanges": [
                {
                    "range": "xx.xx.xx.xx/28",
                    "description": "custom route from host project 1 vpc spoke 3"
                },
                {
                    "range": "xx.xx.xx.xx/28",
                    "description": "from host project 1 central hub vpc"
                }
            ],
            "enable": "TRUE"
        }
    ],
    "bgp": {
        "asn": 65511,
        "advertiseMode": "DEFAULT"
    },
    "nats": [
        {
            "name": "asia-vpc-spoke4",
            "autoNetworkTier": "PREMIUM",
            "endpointTypes": [
                "ENDPOINT_TYPE_VM"
            ],
            "sourceSubnetworkIpRangesToNat": "ALL_SUBNETWORKS_ALL_IP_RANGES",
            "natIpAllocateOption": "AUTO_ONLY",
            "logConfig": {
                "enable": false,
                "filter": "ALL"
            },
            "enableEndpointIndependentMapping": false
        }
    ],
    "selfLink": "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/us-central1/routers/central-cloud-router-2",
    
    "encryptedInterconnectRouter": false
}
```

</details>
<br />

## Google Load Balancer
### Introduction
The configuration of the Google Load Balancer is dependent on the Google API response of the Forwarding Rules as the primary response. There is no distinct between internal or external, Layer 4 or Layer 7, and each load balance is the combination of various resources. The full resource configuration consists of some associated resources' API data, including backendService, group. 

### Content
Below are the Google APIs used to generate this configuration.
|Resource/Action|Relationship|Google API Version|Google API document|
|------|------|------|------|
| Load Balancers - Get | self | v1 | https://cloud.google.com/compute/docs/reference/rest/v1/forwardingRules/get
| Target HTTP Proxies - Get | target | v1 | https://cloud.google.com/compute/docs/reference/rest/v1/targetHttpProxies/get
| Url Map - Get | target.urlMap | v1 | https://cloud.google.com/compute/docs/reference/rest/v1/regionUrlMaps/get
| Default Service - Get | target.urlMap.pathMatchers.defaultService | v1 | https://cloud.google.com/compute/docs/reference/rest/v1/backendServices/get
| Service - Get | target.urlMap.pathMatchers.pathRules.service | v1 | https://cloud.Default.com/compute/docs/reference/rest/v1/backendServices/get
| Default Service - Get | target.urlMap.defaultService | v1 | https://cloud.google.com/compute/docs/reference/rest/v1/backendServices/get
| Backend Service - Get | backendService | v1 | https://cloud.google.com/compute/docs/reference/rest/v1/backendServices/get
| Instance Groups - Get | backendService.backends.group | v1 | https://cloud.google.com/compute/docs/reference/rest/v1/instanceGroups/get

### Sample
<details><summary>Configuration File</summary>

```json
{
    "netbrainNotes": "This config file is generated via API",
    "netbrainHostName": "host-proj2-tcp-internal-lb1(<ldb_id>-internal-tcp-loadbalancer)",
    "kind": "compute#forwardingRule",
    "id": "<ldb_id>",
    "creationTimestamp": "2021-07-15T18:35:38.345-07:00",
    "name": "host-proj2-tcp-internal-lb1-frontend",
    "description": "",
    "region": "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/us-central1",
    "IPAddress": "xx.xx.xx.xx",
    "IPProtocol": "TCP",
    "portRange": "80-80",
    # Target: https://cloud.google.com/compute/docs/reference/rest/v1/targetHttpProxies/get
    "target": {
        "kind": "compute#targetHttpProxy",
        "id": "3267693297597854555",
        "creationTimestamp": "2021-05-05T14:27:16.252-07:00",
        "name": "lb5-http-internal-lb-target-proxy",
        "selfLink": "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/us-east1/targetHttpProxies/lb5-http-internal-lb-target-proxy",
        # UrlMap: https://cloud.google.com/compute/docs/reference/rest/v1/regionUrlMaps/get
        "urlMap": {
            "kind": "compute#urlMap",
            "id": "<url-map-id>",
            "creationTimestamp": "2021-05-05T14:27:12.247-07:00",
            "name": "lb5-http-internal-lb",
            "selfLink": "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/us-east1/urlMaps/lb5-http-internal-lb",
            "hostRules": [
                {
                    "hosts": [
                        "<hostname>"
                    ],
                    "pathMatcher": "path-matcher-1"
                }
            ],
            "pathMatchers": [
                {
                    "name": "path-matcher-1",
                    # BackendService: https://cloud.google.com/compute/docs/reference/rest/v1/backendServices/get
                    "defaultService": {
                        "kind": "compute#backendService",
                        "id": "<default-service-id>",
                        "creationTimestamp": "2021-05-05T14:26:55.156-07:00",
                        "name": "lb5-http-internal-lb-backend",
                        "description": "",
                        "selfLink": "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/us-east1/backendServices/lb5-http-internal-lb-backend",
                        "backends": [
                            {
                                "description": "",
                                "group": "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/us-east1/instanceGroups/traffic-director-ig",
                                "balancingMode": "UTILIZATION",
                                "maxUtilization": 0.8,
                                "capacityScaler": 1
                            }
                        ],
                        "healthChecks": [
                            "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/us-east1/healthChecks/lb5-http-internal-lb-health-check"
                        ],
                        "timeoutSec": 30,
                        "port": 80,
                        "protocol": "HTTP",
                        "fingerprint": "gHQAFZlsf3w=",
                        "portName": "http",
                        "sessionAffinity": "NONE",
                        "affinityCookieTtlSec": 0,
                        "region": "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/us-east1",
                        "loadBalancingScheme": "INTERNAL_MANAGED",
                        "connectionDraining": {
                            "drainingTimeoutSec": 300
                        },
                        "logConfig": {
                            "enable": false,
                            "optionalMode": "EXCLUDE_ALL_OPTIONAL"
                        },
                        "localityLbPolicy": "ROUND_ROBIN"
                    },
                    "pathRules": [
                         {
                            # BackendService: https://cloud.google.com/compute/docs/reference/rest/v1/backendServices/get
                            "service": {
                                "kind": "compute#backendService",
                                "id": "<backend-service-id>",
                                "creationTimestamp": "2021-05-05T14:34:50.741-07:00",
                                "name": "lb4-http-internal-lb-backend",
                                "description": "",
                                "selfLink": "https://www.googleapis.com/compute/v1/projects/<project-id>9/regions/us-east1/backendServices/lb4-http-internal-lb-backend",
                                "backends": [
                                    {
                                        "description": "",
                                        "group": "https://www.googleapis.com/compute/v1/projects/<project-id>9/regions/us-east1/instanceGroups/lb5-http-internal-lb-instance-group",
                                        "balancingMode": "UTILIZATION",
                                        "maxUtilization": 0.8,
                                        "capacityScaler": 1
                                    }
                                ],
                                "healthChecks": [
                                    "https://www.googleapis.com/compute/v1/projects/<project-id>9/regions/us-east1/healthChecks/lb4-http-internal-lb-health-check"
                                ],
                                "timeoutSec": 30,
                                "port": 80,
                                "protocol": "HTTP",
                                "fingerprint": "xxxxxxxxxxxx",
                                "portName": "http",
                                "sessionAffinity": "NONE",
                                "affinityCookieTtlSec": 0,
                                "region": "https://www.googleapis.com/compute/v1/projects/<project-id>9/regions/us-east1",
                                "loadBalancingScheme": "INTERNAL_MANAGED",
                                "connectionDraining": {
                                    "drainingTimeoutSec": 300
                                },
                                "logConfig": {
                                    "enable": false,
                                    "optionalMode": "EXCLUDE_ALL_OPTIONAL"
                                },
                                "localityLbPolicy": "ROUND_ROBIN"
                            },
                            "paths": [
                                "/image"
                            ]
                        }
                    ]
                }
            ],
            # BackendService: https://cloud.google.com/compute/docs/reference/rest/v1/backendServices/get
            "defaultService": {
                "kind": "compute#backendService",
                "id": "<default-service-id>",
                "creationTimestamp": "2021-05-05T14:26:55.156-07:00",
                "name": "lb5-http-internal-lb-backend",
                "description": "",
                "selfLink": "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/us-east1/backendServices/lb5-http-internal-lb-backend",
                "backends": [
                    {
                        "description": "",
                        "group": "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/us-east1/instanceGroups/traffic-director-ig",
                        "balancingMode": "UTILIZATION",
                        "maxUtilization": 0.8,
                        "capacityScaler": 1
                    }
                ],
                "healthChecks": [
                    "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/us-east1/healthChecks/lb5-http-internal-lb-health-check"
                ],
                "timeoutSec": 30,
                "port": 80,
                "protocol": "HTTP",
                "fingerprint": "gHQAFZlsf3w=",
                "portName": "http",
                "sessionAffinity": "NONE",
                "affinityCookieTtlSec": 0,
                "region": "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/us-east1",
                "loadBalancingScheme": "INTERNAL_MANAGED",
                "connectionDraining": {
                    "drainingTimeoutSec": 300
                },
                "logConfig": {
                    "enable": false,
                    "optionalMode": "EXCLUDE_ALL_OPTIONAL"
                },
                "localityLbPolicy": "ROUND_ROBIN"
            },
            "fingerprint": "xxxxxxxxxxx",
            "region": "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/us-east1"
        },
        "region": "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/us-east1",
        "fingerprint": "xxxxxxxxxxx"
    },
    "selfLink": "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/us-central1/forwardingRules/host-proj2-tcp-internal-lb1-frontend",
    "loadBalancingScheme": "INTERNAL",
    "subnetwork": "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/us-central1/subnetworks/subnet-1",
    "network": "https://www.googleapis.com/compute/v1/projects/<project-id>/global/networks/central-vpc-hub",
    # Backend Service: https://cloud.google.com/compute/docs/reference/rest/v1/backendServices/get
    "backendService": {
        "kind": "compute#backendService",
        "id": "<backend-service-id>",
        "creationTimestamp": "2021-07-15T18:35:31.122-07:00",
        "name": "host-proj2-tcp-internal-lb1",
        "description": "",
        "selfLink": "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/us-central1/backendServices/host-proj2-tcp-internal-lb1",
        "backends": [
            {
                # Instance Groups: https://cloud.google.com/compute/docs/reference/rest/v1/instanceGroups/get
                "group": {
                    "kind": "compute#instanceGroup",
                    "id": "<group_id>",
                    "creationTimestamp": "2021-07-15T18:31:27.956-07:00",
                    "name": "host-proj2-ig",
                    "description": "This instance group is controlled by Instance Group Manager 'host-proj2-ig'. To modify instances in this group, use the Instance Group Manager API: https://cloud.google.com/compute/docs/reference/latest/instanceGroupManagers",
                    "network": "https://www.googleapis.com/compute/v1/projects/<project-id>/global/networks/central-vpc-hub",
                    "fingerprint": "xxxxxxxxxxx",
                    "zone": "https://www.googleapis.com/compute/v1/projects/<project-id>/zones/us-central1-a",
                    "selfLink": "https://www.googleapis.com/compute/v1/projects/<project-id>/zones/us-central1-a/instanceGroups/host-proj2-ig",
                    "size": 1,
                    "subnetwork": "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/us-central1/subnetworks/subnet-1"
                },
                "balancingMode": "CONNECTION",
                "failover": true
            }
        ],
        "healthChecks": [
            "https://www.googleapis.com/compute/v1/projects/<project-id>/global/healthChecks/tcp-health-check"
        ],
        "timeoutSec": 30,
        "protocol": "TCP",
        "fingerprint": "OgJSiZYsVAU=",
        "sessionAffinity": "CLIENT_IP_PORT_PROTO",
        "region": "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/us-central1",
        "failoverPolicy": {
            "disableConnectionDrainOnFailover": false,
            "dropTrafficIfUnhealthy": false,
            "failoverRatio": 0
        },
        "loadBalancingScheme": "INTERNAL",
        "connectionDraining": {
            "drainingTimeoutSec": 300
        },
        "network": "https://www.googleapis.com/compute/v1/projects/<project-id>/global/networks/central-vpc-hub"
    },
    "serviceLabel": "lb",
    "serviceName": "lb.host-proj2-tcp-internal-lb1-frontend.il4.us-central1.lb.<project-id>.internal",
    "networkTier": "PREMIUM",
    "labelFingerprint": "xxxxxxxxxxx",
    "fingerprint": "xxxxxxxxxxx",
    "allPorts": true
}
```


</details>
<br />



## Google Firewall

### Introduction
The configuration of the  Google Firewall relies solely on the corresponding Google API of its Networks. The Google API provides detailed information regarding the configuration of the Google Firewall, including its firewalls, firewallPolicys, etc.

### Content
Below are the Google APIs used to generate this configuration.
|Resource/Action|Relationship|Google API Version|Google API document|
|------|------|------|------|
| Google Firewall - Get | self | v1 | https://cloud.google.com/compute/docs/reference/rest/v1/networks/getEffectiveFirewalls


### Sample
<details><summary>Configuration File</summary>

```json
{
    "netbrainNotes": "This config file is generated via API",
    "netbrainHostName": "<network-name>-firewall(<firewall-id>-firewall)",
    "id": "<firewall-id>-firewall",
    "name": "<network-name>-firewall(<firewall-id>-firewall)",
    "networkName": "<network-name>",
    "firewalls": [
        {
            "kind": "compute#firewall",
            "id": "<firewall-rule-id>",
            "creationTimestamp": "2021-08-18T16:38:53.844-07:00",
            "name": "allowalltraff",
            "description": "",
            "network": "https://www.googleapis.com/compute/v1/projects/<proj-id>/global/networks/<network-name>",
            "priority": 1000,
            "sourceRanges": [
                "0.0.0.0/0"
            ],
            "allowed": [
                {
                    "IPProtocol": "all"
                }
            ],
            "direction": "INGRESS",
            "logConfig": {
                "enable": false
            },
            "disabled": false,
            "selfLink": "https://www.googleapis.com/compute/v1/projects/<proj-id>/global/firewalls/allowalltraff"
        }
    ],
    "firewallPolicys": [
        {
            "name": "xxxxxxx",
            "type": "HIERARCHY",
            "shortName": "firewall-policy-org",
            "displayName": "firewall-policy-org",
            "rules": [
                {
                    "kind": "compute#firewallPolicyRule",
                    "description": "",
                    "priority": 500,
                    "match": {
                        "destIpRanges": [
                            "172.18.3.82"
                        ],
                        "layer4Configs": [
                            {
                                "ipProtocol": "tcp",
                                "ports": [
                                    "86"
                                ]
                            }
                        ]
                    },
                    "action": "allow",
                    "direction": "EGRESS",
                    "enableLogging": false
                }
            ]
        }
    ]
}
```

</details>
<br />


## Google Cloud NAT

### Introduction
The configuration of the Google Cloud NAT relies solely on the corresponding Google API of its cloud routers. The Google API provides detailed information regarding the configuration of the Google Cloud NAT, including its endpointTypes, natIps, etc.

### Content
Below are the Google APIs used to generate this configuration.
|Resource/Action|Relationship|Google API Version|Google API document|
|------|------|------|------|
| Cloud Routers - Get | self | v1 | https://cloud.google.com/compute/docs/reference/rest/v1/routers/get

### Sample
<details><summary>Configuration File</summary>

```json
{
    "netbrainNotes": "This config file is generated via API",
    "netbrainHostName": "asia-vpc-spoke4(<vpc-id>-asia-vpc-spoke4)",
    "name": "asia-vpc-spoke4",
    "endpointTypes": [
        "ENDPOINT_TYPE_VM"
    ],
    "sourceSubnetworkIpRangesToNat": "ALL_SUBNETWORKS_ALL_IP_RANGES",
    "natIps": [
        "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/us-west1/addresses/asia-vpc-spoke4-natip"
    ],
    "subnetworks": [
        {
            "name": "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/us-central1/subnetworks/subnet-2",
            "sourceIpRangesToNat": [
                "PRIMARY_IP_RANGE"
            ]
        }
    ],
    "natIpAllocateOption": "AUTO_ONLY",
    "minPortsPerVm": 64,
    "udpIdleTimeoutSec": 30,
    "icmpIdleTimeoutSec": 30,
    "tcpEstablishedIdleTimeoutSec": 1200,
    "tcpTransitoryIdleTimeoutSec": 30,
    "logConfig": {
        "enable": false,
        "filter": "ALL"
    },
    "enableEndpointIndependentMapping": false
}
```

</details>
<br />

## Google Virtual Machine

### Introduction
Configuration feature is not supported for Google Virtual Machine yet. Please send API to get the resource data instead (e.g. Sample 1 of Fetch Resource Data). GCP document reference: https://cloud.google.com/compute/docs/reference/rest/v1/instances/get


## Google Internet Gateway
### Introduction
Google Cloud does not provide an independent resource for an Google Internet Gateway. Instead, the Internet gateway is created for each Virtual Private Cloud (VPC) to illustrate the path of network traffic to the Internet.

### Sample
<details><summary>Configuration File</summary>

```json
{
    "netbrainNotes": "This config file is generated via API",
    "netbrainHostName": "europe-west-vpc-igw(<ig-id>-igw)",
    "id": "<ig-id>-igw",
    "name": "europe-west-vpc-igw(<ig-id>-igw)",
    "networkId": "<ig-id>",
    "networkLink": "https://www.googleapis.com/compute/v1/projects/<project-id>/global/networks/europe-west-vpc"
}
```

</details>
<br/>

## Google Global Internet Gateway
### Introduction
Google Cloud does not provide an independent resource for an Google Global Internet Gateway. Instead, the Google Global Internet Gateway is created to illustrate the path of network traffic to the Internet.

### Sample
<details><summary>Configuration File</summary>

```json
{
    "netbrainNotes": "This config file is generated via API",
    "netbrainHostName": "Global-igw(<global-ig-id>)",
    "id": "<global-ig-id>",
    "name": "Global-igw(<global-ig-id>)"
}
```

</details>
<br/>

## Google Partner Interconnect <a id="partner-google-interconnect"></a>
### Introduction
The configuration of the Google Partner Interconnect relies solely on the corresponding Google API of the interconnectAttachments. The Google API provides detailed information regarding the configuration of the instance, including its bandwidth, partnerMetadata, etc.

### Content
Below are the Google APIs used to generate this configuration.
|Resource/Action|Relationship|Google API Version|Google API document|
|------|------|------|------|
| Partner Google Interconnect  - Get | self | v1 | https://cloud.google.com/compute/docs/reference/rest/v1/interconnectAttachments/get

### Sample
<details><summary>Configuration File</summary>

```json
{
    "netbrainNotes": "This config file is generated via API",
    "netbrainHostName": "chicago-zone2-cgcil02(<interconnect-id>-partnerinterconnect)",
    "kind": "compute#interconnectAttachment",
    "selfLink": "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/us-east4/interconnectAttachments/gcptonetbondb",
    "id": "<interconnect-id>",
    "creationTimestamp": "2021-04-08T12:44:16.166-07:00",
    "name": "gcptonetbondb",
    "router": "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/us-east4/routers/gcp-router-b",
    "region": "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/us-east4",
    "mtu": 1500,
    "cloudRouterIpAddress": "xx.xx.xx.xx/29",
    "customerRouterIpAddress": "xx.xx.xx.xx/29",
    "type": "PARTNER",
    "pairingKey": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx/us-east4/2",
    "adminEnabled": true,
    "vlanTag8021q": 1141,
    "edgeAvailabilityDomain": "AVAILABILITY_DOMAIN_2",
    "bandwidth": "BPS_50M",
    "partnerMetadata": {
        "partnerName": "AT&T",
        "interconnectName": "chicago-zone2-cgcil02",
        "portalUrl": "https://synaptic.att.com/"
    },
    "labelFingerprint": "42WmSpB8rSM=",
    "state": "ACTIVE",
    "partnerAsn": "8030",
    "encryption": "NONE",
    "dataplaneVersion": 1,
    "stackType": "IPV4_ONLY"
}
```
</details>
<br />

## Google Dedicated Interconnect <a id="dedicated-google-interconnect"></a> 
### Introduction
The configuration of the Google Dedicated Interconnect is dependent on the Google API response of the interconnects as the primary response. The full resource configuration consists of some associated resources' Interconnect Attachments.

### Content
Below are the Google APIs used to generate this configuration.
|Resource/Action|Relationship|Google API Version|Google API document|
|------|------|------|------|
| Dedicated Google Interconnect  - Get | self | v1 | https://cloud.google.com/compute/docs/reference/rest/v1/interconnects/get
| Interconnect Attachments - Get | interconnectAttachments | v1 | https://cloud.google.com/compute/docs/reference/rest/v1/interconnectAttachments/get

### Sample
<details><summary>Configuration File</summary>

```json
{
    "netbrainNotes": "This config file is generated via API",
    "netbrainHostName": "chicago-zone2-cgcil02(<interconnect-id>-dedicateinterconnect)",
    "kind": "compute#interconnec",
    "description": "dedicated-interconnec",
    "selfLink": "https://compute.googleapis.com/compute/v1/projects/{project}/global/interconnects/{interconnect-name}",
    "id": "{interconnect-id}",
    "creationTimestamp": "2021-04-08T12:44:16.166-07:00",
    "name": "gcptonetbondb2",
    "location": "us-east4",
    "linkType": "LINK_TYPE_ETHERNET_10G_LR",
    "requestedLinkCount": 1,
    "interconnectType": "DEDICATED",
    "adminEnabled": false,
    "nocContactEmail": "example@ex.com",
    "customerName": "example-name",
    "operationalStatus": "OS_ACTIVE",
    "provisionedLinkCount": 1,
    # Interconnect Attachments: https://cloud.google.com/compute/docs/reference/rest/v1/interconnectAttachments/get
    "interconnectAttachments": [
        {

            "kind": "compute#interconnectAttachment",
            "selfLink": "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/us-east4/interconnectAttachments/gcptonetbondb",
            "id": "<interconnect-id>",
            "creationTimestamp": "2021-04-08T12:44:16.166-07:00",
            "name": "gcptonetbondb",
            "router": "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/us-east4/routers/gcp-router-b",
            "region": "https://www.googleapis.com/compute/v1/projects/<project-id>/regions/us-east4",
            "mtu": 1500,
            "cloudRouterIpAddress": "xx.xx.xx.xx/29",
            "customerRouterIpAddress": "xx.xx.xx.xx/29",
            "type": "PARTNER",
            "pairingKey": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx/us-east4/2",
            "adminEnabled": true,
            "vlanTag8021q": 1141,
            "edgeAvailabilityDomain": "AVAILABILITY_DOMAIN_2",
            "bandwidth": "BPS_50M",
            "labelFingerprint": "42WmSpB8rSM=",
            "state": "ACTIVE",
            "partnerAsn": "8030",
            "encryption": "NONE",
            "dataplaneVersion": 1,
            "stackType": "IPV4_ONLY"
            # ... with some additional params 
        }
    ],
    "peerIpAddress": "xx.xx.xx.xx",
    "googleIpAddress": "xx.xx.xx.xx",
    "googleReferenceId": "xxxxxxxxxx",
    "expectedOutages": [
        {
        "name": "outage-name",
        "description": "this is expected outage",
        "source": "GOOGLE",
        "state": "ACTIVE",
        "issueType": "OUTAGE",
        "affectedCircuits": [
            "IT_PARTIAL_OUTAGE"
        ],
        "startTime": "2021-04-08T12:44:16.166-07:00",
        "endTime": "2021-04-09T12:44:16.166-07:00"
        }
    ],
    "circuitInfos": [
        {
        "googleCircuitId": "xxxxxxxxxx",
        "googleDemarcId": "xxxxxxxxxx",
        "customerDemarcId": "xxxxxxxxxx"
        }
    ],
    "labels": {
        "label_key": "label_value"
    },
    "labelFingerprint": "xxxxxxxx",
    "state": "ACTIVE",
    "satisfiesPzs": true,
    "remoteLocation": "somewhere"
}
```

</details>
<br />

## Google Private Service Connect Endpoint

### Introduction
The configuration of the Google Private Service Connect Endpoint relies solely on the corresponding Google API of the globalForwardingRules. The Google API provides detailed information regarding the configuration of the instance, including its IPAddress, serviceDirectoryRegistrations, etc.

### Content
Below are the Google APIs used to generate this configuration.
|Resource/Action|Relationship|Google API Version|Google API document|
|------|------|------|------|
| Cloud Private Service Connect Endpoint - Get | self | v1 | https://cloud.google.com/compute/docs/reference/rest/v1/globalForwardingRules/get

### Sample
<details><summary>Configuration File</summary>

```json
{
    "netbrainNotes": "This config file is generated via API",
    "netbrainHostName": "psctest1(<psc-id>)",
    "kind": "compute#forwardingRule",
    "id": "<psc-id>",
    "creationTimestamp": "2022-09-08T07:37:00.890-07:00",
    "name": "psctest1",
    "IPAddress": "xx.xx.xx.xx",
    "IPProtocol": "TCP",
    "target": "all-apis",
    "selfLink": "https://www.googleapis.com/compute/v1/projects/<project-id>/global/forwardingRules/psctest1",
    "network": "https://www.googleapis.com/compute/v1/projects/<project-id>/global/networks/central-vpc-hub",
    "serviceDirectoryRegistrations": [
        {
            "namespace": "goog-psc-central-vpc-hub-5165306842222363136",
            "serviceDirectoryRegion": "us-central1"
        }
    ],
    "labelFingerprint": "xxxxxxxxxx",
    "pscConnectionId": "xxxxxxxxxxxxxx"
}
```

</details>
<br />