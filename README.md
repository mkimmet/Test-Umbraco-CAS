# Umbraco-CAS-Membership
This is a down and dirty implementation of CAS with Umbraco for membership logins.  I'm sure there are better ways to implement CAS with umbraco, using the [CAS .NET module](https://wiki.jasig.org/display/casc/.net+cas+client) but I have not been able to figure it out in a way that is satisfactory.

## How to Setup Umbraco with CAS for Memberships
This walks you through setting up Umbraco with CAS for memberships only, not for the backend Umbraco admin logins.

### Create a login page in Umbraco
Step one is to create a new page in the Umbraco Admin for your login page.

Then edit the layout view for this page and replace the code with the code from MembershipLogin.cshtml

In the code, replace the 4 variables at the top with your own site and CAS settings:

string SITE_LOGIN_URL = "https://localhost/login"; //The url to this page

string CAS_LOGIN_URL = "https://login.yourorganization.com/cas/login"; //your CAS login url

string CAS_VALIDATE_URL = "https://login.yourorganization.com/cas/serviceValidate"; //Your CAS service validator URL

string DEFAULT_PAGE = "/home"; //this is where to send people if they are already logged on.

Currently if there is an error the error details are just posted to the login page.  If you want to redirect to a different page in the event of an error you will need todo this yourself.

Save the file.

### Set a page as being protected
In the Umbraco Admin set up a Member Group if you haven't already.  Click on the Members menu item and then right click on Member Groups and click Create.  Give the group a name and select create.  

Next, right click on Members and click create and then member.  Give the Member a full name at the top, then click on the properties tab and for Login type in their username.  MAKE SURE this username is identical to the one that is used by CAS.  If you don't do this then Umbraco won't know about the user even though CAS can authenticate them.  Type in their email and then click Save.

Finally, click on the Contents menu icon and then right click on a page you want to protect and select Public access.  Pick the role that you just created and move it to the column labeled "Member of Groups."  Under Login Page, click Choose and select the login page that you created above.  For the Error page you can choose a page for errors to go to if you have created a page for it, we haven't so I just selected the login page again.  Then click save.

### Go to your page and give it a try
At this point everything should be wired up.  Go to the page that you protected and see if it redirects you to CAS, and lets you log in. In your code on your protected pags you can use User.Identity.Name to obtain the username of the person that has logged into Umbraco through CAS.
