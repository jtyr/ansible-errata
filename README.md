errata
======

Ansible role which helps to patch RedHat-based systems.

The configuration of the role is done in such way that it should not be
necessary to change the role for any kind of configuration. All can be
done either by changing role parameters or by declaring completely new
configuration as a variable. That makes this role absolutely
universal. See the examples below for more details.

Please report any issues or send PR.


Examples
--------

```
---

- name: Fix all erratas
  hosts: myhost1
  roles:
    - errata

- name: Patch only security erratas
  hosts: myhost2
  vars:
    # Enable security erratas only
    errata_security: yes
  roles:
    - errata

- name: Patch only security and bugfixes erratas
  hosts: myhost3
  vars:
    # Enable security erratas
    errata_security: yes
    # Enable bugfixes erratas
    errata_bugfix: yes
  roles:
    - errata

- name: Patch only Critical and Important security erratas
  hosts: myhost4
  vars:
    # Enable security erratas only
    errata_security: yes
    # Limit the severity to Critical and Important patches
    errata_security_severity:
      - Critical
      - Important
  roles:
    - errata

- name: Patch only specific security erratas if found
  hosts: myhost5
  vars:
    # Enable security erratas
    errata_security: yes
    # Enable bugfixes erratas
    errata_bugfix: yes
    # List of erratas to patch
    errata_list:
      - RHBA-2016:2748
      - RHSA-2016:1940
      - RHSA-2016:2702
      - RHSA-2016:2674
  roles:
    - errata

- name: Print matching erratas but don't fix them
  hosts: myhost6
  vars:
    # Enable printing of matching erratas
    errata_print_match: yes
    # Disable fixing
    errata_fix: no
  roles:
    - errata
```


Role variables
--------------

Variables used by the role is as follows:

```
# YUM security plugin package (explicit version can be specified here)
errata_plugin_pkg: yum-plugin-security

# Whether to clean YUM cache before searching for erratas
errata_clean: yes

# List of erratas to check
# (set to 'all' to fix all erratas)
errata_list: all

# Whether to check for bugfixes advisories
errata_bugfix: no

# Whether to check for security advisories
errata_security: no

# List of security severities
errata_security_severity:
  - Critical
  - Important
  - Moderate
  - Low

# Whether to print the list of found erratas
errata_print_found: no

# Whether to print the list of matching erratas
errata_print_match: no

# Whether to fix the found erratas
errata_fix: yes

# Filter which produces the final list of found erratas
errata_filter: "| grep -v 'not found' | sed 's/\ .*//' | sort | uniq"
```


License
-------

MIT


Author
------

Jiri Tyr
