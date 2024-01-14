# Powershell-to-list-files-and-folders-recursively
Powershell command to list files and folders recursively.

- Open Powershell
- Paste the following command
- and change the:
$folderPath = "C:\Path\To\Your\FolderOrDrive"
value before running the command.

``` powershell

  # Specify the folder or drive you want to list
$folderPath = "C:\Path\To\Your\FolderOrDrive"

# Get the current date and time for timestamp
$timestamp = Get-Date -Format "yyyyMMdd_HHmmss"

# Include the $folderPath in the log file name
$folderPathInLogFileName = $folderPath -replace '\\|:', '_'
$logFileName = "FileList_${folderPathInLogFileName}_$timestamp.txt"
$logFile = Join-Path -Path $folderPath -ChildPath $logFileName

# Write the top directory (folderPath) to the log file
"Top Directory: $folderPath" | Out-File -FilePath $logFile

# Function to list files and folders recursively
function List-FilesAndFolders {
    param (
        [string]$path,
        [string]$indent = ""
    )

    # Get all items in the current directory
    $items = Get-ChildItem -Path $path

    foreach ($item in $items) {
        # Check if it's a directory (folder)
        if ($item.PSIsContainer) {
            Write-Output "$indent Directory: $($item.FullName)" | Out-File -Append -FilePath $logFile
            # Recursively list items in the subdirectory
            List-FilesAndFolders -path $item.FullName -indent ("$indent  ")
        }
        else {
            Write-Output "$indent File: $($item.FullName)" | Out-File -Append -FilePath $logFile
        }
    }
}

# Call the function to list files and folders
List-FilesAndFolders -path $folderPath

Write-Host "Listing completed. Results saved to $logFile"

```
