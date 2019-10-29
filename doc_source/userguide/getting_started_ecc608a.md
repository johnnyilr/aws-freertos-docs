# Getting Started with the ECC608a Secure Element \+ Windows Simulator<a name="getting_started_ecc608a"></a>

This tutorial provides instructions for getting started with the ECC608a Secure Element with Windows Simulator\.

You will need the following hardware\.
+ ECC608a secure element clickboard
+ SAMD21 XPlained Pro
+ mikroBUS Xplained Pro adapter

Before you begin, you must configure AWS IoT and your Amazon FreeRTOS download to connect your device to the AWS Cloud\.  In this tutorial, the path to the Amazon FreeRTOS download directory is referred to as *<amazon\-freertos>*\.

## Overview<a name="gsg-ecc608a-overview"></a>

This tutorial contains the following getting started steps:

1. Connect your board to a host machine\.

1. Install software on the host machine for developing and debugging embedded applications for your microcontroller board\.

1. Cross compile an Amazon FreeRTOS demo application to a binary image\.

1. Load the application binary image to your board, and then run the application\.

## Set Up the ECC608a Hardware<a name="gsg-ecc608a-setup"></a>

Before you can interact with your ECC608a device, you must first program the SAMD21\.

**Set up the SAMD21 XPlained Pro board**

