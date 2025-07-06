# DevOps Build Series: Project 1 - Part 1
## Setting Up a Web App in the Cloud

### Project Overview
This project is Part 1 of the DevOps Build Series focused on building a CI/CD pipeline from scratch. The goal is to set up the foundational components for a web application that will be deployed in the cloud using AWS services.

**Learning Objectives:**
- Launch and configure an EC2 instance
- Set up secure IAM user access
- Install and configure VS Code for remote development
- Generate a Java web application using Maven
- Establish SSH connections to cloud resources
- Understand basic DevOps concepts and tools

**Prerequisites:**
- AWS account (free tier eligible)
- No prior AWS or web development experience required
- Basic computer literacy

### Step 1: Setting Up IAM User

#### Why IAM Users?
When you create an AWS account, you automatically get a root user. However, it's a security best practice to create an IAM (Identity and Access Management) admin user for daily operations. This provides better security by separating administrative tasks from root access.

#### Creating an IAM Admin User

1. **Access IAM Console**
   - Log into AWS Management Console as root user
   - Search for "IAM" in the services search bar
   - Click on the first result to access IAM console

2. **Navigate to Users**
   - Click on "Users" in the left-hand navigation panel
   - Click "Create user" button

3. **Configure User Details**
   - Enter username: `[YourName]-iam-admin` (e.g., "devops-iam-admin")
   - Select checkbox: "Provide user access to the AWS Management Console"
   - Enter a custom password that meets AWS requirements
   - **Important:** Uncheck "Users must create a new password at next sign-in"

4. **Set Permissions**
   - Select "AdministratorAccess" permission
   - This gives the user access to all AWS services except billing information

5. **Complete Setup**
   - Click "Create user"
   - **Critical:** Download the CSV file containing sign-in details
   - Save the console sign-in URL and credentials securely

6. **Switch to IAM User**
   - Log out of root user account
   - Use the console sign-in URL to log in as your IAM admin user
   - Use the credentials from the downloaded CSV file

### Step 2: Launching an EC2 Instance

#### Understanding EC2
EC2 (Elastic Compute Cloud) provides virtual computers in the cloud. Think of it as having a remote computer that you can access over the internet to run applications, store files, and perform computing tasks.

#### Instance Configuration

1. **Select Region**
   - Choose a region from the recommended list for the DevOps Build Series:
     - US West 2 (Oregon) - Recommended
     - US East 1 (N. Virginia)
     - US East 2 (Ohio)
     - US West 1 (N. California)
     - Europe (Ireland)
     - Europe (London)
     - Europe (Frankfurt)
     - Asia Pacific (Tokyo)
     - Asia Pacific (Singapore)
     - Asia Pacific (Sydney)
     - Asia Pacific (Seoul)
     - Asia Pacific (Mumbai)
     - South America (São Paulo)

2. **Access EC2 Console**
   - Search for "EC2" in AWS services
   - Click on EC2 to open the console
   - Click "Instances" in the left navigation panel
   - Click "Launch instances"

3. **Configure Instance Details**
   - **Name:** `devopsbuild-devops-[YourName]` (e.g., "devopsbuild-devops-mazid")
   - **Machine Image:** Select "Free tier eligible" AMI (Amazon Machine Image)
   - **Instance Type:** t2.micro (Free tier eligible)
   - **Key Pair:** Create new key pair named "devopsbuild-keypair"
   - **Network Settings:** 
     - Allow SSH traffic from your IP address (not "anywhere")
     - This ensures only you can connect to your instance

4. **Launch Instance**
   - Review settings and click "Launch instance"
   - Download the private key file (.pem)
   - Store it securely in a folder on your desktop

#### Key Pair Security
- The private key (.pem file) is your authentication method
- Store it in a dedicated folder (e.g., "devops" on desktop)
- Change permissions: `chmod 400 devopsbuild-keypair.pem` (Linux/Mac)
- For Windows: Use appropriate permission commands

