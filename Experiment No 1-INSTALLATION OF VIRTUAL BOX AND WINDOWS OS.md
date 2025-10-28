## **AIM**

To install Oracle VirtualBox and configure a Virtual Machine (VM) for installing and running the Windows operating system.

---

## **REQUIREMENTS**

- Host system with minimum 8 GB RAM and 50 GB free disk space
    
- VirtualBox setup file (latest version)
    
- Windows ISO image file (e.g., Windows 10 or Windows 11)
    

---

## **PROCEDURE**

### **Step 1: Install VirtualBox**

1. Download the latest VirtualBox installer from the [official website](https://www.virtualbox.org/).
    
2. Run the installer on your host system.
    
    - Accept the license agreement.
        
    - Choose the default installation options and click **Next**.
        
    - Click **Install** to begin installation.
        
3. Once installation is complete, open VirtualBox to verify successful installation.
    

---

### **Step 2: Create a New Virtual Machine**

1. Launch VirtualBox and click **New**.
    
2. Enter a name for the virtual machine (e.g., `Windows10_VM`).
    
3. Set:
    
    - **Type:** Microsoft Windows
        
    - **Version:** Windows 10 (or Windows 11)
        
4. Allocate memory (RAM) — recommended **4096 MB (4 GB)** or more.
    
5. Create a virtual hard disk:
    
    - Select **Create a virtual hard disk now**.
        
    - Choose **VDI (VirtualBox Disk Image)**.
        
    - Select **Dynamically allocated** and assign around **50 GB** storage.
        
    - Click **Create** to finish.
        

---

### **Step 3: Install Windows OS on the Virtual Machine**

1. Select the newly created VM and click **Settings**.
    
2. In the **Storage** section, select the empty disk under _Controller: IDE_.
    
3. Click the disk icon and browse to select your Windows ISO file.
    
4. Click **OK** to save settings and start the VM using **Start**.
    
5. The VM will boot from the ISO file — follow the Windows installation steps:
    
    - Choose language, time, and keyboard layout.
        
    - Click **Install Now**.
        
    - Enter the product key (or skip).
        
    - Choose **Custom Installation**.
        
    - Select the virtual disk and continue installation.
        
6. Once installation completes, the VM will reboot and prompt for initial setup (user account, password, region, etc.).
    

---
## **OUTPUT**

	The Windows Operating System was Created and installed in Virtual Box
## **RESULT**

VirtualBox was successfully installed, and a Windows operating system was created, installed, and configured inside the virtual machine. The VM runs independently within the host environment, enabling testing and experimentation without affecting the host OS.
