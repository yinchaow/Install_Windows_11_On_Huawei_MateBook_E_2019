# How to install Windows 11 on Huawei MateBook E 2019

## Prerequisites
Download the following files:
1. [Windows 11 Inside Preview **[ARM64 version]** Installation ISO File](https://uup.rg-adguard.net/)
2. Huawei MateBook E 2019 Official Drivers Installer: [MateBook_E_2019_OneKey_2.0.0.12.zip](https://consumer-tkbdownload.huawei.com/ctkbfm/servlet/download/downloadServlet/H4sIAAAAAAAAAD2QW0vDQBCF_8s-lzKz9_hkbgsiGiH1OWyym7pYk5ImSiv-dzcYZJ4-5hzOmfkmy8VPh-vZkztCyY648WvYUEbsw8k_248Vn-zss3F8b8qGAiZNNfhHf23oHuIg3d_CeTO82PktGkRPLTDHRA8dR2h13wpQmrd953SnXVS34fbgorSu7muUgIxK1GtuN3k7h3E4hDU7rhLOgAMCwI5cwnGw8zKtrZhMuQQquBZ5ioblPFEmZ5hLVYrSZKpMVMGM5plURcGy3GAGgiGmhhko5V-J7eK6ivhpT8G9_n9lnhb_8wsAkn0nJwEAAA%3D%3D.zip)
3. [Rufus](https://rufus.ie/)


## Make Windows 11 Installation Flash Drive
Use rufus to make the installation flash drive.


## Get the MateBook E 2019 Drivers
1. Extract MateBook_E_2019_OneKey_2.0.0.12.zip, install, and find boot.wim in `C:\ProgramData\Huawei\Driver\OneKey Driver`. Or you may also download the boot.wim image [here](boot_wim_huawei_matebook_e_2019).
2. Make a directory named *temp* in `C:\ProgramData\Huawei\Driver\OneKey Driver`, which is `C:\ProgramData\Huawei\Driver\OneKey Driver\temp`.
3. Mount the boot.wim image. Open an elevated command prompt window, and run the following command:
```cmd
DISM /Mount-Wim /WimFile:"C:\ProgramData\Huawei\Driver\OneKey Driver\boot.wim" /index:1 /MountDir:"C:\ProgramData\Huawei\Driver\OneKey Driver\temp"
```
4. Copy the directory `C:\ProgramData\Huawei\Driver\OneKey Driver\temp\Windows/System32/DriverStore` which contains all the matebook drivers we need to `C:\ProgramData\Huawei\Driver\OneKey Driver`. Now the directory should be `C:\ProgramData\Huawei\Driver\OneKey Driver\DriverStore`.
5. Unmount the boot.wim image and discard the changes. In the elevated command prompt window, run the following command:
```cmd
DISM /Unmount-Wim /MountDir:"C:\ProgramData\Huawei\Driver\OneKey Driver\temp" /discard
```
Now the *temp* directory should be empty.


## Add Drivers to the Windows 11 Installation Image
1. Plugin the Windows 11 installation flash drive. Make it U:\ for example.
2. Mount the boot.wim file from Windows 11. In the elevated command prompt window, run the following command:
```cmd
DISM /Mount-Wim /WimFile:"U:\sources\boot.wim" /index:1 /MountDir:"C:\ProgramData\Huawei\Driver\OneKey Driver\temp"
```
3. Add the matebook drivers to the mounted boot.wim image in the last step. In the elevated command prompt window, run the following command:
```cmd
DISM /Image:"C:\ProgramData\Huawei\Driver\OneKey Driver\temp" /Add-Driver /Driver:"C:\ProgramData\Huawei\Driver\OneKey Driver\DriverStore" /recurse
```
4. Unmount the boot.wim image and commit the changes. In the elevated command prompt window, run the following command:
```cmd
DISM /Unmount-Wim /MountDir:"C:\ProgramData\Huawei\Driver\OneKey Driver\temp" /commit
```
5. Make the same changes to the `U:\sources\install.wim` image. Make sure to commit the changes. 


## Install Windows 11 on MateBook E 2019
1. Change *Secure Boot* to **OFF** in BIOS.
2. Plugin the flash drive and start the installation, until you see "This PC can't run Windows 11".
3. Press `Shift`+`F10`, and it will open a command prompt window. Type in **regedit** and press enter.
4. In the Registry Editor, make the following settings:
    - Go to `Computer\HKEY_LOCAL_MACHINE\SYSTEM\Setup`. Right click on **Setup** and click New > Key. Name it **LabConfig**.
    - Click on **LabConfig**, then right click on the right pane, and click New > DWORD (32-bit Value). Name it **BypassTPMCheck**. Double click on **ByPassTPMCheck** and change the Value data to **1**, and press OK.
    - Create another DWORD and change the Value data to 1 just like the above step and name it **BypassSecureBootCheck**.
5. Close the Registry Editor and the command prompt.
6. Click on the Back button where you left off at the "This PC can't run Windows 11" message, and continue the installation.


## References
1. [Huawei Matebook E 2019 - ARM64 Windows 10 Drivers](https://community.spiceworks.com/topic/2256759-huawei-matebook-e-2019-arm64-windows-10-drivers)
2. [How To Mount and Update Windows Image Files (WIM)](https://www.kjctech.net/how-to-mount-and-update-windows-image-files-wim/)
3. [How to Bypass Secure Boot & Trusted Platform Module to Install Windows 11](https://www.majorgeeks.com/content/page/bypass_tpm.html)