**Linux Commands for Key Pair Setup:**
```bash
# Create devops directory on desktop
mkdir ~/Desktop/devops

# Move key pair to devops directory
mv ~/Downloads/devopsbuild-keypair.pem ~/Desktop/devops/

# Set correct permissions (read-only for owner)
chmod 400 ~/Desktop/devops/devopsbuild-keypair.pem

# Verify permissions
ls -la ~/Desktop/devops/devopsbuild-keypair.pem

# Check if key pair is readable
cat ~/Desktop/devops/devopsbuild-keypair.pem
```

### Step 3: Installing VS Code

#### What is VS Code?
VS Code is an Integrated Development Environment (IDE) - software that helps developers write and edit code. It's one of the most popular IDEs globally and provides features like syntax highlighting, debugging, and extensions for various development tasks.

#### Installation Process

1. **Download VS Code**
   - Visit https://code.visualstudio.com/
   - Select the appropriate version for your operating system:
     - **Mac:** Choose Apple Silicon or Intel chip version
     - **Windows:** Download the Windows installer
     - **Linux:** Select your distribution

2. **Install and Configure**
   - Run the installer and follow setup instructions
   - Open VS Code after installation
   - Take a screenshot of the welcome page for documentation

3. **Open Terminal in VS Code**
   - Go to Terminal → New Terminal
   - Navigate to your devops folder: `cd desktop/devops`
   - Verify your private key file is present: `ls` (Linux/Mac) or `dir` (Windows)

**Linux Commands for VS Code Setup:**
```bash
# Navigate to devops directory
cd ~/Desktop/devops

# List files to verify key pair is present
ls -la

# Check file permissions
stat devopsbuild-keypair.pem

# Verify file is not empty
wc -l devopsbuild-keypair.pem
```

### Step 4: Connecting to EC2 Instance

#### Understanding SSH
SSH (Secure Shell) is a protocol that allows secure access to remote servers. It provides both authentication (verifying you have permission to access) and encrypted data transfer between your local computer and the remote server.

#### Establishing Connection

1. **Get Instance Details**
   - Go to EC2 console → Instances
   - Select your instance
   - Copy the Public IPv4 DNS address

2. **Connect via Terminal**
   - In VS Code terminal, run SSH command:
   ```bash
   ssh -i /path/to/devopsbuild-keypair.pem ec2-user@[your-public-ipv4-dns]
   ```
   - Replace `/path/to/` with actual path to your .pem file
   - Replace `[your-public-ipv4-dns]` with your instance's DNS address

3. **Verify Connection**
   - Terminal prompt should change to show `ec2-user@[ip-address]`
   - You should see a welcome message with an ASCII art bird

#### Troubleshooting Connection Issues

**Linux Commands for Troubleshooting:**

```bash
# 1. Check your current IP address
curl ifconfig.me
# or
wget -qO- ifconfig.co
# or
dig +short myip.opendns.com @resolver1.opendns.com

# 2. Verify private key permissions
ls -la ~/Desktop/devops/devopsbuild-keypair.pem
# Should show: -r-------- (read-only for owner)

# 3. Test SSH connection with verbose output
ssh -v -i ~/Desktop/devops/devopsbuild-keypair.pem ec2-user@[your-public-ipv4-dns]

# 4. Check if instance is reachable (ping test)
ping -c 4 [your-public-ipv4-dns]

# 5. Test SSH port connectivity
telnet [your-public-ipv4-dns] 22
# or
nc -zv [your-public-ipv4-dns] 22

# 6. Verify key pair format
file ~/Desktop/devops/devopsbuild-keypair.pem
# Should show: PEM certificate

# 7. Check SSH key fingerprint
ssh-keygen -lf ~/Desktop/devops/devopsbuild-keypair.pem

# 8. Test connection with specific username
ssh -i ~/Desktop/devops/devopsbuild-keypair.pem -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no ec2-user@[your-public-ipv4-dns]

# 9. Check SSH configuration
ssh -T -i ~/Desktop/devops/devopsbuild-keypair.pem ec2-user@[your-public-ipv4-dns]

# 10. Verify instance state from AWS CLI (if installed)
aws ec2 describe-instances --instance-ids [your-instance-id] --query 'Reservations[0].Instances[0].State.Name'
```

