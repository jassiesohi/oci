
# "Provisioning VMs (Micorosoft Windows & Oracle Linux 9) in Oracle Cloud Infrastructure (OCI)"
# Prerequisites:
1. Active Oracle Cloud account with required permissions
2. Sufficient quota in your compartment
3. Virtual Cloud Network (VCN) with proper subnets configured
4. SSH key pair (for Oracle Linux VM)


# Create a compartment for your VMs

# Name the compartment as "Oci-Labs

# Create a VCN for your VMs

# create the vcn with cidr 10.50.0.0/16

# Part A: Creating Oracle Linux 9 VM

# Part B: Creating Windows Server 2019 VM

# After successful Tests delete all resources on OCI



# Notes
- This is a silent tutorial focused on commands and configurations
- Steps are precise and actionable
- Assumes familiarity with basic Linux commands
- Designed for educational purposes


# Part A: Creating Oracle Linux 9 VM

1. Sign in to OCI Console:
   - Go to cloud.oracle.com
   - Click "Sign In"
   - Select your region

2. Navigate to Compute Instance:
   - Open hamburger menu
   - Click "Compute"
   - Select "Instances"

3. Create Instance:
   - Click "Create Instance"
   - Name your instance (e.g., "oracle-linux-9")
   - Select compartment

4. Configure Image and Shape:
   - Click "Change Image"
   - Select "Oracle Linux"
   - Choose version 9
   - Select shape (e.g., VM.Standard2.1)
   - Configure OCPU and memory as needed

5. Configure Networking:
   - Choose your VCN
   - Select subnet
   - Optionally assign public IP if needed

6. Add SSH Keys:
   - Choose "Paste public key"
   - Paste your SSH public key
   - Or generate new key pair

7. Review and Launch:
   - Click "Create"
   - Wait for instance to provision

Part B: Creating Windows Server 2019 VM

1. Navigate to Compute Instance:
   - Same steps as above
   - Click "Create Instance"

2. Configure Basic Details:
   - Name your instance (e.g., "windows-2019")
   - Select compartment

3. Configure Image:
   - Click "Change Image"
   - Select "Windows"
   - Choose "Windows Server 2019 Standard"
   - Select shape (e.g., VM.Standard2.1)

4. Configure Networking:
   - Choose your VCN
   - Select subnet
   - Assign public IP if needed

5. Configure Windows-specific settings:
   - Set initial password
   - Note down the username (default: opc)

6. Review and Launch:
   - Click "Create"
   - Wait for instance to provision

Post-Provisioning Steps:

For Oracle Linux 9:
1. Note down the public IP address
2. Connect via SSH:
   
   ssh -i <private_key_path> opc@<public_ip>
   
3. Update the system:
   
   sudo dnf update -y
   

For Windows Server 2019:
1. Note down the public IP address
2. Get the initial password:
   - Go to instance details
   - Click "Instance Information"
   - Click "Initial Password"
3. Connect via RDP:
   - Use Remote Desktop Connection
   - Enter public IP
   - Use the username and initial password

Security Recommendations:
1. Configure Security Lists/Network Security Groups
2. Allow only required ports:
   - Linux: Port 22 (SSH)
   - Windows: Port 3389 (RDP)
3. Update both systems regularly
4. Change default passwords
5. Configure Windows Firewall
6. Set up backup policy

