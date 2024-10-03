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

# React and Go App Setup #

## React Setup
1. Folder Structure Setup
   ```bash
    cd ~/hyperledger-fabric/fabric-samples
    mkdir my-app
    cd my-app
    ```
2. Initialize the React Frontend
   ```bash
    npx create-react-app frontend
    cd frontend
    ```
   To install dependencies required for making API requests:

   ```bash
   npm install axios
   ```
3. Navigate to src folder
    ```bash
    cd src
    ```
4. Open ***App.js*** file and write the following code in the file
   ```bash
   import React from 'react';
    import axios from 'axios';

    function App() {
        const handleClick = async () => {
            try {
            const response = await axios.get('http://localhost:8080/api');
            console.log(response.data);
            } catch (error) {
            console.error('Error making request:', error);
            }
        };

        return (
            <div className="App">
            <h1>React + Go Backend</h1>
            <button onClick={handleClick}>Click Me</button>
            </div>
        );
    }

    export default App;
    ```
5. Run the React Frontend
   ```bash
   npm start
   ```
   This will serve your React app at http://localhost:3000.


## Go Setup ##

1. Navigate back to the my-app folder and create a backend directory for the Go server:

    ```bash
    cd ..
    mkdir backend
    cd backend
    ```
2. Initialize the Go module:
   ```bash
   go mod init my-app/backend
    ```
3. Create file and Write the Go Backend Code:
    ```bash
    touch main.go
    ```
   Open the file and write the following code:
   ```bash
   package main

    import (
        "fmt"
        "log"
        "net/http"
    )

    func handleRequest(w http.ResponseWriter, r *http.Request) {
        fmt.Println("Button clicked, request received!")
        w.Write([]byte("Backend response: Success!"))
    }

    func main() {
        http.HandleFunc("/api", handleRequest)
        fmt.Println("Backend running at http://localhost:8080")
        log.Fatal(http.ListenAndServe(":8080", nil))
    } 
    ```
    
4. Run the Go Backend
    ```bash
    go run main.go
    ```
    This will start your Go backend at http://localhost:8080.


Click the "Click Me" button.
If successful, you should see the message “Button clicked, request received!” printed in your terminal where the Go server is running.

### Errors ###
If you see any cors issues you can do the following:

1. In your Go backend directory, run:
    ```bash
    go get github.com/rs/cors
    ```

2. Update Your Go Code in main.go:
   ```bash
   package main

    import (
        "fmt"
        "log"
        "net/http"
        "github.com/rs/cors"
    )

    func handleRequest(w http.ResponseWriter, r *http.Request) {
        fmt.Println("Button clicked, request received!")
        w.Write([]byte("Backend response: Success!"))
    }

    func main() {
        mux := http.NewServeMux()
        mux.HandleFunc("/api", handleRequest)

        // Enable CORS for all origins
        c := cors.New(cors.Options{
            AllowedOrigins:   []string{"*"},
            AllowCredentials: true,
        })

        handler := c.Handler(mux)
        fmt.Println("Backend running at http://localhost:8080")
        log.Fatal(http.ListenAndServe(":8080", handler))
    }
    ```
3. Run again:
   ```bash
   go run main.go
    ```

Hopefully, this will resolve those errors. 

   