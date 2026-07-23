# Azure-Project-Core-Infrastructure
Collection of Azure projects.
## Network Architecture

![Network Diagram](Azure Architecture Diagram.png)

## NSG Configuration

Web subnet NSG — allows HTTP and SSH from my IP only:
![NSG Web Rules](screenshots/02-nsg-rules-web.png)

Data subnet NSG — allows only port 3306 from the web subnet, deny-all beneath it:
![NSG Data Rules](screenshots/03-nsg-rules-data.png)

## Load Balancing Verification

![Server 1 Response](screenshots/04-lb-response-server1.png)
![Server 2 Response](screenshots/05-lb-response-server2.png)

## Network Segmentation Proof

Direct SSH to the data VM fails from my local machine; succeeds only when
routed through the web VM (jump-box pattern):
![SSH Segmentation Test](screenshots/06-ssh-blocked-vs-jumpbox.png)

## Lessons Learned

- Standard Load Balancer backend pool membership removes default outbound
  internet access for VMs without a public IP — had to add a NAT Gateway
  to the web subnet to restore connectivity for package installation.
- Standard Load Balancer uses 5-tuple hashing rather than strict
  round-robin, so browser refreshes on the same connection can appear
  "sticky" to one backend; verified true distribution using repeated
  curl requests instead.