**Common Issues and Solutions:**

```bash
# Permission denied error - fix key permissions
chmod 400 ~/Desktop/devops/devopsbuild-keypair.pem

# Connection timeout - check security group
# (This requires AWS console access, no CLI command)

# Host key verification failed - add to known hosts
ssh-keyscan -H [your-public-ipv4-dns] >> ~/.ssh/known_hosts

# Connection refused - check if instance is running
# (Use AWS console or CLI to verify instance state)
```

### Step 5: Installing Development Tools

#### Installing Maven

**What is Maven?**
Apache Maven is a build automation tool that helps manage Java projects. It uses archetypes (templates) to quickly generate project structures and manages dependencies.

**Installation Commands:**
```bash
# Download Maven
wget https://archive.apache.org/dist/maven/maven-3/3.5.2/binaries/apache-maven-3.5.2-bin.tar.gz

# Extract to /opt directory
sudo tar xf apache-maven-3.5.2-bin.tar.gz -C /opt

# Create symbolic link
sudo ln -s /opt/apache-maven-3.5.2 /opt/maven

# Set environment variables
echo 'export M2_HOME=/opt/maven' >> ~/.bashrc
echo 'export PATH=${M2_HOME}/bin:${PATH}' >> ~/.bashrc

# Reload bash configuration
source ~/.bashrc

# Verify installation
mvn -version
```

#### Installing Java (Amazon Corretto 8)

**What is Java?**
Java is a programming language used to develop applications. Amazon Corretto 8 is Amazon's distribution of OpenJDK 8, providing a reliable and free version of Java.

**Installation Commands:**
```bash
# Update package manager
sudo yum update -y

# Install Java Corretto 8
sudo yum install java-1.8.0-amazon-corretto-devel -y

# Set environment variables
echo 'export JAVA_HOME=/usr/lib/jvm/java-1.8.0-amazon-corretto' >> ~/.bashrc
echo 'export PATH=$JAVA_HOME/bin:$PATH' >> ~/.bashrc

# Reload bash configuration
source ~/.bashrc

# Verify installation
java -version
javac -version
```

#### Verification Commands
```bash
# Check Maven version
mvn -version

# Check Java version
java -version

# Check Java compiler version
javac -version

# Verify environment variables
echo $JAVA_HOME
echo $M2_HOME
echo $PATH

# Test Java installation
java -cp . HelloWorld 2>/dev/null || echo "Java is working but no HelloWorld class found"
```

### Step 6: Generating the Web Application

#### Using Maven Archetype
Maven archetypes are templates that help quickly generate project structures. We'll use the webapp archetype to create a Java web application.

**Generate Web App:**
```bash
# Generate web application using Maven archetype
mvn archetype:generate -DgroupId=com.devopsbuild -DartifactId=devopsbuild-web-project -DarchetypeArtifactId=maven-archetype-webapp -DinteractiveMode=false

# Navigate to project directory
cd devopsbuild-web-project

# List project structure
ls -la

# View project structure in detail
tree . || find . -type f -name "*.xml" -o -name "*.jsp" -o -name "*.java"
```

**What This Creates:**
- A complete Java web application structure
- Source code folders (src/main/java, src/main/webapp)
- Configuration files (pom.xml)
- Basic web page (index.jsp)

**Verify Generation:**
```bash
# Check for BUILD SUCCESS message
echo "Checking build status..."

# Verify project structure
ls -la devopsbuild-web-project/

# Check Maven project file
cat devopsbuild-web-project/pom.xml

# Verify web app directory
ls -la devopsbuild-web-project/src/main/webapp/

# Check if index.jsp exists
ls -la devopsbuild-web-project/src/main/webapp/index.jsp
```

### Step 7: Connecting VS Code to EC2

#### Installing Remote SSH Extension

1. **Install Extension**
   - In VS Code, go to Extensions (Ctrl+Shift+X)
   - Search for "Remote - SSH"
   - Click "Install"

