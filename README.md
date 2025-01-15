# ExeFile Resizer (Increaser) Free
ExeFile Resizer is a utility designed to resize .exe files, allowing you to easily modify the file size for various purposes, such as evading file size restrictions or experimenting with file properties. With a straightforward interface and minimal steps, this tool helps you quickly adjust the size of your executable files without causing any harm to the file's functionality.
## Features
![ExeFile Resizer interface](https://github.com/user-attachments/assets/d4761480-3ed2-41b4-a5c5-bc9224b186a8)

* Resize .exe Files: Increase the size of .exe files to the desired size.
* Intuitive Interface: User-friendly system interface with simple steps to modify file size.
* Windows Compatibility: Works on different versions of Windows, ensuring broad usability.
* Safe and Secure: [Verified on VirusTotal](https://www.virustotal.com/gui/file/d7cfb98c11a51154abedff761426d936492e22a3537a8a20efb999d01367f4b6/detection), ensuring no hidden threats in the tool.
* Quick Processing: Modifications are completed swiftly, without delays.
## How it Works
> ExeFile Resizer works by taking an input .exe file and allowing you to specify a target size. The tool will then append data to the file until it reaches the specified size. The integrity of the original executable is preserved, and the resulting file remains functional.

## Requirements
* Operating System: Windows 7 or higher.
* Storage: Minimum 5 MB of free space for the application (At the moment the file size doesn't even get to 1MB, and that's a good thing).
* Permissions: Administrator privileges for modifying system-level files (optional, depending on the file).

## Anti-Virus Information
> ExeFile Resizer is designed to be safe and harmless. However, because it modifies the size of executable files, some antivirus software may flag the program as a potential threat. This is a false positive, and we recommend that you whitelist ExeFile Resizer in your antivirus software.

## Compatibility
* Windows 7: Fully supported ✓
* Windows 8/8.1: Supported ✓
* Windows 10/11: Fully supported ✓
## Limitations
* File Size Limitations: The tool can only resize files within the file size limits set by your system (typically 4 GB).
* Not Suitable for All File Types: ExeFile Resizer only works with .exe files and may not function correctly with other file formats.
* File size **increase** only. No reduction.
# Open Source Code (Version 1.0.0 only)
> This program is written in C# (C-Sharp) and is designed to work within the .NET Framework. Type of Application: Console-based with GUI Dialogs. Used program: Visual Studio 2022.
## Full Code
```
using System;
using System.IO;
using System.Windows.Forms;

namespace ExeFileResizer
{
    class Program
    {
        [STAThread]
        static void Main()
        {
            try
            {
                while (true)
                {
                    OpenFileDialog openFileDialog = new OpenFileDialog
                    {
                        Filter = "Executable Files (*.exe)|*.exe",
                        Title = "Select an EXE File"
                    };

                    if (openFileDialog.ShowDialog() != DialogResult.OK)
                    {
                        MessageBox.Show("No file selected.", "Error", MessageBoxButtons.OK, MessageBoxIcon.Error);
                        return;
                    }

                    string selectedFilePath = openFileDialog.FileName;
                    long originalSizeBytes = new FileInfo(selectedFilePath).Length;

                    string input = Microsoft.VisualBasic.Interaction.InputBox(
                        $"The current size is {originalSizeBytes / (1024.0 * 1024):F2} MB.\nEnter the desired size in MB:",
                        "Enter Desired Size",
                        "10.0"
                    );

                    if (!double.TryParse(input, out double desiredSizeMb) || desiredSizeMb <= 0)
                    {
                        MessageBox.Show("Invalid size entered.", "Error", MessageBoxButtons.OK, MessageBoxIcon.Error);
                        continue;
                    }

                    long desiredSizeBytes = (long)(desiredSizeMb * 1024 * 1024);

                    if (desiredSizeBytes <= originalSizeBytes)
                    {
                        DialogResult result = MessageBox.Show(
                            "The desired size must be larger than the original file size.",
                            "Error",
                            MessageBoxButtons.RetryCancel,
                            MessageBoxIcon.Error
                        );

                        if (result == DialogResult.Cancel)
                            return;

                        continue;
                    }

                    SaveFileDialog saveFileDialog = new SaveFileDialog
                    {
                        Filter = "Executable Files (*.exe)|*.exe",
                        Title = "Save Resized File"
                    };

                    if (saveFileDialog.ShowDialog() != DialogResult.OK)
                    {
                        MessageBox.Show("No save location selected.", "Error", MessageBoxButtons.OK, MessageBoxIcon.Error);
                        return;
                    }

                    string saveFilePath = saveFileDialog.FileName;

                    using (FileStream fs = new FileStream(saveFilePath, FileMode.Create, FileAccess.Write))
                    {
                        byte[] originalFileContent = File.ReadAllBytes(selectedFilePath);
                        fs.Write(originalFileContent, 0, originalFileContent.Length);

                        byte[] padding = new byte[desiredSizeBytes - originalSizeBytes];
                        fs.Write(padding, 0, padding.Length);
                    }

                    DialogResult successResult = MessageBox.Show(
                        "File size increased and saved successfully!\n\nAuthor: t.me/DexonOff",
                        "Success",
                        MessageBoxButtons.OKCancel,
                        MessageBoxIcon.Information
                    );

                    if (successResult == DialogResult.Cancel)
                        break;
                }
            }
            catch (Exception ex)
            {
                MessageBox.Show($"An error occurred: {ex.Message}", "Error", MessageBoxButtons.OK, MessageBoxIcon.Error);
            }
        }
    }
}

```

## Explanation
### Program Start and Loop Setup
```
while (true)
{
    OpenFileDialog openFileDialog = new OpenFileDialog
    {
        Filter = "Executable Files (*.exe)|*.exe",
        Title = "Select an EXE File"
    };

    if (openFileDialog.ShowDialog() != DialogResult.OK)
    {
        MessageBox.Show("No file selected.", "Error", MessageBoxButtons.OK, MessageBoxIcon.Error);
        return;
    }

    string selectedFilePath = openFileDialog.FileName;
    long originalSizeBytes = new FileInfo(selectedFilePath).Length;
}
```
### What happens here:
* The program starts with an infinite loop while (true) to allow the user to try again if there’s an issue.
* A file selection dialog (OpenFileDialog) opens, allowing the user to pick an .exe file.
* If the user cancels the dialog, the program shows an error message and exits.
* If a file is selected, its current size is determined using FileInfo.
### Input Desired Size
```
string input = Microsoft.VisualBasic.Interaction.InputBox(
    $"The current size is {originalSizeBytes / (1024.0 * 1024):F2} MB.\nEnter the desired size in MB:",
    "Enter Desired Size",
    "10.0"
);

if (!double.TryParse(input, out double desiredSizeMb) || desiredSizeMb <= 0)
{
    MessageBox.Show("Invalid size entered.", "Error", MessageBoxButtons.OK, MessageBoxIcon.Error);
    continue;
}

long desiredSizeBytes = (long)(desiredSizeMb * 1024 * 1024);

```
### What happens here:
* The user is prompted to enter the desired file size in megabytes through an input dialog (InputBox).
* The current file size is displayed to help the user decide on the new size.
* If the input is invalid (e.g., non-numeric or zero/negative), the program shows an error message and lets the user try again.
* If valid, the desired size is converted from megabytes to bytes.
### Validate Desired Size
```
if (desiredSizeBytes <= originalSizeBytes)
{
    DialogResult result = MessageBox.Show(
        "The desired size must be larger than the original file size.",
        "Error",
        MessageBoxButtons.RetryCancel,
        MessageBoxIcon.Error
    );

    if (result == DialogResult.Cancel)
        return;

    continue;
}
```
### What happens here:
* The program checks if the desired size is larger than the original file size.
* If the desired size is too small, an error message is shown, and the user can retry or cancel.
* Choosing "Retry" restarts the process, while "Cancel" exits the program.
### Save File Selection
```
SaveFileDialog saveFileDialog = new SaveFileDialog
{
    Filter = "Executable Files (*.exe)|*.exe",
    Title = "Save Resized File"
};

if (saveFileDialog.ShowDialog() != DialogResult.OK)
{
    MessageBox.Show("No save location selected.", "Error", MessageBoxButtons.OK, MessageBoxIcon.Error);
    return;
}

string saveFilePath = saveFileDialog.FileName;
```
### What happens here:
* A dialog opens to let the user choose where to save the resized file.
* If the user cancels, the program shows an error message and exits.
### Resize File
```
using (FileStream fs = new FileStream(saveFilePath, FileMode.Create, FileAccess.Write))
{
    byte[] originalFileContent = File.ReadAllBytes(selectedFilePath);
    fs.Write(originalFileContent, 0, originalFileContent.Length);

    byte[] padding = new byte[desiredSizeBytes - originalSizeBytes];
    fs.Write(padding, 0, padding.Length);
}
```
### What happens here:
* A new file is created at the chosen location (FileMode.Create).
* The original file content is read and written to the new file.
* Additional empty data (padding) is added to increase the file size to the desired amount.
### Success Notification
```
DialogResult successResult = MessageBox.Show(
    "File size increased and saved successfully!\n\nAuthor: t.me/DexonOff",
    "Success",
    MessageBoxButtons.OKCancel,
    MessageBoxIcon.Information
);

if (successResult == DialogResult.Cancel)
    break;
```
### What happens here:
* After successfully resizing the file, the program shows a success message.
* The user can choose "OK" to resize another file or "Cancel" to exit the program.
### Error Handling
```
catch (Exception ex)
{
    MessageBox.Show($"An error occurred: {ex.Message}", "Error", MessageBoxButtons.OK, MessageBoxIcon.Error);
}
```
### What happens here:
* If something goes wrong during the process (e.g., file access issues), the program catches the error and displays the error message to the user.

## Feedback & Support
I value your feedback:) If you encounter any issues or have feature requests, feel free to `create an issue` or `start a discussion` GitHub page.

## Version History
v1.0.0 - Initial release of ExeFile Resizer.
## Credits
Author: @DexonOff (GitHub/Tg)
