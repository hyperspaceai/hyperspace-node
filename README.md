# Hyperspace Desktop App: Revolutionize AI Accessibility.
This desktop application aims to enable seamless participation in a decentralized AI network, empowering users with limited GPU resources to contribute to and benefit from AI innovations

Download the Alpha Release: The Hyperspace Desktop App is available exclusively for macOS users in its early alpha stages. Embark on this pioneering journey by downloading the app and be among the first to experience the forefront of decentralized AI computing.

As an alpha version, the app is in its initial development phase and we welcome user feedback to enhance its functionality.

### How to Install and Run Hyperspace App
`Important Notice:` Our application, Hyperspace, is currently in the process of obtaining code signing certification. In the meantime, macOS might apply certain security attributes that prevent the app from running properly. Below are the steps to assist you in installing and running Hyperspace.

### Step 1: Removing Extended Attributes

macOS may mark the downloaded Hyperspace app with extended attributes that can prevent it from opening. To remove these attributes:

1. Download and move the Hyperspace app to your Applications folder.
2. Open the Terminal app (found in the Utilities folder within your Applications folder).
3. In the Terminal, type the following command and press Enter:

```bash
xattr -cr /Applications/Hyperspace.app/
```

### Step 2: Opening the Application

After performing the above step, you should be able to open the Hyperspace app normally. If you still encounter any warnings or issues:

### Use Gatekeeper Settings (Optional)

If the app still doesn't open, you might need to temporarily disable Gatekeeper:

1. In Terminal, type the following command and press Enter:

```bash
sudo spctl --master-disable
```

2. You will be prompted to enter your password.
3. After disabling Gatekeeper, try opening the app again.
4. Once the app is running, we strongly recommend re-enabling Gatekeeper for your security. Use this command:

```bash
sudo spctl --master-enable
```


## Contact Us
For any issues, concerns, or questions about this process, please reach out to us on twitter.

**Disclaimer:** Please be aware that these instructions modify system settings and the attributes of the Hyperspace app. Proceed only if you trust the source of the application. We are not responsible for any harm or loss resulting from these actions.