2. **Configure SSH Connection**
   - Click the green button (Open a Remote Window) in bottom-left corner
   - Select "Connect to Host" → "Add New SSH Host"
   - Enter the same SSH command used in terminal:
   ```bash
   ssh -i /path/to/devopsbuild-keypair.pem ec2-user@[your-public-ipv4-dns]
   ```
   - Select the default config file to save settings

3. **Connect to Instance**
   - Click the green button again
   - Select "Connect to Host"
   - Choose your configured EC2 instance
   - VS Code will open a new window connected to your instance

#### Accessing Web App Files

1. **Open Project Folder**
   - In the remote VS Code window, go to File → Open Folder
   - Navigate to and select "devopsbuild-web-project"
   - Click "OK"

2. **Explore Project Structure**
   - **src/main/webapp:** Contains web application files
   - **src/main/resources:** Contains configuration files
   - **pom.xml:** Maven project configuration
   - **index.jsp:** Main web page file

**Linux Commands for File Exploration:**
```bash
# Navigate to project directory
cd devopsbuild-web-project

# Show complete directory structure
tree . || find . -type d

# List all files with details
ls -la

# Show file contents
cat src/main/webapp/index.jsp

# Check file permissions
ls -la src/main/webapp/

# Search for specific file types
find . -name "*.jsp"
find . -name "*.xml"
find . -name "*.java"

# Count lines in files
wc -l src/main/webapp/index.jsp
wc -l pom.xml
```

### Step 8: Editing the Web Application

#### Understanding JSP Files
JSP (JavaServer Pages) files combine HTML markup with Java code to create dynamic web content. They can display both static content (like HTML) and dynamic content that changes based on server-side logic.

#### Modifying index.jsp

1. **Open index.jsp**
   - Navigate to src/main/webapp/index.jsp
   - Click to open the file

2. **Edit Content**
   - Replace "Hello World!" with "Hello [YourName]!"
   - Add a paragraph: "This is my web app working!"
   - Save the file (Ctrl+S)

**Example Modified Content:**
```html
<html>
<body>
<h2>Hello Mazid!</h2>
<p>This is my web app working!</p>
</body>
</html>
```

**Linux Commands for File Editing:**
```bash
# View current content
cat src/main/webapp/index.jsp

# Backup original file
cp src/main/webapp/index.jsp src/main/webapp/index.jsp.backup

# Edit file using nano
nano src/main/webapp/index.jsp

# Edit file using vim (alternative)
vim src/main/webapp/index.jsp

# Edit file using sed (command line editing)
sed -i 's/Hello World!/Hello Mazid!/g' src/main/webapp/index.jsp

# Verify changes
cat src/main/webapp/index.jsp

# Compare with backup
diff src/main/webapp/index.jsp src/main/webapp/index.jsp.backup

# Check file size and modification time
ls -la src/main/webapp/index.jsp
```

### Secret Mission: Terminal-Based Editing

#### Purpose
This optional exercise demonstrates why IDEs are valuable by showing how to edit code using only terminal commands.

#### Steps

1. **Switch to Local VS Code Window**
   - Close the remote SSH window
   - Return to your local VS Code window

2. **Connect via Terminal**
   - Ensure your terminal is connected to EC2 instance
   - Navigate to the web app directory:
   ```bash
   cd devopsbuild-web-project/src/main/webapp
   ```

3. **Edit with Nano**
   - Open index.jsp in nano editor: `nano index.jsp`
   - Use arrow keys to navigate
   - Add text: "I'm writing this line using Nano instead of an IDE"
   - Save: Ctrl+S, Exit: Ctrl+X

4. **Compare Experiences**
   - Reflect on the difference between IDE and terminal editing
   - Consider the benefits of each approach

**Advanced Terminal Editing Commands:**
```bash
# Navigate to web app directory
cd devopsbuild-web-project/src/main/webapp

# View file before editing
cat index.jsp

# Edit with nano
nano index.jsp

# Alternative editors
vim index.jsp
# or
emacs index.jsp

# Command line editing with sed
sed -i '/<body>/a\    <p>I'\''m writing this line using Nano instead of an IDE</p>' index.jsp

# Verify changes
cat index.jsp

# Check file permissions after editing
ls -la index.jsp

# Create a diff of changes
diff index.jsp index.jsp.backup
```

