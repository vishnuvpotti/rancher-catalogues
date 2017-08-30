Working with Rancher+NFS

I was trying to setup Jenkins in Rancher server using NFS (Shared storage)
I ran into many issues and finally solved it. Thought will share the steps so that others will benefit.

1) Before getting started with, make sure you have hosts setup in Rancher Environment.

2) Next follow this steps on this link 
    http://rancher.com/docs/rancher/v1.6/en/rancher-services/storage-service/rancher-nfs/

3) Click on Catalog menu in Rancher server and search for nfs. Click on view details button.

4) You will see a screen like below. Enter the details as seen in screenshot. HostIP will be your NFS server.
    Host IP : NFS Server
    Export Base Directory : /nfs
    Mount Options : Blank
    On Remove : retain / purge ( This cna be changed later when deploying containers as well.
    NFS version : 4 or latest.
    Debug : false

    Note : The export base directory should be same as the directory you created as part of Step 2.
    Click on launch button.

5) Navigate to Infrastructure tab to double check storage driver is active. 

6) Now proceed to Stack menu and create a stack.
   I will be creating a Jenkins stack using docker-compose.yml and rancher-compose.yml in this repository

7) You may see an error like 
    Expected state running but got error: Error response from daemon: VolumeDriver.Mount: Failed nsenter -t 5239 -n mount -o ,nfsvers=4     10.0.x.x://xxx /var/lib/rancher/volumes/rancher-nfs/xxx


8) Go to your host machine and edit /etc/exports and add "no_root_squash" to the mount option.     
    It will look like  /nfs    *(rw,sync,no_subtree_check,no_root_squash)

9) Restart the nfs-server with command.    
   systemctl restart nfs-server  ( It varies on linux versions)

10) Delete and recreate the stack as per **Step 6**


11) A jenkins stack with load balancer will be created and accessible now.





