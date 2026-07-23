# Azure-Project-Core-Infrastructure
Collection of Azure projects.
## Network Architecture

![Network Diagram](Azure_Architecture_Diagram.png)

## NSG Configuration

Web subnet NSG — allows HTTP and SSH from my IP only:
![NSG Web Rules](web-ns_rules.png)

Data subnet NSG — allows only port 3306 from the web subnet, deny-all beneath it:
![NSG Data Rules](data_nsg_rules.png)

## Load Balancing Verification

![Server 1 Response](vm-web-01-response.png)
![Server 2 Response](vm-web-02-response.png)
![Server 3 Response](Nginx_default_index.png)

## Network Segmentation Proof

Direct SSH to the data VM fails from my local machine; succeeds only when
routed through the web VM (jump-box pattern):
![SSH Segmentation Test](vm-data-from-local-refusal.png)
![SSH Segmentation Test1](web-vm1-to-data-vm.png)

## Lessons Learned

- Standard Load Balancer backend pool membership removes default outbound
  internet access for VMs without a public IP — had to add a NAT Gateway
  to the web subnet to restore connectivity for package installation.
- Standard Load Balancer uses 5-tuple hashing rather than strict
  round-robin, so browser refreshes on the same connection can appear
  "sticky" to one backend; verified true distribution using repeated
  curl requests instead.
