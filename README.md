# django-sip-phonebook
[![PyPi packaged](https://badge.fury.io/py/django-sip-phonebook.svg)](http://badge.fury.io/py/django-sip-phonebook)

A django-powered central phonebook and provisioning server for Grandstream telephones. For more information on the format see http://www.grandstream.com/sites/default/files/Resources/gxp_wp_xml_phonebook.pdf





Quick start
-----------

1. Add "phonebook" to your INSTALLED_APPS setting like this::
```
    INSTALLED_APPS = [
        ...
        'phonebook',
    ]
```
2. Include the phonebook URLconf in your project urls.py like this::
```
    url(r'^phonebook/', include('phonebook.urls')),
```
3. Run `python manage.py migrate` to create the phonebook models.

4. Start the development server and visit http://127.0.0.1:8000/admin/
   to setup phone book entries (you'll need the Admin app enabled).
   
	`python manage.py runserver`

5. Create a new Django user to represent the user that can fetch the phonebook.
  - From the admin area, manually create a new user, specifying a password. 
  - Verify the User is *active*, but does not have Staff or Superuser status.
  - Grant this user the permission `"phonebook | contact | Download phonebook via. basic auth"`

  - Whilst logged in as admin/superuser verify that you can see an XML file at this location: http://127.0.0.1:8000/phonebook/phonebook.xml

  - Now test with an incognito tab (ie. different browser session) to:
	 http://127.0.0.1:8000/phonebook/phonebook.xml

	Within an incognito tab it should trigger the HTTP 1.0 Basic authentication dialog which should accept *only* the username and password created above, and return the phone book XML document.

6.  Release to a real webserver, or restart the development server to listen on a real ip address, not *localhost*:
	`$ python manage.py runserver 192.168.1.xxx:8000`
	
7. Setup your Grandstream IP phone to fetch a remote phonebook 
	Set the server to be:
		`127.0.0.1:8000/phonebook`
	No protocol or filename is required. User the username and password above. Sync every 5 minutes. 
	The filename phonebook.xml is automatically appended by the handset.
