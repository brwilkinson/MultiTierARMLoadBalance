# MultiTierARMLoadBalance
Create a domain with 2 DC's, Mid Tier Servers and Member Servers behind a Load Balancer

* There is only a single Public IP on the Load Balancer

* The member server/s are behind NAT on different ports for WSMAN and RDP (for examples)

* The Mid Tier Server/s are Load Balanced on port 80 (for examples)

* There is not direct access via WSMAN or RDP other than the member server/s



## Steps to get up and running
    1. Download the zip file or clone the repo
    2. Right click and open the solution 2DCXMSXMT.sln
    3. In solution explorer, Right click on the solution and click Build Solution

 You should now be ready deploy:

* Ensure you setup the KeyVault for your credentials during deployment, in the same region that you want to deploy 

* View the DeploymentFeatures.md file
