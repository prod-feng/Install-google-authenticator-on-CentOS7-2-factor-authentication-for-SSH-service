# Install-google-authenticator-on-CentOS7-2-factor-authentication-for-SSH-service

The following describes the process I did to install and configure the 2 factor authentication of SSH service using google-authenticator on CentOS 7.

1.) Down load the source : https://github.com/google/google-authenticator

2.) > unzip  google-authenticator-master.zip

   >cd libpam/
   
   >./bootstrap.sh
   
   >./configure
   
   >make
   
   >sudo make install
   
If the "./configure" failed for some reason, that means you may need to install some packages, use yum and any tools to install those packages.


3.) Modify the file "/etc/pam.d/sshd", add one line like:

   >auth required pam_sepermit.so

   >auth required /usr/local/lib/security/pam_google_authenticator.so

   >...

4.) Modify the file "/etc/ssh/sshd_config, change:

  > ChallengeResponseAuthentication yes
  
5.) restart SSH

  >service restart sshd
  
6.) As a regular user, login, and run:

> google-authenticator  (should be in /usr/local/bin according the above configure)

You can anwser "yes" to all questions. Save a copy of all the keys and secrets on the screen.


7.) Use you smar phone, install "Google Autheticator" app, scan the code on the screen generate by above step, or input codes manually. Done.


When this step is done, you are all set. 

When you try to login again, you will be prompted as:

>>ssh <your server>

>>Verification code:  (input the code on your smar phone)

>>Password:

If you want any other users who don't set the google authenticator to login as usuall, you can change the line file "/etc/pam.d/sshd":

 >auth required /usr/local/lib/security/pam_google_authenticator.so
 
 to be:
 
  >auth required /usr/local/lib/security/pam_google_authenticator.so nullok
  
  Done.
