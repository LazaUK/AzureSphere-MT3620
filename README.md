# Secure voting machine - powered by Azure Sphere
<p align="center">
  <img src="/images/Overview_AvNetSK.jpg">
</p>

## Infrastructure Setup:
1.	In Azure portal, create and link to each other Azure IoT Hub and Azure IoT Hub Device Provisioning Service (DPS) resources, using the following Quick Setup guide: https://docs.microsoft.com/en-us/azure/iot-dps/quick-setup-auto-provision.

> **Note**: To use Device Twin capability, IoT Hub should be on the Standard pricing tier.
 
2.	In Azure Sphere Developer Command Prompt, download CA certificate from Azure Sphere tenant using the following command:
```
azsphere tenant download-CA-certificate --output CAcertificate.cer
```
You should see confirmation that the CA certificate has been saved.
 
3.	In Azure portal, upload certificate to Azure IoT DPS -> Certificates. After upload it will show new entry with an “Unverified” status.
 
4.	Then open certificate record and click “Generate Verification Code” button.
 
5.	In Azure Sphere Developer Command Prompt, download validation certificate signed with the DPS verification code from Step 4 above using the following command:
azsphere tenant download-validation-certificate --output ValidationCertification.cer --verificationcode <DPS_VERIFICATION_CODE>
You should see confirmation that the validation certificate has been saved.
 
6.	In Azure portal, upload validation certificate into “Verification Certificate” field of the record window from Step 4 and click “Verify” button. After validation, Azure will change the status of your certificate to “Verified”.
 
7.	Switch to Azure IoT DPS -> Manage Enrolments menu and add new enrolment group with the primary certificate that we verified in Step 6 above.
 
## Software configuration:
1.	In Azure Sphere Developer Command Prompt, execute the following command to get Azure Sphere tenant’s ID (1ed8d750-7cd9-43ff-8714-b68ba7d65e4e):
azsphere tenant show-selected
2.	In Azure portal, switch to Azure IoT DPS -> Overview and copy DPS ID Scope value (0ne0009B33D).
3.	From Azure IoT DPS -> Linked IoT Hubs copy the full name of the linked IoT Hub (Laziz-IoTHub.azure-devices.net).
4.	Download content of Git repo.
5.	In Visual Studio, click File -> Open -> CMake, navigate to the repo’s Software -> VotingApp and then open app_manifest.json file.
 
6.	Update placeholders highlighted in the screenshot below with the values of your Azure Sphere tenant’s ID, DPS ID Scope and IoT Hub names collected in Steps 1, 2 and 3 above.

7.	Select CMakeLists.txt file and then click Build -> Rebuild Current Document (CMakeLists.txt). Verify that an image package is being generated as shown below.
 
8.	In the toolbar, choose “GDB Debugger (HLCore)” as the target.
 
9.	In Azure Sphere Developer Command Prompt, enable application development capability on Azure Sphere device:
azsphere device enable-development
10.	Back in Visual Studio, click Debug -> Start (or press F5) to deploy Voting Machine app to the device. If successful, Visual Studio will execute Voting Machine app on Azure Sphere device. In Output window, choose “Device Output” option. If you will press any of 2 buttons, you should see the message updates in the debug window.
 
## Analytics configuration:
1.	In Azure, create new Stream Analytics job and add IoT Hub as its stream input
 
2.	Add PowerBI as the output for Stream Analytics job and click Authorize.
 
3.	Provide relevant output alias name, so that you can use it in the Stream Analytics query.
 
4.	Alternatively, you can setup Azure storage account or a database as an output for Azure Stream Analytics job.
 
5.	Last part is to create your dashboard in PowerBI. You may configure it directly at https://powerbi.microsoft.com/en-us/ or use richer functionality with the PowerBI desktop client.

## Working model - YouTube video
You can find short demo of the working solution here on YouTube

## Credits:
<div class="text-green mb-2 ml-4">
  This solution is based on the original sample code from Microsoft Azure Sphere team, published
</div>[here](https://github.com/Azure/azure-sphere-samples)
