* What are VPC Flow logs?

* It comprised of IP traffic information
* These logs are useful for troubleshooting network conversations and can be assigned to(Capture Points)

    ```sh
    * VPC
    * Subnet
    * Elastic Network Interface
    ```

###### NOTE: Flow logs don’t capture data, so you can’t do packet analysis even if its un-encrypted.

* Setup VPC Flow Logs

    ```sh
    Go To AWS Console --> VPC --> Select your VPC --> Flow logs --> Create flow log
    ```

```sh
* Filter: Select All(Other options Accept/Reject)
* Destination: Send to CloudWatch Logs
* Destination log group: Go to AWS Console --> CloudWatch --> Logs --> Create log group
```

* After creating flow logs, if you look for the subnet of the VPC, you will see subnet have flow logs associated with it and this is because it inherited flowlogs from VPC.

* Flow Log Record Syntax

    * A flow log record is a space-separated string that has the following format:

```sh
<version> <account-id> <interface-id> <srcaddr> <dstaddr> <srcport> <dstport> <protocol> <packets> <bytes> <start> <end> <action> <log-status>
2 123456789010 eni-abc123de 172.31.16.139 172.31.16.21 20641 22 6 20 4249 1418530010 1418530070 ACCEPT OK
```

![**SampleImage**](https://miro.medium.com/max/1400/1*dg55BLfizLIgkMbEcO8qLw.png)

Reference: [**here**](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html)

* As I applied flow logs at the VPC level and as we see that subnet automatically inherited it, if we go at the instance create under this VPC and check the Network Interface, you will see that even your Elastic Network Interface(ENI) inherited these logs

* Flow logs do not capture all IP traffic. The following types of traffic are not logged:

    * Traffic generated by instances when they contact the Amazon DNS server. If you use your own DNS server, then all traffic to that DNS server is logged.
    * Traffic generated by a Windows instance for Amazon Windows license activation.
    * Traffic to and from 169.254.169.254 for instance metadata.
    * Traffic to and from 169.254.169.123 for the Amazon Time Sync Service.
    * DHCP traffic.
    * Traffic to the reserved IP address for the default VPC router.
    * Traffic between an endpoint network interface and a Network Load Balancer network interface.


### Using Terraform

Check [**flowlogs.tf](https://github.com/rufilboy/100DaysOfDevOps/blob/main/Day%2028%20-Introduction%20to%20VPC%20Flow%20Logs/flowlogs.tf)