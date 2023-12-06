# Usage
To retrieve the configuration data for a resource, you can utilize NetBrain's built-in configuration file function, which does not require coding. This function enables you to quickly obtain the resource configuration.


# Table of Contents
* [AWS VPC Router](#aws-vpc-router)
* [AWS Gateway/Interface VPC Endpoint](#aws-vpc-endpoint)
* [AWS Virtual Private Gateway](#aws-virtual-private-gateway)
* [AWS Transit Gateway](#aws-transit-gateway)
* [AWS Egress Internet Gateway](#aws-egress-internet-gateway)
* [AWS Internet Gateway](#aws-internet-gateway)
* [AWS Classic Load Balancer](#aws-load-balancer)
* [AWS Network/Application/Gateway Load Balancer](#aws-load-balancer-v2)
* [AWS Direct Connect Gateway](#aws-direct-connect-gateway)
* [AWS NAT Gateway](#aws-nat-gateway)
* [AWS Firewall](#aws-firewall)
* [AWS DX Router](#aws-dx-router)

## AWS VPC Router

### Introduction
The configuration of the AWS VPC Router is dependent on the AWS SDK response of the AWS virtual private cloud (VPC) as the primary response. The full resource configuration consists of some associated resources' data, including `vpc classic link`, `subnet`, and `vpc peering connections`.

### Content
Below are the AWS SDK used to generate this configuration.
|**Resource/Action**|**Relationship**|**AWS SDK document**|
|------|------|------|
| describe_vpcs | self | https://boto3.amazonaws.com/v1/documentation/api/1.26.86/reference/services/ec2/client/describe_vpcs.html|
| describe_vpc_classic_link | VpcClassicLinks | https://boto3.amazonaws.com/v1/documentation/api/1.26.86/reference/services/ec2/client/describe_vpc_classic_link.html |
| describe_subnets | Subnets | https://boto3.amazonaws.com/v1/documentation/api/1.26.86/reference/services/ec2/client/describe_subnets.html | 
| describe_vpc_peering_connections | VpcPeeringConnections | https://boto3.amazonaws.com/v1/documentation/api/1.26.86/reference/services/ec2/client/describe_vpc_peering_connections.html |


### Sample
<details><summary>Configuration File</summary>

```json
{
    "netbrainNotes": "This config file is generated via API",
    "netbrainHostName": "xxx(vpc-xxxxxxxxxxxxxxxxx)",
    "CidrBlock": "192.168.0.0/24"
    "DhcpOptionsId": "xxxx-xxxxxxxxx",
    "State": "available",
    "VpcId": "vpc-xxxxxxxxxxxxxxxxx",
    "OwnerId": "xxx",
    "InstanceTenancy": "default",
    "CidrBlockAssociationSet": [],
    "IsDefault": false,
    "Tags": [
        {
            "Key": "Name",
            "Value": "xxx"
        }
    ],
    # VPC Classic Links: https://boto3.amazonaws.com/v1/documentation/api/1.26.86/reference/services/ec2/client/describe_vpc_classic_link.html
    "VpcClassicLinks": [],
    # Subnets: https://boto3.amazonaws.com/v1/documentation/api/1.26.86/reference/services/ec2/client/describe_subnets.html
    "Subnets": [
        {
            "AvailabilityZone": "us-east-1a",
            "AvailabilityZoneId": "use1-az1",
            "AvailableIpAddressCount": 10,
            "CidrBlock": "192.168.0.0/24"
            "DefaultForAz": false,
            "MapPublicIpOnLaunch": false,
            "MapCustomerOwnedIpOnLaunch": false,
            "State": "available",
            "SubnetId": "subnet-xxxxxxxxxxxxxxxxx",
            "VpcId": "vpc-xxxxxxxxxxxxxxxxx",
            "OwnerId": "xxx",
            "AssignIpv6AddressOnCreation": false,
            "Ipv6CidrBlockAssociationSet": [],
            "Tags": [
                {
                    "Key": "Name",
                    "Value": "xxx"
                }
            ],
            "SubnetArn": "xxx",
            "EnableDns64": false,
            "Ipv6Native": false,
            "PrivateDnsNameOptionsOnLaunch": {
                "HostnameType": "ip-name",
                "EnableResourceNameDnsARecord": false,
                "EnableResourceNameDnsAAAARecord": false
            }
        }
    ],
    # VPC Peering Connections: https://boto3.amazonaws.com/v1/documentation/api/1.26.86/reference/services/ec2/client/describe_vpc_peering_connections.html
    "VpcPeeringConnections": [
        {
            "AccepterVpcInfo": {
                "CidrBlock": "192.168.0.0/24"
                "CidrBlockSet": [
                    {
                        "CidrBlock": "192.168.0.0/24"
                    },
                    {
                        "CidrBlock": "192.168.0.0/24"
                    },
                    {
                        "CidrBlock": "192.168.0.0/24"
                    }
                ],
                "OwnerId": "xxx",
                "PeeringOptions": {
                    "AllowDnsResolutionFromRemoteVpc": false,
                    "AllowEgressFromLocalClassicLinkToRemoteVpc": false,
                    "AllowEgressFromLocalVpcToRemoteClassicLink": false
                },
                "VpcId": "vpc-xxxxxxxxxxxxxxxxx",
                "Region": "us-east-1"
            },
            "RequesterVpcInfo": {
                "CidrBlock": "192.168.0.0/24"
                "CidrBlockSet": [
                    {
                        "CidrBlock": "192.168.0.0/24"
                    }
                ],
                "OwnerId": "xxx",
                "PeeringOptions": {
                    "AllowDnsResolutionFromRemoteVpc": false,
                    "AllowEgressFromLocalClassicLinkToRemoteVpc": false,
                    "AllowEgressFromLocalVpcToRemoteClassicLink": false
                },
                "VpcId": "vpc-xxxxxxxxxxxxxxxxx",
                "Region": "us-east-1"
            },
            "Status": {
                "Code": "active",
                "Message": "Active"
            },
            "Tags": [
                {
                    "Key": "Name",
                    "Value": "xxx"
                }
            ],
            "VpcPeeringConnectionId": "pcx-xxxxxxxxxxxxxxxxx"
        },
        {
            "AccepterVpcInfo": {
                "CidrBlock": "192.168.0.0/24"
                "CidrBlockSet": [
                    {
                        "CidrBlock": "192.168.0.0/24"
                    },
                    {
                        "CidrBlock": "192.168.0.0/24"
                    }
                ],
                "OwnerId": "xxx",
                "PeeringOptions": {
                    "AllowDnsResolutionFromRemoteVpc": false,
                    "AllowEgressFromLocalClassicLinkToRemoteVpc": false,
                    "AllowEgressFromLocalVpcToRemoteClassicLink": false
                },
                "VpcId": "vpc-xxxxxxxxxxxxxxxxx",
                "Region": "us-east-2"
            },
            "RequesterVpcInfo": {
                "CidrBlock": "192.168.0.0/24"
                "CidrBlockSet": [
                    {
                        "CidrBlock": "192.168.0.0/24"
                    }
                ],
                "OwnerId": "xxx",
                "PeeringOptions": {
                    "AllowDnsResolutionFromRemoteVpc": false,
                    "AllowEgressFromLocalClassicLinkToRemoteVpc": false,
                    "AllowEgressFromLocalVpcToRemoteClassicLink": false
                },
                "VpcId": "vpc-xxxxxxxxxxxxxxxxx",
                "Region": "us-east-1"
            },
            "Status": {
                "Code": "active",
                "Message": "Active"
            },
            "Tags": [
                {
                    "Key": "Name",
                    "Value": "xxx"
                }
            ],
            "VpcPeeringConnectionId": "pcx-xxxxxxxxxxxxxxxxx"
        },
        {
            "AccepterVpcInfo": {
                "CidrBlock": "192.168.0.0/24"
                "Ipv6CidrBlockSet": [
                    {
                        "Ipv6CidrBlock": "xxxx:xxxx:xxx:xxxx::/xx"
                    }
                ],
                "CidrBlockSet": [
                    {
                        "CidrBlock": "192.168.0.0/24"
                    },
                    {
                        "CidrBlock": "192.168.0.0/24"
                    }
                ],
                "OwnerId": "xxx",
                "PeeringOptions": {
                    "AllowDnsResolutionFromRemoteVpc": false,
                    "AllowEgressFromLocalClassicLinkToRemoteVpc": false,
                    "AllowEgressFromLocalVpcToRemoteClassicLink": false
                },
                "VpcId": "vpc-xxxxxxxxxxxxxxxxx",
                "Region": "ca-central-1"
            },
            "RequesterVpcInfo": {
                "CidrBlock": "192.168.0.0/24"
                "CidrBlockSet": [
                    {
                        "CidrBlock": "192.168.0.0/24"
                    }
                ],
                "OwnerId": "xxx",
                "PeeringOptions": {
                    "AllowDnsResolutionFromRemoteVpc": false,
                    "AllowEgressFromLocalClassicLinkToRemoteVpc": false,
                    "AllowEgressFromLocalVpcToRemoteClassicLink": false
                },
                "VpcId": "vpc-xxxxxxxxxxxxxxxxx",
                "Region": "us-east-1"
            },
            "Status": {
                "Code": "active",
                "Message": "Active"
            },
            "Tags": [
                {
                    "Key": "Name",
                    "Value": "xxx"
                }
            ],
            "VpcPeeringConnectionId": "pcx-xxxxxxxxxxxxxxxxx"
        }
    ]
}
```

</details>
<br />


## AWS VPC Endpoint

### Introduction
**AWS gateway VPC endpoint** and **AWS interface VPC endpoint** have the same configuration file structure. The configuration of the AWS VPC Endpoint is dependent on the AWS SDK response of the VPC Endpoint as the primary response. The full resource configuration consists of some associated resources' data, including `Vpc Endpoint Connections`.

### Content
Below are the AWS SDK used to generate this configuration.
|**Resource/Action**|**Relationship**|**AWS SDK document**|
|------|------|------|
| describe_vpc_endpoints | self | https://boto3.amazonaws.com/v1/documentation/api/1.26.86/reference/services/ec2/client/describe_vpc_endpoints.html|
| describe_vpc_endpoint_connections | VpcEndpointConnections | https://boto3.amazonaws.com/v1/documentation/api/1.26.86/reference/services/ec2/client/describe_vpc_endpoint_connections.html |

### Sample
<details><summary>Configuration File</summary>

```json
{
    "netbrainNotes": "This config file is generated via API",
    "netbrainHostName": "xxx(vpce-xxxxxxxxxxxxxxxxx)",
    "VpcEndpointId": "vpce-xxxxxxxxxxxxxxxxx",
    "VpcEndpointType": "Gateway",
    "VpcId": "vpc-xxxxxxxxxxxxxxxxx",
    "ServiceName": "com.amazonaws.us-east-1.s3",
    "State": "available",
    "PolicyDocument": "{\"Version\":\"2008-10-17\",\"Statement\":[{\"Effect\":\"Allow\",\"Principal\":\"*\",\"Action\":\"*\",\"Resource\":\"*\"}]}",
    "RouteTableIds": [
        "rtb-xxxxxxxxxxxxxxxxx"
    ],
    "SubnetIds": [],
    "Groups": [],
    "PrivateDnsEnabled": false,
    "RequesterManaged": false,
    "NetworkInterfaceIds": [],
    "DnsEntries": [],
    "CreationTimestamp": "2020-08-20 20:55:19+00:00",
    "Tags": [
        {
            "Key": "Name",
            "Value": "xxx"
        }
    ],
    "OwnerId": "xxx",
    # VPC Endpoint Connections: https://boto3.amazonaws.com/v1/documentation/api/1.26.86/reference/services/ec2/client/describe_vpc_endpoint_connections.html
    "VpcEndpointConnections": []
}
```

</details>
<br />


## AWS Virtual Private Gateway

### Introduction
The configuration of the AWS Virtual Private Gateway is dependent on the AWS SDK response of the AWS Virtual Private Gateway as the primary response. The full resource configuration consists of some associated resources' data, including `Vpn Connections`, `Customer Gateways`.

### Content
Below are the AWS SDK used to generate this configuration.
|**Resource/Action**|**Relationship**|**AWS SDK document**|
|------|------|------|
| describe_vpn_gateways | self | https://boto3.amazonaws.com/v1/documentation/api/1.26.86/reference/services/ec2/client/describe_vpn_gateways.html|
| describe_vpn_connections | VpnConnections | https://boto3.amazonaws.com/v1/documentation/api/1.26.86/reference/services/ec2/client/describe_vpn_connections.html |
| describe_customer_gateways | VpnConnections.CustomerGateways | https://boto3.amazonaws.com/v1/documentation/api/1.26.86/reference/services/ec2/client/describe_customer_gateways.html |


### Sample
<details><summary>Configuration File</summary>

```json
{
    "netbrainNotes": "This config file is generated via API",
    "netbrainHostName": "xxx(vgw-xxxxxxxxxxxxxxxxx)",
    "State": "available",
    "Type": "ipsec.1",

    "VpcAttachments": [
        {
            "State": "attached",
            "VpcId": "vpc-xxxxxxxxxxxxxxxxx"
        }
    ],
    "VpnGatewayId": "vgw-xxxxxxxxxxxxxxxxx",
    "AmazonSideAsn": 12345,
    "Tags": [
        {
            "Key": "Name",
            "Value": "xxx"
        }
    ],
    # VPN Connections: https://boto3.amazonaws.com/v1/documentation/api/1.26.86/reference/services/ec2/client/describe_vpn_connections.html
    "VpnConnections": [
        {
            "CustomerGatewayConfiguration": "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<vpn_connection id=\"vpn-xxxxxxxxxxxxxxxxx\">\n  <customer_gateway_id>cgw-xxxxxxxxxxxxxxxxx</customer_gateway_id>\n  <vpn_gateway_id>vgw-xxxxxxxxxxxxxxxxx</vpn_gateway_id>\n  <vpn_connection_type>ipsec.1</vpn_connection_type>\n  <ipsec_tunnel>\n    <customer_gateway>\n      <tunnel_outside_address>\n        <ip_address>1.2.3.4</ip_address>\n      </tunnel_outside_address>\n      <tunnel_inside_address>\n        <ip_address>192.168.0.1</ip_address>\n        <network_mask>255.255.255.252</network_mask>\n        <network_cidr>30</network_cidr>\n      </tunnel_inside_address>\n      <bgp>\n        <asn>12345</asn>\n        <hold_time>30</hold_time>\n      </bgp>\n    </customer_gateway>\n    <vpn_gateway>\n      <tunnel_outside_address>\n        <ip_address>1.2.3.4</ip_address>\n      </tunnel_outside_address>\n      <tunnel_inside_address>\n        <ip_address>192.168.0.1</ip_address>\n        <network_mask>255.255.255.252</network_mask>\n        <network_cidr>30</network_cidr>\n      </tunnel_inside_address>\n      <bgp>\n        <asn>12345</asn>\n        <hold_time>30</hold_time>\n      </bgp>\n    </vpn_gateway>\n    <ike>\n      <authentication_protocol>sha</authentication_protocol>\n      <encryption_protocol>aes-128-cbc</encryption_protocol>\n      <lifetime>28800</lifetime>\n      <perfect_forward_secrecy>group2</perfect_forward_secrecy>\n      <mode>main</mode>\n      <pre_shared_key>12345</pre_shared_key>\n    </ike>\n    <ipsec>\n      <protocol>esp</protocol>\n      <authentication_protocol>sha</authentication_protocol>\n      <encryption_protocol>aes-128-cbc</encryption_protocol>\n      <lifetime>3600</lifetime>\n      <perfect_forward_secrecy>group2</perfect_forward_secrecy>\n      <mode>tunnel</mode>\n      <clear_df_bit>true</clear_df_bit>\n      <fragmentation_before_encryption>true</fragmentation_before_encryption>\n      <tcp_mss_adjustment>1379</tcp_mss_adjustment>\n      <dead_peer_detection>\n        <interval>10</interval>\n        <retries>3</retries>\n      </dead_peer_detection>\n    </ipsec>\n  </ipsec_tunnel>\n  <ipsec_tunnel>\n    <customer_gateway>\n      <tunnel_outside_address>\n        <ip_address>1.2.3.4</ip_address>\n      </tunnel_outside_address>\n      <tunnel_inside_address>\n        <ip_address>192.168.0.1</ip_address>\n        <network_mask>255.255.255.252</network_mask>\n        <network_cidr>30</network_cidr>\n      </tunnel_inside_address>\n      <bgp>\n        <asn>12345</asn>\n        <hold_time>30</hold_time>\n      </bgp>\n    </customer_gateway>\n    <vpn_gateway>\n      <tunnel_outside_address>\n        <ip_address>1.2.3.4</ip_address>\n      </tunnel_outside_address>\n      <tunnel_inside_address>\n        <ip_address>192.168.0.1</ip_address>\n        <network_mask>255.255.255.252</network_mask>\n        <network_cidr>30</network_cidr>\n      </tunnel_inside_address>\n      <bgp>\n        <asn>12345</asn>\n        <hold_time>30</hold_time>\n      </bgp>\n    </vpn_gateway>\n    <ike>\n      <authentication_protocol>sha</authentication_protocol>\n      <encryption_protocol>aes-128-cbc</encryption_protocol>\n      <lifetime>28800</lifetime>\n      <perfect_forward_secrecy>group2</perfect_forward_secrecy>\n      <mode>main</mode>\n      <pre_shared_key>12345</pre_shared_key>\n    </ike>\n    <ipsec>\n      <protocol>esp</protocol>\n      <authentication_protocol>sha</authentication_protocol>\n      <encryption_protocol>aes-128-cbc</encryption_protocol>\n      <lifetime>3600</lifetime>\n      <perfect_forward_secrecy>group2</perfect_forward_secrecy>\n      <mode>tunnel</mode>\n      <clear_df_bit>true</clear_df_bit>\n      <fragmentation_before_encryption>true</fragmentation_before_encryption>\n      <tcp_mss_adjustment>1379</tcp_mss_adjustment>\n      <dead_peer_detection>\n        <interval>10</interval>\n        <retries>3</retries>\n      </dead_peer_detection>\n    </ipsec>\n  </ipsec_tunnel>\n</vpn_connection>",
            "CustomerGatewayId": "cgw-xxxxxxxxxxxxxxxxx",
            "Category": "VPN",
            "State": "available",
            "Type": "ipsec.1",
            "VpnConnectionId": "vpn-xxxxxxxxxxxxxxxxx",
            "VpnGatewayId": "vgw-xxxxxxxxxxxxxxxxx",
            "GatewayAssociationState": "associated",
            "Options": {
                "EnableAcceleration": false,
                "StaticRoutesOnly": false,
                "LocalIpv4NetworkCidr": "0.0.0.0/0",
                "RemoteIpv4NetworkCidr": "0.0.0.0/0",
                "OutsideIpAddressType": "PublicIpv4",
                "TunnelInsideIpVersion": "ipv4"
            },
            "Routes": [],
            "Tags": [
                {
                    "Key": "Name",
                    "Value": "xxx"
                }
            ],
            "VgwTelemetry": [
                {
                    "AcceptedRouteCount": 0,
                    "LastStatusChange": "2023-05-02 10:47:58+00:00",
                    "OutsideIpAddress": "xxx.xxx.xxx.xxx",
                    "Status": "UP",
                    "StatusMessage": "0 BGP ROUTES"
                },
                {
                    "AcceptedRouteCount": 0,
                    "LastStatusChange": "2023-04-29 01:09:41+00:00",
                    "OutsideIpAddress": "xxx.xxx.xxx.xxx",
                    "Status": "UP",
                    "StatusMessage": "0 BGP ROUTES"
                }
            ],
            # Customer Gateways: https://boto3.amazonaws.com/v1/documentation/api/1.26.86/reference/services/ec2/client/describe_customer_gateways.html
            "CustomerGateways": [
                {
                    "BgpAsn": "64513",
                    "CustomerGatewayId": "cgw-xxxxxxxxxxxxxxxxx",
                    "IpAddress": "xxx.xxx.xxx.xxx",
                    "State": "available",
                    "Type": "ipsec.1",
                    "Tags": [
                        {
                            "Key": "Name",
                            "Value": "xxx"
                        }
                    ]
                }
            ]
        },
        {
            "CustomerGatewayConfiguration": "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<vpn_connection id=\"vpn-xxxxxxxxxxxxxxxxx\">\n  <customer_gateway_id>cgw-xxxxxxxxxxxxxxxxx</customer_gateway_id>\n  <vpn_gateway_id>vgw-xxxxxxxxxxxxxxxxx</vpn_gateway_id>\n  <vpn_connection_type>ipsec.1</vpn_connection_type>\n  <ipsec_tunnel>\n    <customer_gateway>\n      <tunnel_outside_address>\n        <ip_address>1.2.3.4</ip_address>\n      </tunnel_outside_address>\n      <tunnel_inside_address>\n        <ip_address>192.168.0.0</ip_address>\n        <network_mask>255.255.255.252</network_mask>\n        <network_cidr>30</network_cidr>\n      </tunnel_inside_address>\n      <bgp>\n        <asn>12345</asn>\n        <hold_time>30</hold_time>\n      </bgp>\n    </customer_gateway>\n    <vpn_gateway>\n      <tunnel_outside_address>\n        <ip_address>1.2.3.4</ip_address>\n      </tunnel_outside_address>\n      <tunnel_inside_address>\n        <ip_address>192.168.0.1</ip_address>\n        <network_mask>255.255.255.252</network_mask>\n        <network_cidr>30</network_cidr>\n      </tunnel_inside_address>\n      <bgp>\n        <asn>12345</asn>\n        <hold_time>30</hold_time>\n      </bgp>\n    </vpn_gateway>\n    <ike>\n      <authentication_protocol>sha</authentication_protocol>\n      <encryption_protocol>aes-128-cbc</encryption_protocol>\n      <lifetime>28800</lifetime>\n      <perfect_forward_secrecy>group2</perfect_forward_secrecy>\n      <mode>main</mode>\n      <pre_shared_key>12345</pre_shared_key>\n    </ike>\n    <ipsec>\n      <protocol>esp</protocol>\n      <authentication_protocol>sha</authentication_protocol>\n      <encryption_protocol>aes-128-cbc</encryption_protocol>\n      <lifetime>3600</lifetime>\n      <perfect_forward_secrecy>group2</perfect_forward_secrecy>\n      <mode>tunnel</mode>\n      <clear_df_bit>true</clear_df_bit>\n      <fragmentation_before_encryption>true</fragmentation_before_encryption>\n      <tcp_mss_adjustment>1379</tcp_mss_adjustment>\n      <dead_peer_detection>\n        <interval>10</interval>\n        <retries>3</retries>\n      </dead_peer_detection>\n    </ipsec>\n  </ipsec_tunnel>\n  <ipsec_tunnel>\n    <customer_gateway>\n      <tunnel_outside_address>\n        <ip_address>1.2.3.4</ip_address>\n      </tunnel_outside_address>\n      <tunnel_inside_address>\n        <ip_address>192.168.0.1</ip_address>\n        <network_mask>255.255.255.252</network_mask>\n        <network_cidr>30</network_cidr>\n      </tunnel_inside_address>\n      <bgp>\n        <asn>12345</asn>\n        <hold_time>30</hold_time>\n      </bgp>\n    </customer_gateway>\n    <vpn_gateway>\n      <tunnel_outside_address>\n        <ip_address>1.2.3.4</ip_address>\n      </tunnel_outside_address>\n      <tunnel_inside_address>\n        <ip_address>192.168.0.1</ip_address>\n        <network_mask>255.255.255.252</network_mask>\n        <network_cidr>30</network_cidr>\n      </tunnel_inside_address>\n      <bgp>\n        <asn>123456</asn>\n        <hold_time>30</hold_time>\n      </bgp>\n    </vpn_gateway>\n    <ike>\n      <authentication_protocol>sha</authentication_protocol>\n      <encryption_protocol>aes-128-cbc</encryption_protocol>\n      <lifetime>28800</lifetime>\n      <perfect_forward_secrecy>group2</perfect_forward_secrecy>\n      <mode>main</mode>\n      <pre_shared_key>12345</pre_shared_key>\n    </ike>\n    <ipsec>\n      <protocol>esp</protocol>\n      <authentication_protocol>sha</authentication_protocol>\n      <encryption_protocol>aes-128-cbc</encryption_protocol>\n      <lifetime>3600</lifetime>\n      <perfect_forward_secrecy>group2</perfect_forward_secrecy>\n      <mode>tunnel</mode>\n      <clear_df_bit>true</clear_df_bit>\n      <fragmentation_before_encryption>true</fragmentation_before_encryption>\n      <tcp_mss_adjustment>1379</tcp_mss_adjustment>\n      <dead_peer_detection>\n        <interval>10</interval>\n        <retries>3</retries>\n      </dead_peer_detection>\n    </ipsec>\n  </ipsec_tunnel>\n</vpn_connection>",
            "CustomerGatewayId": "cgw-xxxxxxxxxxxxxxxxx",
            "Category": "VPN",
            "State": "available",
            "Type": "ipsec.1",
            "VpnConnectionId": "vpn-xxxxxxxxxxxxxxxxx",
            "VpnGatewayId": "vgw-xxxxxxxxxxxxxxxxx",
            "GatewayAssociationState": "associated",
            "Options": {
                "EnableAcceleration": false,
                "StaticRoutesOnly": false,
                "LocalIpv4NetworkCidr": "0.0.0.0/0",
                "RemoteIpv4NetworkCidr": "0.0.0.0/0",
                "OutsideIpAddressType": "PublicIpv4",
                "TunnelInsideIpVersion": "ipv4"
            },
            "Routes": [],
            "Tags": [
                {
                    "Key": "Name",
                    "Value": "xxx"
                }
            ],
            "VgwTelemetry": [
                {
                    "AcceptedRouteCount": 3,
                    "LastStatusChange": "2023-04-11 12:31:37+00:00",
                    "OutsideIpAddress": "xxx.xxx.xxx.xxx",
                    "Status": "UP",
                    "StatusMessage": "3 BGP ROUTES"
                },
                {
                    "AcceptedRouteCount": 3,
                    "LastStatusChange": "2023-04-11 12:30:49+00:00",
                    "OutsideIpAddress": "xxx.xxx.xxx.xxx",
                    "Status": "UP",
                    "StatusMessage": "3 BGP ROUTES"
                }
            ],
            "CustomerGateways": [
                {
                    "BgpAsn": "64513",
                    "CustomerGatewayId": "cgw-xxxxxxxxxxxxxxxxx",
                    "IpAddress": "xxx.xxx.xxx.xxx",
                    "State": "available",
                    "Type": "ipsec.1",
                    "Tags": [
                        {
                            "Key": "Name",
                            "Value": "xxx"
                        }
                    ]
                }
            ]
        }
    ]
}
```
</details>
<br />

  
## AWS Transit Gateway

### Introduction
The configuration of the AWS Transit Gateway is dependent on the AWS SDK response of the AWS Transit Gateway as the primary response. The full resource configuration consists of some associated resources' data, including `transit gateway connects`, ` transit gateway vpc attachments`.

### Content
Below are the AWS SDK used to generate this configuration.
|**Resource/Action**|**Relationship**|**AWS SDK document**|
|------|------|------|
| describe_transit_gateways | self | https://boto3.amazonaws.com/v1/documentation/api/1.26.86/reference/services/ec2/client/describe_transit_gateways.html|
| describe_transit_gateway_connects | TransitGatewayConnects | https://boto3.amazonaws.com/v1/documentation/api/1.26.86/reference/services/ec2/client/describe_transit_gateway_connects.html |
| describe_transit_gateway_vpc_attachments | TransitGatewayVpcAttachments | https://boto3.amazonaws.com/v1/documentation/api/1.26.86/reference/services/ec2/client/describe_transit_gateway_vpc_attachments.html |


### Sample
<details><summary>Configuration File</summary>

```json
{
    "netbrainNotes": "This config file is generated via API",
    "netbrainHostName": "xxx(tgw-xxxxxxxxxxxxxxxxx)",
    "TransitGatewayId": "tgw-xxxxxxxxxxxxxxxxx",
    "TransitGatewayArn": "xxx",
    "State": "available",
    "OwnerId": "xxx",
    "Description": "",
    "CreationTime": "2019-10-17 14:34:58+00:00",
    "Options": {
        "AmazonSideAsn": 12345,
        "TransitGatewayCidrBlocks": [
            "192.168.0.0/24",
            "192.168.0.0/24"
        ],
        "AutoAcceptSharedAttachments": "enable",
        "DefaultRouteTableAssociation": "enable",
        "AssociationDefaultRouteTableId": "tgw-rtb-xxxxxxxxxxxxxxxxx",
        "DefaultRouteTablePropagation": "enable",
        "PropagationDefaultRouteTableId": "tgw-rtb-xxxxxxxxxxxxxxxxx",
        "VpnEcmpSupport": "enable",
        "DnsSupport": "enable",
        "MulticastSupport": "disable"
    },
    "Tags": [
        {
            "Key": "xxName",
            "Value": "xxx"
        },
        {
            "Key": "xxName",
            "Value": "xxx"
        },
        {
            "Key": "Name",
            "Value": "xxx"
        }
    ],
    # Transit Gateway Connects: https://boto3.amazonaws.com/v1/documentation/api/1.26.86/reference/services/ec2/client/describe_transit_gateway_connects.html
    "TransitGatewayConnects": [],
    # Transit Gateway Vpc Attachments: https://boto3.amazonaws.com/v1/documentation/api/1.26.86/reference/services/ec2/client/describe_transit_gateway_vpc_attachments.html
    "TransitGatewayVpcAttachments": [
        {
            "TransitGatewayAttachmentId": "tgw-attach-xxxxxxxxxxxxxxxxx",
            "TransitGatewayId": "tgw-xxxxxxxxxxxxxxxxx",
            "VpcId": "vpc-xxxxxxxxxxxxxxxxx",
            "VpcOwnerId": "xxxxxxxxxxxxxxxxx",
            "State": "available",
            "SubnetIds": [
                "subnet-xxxxxxxxxxxxxxxxx",
                "subnet-xxxxxxxxxxxxxxxxx"
            ],
            "CreationTime": "2019-12-12 20:23:19+00:00",
            "Options": {
                "DnsSupport": "enable",
                "Ipv6Support": "disable",
                "ApplianceModeSupport": "disable"
            },
            "Tags": [
                {
                    "Key": "Name",
                    "Value": "xxx"
                }
            ]
        },
        {
            "TransitGatewayAttachmentId": "tgw-attach-xxxxxxxxxxxxxxxxx",
            "TransitGatewayId": "tgw-xxxxxxxxxxxxxxxxx",
            "VpcId": "vpc-xxxxxxxxxxxxxxxxx",
            "VpcOwnerId": "xxxxxxxxxxxxxxxxx",
            "State": "available",
            "SubnetIds": [
                "subnet-xxxxxxxxxxxxxxxxx",
                "subnet-xxxxxxxxxxxxxxxxx",
                "subnet-xxxxxxxxxxxxxxxxx",
                "subnet-xxxxxxxxxxxxxxxxx",
                "subnet-xxxxxxxxxxxxxxxxx",
                "subnet-xxxxxxxxxxxxxxxxx"
            ],
            "CreationTime": "2019-10-17 14:41:07+00:00",
            "Options": {
                "DnsSupport": "enable",
                "Ipv6Support": "disable",
                "ApplianceModeSupport": "disable"
            },
            "Tags": [
                {
                    "Key": "Name",
                    "Value": "xxx"
                }
            ]
        },
        {
            "TransitGatewayAttachmentId": "tgw-attach-xxxxxxxxxxxxxxxxx",
            "TransitGatewayId": "tgw-xxxxxxxxxxxxxxxxx",
            "VpcId": "vpc-xxxxxxxxxxxxxxxxx",
            "VpcOwnerId": "xxxxxxxxxxxxxxxxx",
            "State": "available",
            "SubnetIds": [
                "subnet-xxxxxxxxxxxxxxxxx",
                "subnet-xxxxxxxxxxxxxxxxx"
            ],
            "CreationTime": "2023-04-19 02:05:28+00:00",
            "Options": {
                "DnsSupport": "enable",
                "Ipv6Support": "disable",
                "ApplianceModeSupport": "disable"
            },
            "Tags": []
        },
        {
            "TransitGatewayAttachmentId": "tgw-attach-xxxxxxxxxxxxxxxxx",
            "TransitGatewayId": "tgw-xxxxxxxxxxxxxxxxx",
            "VpcId": "vpc-xxxxxxxxxxxxxxxxx",
            "VpcOwnerId": "xxxxxxxxxxxxxxxxx",
            "State": "available",
            "SubnetIds": [
                "subnet-xxxxxxxxxxxxxxxxx",
                "subnet-xxxxxxxxxxxxxxxxx",
                "subnet-xxxxxxxxxxxxxxxxx",
                "subnet-xxxxxxxxxxxxxxxxx",
                "subnet-xxxxxxxxxxxxxxxxx",
                "subnet-xxxxxxxxxxxxxxxxx"
            ],
            "CreationTime": "2021-03-08 01:33:15+00:00",
            "Options": {
                "DnsSupport": "enable",
                "Ipv6Support": "disable",
                "ApplianceModeSupport": "disable"
            },
            "Tags": []
        },
        {
            "TransitGatewayAttachmentId": "tgw-attach-xxxxxxxxxxxxxxxxx",
            "TransitGatewayId": "tgw-xxxxxxxxxxxxxxxxx",
            "VpcId": "vpc-xxxxxxxxxxxxxxxxx",
            "VpcOwnerId": "xxxxxxxxxxxxxxxxx",
            "State": "available",
            "SubnetIds": [
                "subnet-xxxxxxxxxxxxxxxxx",
                "subnet-xxxxxxxxxxxxxxxxx"
            ],
            "CreationTime": "2023-04-19 03:42:13+00:00",
            "Options": {
                "DnsSupport": "enable",
                "Ipv6Support": "disable",
                "ApplianceModeSupport": "disable"
            },
            "Tags": [
                {
                    "Key": "Name",
                    "Value": "xxx"
                }
            ]
        },
        {
            "TransitGatewayAttachmentId": "tgw-attach-xxxxxxxxxxxxxxxxx",
            "TransitGatewayId": "tgw-xxxxxxxxxxxxxxxxx",
            "VpcId": "vpc-xxxxxxxxxxxxxxxxx",
            "VpcOwnerId": "xxxxxxxxxxxxxxxxx",
            "State": "available",
            "SubnetIds": [
                "subnet-xxxxxxxxxxxxxxxxx"
            ],
            "CreationTime": "2023-04-19 01:59:31+00:00",
            "Options": {
                "DnsSupport": "enable",
                "Ipv6Support": "disable",
                "ApplianceModeSupport": "disable"
            },
            "Tags": [
                {
                    "Key": "Name",
                    "Value": "xxx"
                }
            ]
        }
    ]
}
```
</details>
<br />

## AWS Internet Gateway

### Introduction
The configuration of the AWS Internet Gateway relies solely on the corresponding AWS SDK of the internet gateway. The AWS SDK provides detailed information regarding the configuration of the internet gateway, including its connectivity, security, etc.

### Content
Below are the AWS SDK used to generate this configuration.
|**Resource/Action**|**Relationship**|**AWS SDK document**|
|------|------|------|
| describe_internet_gateways | self | https://boto3.amazonaws.com/v1/documentation/api/1.26.86/reference/services/ec2/client/describe_internet_gateways.html|


### Sample
<details><summary>Configuration File</summary>

```json
{
    "netbrainNotes": "This config file is generated via API",
    "netbrainHostName": "xxx(igw-xxxxxxxxxxxxxxxxx)",
    "Attachments": [
        {
            "State": "available",
            "VpcId": "vpc-xxxxxxxxxxxxxxxxx"
        }
    ],
    "InternetGatewayId": "igw-xxxxxxxxxxxxxxxxx",
    "OwnerId": "xxx",
    "Tags": [
        {
            "Key": "DeletedName",
            "Value": "xxx"
        },
        {
            "Key": "Name",
            "Value": "xxx"
        }
    ]
}
```
</details>
<br />
  

## AWS Egress Internet Gateway

### Introduction
The configuration of the AWS Egress Internet Gateway relies solely on the corresponding AWS SDK of the egress internet gateway. The AWS SDK provides detailed information regarding the configuration of the egress internet gateway, including its connectivity, security, etc.

### Content
Below are the AWS SDK used to generate this configuration.
|**Resource/Action**|**Relationship**|**AWS SDK document**|
|------|------|------|
| describe_egress_only_internet_gateways | self | https://boto3.amazonaws.com/v1/documentation/api/1.26.86/reference/services/ec2/client/describe_egress_only_internet_gateways.html|


### Sample
<details><summary>Configuration File</summary>

```json
{
    "netbrainNotes": "This config file is generated via API",
    "netbrainHostName": "xxx(eigw-xxxxxxxxxxxxxxxxx)",
    "Attachments": [
        {
            "State": "attached",
            "VpcId": "vpc-xxxxxxxxxxxxxxxxx"
        }
    ],
    "EgressOnlyInternetGatewayId": "eigw-xxxxxxxxxxxxxxxxx",
    "Tags": [
        {
            "Key": "DeletedName",
            "Value": "xxx"
        },
        {
            "Key": "Name",
            "Value": "xxx"
        }
    ]
}
```
</details>
<br />
  

## AWS Load Balancer

### Introduction
The configuration of the **AWS Classic Load Balancer** is dependent on the AWS SDK response of the Load Balancer as the primary response. The full resource configuration consists of some associated resources' data, including `load balancer policies`, `instance health`.

### Content
Below are the AWS SDK used to generate this configuration.
|**Resource/Action**|**Relationship**|**AWS SDK document**|
|------|------|------|
| describe_load_balancers | self | https://boto3.amazonaws.com/v1/documentation/api/1.26.86/reference/services/ec2/client/describe_load_balancers.html |
| describe_load_balancer_policies | PolicyDescriptions | https://boto3.amazonaws.com/v1/documentation/api/1.26.86/reference/services/ec2/client/describe_load_balancer_policies.html |
| describe_instance_health | InstanceStates | https://boto3.amazonaws.com/v1/documentation/api/1.26.86/reference/services/ec2/client/describe_instance_health.html |

### Sample
<details><summary>Configuration File</summary>

```json
{
    "netbrainNotes": "This config file is generated via API",
    "netbrainHostName": "xxx(us-east-1-CLB-Test)",
    "LoadBalancerName": "xxx",
    "DNSName": "xxx",
    "CanonicalHostedZoneName": "xxx",
    "CanonicalHostedZoneNameID": "xxxxxxxxxxxxx",
    "ListenerDescriptions": [
        {
            "Listener": {
                "Protocol": "HTTP",
                "LoadBalancerPort": 80,
                "InstanceProtocol": "HTTP",
                "InstancePort": 80
            },
            "PolicyNames": []
        }
    ],
    "Policies": {
        "AppCookieStickinessPolicies": [],
        "LBCookieStickinessPolicies": [],
        "OtherPolicies": []
    },
    "BackendServerDescriptions": [],
    "AvailabilityZones": [
        "us-east-1a",
        "us-east-1f"
    ],
    "Subnets": [
        "subnet-xxxxxxxxxxxxxxxxx",
        "subnet-xxxxxxxxxxxxxxxxx"
    ],
    "VPCId": "vpc-xxxxxxxxxxxxxxxxx",
    "Instances": [
        {
            "InstanceId": "i-xxxxxxxxxxxxxxxxx"
        },
        {
            "InstanceId": "i-xxxxxxxxxxxxxxxxx"
        }
    ],
    "HealthCheck": {
        "Target": "HTTP:80/",
        "Interval": 30,
        "Timeout": 5,
        "UnhealthyThreshold": 2,
        "HealthyThreshold": 10
    },
    "SourceSecurityGroup": {
        "OwnerAlias": "xxxxxxxxxxxxxxxxx",
        "GroupName": "12345"
    },
    "SecurityGroups": [
        "sg-xxxxxxxxxxxxxxxxx",
        "sg-xxxxxxxxxxxxxxxxx"
    ],
    "CreatedTime": "2020-06-12 01:15:52.980000+00:00",
    "Scheme": "internet-facing",
    # Policy Descriptions: https://boto3.amazonaws.com/v1/documentation/api/1.26.86/reference/services/ec2/client/describe_load_balancer_policies.html
    "PolicyDescriptions": [],
    # Instance States: https://boto3.amazonaws.com/v1/documentation/api/1.26.86/reference/services/ec2/client/describe_instance_health.html
    "InstanceStates": [
        {
            "InstanceId": "i-xxxxxxxxxxxxxxxxx",
            "State": "InService",
            "ReasonCode": "N/A",
            "Description": "N/A"
        },
        {
            "InstanceId": "i-xxxxxxxxxxxxxxxxx",
            "State": "InService",
            "ReasonCode": "N/A",
            "Description": "N/A"
        }
    ]
}
```
</details>
<br />
  

## AWS Load Balancer v2

### Introduction
AWS Load Balancer v2 includes **Network Load Balancer**, **Application Load Balancer**, and **Gateway Load Balancer**. The configuration of the AWS Load Balancer v2 is dependent on the AWS SDK response of the Load Balancer v2 as the primary response. The full resource configuration consists of some associated resources' data, including `load balancer policies`, `instance health`.

### Content
Below are the AWS SDK used to generate this configuration.
|**Resource/Action**|**Relationship**|**AWS SDK document**|
|------|------|------|
| describe_load_balancers | self | https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/elbv2.html |


### Sample
<details><summary>Configuration File</summary>

```json
{
    "netbrainNotes": "This config file is generated via API",
    "netbrainHostName": "xxx(xxxxxxxxxxxxxxxxx)",
    "LoadBalancerArn": "xxx",
    "DNSName": "xxx",
    "CanonicalHostedZoneId": "12345",
    "CreatedTime": "2020-04-16 18:30:00.094000+00:00",
    "LoadBalancerName": "xxx",
    "Scheme": "internet-facing",
    "VpcId": "vpc-xxxxxxxxxxxxxxxxx",
    "State": {
        "Code": "active"
    },
    "Type": "network",
    "AvailabilityZones": [
        {
            "ZoneName": "us-east-1f",
            "SubnetId": "subnet-xxxxxxxxxxxxxxxxx",
            "LoadBalancerAddresses": []
        },
        {
            "ZoneName": "us-east-1b",
            "SubnetId": "subnet-xxxxxxxxxxxxxxxxx",
            "LoadBalancerAddresses": []
        },
        {
            "ZoneName": "us-east-1a",
            "SubnetId": "subnet-xxxxxxxxxxxxxxxxx",
            "LoadBalancerAddresses": []
        }
    ],
    "IpAddressType": "ipv4"
}
```
</details>
<br />
  

## AWS Direct Connect Gateway

### Introduction
The configuration of the AWS direct connect gateway is dependent on the AWS SDK response of the direct connect gateway as the primary response. The full resource configuration consists of some associated resources' data, including `attachments`, `associations`, and `association proposals`.

### Content
Below are the AWS SDK used to generate this configuration.
|**Resource/Action**|**Relationship**|**AWS SDK document**|
|------|------|------|
| describe_direct_connect_gateways | self | https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/directconnect/client/describe_virtual_interfaces.html |
| describe_direct_connect_gateway_attachments | directConnectGatewayAttachments | https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/directconnect/client/describe_direct_connect_gateway_attachments.html |
| describe_direct_connect_gateway_associations | directConnectGatewayAssociations | https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/directconnect/client/describe_direct_connect_gateway_associations.html |
| describe_direct_connect_gateway_association_proposals | directConnectGatewayAssociationProposals | https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/directconnect/client/describe_direct_connect_gateway_association_proposals.html |

### Sample
<details><summary>Configuration File</summary>

```json
{
    "netbrainNotes": "This config file is generated via API",
    "netbrainHostName": "xxx(xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx)",
    "directConnectGatewayId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "directConnectGatewayName": "xxx",
    "amazonSideAsn": 12345,
    "ownerAccount": "xxxxxxxxxxxx",
    "directConnectGatewayState": "available",
    # Direct Connect Gateway Attachments: https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/directconnect/client/describe_direct_connect_gateway_attachments.html
    "directConnectGatewayAttachments": [
        {
            "directConnectGatewayId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "virtualInterfaceId": "dxvif-xxxxxxxxxxxx",
            "virtualInterfaceRegion": "us-east-2",
            "virtualInterfaceOwnerAccount": "xxxxxxxxxxxx",
            "attachmentState": "attached",
            "attachmentType": "privateVirtualInterface"
        },
        {
            "directConnectGatewayId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "virtualInterfaceId": "dxvif-xxxxxxxx",
            "virtualInterfaceRegion": "us-east-2",
            "virtualInterfaceOwnerAccount": "xxxxxxxxxxxx",
            "attachmentState": "attached",
            "attachmentType": "privateVirtualInterface"
        }
    ],
    # Direct Connect Gateway Associations: https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/directconnect/client/describe_direct_connect_gateway_associations.html
    "directConnectGatewayAssociations": [
        {
            "directConnectGatewayId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "directConnectGatewayOwnerAccount": "xxxxxxxxxxxx",
            "associationState": "associated",
            "associatedGateway": {
                "id": "vgw-xxxxxxxxxxxx",
                "type": "virtualPrivateGateway",
                "ownerAccount": "xxxxxxxxxxxx",
                "region": "us-west-2"
            },
            "associationId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "allowedPrefixesToDirectConnectGateway": [
                {
                    "cidr": "192.168.0.0/24"
                }
            ],
            "virtualGatewayId": "vgw-xxxxxxxxxxxx",
            "virtualGatewayRegion": "us-west-2",
            "virtualGatewayOwnerAccount": "xxxxxxxxxxxx"
        },
        {
            "directConnectGatewayId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "directConnectGatewayOwnerAccount": "xxxxxxxxxxxx",
            "associationState": "associated",
            "associatedGateway": {
                "id": "vgw-xxxxxxxxxxxx",
                "type": "virtualPrivateGateway",
                "ownerAccount": "xxxxxxxxxxxx",
                "region": "us-east-1"
            },
            "associationId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "allowedPrefixesToDirectConnectGateway": [
                {
                    "cidr": "192.168.0.0/24"
                },
                {
                    "cidr": "192.168.0.0/24"
                }
            ],
            "virtualGatewayId": "vgw-xxxxxxxxxxxx",
            "virtualGatewayRegion": "us-east-1",
            "virtualGatewayOwnerAccount": "xxxxxxxxxxxx"
        },
        {
            "directConnectGatewayId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "directConnectGatewayOwnerAccount": "xxxxxxxxxxxx",
            "associationState": "associated",
            "associatedGateway": {
                "id": "vgw-xxxxxxxxxxxx",
                "type": "virtualPrivateGateway",
                "ownerAccount": "xxxxxxxxxxxx",
                "region": "us-east-2"
            },
            "associationId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "allowedPrefixesToDirectConnectGateway": [
                {
                    "cidr": "192.168.0.0/24"
                },
                {
                    "cidr": "192.168.0.0/24"
                }
            ],
            "virtualGatewayId": "vgw-xxxxxxxxxxxx",
            "virtualGatewayRegion": "us-east-2",
            "virtualGatewayOwnerAccount": "xxxxxxxxxxxx"
        },
        {
            "directConnectGatewayId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "directConnectGatewayOwnerAccount": "xxxxxxxxxxxx",
            "associationState": "associated",
            "associatedGateway": {
                "id": "vgw-xxxxxxxxxxxx",
                "type": "virtualPrivateGateway",
                "ownerAccount": "xxxxxxxxxxxx",
                "region": "us-east-2"
            },
            "associationId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "allowedPrefixesToDirectConnectGateway": [
                {
                    "cidr": "192.168.0.0/24"
                }
            ],
            "virtualGatewayId": "vgw-xxxxxxxxxxxx",
            "virtualGatewayRegion": "us-east-2",
            "virtualGatewayOwnerAccount": "xxxxxxxxxxxx"
        }
    ],
    # Direct Connect Gateway Association Proposals: https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/directconnect/client/describe_direct_connect_gateway_association_proposals.html
    "directConnectGatewayAssociationProposals": []
}
```
</details>
<br />
  

## AWS NAT Gateway

### Introduction
The configuration of the AWS NAT gateway is dependent on the AWS SDK response of the NAT gateway as the primary response. The full resource configuration consists of some associated resources' data, including `subnets` as well.

### Content
Below are the AWS SDK used to generate this configuration.
|**Resource/Action**|**Relationship**|**AWS SDK document**|
|------|------|------|
| describe_nat_gateways | self | https://boto3.amazonaws.com/v1/documentation/api/1.26.86/reference/services/ec2/client/describe_nat_gateways.html |
| describe_subnets | Subnet | https://boto3.amazonaws.com/v1/documentation/api/1.26.86/reference/services/ec2/client/describe_subnets.html |

### Sample
<details><summary>Configuration File</summary>

```json
{
    "netbrainNotes": "This config file is generated via API",
    "netbrainHostName": "xxx(nat-xxxxxxxxxxxx)",
    "CreateTime": "2020-03-30 16:02:01+00:00",
    "NatGatewayAddresses": [
        {
            "AllocationId": "eipalloc-xxxxxxxxxxxx",
            "NetworkInterfaceId": "eni-xxxxxxxxxxxx",
            "PrivateIp": "xxx.xxx.xxx.xxx",
            "PublicIp": "xxx.xxx.xxx.xxx"
        }
    ],
    "NatGatewayId": "nat-xxxxxxxxxxxx",
    "State": "available",
    "SubnetId": "subnet-xxxxxxxxxxxx",
    "VpcId": "vpc-xxxxxxxxxxxx",
    "Tags": [
        {
            "Key": "Name",
            "Value": "xxx"
        }
    ],
    "ConnectivityType": "public",
    # Subnet: https://boto3.amazonaws.com/v1/documentation/api/1.26.86/reference/services/ec2/client/describe_subnets.html
    "Subnet": [
        {
            "AvailabilityZone": "ca-central-1b",
            "AvailabilityZoneId": "cac1-az2",
            "AvailableIpAddressCount": 247,
            "CidrBlock": "192.168.0.0/24"
            "DefaultForAz": false,
            "MapPublicIpOnLaunch": false,
            "MapCustomerOwnedIpOnLaunch": false,
            "State": "available",
            "SubnetId": "subnet-xxxxxxxxxxxx",
            "VpcId": "vpc-xxxxxxxxxxxx",
            "OwnerId": "xxx",
            "AssignIpv6AddressOnCreation": false,
            "Ipv6CidrBlockAssociationSet": [],
            "Tags": [
                {
                    "Key": "Name",
                    "Value": "xxx"
                }
            ],
            "SubnetArn": "xxx",
            "EnableDns64": false,
            "Ipv6Native": false,
            "PrivateDnsNameOptionsOnLaunch": {
                "HostnameType": "ip-name",
                "EnableResourceNameDnsARecord": false,
                "EnableResourceNameDnsAAAARecord": false
            }
        }
    ]
}
```
</details>
<br />


## AWS Firewall

### Introduction
The configuration of the AWS Firewall relies solely on the corresponding AWS SDK of the AWS network firewall. The AWS SDK provides detailed information regarding the configuration of the network firewall, including its connectivity, security, etc.

### Content
Below are the AWS SDK used to generate this configuration.
|**Resource/Action**|**Relationship**|**AWS SDK document**|
|------|------|------|
| describe_firewall | self | https://boto3.amazonaws.com/v1/documentation/api/1.26.86/reference/services/network-firewall/client/describe_firewall.html |

### Sample
<details><summary>Configuration File</summary>

```json
{
    "netbrainNotes": "This config file is generated via API",
    "netbrainHostName": "xxx(us-east-1-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx)",
    "UpdateToken": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "Firewall": {
        "FirewallName": "xxx",
        "FirewallArn": "xxx",
        "FirewallPolicyArn": "xxx",
        "VpcId": "vpc-xxxxxxxxxxxx",
        "SubnetMappings": [
            {
                "SubnetId": "subnet-xxxxxxxxxxxx"
            },
            {
                "SubnetId": "subnet-xxxxxxxxxxxx"
            }
        ],
        "DeleteProtection": false,
        "SubnetChangeProtection": false,
        "FirewallPolicyChangeProtection": false,
        "FirewallId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "Tags": [],
        "EncryptionConfiguration": {
            "KeyId": "AWS_OWNED_KMS_KEY",
            "Type": "AWS_OWNED_KMS_KEY"
        }
    },
    "FirewallStatus": {
        "Status": "READY",
        "ConfigurationSyncStateSummary": "IN_SYNC",
        "SyncStates": {
            "us-east-1a": {
                "Attachment": {
                    "SubnetId": "subnet-xxxxxxxxxxxx",
                    "EndpointId": "vpce-xxxxxxxxxxxx",
                    "Status": "READY"
                },
                "Config": {
                    "arn:aws:network-firewall:xxx:xxxxxxxxxxxx:stateful-rulegroup/xxx": {
                        "SyncStatus": "IN_SYNC",
                        "UpdateToken": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
                    }
                }
            },
            "us-east-1f": {
                "Attachment": {
                    "SubnetId": "subnet-xxxxxxxxxxxx",
                    "EndpointId": "vpce-xxxxxxxxxxxx",
                    "Status": "READY"
                },
                "Config": {
                    "arn:aws:network-firewall:us-east-1:xxxxxxxxxxxx:stateful-rulegroup/2ND-Stateful-Rule-Group": {
                        "SyncStatus": "IN_SYNC",
                        "UpdateToken": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
                    },
                    "arn:aws:network-firewall:us-east-1:xxxxxxxxxxxx:stateless-rulegroup/Fw-to-Stateful-Rule": {
                        "SyncStatus": "IN_SYNC",
                        "UpdateToken": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
                    },
                    "arn:aws:network-firewall:us-east-1:aws-managed:stateful-rulegroup/MalwareDomainsStrictOrder": {
                        "SyncStatus": "IN_SYNC",
                        "UpdateToken": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
                    },
                    "arn:aws:network-firewall:us-east-1:xxxxxxxxxxxx:firewall-policy/Lab-Firewall-2": {
                        "SyncStatus": "IN_SYNC",
                        "UpdateToken": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
                    },
                    "arn:aws:network-firewall:us-east-1:xxxxxxxxxxxx:stateless-rulegroup/Lab-Firewall-1-Stateless-Rule": {
                        "SyncStatus": "IN_SYNC",
                        "UpdateToken": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
                    }
                }
            }
        }
    },
    "ResponseMetadata": {
        "RequestId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "HTTPStatusCode": 200,
        "HTTPHeaders": {
            "x-amzn-requestid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "content-type": "application/x-amz-json-1.0",
            "content-length": "2776",
            "date": "Fri, 02 Jun 2023 01:59:34 GMT"
        },
        "RetryAttempts": 0
    }
}
```
</details>
<br />
  


## AWS DX Router

### Introduction
The configuration of the AWS DX Router relies on the corresponding AWS SDK of the AWS direct connect virtual interface and connections. The AWS SDK provides detailed information regarding the configuration of the dx router, including its connectivity, security, etc.

### Content
Below are the AWS SDK used to generate this configuration.
|**Resource/Action**|**Relationship**|**AWS SDK document**|
|------|------|------|
| describe_virtual_interfaces | self | https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/directconnect/client/describe_virtual_interfaces.html |
| describe_connections | connections | https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/directconnect/client/describe_connections.html |

### Sample
<details><summary>Configuration File</summary>

```json
{
    "netbrainNotes": "This config file is generated via API",
    "netbrainHostName": "EqDC2-370kou9loeqh0(dxcon-fgw645uf)",
    "ownerAccount": "xxxxxxxxxxx",
    "virtualInterfaceId": "dxvif-ffpma0ph",
    "location": "EqDC2",
    "connectionId": "xxxxxxxxxxx",
    "virtualInterfaceType": "private",
    "virtualInterfaceName": "Virginia-VLAN-2",
    "vlan": 468,
    "asn": 8030,
    "amazonSideAsn": 64520,
    "amazonAddress": "172.16.253.18/30",
    "customerAddress": "172.16.253.17/30",
    "addressFamily": "ipv4",
    "virtualInterfaceState": "available",
    "mtu": 1500,
    "jumboFrameCapable": true,
    "virtualGatewayId": "vgw-0ed7b3566fcdcc27e",
    "directConnectGatewayId": "",
    "routeFilterPrefixes": [],
    "bgpPeers": [
        {
            "bgpPeerId": "dxpeer-fgiab2af",
            "asn": 8030,
            "addressFamily": "ipv4",
            "amazonAddress": "172.16.253.18/30",
            "customerAddress": "172.16.253.17/30",
            "bgpPeerState": "available",
            "bgpStatus": "up",
            "awsDeviceV2": "EqDC2-370kou9loeqh0",
            "awsLogicalDeviceId": "EqDC2-370kou9loeqh0"
        }
    ],
    "region": "us-east-1",
    "awsDeviceV2": "EqDC2-370kou9loeqh0",
    "awsLogicalDeviceId": "EqDC2-370kou9loeqh0",
    "tags": [],
    "siteLinkEnabled": false,
    "connections": [
        {
            "ownerAccount": "xxxxxxxxxxx",
            "connectionId": "xxxxxxxxxxx",
            "connectionName": "xxxxxxxxxxx",
            "connectionState": "xxxxxxxxxxx",
            "region": "xxxxxxxxxxx",
            "location": "xxxxxxxxxxx",
            "bandwidth": "xxxxxxxxxxx",
            "vlan": 123,
            "partnerName": "xxxxxxxxxxx",
            "loaIssueTime": "xxxxxxxxxxx",
            "lagId": "xxxxxxxxxxx",
            "awsDevice": "xxxxxxxxxxx",
            "jumboFrameCapable": true,
            "awsDeviceV2": "xxxxxxxxxxx",
            "awsLogicalDeviceId": "xxxxxxxxxxx",
            "hasLogicalRedundancy": "xxxxxxxxxxx",
            "tags": [
                {
                    "key": "xxxxxxxxxxx",
                    "value": "xxxxxxxxxxx"
                }
            ],
            "providerName": "xxxxxxxxxxx",
            "macSecCapable": true,
            "portEncryptionStatus": "xxxxxxxxxxx",
            "encryptionMode": "xxxxxxxxxxx",
            "macSecKeys": [
                {
                    "secretARN": "xxxxxxxxxxx",
                    "ckn": "xxxxxxxxxxx",
                    "state": "xxxxxxxxxxx",
                    "startOn": "xxxxxxxxxxx"
                }
            ]
        }
    ]
}
```
</details>
<br />
  