### Cleanup and Resource Management

#### Terminating Resources

1. **Terminate EC2 Instance**
   - Go to EC2 console → Instances
   - Select your instance
   - Actions → Instance State → Terminate Instance
   - Confirm termination

2. **Delete Key Pair**
   - Go to EC2 console → Key Pairs
   - Select your key pair
   - Actions → Delete
   - Confirm deletion

#### Why Cleanup Matters
- Prevents unexpected charges
- Develops good cloud resource management habits
- Keeps your AWS account organised

**Linux Commands for Cleanup:**
```bash
# Exit SSH connection
exit

# Remove local key pair file
rm ~/Desktop/devops/devopsbuild-keypair.pem

# Remove devops directory
rm -rf ~/Desktop/devops

# Clear SSH known hosts (optional)
ssh-keygen -R [your-public-ipv4-dns]

# Check for any remaining SSH connections
ps aux | grep ssh

# Kill any remaining SSH processes (if needed)
pkill -f ssh

# Clean up any temporary files
rm -rf /tmp/maven*
rm -rf ~/.m2/repository/com/devopsbuild
```

### Project Summary

#### What We Accomplished
- Set up secure AWS access with IAM user
- Launched and configured an EC2 instance
- Established SSH connections for remote access
- Installed development tools (Maven, Java)
- Generated a Java web application
- Connected VS Code for remote development
- Edited web application code
- Learnt about cloud computing concepts

#### Key Concepts Learnt
- **Cloud Computing:** Using remote servers instead of local machines
- **SSH:** Secure protocol for remote server access
- **IDE:** Integrated Development Environment for coding
- **Maven:** Build automation tool for Java projects
- **JSP:** JavaServer Pages for dynamic web content
- **Key Pairs:** Authentication method for cloud resources

#### Next Steps
This project serves as the foundation for the remaining parts of the DevOps Build Series. The next project will focus on connecting your web application to a GitHub repository for version control.


### Troubleshooting Common Issues

#### Connection Problems
- Verify security group allows your IP
- Check private key permissions
- Ensure instance is running
- Confirm correct username (ec2-user for Amazon Linux)

**Linux Commands for Connection Troubleshooting:**
```bash
# Check your IP address
curl ifconfig.me

# Verify key permissions
ls -la ~/Desktop/devops/devopsbuild-keypair.pem

# Test SSH connection with verbose output
ssh -v -i ~/Desktop/devops/devopsbuild-keypair.pem ec2-user@[your-public-ipv4-dns]

# Check if port 22 is open
telnet [your-public-ipv4-dns] 22

# Test connectivity
ping -c 4 [your-public-ipv4-dns]

# Check SSH configuration
ssh -T -i ~/Desktop/devops/devopsbuild-keypair.pem ec2-user@[your-public-ipv4-dns]
```

#### Permission Issues
- Run `chmod 400` on private key file
- Check file ownership
- Verify SSH key format

**Linux Commands for Permission Troubleshooting:**
```bash
# Fix key permissions
chmod 400 ~/Desktop/devops/devopsbuild-keypair.pem

# Check file ownership
ls -la ~/Desktop/devops/devopsbuild-keypair.pem

# Verify key format
file ~/Desktop/devops/devopsbuild-keypair.pem

# Check key fingerprint
ssh-keygen -lf ~/Desktop/devops/devopsbuild-keypair.pem

# Test key validity
ssh-keygen -y -f ~/Desktop/devops/devopsbuild-keypair.pem
```

#### Installation Problems
- Ensure you're connected to the instance
- Check internet connectivity
- Verify commands are run in correct order
- Look for error messages in output

**Linux Commands for Installation Troubleshooting:**
```bash
# Check internet connectivity
ping -c 4 google.com

# Check if yum is working
sudo yum check-update

# Verify Java installation
java -version
which java

# Verify Maven installation
mvn -version
which mvn

# Check environment variables
echo $JAVA_HOME
echo $M2_HOME
echo $PATH

# Check disk space
df -h

# Check available memory
free -h

# Check system information
uname -a
cat /etc/os-release
```
