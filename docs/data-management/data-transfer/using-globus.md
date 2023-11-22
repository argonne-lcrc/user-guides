# Using Globus

- [Log in](https://globus.org/) to Globus using your Argonne Domain Account credentials by selecting Argonne National Laboratory from the existing organizational list. Alternatively, you can sign up for and use your local Globus username and password.
- Select `File Manager` from the side bar.
  - On the File Manager page, you can view and search the list of available endpoints by clicking the text field labeled `Collection`.
    - You can use the LCRC endpoint **"LCRC Improv DTN"** (as well as other sources or destinations).
    - You can type letters into the box to filter endpoints.
- Once you select the LCRC endpoint:
  - You will be prompted with an "Authentication/Consent Required" page. Click "Continue".
  - On the "Identity Required" page, select your `@anl.gov` identity from the list. You may need to authenticate here with your Argonne credentials.
  - Next, click "Allow" to give consent for the Globus Web App to manage transfers and access data.
- You will see a listing of the contents of your home directory in LCRC by default. Double click on a directory to view its contents.
- You can change the Path to another LCRC directory on the shared filesystem that you have access to as needed.
- Select a file or directory and click on the highlighted `arrow button` to initiate the transfer.

## Transferring Data Between LCRC and Your Machine

To transfer data between LCRC and your laptop or desktop, you can install [Globus Connect Personal](https://www.globus.org/globus-connect-personal) on your machine and access it via Globus. Globus Connect Personal is available as a one-click install for Mac, Linux, and Windows.

- Install Globus Connect Personal following the instructions for your operating system.
- Run Globus Connect Personal on your local machine. The Globus Connect Personal webpage can walk you through the setup instructions.
- You should now see your machine in the list of endpoints on Globus. You can select it to view the contents of your machine and transfer files to and from it as above.
