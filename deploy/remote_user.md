## Overview

Starting with 7.0.0, Seafile supports docking external authentication systems (LemonLdap, Shibboleth.) based on the proxy server request header (HTTP_REMOTE_USER, HTTP_X_AUTH_USER, etc.).

After the front-end proxy server (Apache/Nginx) is successfully authenticated, the user information is set to the request header, and Seafile creates and logs in the user based on this information.

Note: Make sure that the proxy server has a corresponding security mechanism to protect against forgery request header attacks.

Please add the following settings to `conf/seahub_settings.py` to enable this feature.

```
ENABLE_REMOTE_USER_AUTHENTICATION = True

# otional, HTTP header, which is configured in your web server conf file,
# used for Seafile to get user's unique id, default value is 'HTTP_REMOTE_USER'.
REMOTE_USER_HEADER = 'HTTP_REMOTE_USER'

# otional, when value of HTTP_REMOTE_USER is not a valid email address，
# Seafile will use this setting to combine user's unique id to a valid one.
REMOTE_USER_DOMAIN = 'example.com'

# otional, if create new user in Seafile system, default value is True.
# if disable this setting, user not exist in Seafile system will unable to login.
REMOTE_USER_CREATE_UNKNOWN_USER = True

# otional, if active new created user in Seafile system, default value is True.
# if disable this setting, user will unable to login.
# the administrator needs to manually activate this user.
REMOTE_USER_ACTIVATE_USER_AFTER_CREATION = True

# otional, map user attribute in HTTP header and Seafile's user attribute.
REMOTE_USER_ATTRIBUTE_MAP = {
    'HTTP_DISPLAYNAME': 'name',
    'HTTP_MAIL': 'contact_email',

    # for shibboleth user info
    "HTTP_GIVENNAME": 'givenname',
    "HTTP_SN": 'surname',
    "HTTP_ORGANIZATION": 'institution',
    
    # for shibboleth user role
    'HTTP_Shibboleth-affiliation': 'affiliation',
}

# for shibboleth user role
SHIBBOLETH_AFFILIATION_ROLE_MAP = {
    'employee@uni-mainz.de': 'staff',
    'member@uni-mainz.de': 'staff',
    'student@uni-mainz.de': 'student',
    'employee@hu-berlin.de': 'guest',
    'patterns': (
        ('*@hu-berlin.de', 'guest1'),
        ('*@*.de', 'guest2'),
        ('*', 'guest'),
    ),
}
```

Then restart Seafile.
