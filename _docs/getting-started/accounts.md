---
parent: getting-started
layout: docs
sidenav: true
title: Get access to cloud.gov
weight: -100
---

## Get access to cloud.gov

* **If you're in EPA, GSA, or NSF:** You automatically have access and can log in using your agency credentials.
* **If you're in FDIC:** Some FDIC staff automatically have access and can log in using agency credentials. If you try to log in and receive a "status message is null" error, contact the FDIC identity team to be added to the cloud.gov access group.
* **If you're in another agency:** If you have a U.S. federal government email address, you can [sign up for access](https://account.fr.cloud.gov/signup).
* **Otherwise:** If your team uses cloud.gov and you don't have a federal government email address (such as if you're a contractor), ask a teammate to [invite you]({{ site.baseurl }}{% link _docs/orgs-spaces/roles.md %}).

## Log into cloud.gov

### Agency single sign-on accounts

If you have an EPA, FDIC, GSA, or NSF email address, sign in using your agency credentials. Follow [these instructions to log in]({{ site.baseurl }}{% link _docs/getting-started/setup.md %}).

### cloud.gov accounts

If you were invited with an email address that isn't part of an agency with single sign-on authentication to cloud.gov, you have a cloud.gov account. Follow [these instructions to log in]({{ site.baseurl }}{% link _docs/getting-started/setup.md %}). When you log in via a web browser, select the **cloud.gov** option.

Setting up a cloud.gov account requires using a password and Time-based One-Time-Password (TOTP) application. Any application or service which is compatible with RFC [6238](https://tools.ietf.org/html/rfc6238) will work. We recommend leveraging an authenticator application or service which supports cross-device or backup access so you don't need to reach out to cloud.gov support when you need to replace your phone or laptop. Here is a non-exhaustive list of cross-device authenticators which are compatible with cloud.gov:

* [Microsoft Authenticator](https://www.microsoft.com/en-us/account/authenticator)
* [LastPass Password Manager](https://www.lastpass.com/solutions/business-password-manager)
* [1password](https://1password.com/)
* [Authy](https://www.authy.com/app/mobile)

When you log into cloud.gov for the first time, follow the instructions to set this up.

#### To change your password

* **If you know your current password**

*Follow these steps:*

1. Login to your cloud.gov account at <https://login.fr.cloud.gov/>
1. Go to <https://account.fr.cloud.gov/change-password> to change the password
    1. Enter your old password into the `Old Password` input
    1. Then create and enter the new password into `New Password` input and confirm it in the `Repeat New Password` input
    1. Finally, click *CHANGE*


#### To reset your password

* **If you're logged out and forgot your password**
* **If you're logged out and your password has expired**

*Follow these steps:*

1. Go to log into your cloud.gov account at <https://login.fr.cloud.gov>
1. Click on *Forgot your password?* link to reset your password
1. Enter your email address into the `Email address` input and click *SEND EMAIL* to receive the reset password confirmation email
1. Go to your email and click on the verification link in the first step of that email
1. Enter your email address in the `Email address` input on the reset password verification page and click *VERIFY EMAIL*
1. Your email is verified and then copy/save the temporary password under the `Your temporary password`.
1. Login to your cloud.gov account at <https://login.fr.cloud.gov/> with the new temporary password
1. After logging in, go to <https://account.fr.cloud.gov/change-password> to change the password
    1. Enter the temporary password into the `Old Password` input
    1. Then create and enter the new password into `New Password` input and confirm it in the `Repeat New Password` input
    1. Finally, click *CHANGE*

#### If you need to change TOTP to a new device

If you need to register a TOTP token on a new device and still have access to your old device (e.g. you got a new phone and still have your old phone),
you can [reset your TOTP token here](https://account.fr.cloud.gov/reset-totp). Log in with your old device then reset your token. You'll be
logged out and can set up tokens on your new device on your next log in.

#### If you can't access your token codes

If you need to set up a new authentication application, such as if you lose your phone, [email cloud.gov support](mailto:support@cloud.gov?subject=MFA%20reset&body=I%20need%20to%20set%20up%20a%20new%20authentication%20application.%20I%20understand%20this%20means%20the%20contents%20of%20my%20sandbox%20space%20will%20be%20deleted%20if%20I%20have%20one,%20and%20that%20you%20will%20remove%20my%20permissions%20to%20other%20spaces%20and%20orgs.) so that we can allow you to set up a new one. We'll follow this process to mitigate the risk of requests from compromised email addresses:

1. Delete the contents of your sandbox space (if you have one).
2. Remove your permissions to any other spaces and orgs.
3. For those spaces and orgs, notify the Space Managers and Org Managers that we've removed your access because of your request to reset your account's authentication application.
4. Reset your account's authentication application.
5. Let you know this is complete, so that you can set up a new authentication application and request access from your Space Managers and Org Managers again. It is their responsibility to verify that this is a legitimate request from you.

You can copy this into your email (or write something similar), so that we know to go ahead with that process right away:

> I need to set up a new authentication application. I understand this means the contents of my sandbox space will be deleted if I have one, and that you will remove my permissions to other spaces and orgs.

## Use your account responsibly

Acceptable uses of cloud.gov include:

* Building and managing government digital services.
* Making and trying test systems to learn about how cloud.gov works.
* Activities contributing to the development of cloud.gov itself.

In order to help us keep cloud.gov secure, we require that you use your account appropriately. When you use cloud.gov, you agree that you'll respect these rules of behavior:

- Conduct only authorized business on the system.
- Maintain the confidentiality of your authentication credentials; a cloud.gov operator should never ask you to reveal them. We recommend using a password manager and strong credentials.
- Log out when you no longer need session access. Never leave your computer unattended while logged into cloud.gov.
- Report all security incidents or suspected incidents (such as improper or suspicious acts) related to cloud.gov systems and networks to [cloud.gov support]({{ site.baseurl }}/docs/help/).
- Safeguard system resources against waste, loss, abuse, unauthorized use or disclosure, and misappropriation.
- Don't process U.S. classified national security information on the system.
- Don't browse, search or reveal information hosted by cloud.gov except as required to perform your legitimate tasks or assigned duties.
- Don't retrieve information, or in any other way disclose information, for someone who does not have authority to access that information.
- Don't intentionally use a client that makes use of obsolete or insecure encryption algorithms.
- Don't configure your browser to ignore security warnings which may involve your connection with cloud.gov; report warnings that you can't explain to [cloud.gov support]({{ site.baseurl }}/docs/help/).
- If you believe you've been granted more access than necessary to perform your legitimate tasks or assigned duties, immediately notify [cloud.gov support]({{ site.baseurl }}/docs/help/).

If you use a cloud.gov account (instead of using an agency single sign-on account), you have an additional rule of behavior:

- Don't share your account with another person or create anonymous or group accounts. Your account is just for you.

Access to systems and networks owned by cloud.gov is governed by, and subject to, all federal laws, including, but not limited to, the Privacy Act, 5 U.S.C. 552a, if the applicable cloud.gov system maintains individual Privacy Act information. Access to cloud.gov systems constitutes consent to the retrieval and disclosure of the information within the scope of your authorized access, subject to the Privacy Act, and applicable state and federal laws.

Please contact [cloud.gov support]({{ site.baseurl }}/docs/help/) if you have questions about these rules or don't understand them.

## Trouble logging in

If you are a GSA employee encountering issues logging into cloud.gov, please refer to [this knowledge base article]({{ site.baseurl }}/knowledge-base/2021-05-24-secureauth-issues/) for more information.