1. On the [ CryptoAuthentication™ SOIC XPRO Starter Kit](https://www.microchip.com/DevelopmentTools/ProductDetails/DM320109) page, follow the [CryptoAuthSSH\-XSTK \(DM320109\) \- Latest Firmware](http://ww1.microchip.com/downloads/en/DeviceDoc/ATCRYPTOAUTHSSH-XSTK_v1.0.1.zip) link to download a \.zip file containing instructions \(PDF\) and a binary which can be programmed onto the D21\.

1. Download and install the [ Amtel Studio 7](https://www.microchip.com/mplab/avr-support/atmel-studio-7) IDP\. Make sure that you select the **SMART ARM MCU** driver architecture when installing Atmel Studio\.

1. Use a USB 2\.0 Micro B cable to attach the "Debug USB" connector to your computer, and follow the instructions in the PDF that you downloaded in the first step\. \(The "Debug USB" connector is the USB port closest to the POWER led and pins\.\)

**Connect the Hardware**

1. Unplug the micro USB cable from Debug USB\.

1. Plug the mikroBUS XPlained Pro adapter into the SAMD21 board in the EXT1 location\.

1. Plug the ATECC608a Secure 4 Click board into the mikroBUSX XPlained Pro adapter, making sure that the notched corner of the click board matches with the notched icon on the adapter board\.

1. Plug the micro USB cable in to Target USB\.

1. Your setup should look like the picture below\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/freertos/latest/userguide/images/samd21.png)

## Set Up Your Development Environment<a name="gsg-ecc608a-setup-dev-env"></a>

1. If you haven't already, [create and activate an AWS account](https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/)\. To add an IAM user to your AWS account, see [IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/)\.

   To grant your IAM user account access to AWS IoT and Amazon FreeRTOS, you will attach the following IAM policies to your IAM user account in the next steps:
   + `AmazonFreeRTOSFullAccess`
   + `AWSIoTFullAccess`

1. Attach the AmazonFreeRTOSFullAccess policy to your IAM user\.

   1. Browse to the [IAM console](https://console.aws.amazon.com/iam/home), and from the navigation pane, choose ** Users**\.

   1. Enter your user name in the search text box, and then choose it from the list\.

   1. Choose **Add permissions**\.

   1. Choose **Attach existing policies directly**\.

   1. In the search box, enter **AmazonFreeRTOSFullAccess**, choose it from the list, and then choose **Next: Review**\.

   1. Choose **Add permissions**\.

1. Attach the AWSIoTFullAccess policy to your IAM user\.

   1. Browse to the [IAM console](https://console.aws.amazon.com/iam/home), and from the navigation pane, choose ** Users**\.

   1. Enter your user name in the search text box, and then choose it from the list\.

   1. Choose **Add permissions**\.

   1. Choose **Attach existing policies directly**\.

   1. In the search box, enter **AWSIoTFullAccess**, choose it from the list, and then choose **Next: Review**\.

   1. Choose **Add permissions**\.

   For more information about IAM and user accounts, see [IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/)\.

   For more information about policies, see [IAM Permissions and Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction_access-management.html)\.

1. Download the Amazon FreeRTOS repo from the [Amazon FreeRTOS console](https://console.aws.amazon.com/freertos) or from the [Amazon FreeRTOS GitHub repository](https://github.com/aws/amazon-freertos)\.

   To download Amazon FreeRTOS from the Amazon FreeRTOS console:

   1. Go to the [Amazon FreeRTOS console](https://console.aws.amazon.com/freertos)\.

   1. Under **Predefined configurations**, find **Connect to AWS IoT\- *Platform***, and then choose **Download**\.

   1. Unzip the downloaded file to a folder, and make a note of the folder path\. In the tutorials, this folder is referred to as *<amazon\-freertos>*\. 
**Note**  
The maximum length of a file path on Microsoft Windows is 260 characters\. The longest path in the Amazon FreeRTOS download is 122 characters\. To accommodate the files in the Amazon FreeRTOS projects, make sure that the path to the `<amazon-freertos>` directory is fewer than 98 characters long\. For example, `C:\Users\Username\Dev\AmazonFreeRTOS` works, but `C:\Users\Username\Documents\Development\Projects\AmazonFreeRTOS` causes build failures\. 

   To download Amazon FreeRTOS from GitHub:

   1. Browse to the [Amazon FreeRTOS GitHub repository](https://github.com/aws/amazon-freertos)\.

   1. Choose **Clone or download**\.

   1. From the command line on your computer, clone the repository to a directory on your host machine\.

      ```
      git clone https://github.com/aws/amazon-freertos.git --recurse-submodules
      ```

   1. From the *<amazon\-freertos>* directory, check out the branch that you want to use\.
**Note**  
The maximum length of a file path on Microsoft Windows is 260 characters\. Lengthy Amazon FreeRTOS download directory paths can cause build failures\.  
In the Getting Started documentation, the path to the Amazon FreeRTOS download directory is referred to as `<amazon-freertos>`\.

1. Set up your development environment\.

   1. Install the latest version of [WinPCap](https://www.winpcap.org)\.

   1. Install Microsoft Visual Studio\.

      Visual Studio versions 2017 and 2019 are known to work\. All editions of these Visual Studio versions are supported \(Community, Professional, or Enterprise\)\.

      In addition to the IDE, install the **Desktop development with C\+\+ ** component\.

      Install the latest Windows 10 SDK\. You can choose this under the **Optional** section of the **Desktop development with C\+\+** component\. 

   1. Make sure that you have an active hard\-wired Ethernet connection\.

## Build and Run the Amazon FreeRTOS Demo Project<a name="gsg-ecc608a-build-and-run"></a>

**Build and Run the Amazon FreeRTOS Demo Project with the Visual Studio IDE**

1. Load the project into Visual Studio\.

   In Visual Studio, from the **File** menu, choose **Open**\. Choose **File/Solution**, navigate to the `<amazon-freertos>projects\microchip\ecc608a_plus_winsim\visual_studio\aws_demos\aws_demos.sln` file, and then choose **Open**\.

1. Retarget the Demo Project\.

   The provided demo project depends on the Windows SDK, but it does not have a Windows SDK version specified\. By default, the IDE might attempt to build the demo with an SDK version not present on your machine\. To set the Windows SDK version, right\-click on **aws\_demos** and then choose **Retarget Projects**\. This opens the **Review Solution Actions** window\. Choose a Windows SDK Version that is present on your machine \(the initial value in the dropdown is fine\), then choose **OK**\.

1. Build and run the project\.

   From the **Build** menu, choose **Build Solution**, and make sure the solution builds without errors\. Choose **Debug, Start Debugging** to run the project\. On the first run, you will need to [Configure Your Network Interface](getting_started_windows.md#win-network-interface) and recompile\.

1. Provision the ECC608a\.

   Microchip has provided several scripting tools to help with the setup of the ATECC608a parts\. Navigate to `<amazon-freertos>\vendors\microchip\secure_elements\app\example_trust_chain_tool`, and open the README\.md file located there\.

   Follow the instructions in the README\.md file to provision your device\. This will navigate you through:

   1. Creating and registering a certificate authority with AWS\.

   1. Generating your keys on the ECC608a and exporting the public key and device serial number\.

   1. Generating a certificate for the device and registering that certificate with AWS\.

   1. Loading the CA certificate and device certificate onto the device\.

1. Build and Run Amazon FreeRTOS Samples

   Re\-run the demo project again\. This time you should connect\!

## Troubleshooting<a name="ecc680a-troubleshooting"></a>

For general troubleshooting information about Getting Started with Amazon FreeRTOS, see [Troubleshooting Getting Started](gsg-troubleshooting.md)\.