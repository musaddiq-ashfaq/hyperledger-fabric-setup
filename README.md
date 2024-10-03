# Hyperledger Setup #
## Prerequisites ##

1. **Go**  
For go installation use the following commands:

   ```bash
   wget https://go.dev/dl/go1.19.9.linux-amd64.tar.gz
   sudo tar -C /usr/local -xzf go1.19.9.linux-amd64.tar.gz
   ```
    Add Go to PATH
    ```bash
    echo "export PATH=$PATH:/usr/local/go/bin" >> ~/.bashrc
    source ~/.bashrc
    ```
    Verify installation
    ```bash
    go version
    ```

2. **Install Docker and Docker Compose**
   
    Install Docker Desktop from docker website or docker cli. 


3. **Install Node.js and npm**
   
    For installation of node and npm use following commands:
    
    Update package list and install build tools
    ```bash
    sudo apt-get update
    sudo apt-get install -y build-essential
    ```

    Install Node.js (latest LTS)
    ```bash
    curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -

    sudo apt-get install -y nodejs
    ```

    Verify installation
    ```bash
    node -v
    npm -v
    ```
## Download and Set Up Hyperledger Fabric:
### Clone Fabric Samples and Binaries:

Create a directory for your Fabric projects
```bash 
mkdir ~/hyperledger-fabric
cd ~/hyperledger-fabric
```
Download Fabric samples, binaries, and Docker images
```bash
curl -sSL https://raw.githubusercontent.com/hyperledger/fabric/main/scripts/bootstrap.sh | bash -s
```
