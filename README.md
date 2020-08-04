   # INSTALLATION

  - git clone https://github.com/ishantd/subdomains-django.git
  - pip3 install django-subdomains
 # Configuration
- in settings.py add middleware `'subdomains.middleware.SubdomainURLRoutingMiddleware',` before ` 'django.middleware.common.CommonMiddleware',`
- Setup SITE_ID and add django.contrib.sites to installed applications
- Configure subdomain URL CONFS:
```
ROOT_URLCONF = 'subtest.urls'
# A dictionary of urlconf module paths, keyed by their subdomain.
SUBDOMAIN_URLCONFS = {
    None: 'subtest.urls',  # no subdomain, e.g. ``example.com``
    'stest': 'apptwo.urls',
}
```

# ISSUES

### Problem
-   #### TypeError: object() takes no parameters
-   PR: https://github.com/tkaemming/django-subdomains/issues/59
### SOLUTION
-   The issue is resolved by replacing
```class SubdomainMiddleware(object):```
with
```
try:
    from django.utils.deprecation import MiddlewareMixin
except ImportError:
    MiddlewareMixin = object

class SubdomainMiddleware(MiddlewareMixin):
```
in the middleware.py file.

### Problem
-   #### ModuleNotFoundError: No module named 'django.core.urlresolvers
-   PR: https://github.com/tkaemming/django-subdomains/issues/69
### SOLUTION
-   You need replace all occurrences of:

```from django.core.urlresolvers import reverse```

to:

```from django.urls import reverse```