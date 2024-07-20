React Native library to easily integrate Fingerprint Device support in your app (for UIDAI Aadhaar based secure authentication in India). It is only for Android Devices.

As per UIDAI (Aadhaar) guidelines, only registered biometric devices can be used for Aadhaar Authentication. These devices come with RDService drivers (usually available on PlayStore) that exposes a standard API.

This library makes it easy to work with all such devices so that your app can search for installed drivers and get the fingerprint data after a scan.

For reference, you may check out the (Aadhaar Registered Devices by UIDAI).

Installation
npm install react-native-rdservice-fingerprintscanner
Add jitpack in your root build.gradle file at the end of repositories: android/build.gradle

allprojects {
  repositories {
    // ...
    maven { url 'https://jitpack.io' }
  }
}
Usage
import {
  getDeviceInfo,
  captureFinger,
  isDriverFound,
  openFingerPrintScanner,
  AVAILABLE_PACKAGES,
  DEFAULT_PID_OPTIONS,
} from 'react-native-rdservice-fingerprintscanner';

// ...

getDeviceInfo()
  .then((response) => {
    console.log(response, 'DEVICE DRIVER FOUND'); // Response about Device Driver
  })
  .catch((error) => {
    console.log(error, 'DEVICE DRIVER NOT FOUND'); //Failed to get device information
  });

captureFinger(pidOptions) //you can pass pidOptions (optional) to "captureFinger(pidOptions)"" method otherwise it takes DEFAULT_PID_OPTIONS
  .then((response) => {
    console.log(response, 'FINGER CAPTURE'); // FingerPrint Response
  })
  .catch((e) => {
    console.log(e, 'ERROR_FINGER_CAPTURE'); // Failed to capture the Fingerprint
  });

isDriverFound(PACKAGE_NAME) // you can use AVAILABLE_PACKAGES for PACKAGE_NAME
  .then((res) => {
    console.log(res, 'DRIVER CHECK');
  })
  .catch((error) => {
    console.log(error, 'ERROR_DRIVER CHECK');
  });

openFingerPrintScanner(PACKAGE_NAME, pidOptions) //you can pass pidOptions (optional) to "openFingerPrintScanner(pidOptions)"" method otherwise it takes DEFAULT_PID_OPTIONS
  .then((res) => {
    console.log(res, 'FINGER CAPTURE');
  })
  .catch((e) => {
    console.log(e, 'ERROR_FINGER_CAPTURE');
  });
pidOptions is an XML String that you have to pass to captureFinger and openFingerPrintScanner methods. Refer UIDAI Document

DEFAULT_PID_OPTIONS is used when you not passed the pidOptions to the captureFinger() Method.

PACKAGE_NAME is required to check the rd service. (eg) The Package name of StarTek Device is com.acpl.registersdk

You can use AVAILABLE_PACKAGES for PACKAGE_NAME.

Note : Call captureFinger() Method after getting response from getDeviceInfo() method. Calling of captureFinger() method before getDeviceInfo() method, only returns Error in catch block. Refer Example Code. Call openFingerPrintScanner() method once the isDriverFound() method returns true value, if the driver is not found then the openFingerPrintScanner() method returns driver not found error. These added two methods (isDriverFound() and openFingerPrintScanner()) are used to find the selected RD Service which is passed as an argument (device driver package name) to these methods.

Response JSON Object
getDeviceInfo() Method Reponse

Key	Value	Description
status	-1 or 1 or 0	-1 - Device Driver not Found, 1 - READY, 0 - NOTREADY
isWhitelisted	true or false	iT is about the Device is Approved or not. true - Approved, false - Not Approved
rdServiceInfoJson	JSON DATA	The device returns XML DATA of Device Information. this parameter contains converted JSON DATA of XML DATA
rdServiceInfoXML	XML DATA	Device Information
rdServicePackage	Device Package	
message	Message about Success or Failure	
captureFinger() and openFingerPrintScanner() Methods Reponse

Key	Value	Description
status	1 or 0	1 - Fingerprint Captured Successfully, 0 - FingerPrint not Captured (Check Connection of Device and OTG Connection Settings in Mobile)
errorCode	ERROR CODE from RD Service	Refer UIDAI Document and RDService Error Details
errInfo	Error Message according to the ERROR CODE	Refer UIDAI Document and RDService Error Details
pidDataJson	JSON DATA	The device returns PID DATA of Captured Fingerprint. this parameter contains converted JSON pidData of XML pidData
pidDataXML	XML DATA	pidData Captured Fingerprint
rdServicePackage	Device Package	
message	Message about Success or Failure	
isDriverFound() Method Response

Key	Value	Description
isDeviceDriverFound	true or false	true - Driver Found, false - Driver not found
message	Message about the driver found or not	
Tested Devices
Device Name	Result
Startek FM220	✅
MORPHO	✅
MANTRA	✅
Tatvik TMF20	✅
PB510	✅
