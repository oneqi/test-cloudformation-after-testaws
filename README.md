# test-cloudformation-after-testaws

This is a sample cloudformation template

This can be used after creating your VPC and Internet Gateway by using testaws powershell script

You will need the VpcId generated by the testaws script and the 'key name' of your keys

This template can be modified to add additional subnets and security groups

Also the AMI is hard coded, which again, can be modified

```
  "Parameters" : {                                                       
	"VpcId" : {
		"Type" : "String",
		"Description" : "VpcId of your existing Virtual Private Cloud (VPC)" 
													# This allows to input the VcpId we wish to use during the wizard
			}
				},

  "Resources" : {                                                      
    "Web1" : {                                      # Name of your AMI instance you wish to create
      "Type" : "AWS::EC2::Instance",
      "Properties" : {
        "InstanceType" :  "t2.micro",               # This is the type and is hardcoded
		"SubnetId" : {"Ref" : "PublicSubnet1"},
        "KeyName" :  "Blue-Key" ,                   # This is the name of the keys you wish to use
        "ImageId" : "ami-4836a428",                 # This is related to the 'InstanceType' and is hardcoded 
		"Tags" : [ 
					{ 
					"Key" : "Name",
					"Value" : "Web1"                # This adds named tags to the AMI instance
		            }
									
				] 
		             }
                    },

	"PublicSubnet1" : {                             # Name of subnet you wish to create                              
		"Type" : "AWS::EC2::Subnet",
		"Properties" : {
		"CidrBlock" : "192.168.0.0/24",             # The network address and cidr you wish to use
		"VpcId" : { "Ref" : "VpcId" }
  }
},

	"webSecurityGroup" :{                           # Name of security group you wish to create
		"Type" : "AWS::EC2::SecurityGroup",
		"Properties" : {
			"GroupDescription" : "Enable HTTP and SSH",
			"VpcId" : {"Ref" : "VpcId"},
			"SecurityGroupIngress" :  [             # Ingress rules you require
			{
				"IpProtocol" : "tcp",                                        
				"FromPort" : "80",
				"ToPort" : "80",
				"CidrIp" : "0.0.0.0/0"
			},
			{
				"IpProtocol" : "tcp",
				"FromPort" : "22",
				"ToPort" : "22",
				"CidrIp" : "0.0.0.0/0"				
			}
						]
	                 }
              }
	}
}

```
