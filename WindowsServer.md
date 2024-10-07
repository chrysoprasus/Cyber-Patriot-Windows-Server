# Windows Server Documentation

## Check for network shares
### Using Computer Management
1. Right-click on the **Start** button and select **Computer Management**.
2.  Expand **Shared Folders** in the left pane. 
3.  Click on **Shares**. This will display all the shared folders on the server, including their local paths. 

## FInd the SHA 256 hash of a file

### Using Powershell
1. Open **Powershell**
2. Use the following command, replacing <PathToYourFile> with the actual path to your file: 
```
Get-FileHash <PathToYourFile> -Algorithm SHA256 
```