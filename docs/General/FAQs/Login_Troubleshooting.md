> ### Requirements {#prerequisites}
>
> Please make sure you have followed the recommended setup. See
> [Choosing and Configuring Software for Connecting to the
> Clusters](https://support.nesi.org.nz/hc/en-gb/articles/360001016335)
> for more information.

> ### New Command Line Users {#prerequisites}
>
> -   Most terminals do not give an indication of how many characters
>     have been typed when entering a password.
> -   Paste is not usually bound to `ctrl` + `V` and will vary based on
>     your method of access.

Repeatedly asking for First and Second Factor.
==============================================

In addition to using an incorrect First/Second factor there are several
other issues that will cause a similar looking failure to log in. 

    Login Password:
    Login Password:
    Login Password:

OR

    Login Password (First Factor): 
    Authenticator Code (Second Factor):
    Login Password (First Factor): 
    Authenticator Code (Second Factor):
    Login Password (First Factor): 
    Authenticator Code (Second Factor):

### 1. Try logging in to `lander` directly.

You can test what part of your connection has failed by first running:

``` {.highlight}
ssh <user>@lander.nesi.org.nz
```

**If this succeeds**: Run the following:

``` {.highlight}
ssh login.<mahuika/maui>.nesi.org.nz
```

**If this fails:** Are you logging in to the correct cluster?
Mahuika/Māui have separate access control, also Māui requires your
password input in a different format, see
[here](https://support.nesi.org.nz/hc/en-gb/articles/360001244876-Mahuika-M%C4%81ui-Differences).

**If this succeeds**: If you are using a bash terminal, confirm your
.ssh config is [set up
correctly](https://support.nesi.org.nz/hc/en-gb/articles/360000161315#recLinux).

If you are using a ssh client like *MobaXterm* or *WinSCP* make sure
your session is [set up
correctly](https://support.nesi.org.nz/hc/en-gb/articles/360000161315#recMoba).

### 2. Check you are a member of an active project

If you are not a member of an active project, or your project has no
active allocation, you will not be able to log in. You should be able to
find whether you have any active projects with active
allocations [here](https://my.nesi.org.nz/html/view_projects). 

### 3. Confirm you are using the correct username and password

The most common cause of login failure is using incorrect login details.
Make sure you are using your NeSI Username and the password you set when
first logging into the Lander node. See
[my.nesi.org.nz](https://my.nesi.org.nz/).

### 4. Check the time on your device

If the device you are using as authentication token is not using
NZST/NZDT, or is not keeping the correct time, the second factor code
generated will be invalid. Even an error of a few seconds can be enough
to invalidate the second factor code.

If your device can\'t keep time properly for whatever reason, please
contact the person or team responsible for supporting it.

### 5. Ensure you\'re not reusing the same 6-digit code from your token.

Login will fail if the same 6-digit code[ is ]{.dfn
.dictionary-of-numbers}used to access the Māui or Mahuika login node
after it has been used to access the lander node, or for consecutive
login attempts to any node. If in doubt, wait [30 seconds]{.dfn
.dictionary-of-numbers .dictionary-of-numbers-quantity-30s
.dictionary-of-numbers-processed} for a new token to be generated.

### 6. Ensure the correct Second Factor token is being used

Two-factor authentication is becoming a common security measure. Many
people have multiple tokens and occasionally mix them up.

### 7. Wait [four hours]{.dfn .dictionary-of-numbers .dictionary-of-numbers-quantity-14400s .dictionary-of-numbers-processed}

[Six failed login attempts ]{.dfn .dictionary-of-numbers}within [five
minutes]{.dfn .dictionary-of-numbers
.dictionary-of-numbers-quantity-300s .dictionary-of-numbers-processed}
will trigger a four-hour lockout. Users experiencing login issues can
inadvertently trigger the lockout, making diagnosing the original issue
much more difficult.  

Connection closed by \.... (MobaXterm) {#mobaPassPassPass}
======================================

### 1. Skip password prompts.

There is a known MobaXterm bug in which a user who has set a second
factor and is trying to log in to the lander node will be prompted
multiple times for \'Password\' before being prompted for \'First
Factor\'. (On the lander node, you should only be prompted for a
\'password\' if you have no Second Factor set up.)

These initial prompts can be skipped through by pressing \'Enter\'. Any
input before pressing Enter will cause the login to fail.

The expected processes is as follows:

``` {.highlight}
ssh <user>@lander.nesi.org.nz 
<user>@lander.nesi.org.nz's password: <Enter>
<user>@lander.nesi.org.nz's password: <Enter>
<user>@lander.nesi.org.nz's password: <Enter>
Login Password (First Factor): 
Authenticator Code (Second Factor):
```

*Note: Sometimes MobaXterm will prompt with a dialogue box.*

### 2. Update your MobaXTerm client.

Occasionally an outdated client can cause errors.\
MobaXterm can be updated through: \'help\>check for updates\'

### 3. Reinstall your MobaXTerm client.

Asked for \'Password\' instead of \'First Factor\'
==================================================

### 1. Check the status using [my.nesi.org.nz](https://my.nesi.org.nz/) and confirm you have an authentication token registered.

### 2. See [above](#mobaPassPassPass).

Authentication token manipulation error
=======================================

This occurs when your authentication token is out of sync. You will have
to reset your token though [my.nesi.org.nz](https://my.nesi.org.nz/).

Nothing here has helped? {#contactNesi}
========================

[Contact NeSI support](https://support.nesi.org.nz/hc/requests/new).

Helpful things to include:

-   The client you are using (WSL, MobaXterm, Mac terminal, Linux,
    etc.).
-   The nature of the problem, including the precise text of any error
    message you have been receiving.
    -   Did you start out having [one login problem and ]{.dfn
        .dictionary-of-numbers}are now getting a different one? If so,
        when did the change happen, and were you doing anything in
        particular related to logging in at the time things changed?
-   Have you successfully logged in in the past? if so when was the last
    time you successfully logged in, and to what NeSI cluster?
-   Has anything administrative and relevant to NeSI access changed
    since you last logged in? For example:
    -   Have you opened or joined any new NeSI projects, or have any of
        your existing NeSI projects closed?
    -   Have any of your NeSI projects been granted new allocations, had
        a previously granted new allocation actually start, or had an
        existing allocation modified?
    -   Have any of your NeSI projects\' existing allocations ended?
    -   Have any of your NeSI projects had a disk space quota change?
    -   Have you changed your institutional username and password, moved
        to a different institution, or started a new job at an
        institution while also keeping your position at your old
        institution? Might NeSI know about any of these changes?
-   What have you tried so far?
-   Are you on the NIWA network, the NIWA VPN, or neither?

::: {#collapseExample .collapse}
::: {.card .card-body}
 
:::
:::