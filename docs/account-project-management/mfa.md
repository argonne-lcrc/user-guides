# Duo MFA

## Enrolling in Duo MFA

After you have [joined the **lcrc** project or a sub-project of LCRC](../project-management/#join-an-existing-lcrc-project), you will be required to enroll in the CELS Duo MFA system on your next login to the [LCRC Accounts System](https://accounts.lcrc.anl.gov). If you are already logged in, you can logout and log back in to complete the setup right away.

*Note*: If you cannot install the Duo Mobile App in Step *5* below, please first verify that your device OS is up to date. You can also reference the [Duo help documentation](https://help.duo.com) for a list of compatibile devices and versions. If you still cannot install the Duo Mobile App because your device cannot be updated, is not compatible, or you do not have a Smartphone/Tablet, please contact [support@lcrc.anl.gov](mailto:support@lcrc.anl.gov) and we may assign you a [Yubikey](mfa-yubikey.md) instead.

1. Login to [https://accounts.lcrc.anl.gov](https://accounts.lcrc.anl.gov) with your Argonne Domain or Argonne Collaborator account username and password. If you have forgotten your password, please call the Argonne Service Desk at +1-630-252-9999.

2. You should be prompted to configure Duo MFA. Select the device type you want to register for Duo.
![LCRC Duo Config 1](../images/lcrc_duo_1.png)

3. Add your mobile phone number.
![LCRC Duo Config 2](../images/lcrc_duo_2.png)

4. Select your device OS. We will choose Android in this example.
![LCRC Duo Config 3](../images/lcrc_duo_3.png)

5. Install the Duo Mobile App. The official Duo documentation has instructions for both [iOS](https://guide.duo.com/iphone) and [Android](https://guide.duo.com/android) devices. Make sure you are downloading the official Duo Mobile App as it may not be the first search in your device's app store.
![LCRC Duo Config 4](../images/lcrc_duo_4.png)

6. Open the Duo Mobile App, tap the + button, and scan the barcode.
![LCRC Duo Config 5](../images/lcrc_duo_5.png)

7. Choose your default authentication method.
![LCRC Duo Config 6](../images/lcrc_duo_6.png)

8. Continue to login and test your newly added device.
![LCRC Duo Config 7](../images/lcrc_duo_7.png)

As a reminder, please contact [support@lcrc.anl.gov](mailto:support@lcrc.anl.gov) with any issues.

Once Duo MFA is configured, you can now [configure your SSH key](ssh.md) to complete our login requirements. All future logins will require your MFA token.

## Add a Device/Secondary Auth to Duo

You occasionally may need to change or add a device. Adding multiple methods of authentication is important, so that you're able to add new devices in situations when your original device may be no longer in your possession.

To start, visit [https://accounts.lcrc.anl.gov](https://accounts.lcrc.anl.gov) and enter your Argonne login credentials on that screen.  You will be prompted to initiate a "Duo Push" or enter a code.  However, on the left side of the screen is another menu, including "Add a new device" and "My Settings & Devices".  Selecting either of those will allow you to add more authentication devices to your account.

Note, Duo Federal (our provider for this service) does not allow SMS-based authentication, so adding a second device, if you have one, is a good way to ensure you don't get locked out of your account.

## Device No Longer in Your Possession

If you are unable to login due to no longer possessing your registered device, please contact [support@lcrc.anl.gov](mailto:support@lcrc.anl.gov).
