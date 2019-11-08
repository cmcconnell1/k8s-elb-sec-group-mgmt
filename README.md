# k8s-elb-sec-group-mgmt utility 

#### Overview
Dereferences 'child' ELB security groups from the specified parent's (EKS/kubernetes worker's security groups).
  - Generates color-coded report for the specified parent security group which finds references to child security groups (i.e.: from AWS ELB services with 'Kind: LoadBalancer'), security groups for operators, etc.

  - For Terradatum's use case, the parent security groups are our EKS/Kubernetes Worker groups that are quickly growing and hitting their AWS-enforced maximum SG rule limits--due to automated (CI, etc.) processes.  Therefore these parent EKS/Kubernetes Worker security groups require frequent clean-up/purging, etc.

- Color Codes for Reports 1-3:
  - (Report #1 of 3 in green): Shows all referenced security groups from the specified (parent) security group.
  - (Report #2 of 3 in yellow): Filters all referenced child _ELB_ security groups from the specified (parent) security group.
  - (Report #3 of 3 in red): Filters all referenced child _ELB_ security groups from the specified (parent) security group with a _NIC count=0_ thus being our candidates for dereferncing and deleting.

#### TL;DR:
- Step-1 Execute with `mode=report` to report only:
- i.e.:
  ```console
  ./aws/security-group/k8s-elb-sec-group-mgmt parent=sg-00d1eb0508f5c9640 mode=report
  ```
  See screenshot immediately below for example report output:
  ![alt text](https://github.com/cmcconnell1/k8s-elb-sec-group-mgmt/blob/master/images/step1-report-mode.png "Step-1 'Report Mode' mode=report (default)")

- Step-2 Execute with the `mode=purge` option to run in the report + purge mode.
  - The program will prompt for approval (y/n) before derefencing and deleting the orphaned ELB security groups.
  - i.e.:
  ```console
  ./aws/security-group/k8s-elb-sec-group-mgmt parent=sg-00d1eb0508f5c9640 mode=purge
  ```
  - Enter y/n to proceed or cancel/exit.
  See screenshot immediately below for example report output:
  ![alt text](https://github.com/cmcconnell1/k8s-elb-sec-group-mgmt/blob/master/images/step2-purge-mode-prompts-before-deleting.png "Step-2 'Purge Mode' mode=purge (destructive after prompt")

- Step-3 Re-run the utility with 'mode=report' to validate step-2 completed as desired.
  - i.e.:
  ```console
  ./aws/security-group/k8s-elb-sec-group-mgmt parent=sg-00d1eb0508f5c9640 mode=report
  ```
  See screenshot immediately below for example report output:
  ![alt text](https://github.com/cmcconnell1/k8s-elb-sec-group-mgmt/blob/master/images/step3-second-report-mode-validate-cleanup-completed.png "Step-3 second 'Report Mode' mode=report (for validation)")
