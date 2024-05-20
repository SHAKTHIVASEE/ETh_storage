# ETh_storage Phase02

sudo apt update

curl -sL https://deb.nodesource.com/setup_20.x | sudo -E bash -

sudo apt-get install -y nodejs


# How to Build and Deploy a Simple Unstoppable dApp on Ethereum

## Step 1: Install ethfs-cli

First, install the ethfs-cli tool:

```sh
npm i -g ethfs-cli
```

## Step 2: Create a FlatDirectory Contract & Save it

Create a FlatDirectory contract to store the files:

```sh
ethfs-cli create -p <private-key> -c 11155111
```

After the transaction is confirmed, you will get the FlatDirectory address:

```
FlatDirectory Address: 0x49EDFB27a463545337487D39a8349760B345F160
```

## Step 3: Upload the Application

Create the `dist` directory and the necessary files (`index.html` and `degen.jpeg`). Then, upload the `dist` folder to the FlatDirectory.

### Create the `dist` Directory and Files

1. **Create `dist` Directory**:
    - In your terminal, navigate to your project directory and create the `dist` folder:
      ```sh
      mkdir dist
      ```

2. **Create `index.html` File**:
    - Create a new HTML file named `app.html` in the `dist` directory with the following content:
      ```html
      <!DOCTYPE html>
      <html>
          <head>
              <script> 
                  async function fetchData() { 
                      const url = 'web3://0xf14e64285Db115D3711cC5320B37264708A47f89:11155111/greeting'; 
                      const response = await fetch(url); 
                      const data = await response.text(); 
                      document.getElementById('content').textContent = data; 
                  } 
                  window.onload = fetchData; 
              </script>
          </head>
          <body>
              <div id="content"> Loading greeting... </div>
              <br>
              <img src="./degen.jpeg" alt="">    
          </body>    
      </html>
      ```

3. **Download `degen.jpeg` Image**:
    - Download the `degen.jpeg` image and place it in the `dist` directory. You can do this manually or use the `wget` command:
      ```sh
      wget -O dist/degen.jpeg https://gist.github.com/assets/5291653/4526caf3-9218-4a23-8619-02f777e6e7fd
      ```

4. **Verify Directory Structure**:
    - Ensure your project directory structure looks like this:
      ```
      YourProject/
      ├── dist/
      │   ├── index.html
      │   └── degen.jpeg
      ```

### Upload the `dist` Directory

Now, upload the application:

```sh
ethfs-cli upload -f dist -a <flat-directory-address> -c 11155111 -p <private-key> -t 1
```

This should resolve the issue of the missing directory or files and allow you to upload your application successfully.

## Step 4: Use EIP-4844 BLOB to Reduce Uploading Cost

To reduce costs, use BLOBs introduced in Ethereum's Cancun upgrade. First, install the `eth-blob-uploader` tool:

```sh
npm i -g eth-blob-uploader
```

Then, upload the files using BLOBs:

```sh
eth-blob-uploader -r https://small-divine-liquid.ethereum-sepolia.quiknode.pro/0ed67157b8f803feee07b62dba90f4b6aff75e4e/ -p <private-key> -f dist/app.html -t <any-address>
eth-blob-uploader -r https://small-divine-liquid.ethereum-sepolia.quiknode.pro/0ed67157b8f803feee07b62dba90f4b6aff75e4e/ -p <private-key> -f dist/degen.jpeg -t <any-address>
```

## Step 5: Use EthStorage to Store the BLOB Permanently

Use EthStorage to store BLOBs permanently by calling the `putBlob` method and paying the permanent storage fee. The `ethfs-cli` tool simplifies this process.

## Step 6: Deploy the dApp with ETH+EthStorage

First, create another FlatDirectory contract:

```sh
ethfs-cli create -p <private-key> -c 11155111
```

Then, upload the application using EthStorage:

```sh
ethfs-cli upload -f dist -a <flat-directory-address> -c 11155111 -p <private-key> -t 2
```

This method requires only two transactions and significantly reduces the cost.

## Step 7: Access the Unstoppable dApp

You can access your dApp via the web3 URL:

```plaintext
web3://<flat-directory-address>:3333/app.html
```

For example:

```plaintext
web3://0x49EDFB27a463545337487D39a8349760B345F160:sep/app.html
```

You can also use the w3link.io gateway to access the web3 protocol from browsers like Chrome.
