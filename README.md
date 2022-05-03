# THREE-TIER-ARCHITECTURE
## Prerequisites
- Install https://learn.hashicorp.com/tutorials/terraform/install-cli
- Install the https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html
- Sign up for an https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/
- Your preferred IDE (You can use visual studio code)


## Notes Before Getting Started: 
- You can clone this the full code from here
- A typical 3-tier architecture uses a web, application and database layer. In this project, an application layer was created, instances in the application layer was deployed and a Security Group of the application layer was created and some Security Group rules modified and created an application layer security group.
- RDS has Multi-AZ set to true for high availability. For demo purposes to save money, you'd be sure to set this to false


## Input Variables and the Count Parameter: 
- Having repeated static values in your Terraform code can create more work for you in the future. Imagine that you have the same value repeated throughout your file and one day you have a request to change that value. That is where variables (https://www.terraform.io/docs/language/values/variables.html#suppressing-values-in-cli-output) come in to make your job easier. Variables allow you to have a central source where you can import values from. Instead of updating the value in each location of your code, you only have to update them once in a central source. 
- Using the count (https://www.terraform.io/docs/language/meta-arguments/count.html) parameter on resources can simplify your configurations and lets you scale resources by incrementing. The count parameter acts similar to a loop. For instance if you’d like to create two EC2 instances, you could either duplicate your code twice or you could include count = 2.

 ##### IP of aws instance copied to a file ip.txt in local system
 1. Create a new directory for this Terraform Project
    `resource "local_file" "ip" {  
         content  = aws_instance.demo1.public_ip
	 filename = "ip.txt"
    }
 ##### connecting to the Ansible control node using SSH connection
    resource "null_resource" "nullremote1" {
    depends_on = [aws_instance.demo1]
    connection {
        type     = "ssh"
	user     = "root"
	password = "${var.password}"
	   host  = "${var.host}"
      }

   ##### copying the ip.txt file to the Ansible control node from local system:
    provisioner "file" {
          source    = "ip.txt"
	  destination = "/root/ansible_terraform/aws_instance/ip.txt
        }
    }

   ### 
