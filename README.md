# AWS CloudFormation Project: Deploying a Layered Infrastructure

This project demonstrates the best practice of deploying AWS infrastructure in layers using AWS CloudFormation. The project consists of two main tasks: deploying a **networking layer** and deploying an **application layer**. The networking layer is reusable and can be shared across different environments, while the application layer builds on the network resources.

---

## Task 1: Deploying the Networking Layer (VPC)

### Overview
- Create an Amazon Virtual Private Cloud (VPC) and associated networking resources using a CloudFormation template.
- The stack sets foundational networking components that can be reused for multiple environments (development, test, production).
- Naming for this project’s stack: `ikhsan-network`.

### Instructions

1. Download the network template file:  
   Provided original: `ikhsan-network.yaml`  

2. In the AWS Management Console:  
   - Navigate to **CloudFormation** > **Create stack** > **With new resources (standard)**  
   - **Prepare template**: Select *Template is ready*  
   - **Template source**: Upload file >Select `ikhsan-network.yaml`  
   - **Stack name**: Enter `ikhsan-network`  
   - **Tags**: Add tag with `Key: application` and `Value: inventory`  
   - Review and create the stack

3. Wait until the stack status is `CREATE_COMPLETE`.

4. Explore the created resources in the **Resources** tab, check creation logs in **Events**, and view the exported outputs `VPC` and `PublicSubnet` in **Outputs**.  
   These outputs export the VPC and subnet IDs for reference by other stacks.

---

## Task 2: Deploying the Application Layer (EC2 and Security Group)

### Overview
- Deploy an application stack that references outputs from the networking stack.
- Use imported VPC and subnet IDs to correctly place resources.
- Contains an Amazon EC2 instance and associated security group.
- Naming for this project’s stack: `ikhsan-application`.

### Instructions

1. Download the application template file:  
   Provided original: `ikhsan-application.yaml`  

2. In the AWS Management Console:  
   - Navigate to **CloudFormation** > **Create stack** > **With new resources (standard)**  
   - **Prepare template**: Select *Template is ready*  
   - **Template source**: Upload file > Select `ikhsan-application.yaml`  
   - **Stack name**: Enter `ikhsan-application`  
   - Set parameter: `NetworkStackName` → `ikhsan-network` (to import networking outputs)  
   - **Tags**: Add tag with `Key: application` and `Value: inventory`  
   - Review and create the stack

3. Wait until the stack status is `CREATE_COMPLETE`.

4. Check the **Outputs** tab for the URL of the deployed application.  
   Open this URL in a web browser to verify the application is running on the EC2 instance created by the stack.

---

## Key Template Snippets Showing Cross-Stack References

In `ikhsan-application.yaml`, the networking stack outputs are imported and referenced as follows:

