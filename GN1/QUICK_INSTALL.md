# Quick installation guide
The purpose of this section is to provide you with basic informations on the structure and contents of the training Virtual Machine.

In order to simplify the installation procedure, a self-contained package comprising the various executables as well as the data used throughout the training are provided to download or can be viewed online at this link:

 - [GeoNode 3.2.1.ova](https://www.dropbox.com/s/724gqtss4nqifl8/GeoNode%203.2.1.ova?dl=1)

The package is an Oracle VirtualBox `OVA` image which must be loaded into a running Virtual Box instance.

**WARNING**: The package is about `7GB` big, therefore consider downloading it and setting up VirtualBox, as explained below, **before** the training sessions start.

## Installing Oracle VirtualBox
![image](https://user-images.githubusercontent.com/1278021/125089414-a6b9a080-e0ce-11eb-887b-0e9ed1069638.png)

### Minimum Hardware Requirements
 - At least `8GB` of RAM
 - At least `4 CPUs`
 - At least `30GB` of free space on the `HD`

### How to install the Virtual Machine

#### Install Oracle VirtualBox Software
* Download the executable accordingly to your current operative system
  - Windows 10: [VirtualBox-6.1.22-144080-Win.exe](https://www.dropbox.com/s/jzqby6fblqj95ph/VirtualBox-6.1.22-144080-Win.exe?dl=1)
  - Linux: [virtualbox-6.1_6.1.22-1440…buntu_eoan_amd64.deb](https://www.dropbox.com/s/9e4f4gdlmnjso0z/virtualbox-6.1_6.1.22-144080_Ubuntu_eoan_amd64.deb?dl=1)
  - MAC OSX: [Mac OS X build instructions](https://www.virtualbox.org/wiki/Mac%20OS%20X%20build%20instructions)

#### Deploy the GeoNode VM OVA
* Run the Oracle VirtualBox and select `Import Appliance` from the `File` menu
     ![image](https://user-images.githubusercontent.com/1278021/125091753-fb5e1b00-e0d0-11eb-972c-d531d12a45bd.png)

* Select the downloaded file as shown in the figure below
     ![image](https://user-images.githubusercontent.com/1278021/125092546-bdadc200-e0d1-11eb-9477-1b213259fc69.png)

* Click the `Next` button
     ![image](https://user-images.githubusercontent.com/1278021/125092713-e3d36200-e0d1-11eb-99d2-60458c865896.png)

* Leave all the `defaults` option, except for the destination folder; you will need to select a location with at least `30GB` memory free
     ![image](https://user-images.githubusercontent.com/1278021/125092985-2c8b1b00-e0d2-11eb-9949-188f0fdb10bf.png)

* Click the `Import` button and wait for the progress bar to be finished; it will require up to `20 minutes` depending on your system

* If everything goes well, you should be able to se the new `Appliance` available from the Oracle VirtualBox panel
     ![image](https://user-images.githubusercontent.com/1278021/125093258-6c520280-e0d2-11eb-8b6f-b9acfd56ea01.png)

## Start and test the GeoNode Virtual Machine
* Select the GeoNode 3.2.1 image and click on the `Start` button
     ![image](https://user-images.githubusercontent.com/1278021/125098426-4844f000-e0d7-11eb-8cc6-7050ab9045e3.png)

* Wait for the system to startup, you should be able to see the `Ubuntu` desktop
     ![image](https://user-images.githubusercontent.com/1278021/125098808-a70a6980-e0d7-11eb-8aee-508bed48f814.png)

