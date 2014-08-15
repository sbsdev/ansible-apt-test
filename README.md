# ansible-apt-test

Minimal ansible playbook to test apt with local debs

Since Ansible 1.6. the apt module supports the direct installation of
deb packages. This can be useful for packages that you build yourself
and you do not want to maintain a local repository.

``` yaml
# Install a .deb package
- apt: deb=/tmp/mypackage.deb
```

Now if you try to do this for a number of packages you'll get an error

``` yaml
- name: install debs
  apt: deb="/tmp/{{ item }}"
  with_items: debs
```

results in something like 

``` shell
TASK: [install debs] ********************************************************** 
fatal: [xmlp-test] => failed to parse: 
SUDO-SUCCESS-vezxbadxecppwcnzcahcazlrvcajnbze
Traceback (most recent call last):
  File "/home/eglic/.ansible/tmp/ansible-tmp-1408095944.08-178150558729123/apt", line 1906, in <module>
    main()
  File "/home/eglic/.ansible/tmp/ansible-tmp-1408095944.08-178150558729123/apt", line 523, in main
    force=force_yes, dpkg_options=p['dpkg_options'])
  File "/home/eglic/.ansible/tmp/ansible-tmp-1408095944.08-178150558729123/apt", line 299, in install_deb
    pkg = apt.debfile.DebPackage(deb_file)
  File "/usr/lib/python2.7/dist-packages/apt/debfile.py", line 57, in __init__
    self.open(filename)
  File "/usr/lib/python2.7/dist-packages/apt/debfile.py", line 66, in open
    self._debfile = apt_inst.DebFile(self.filename)
SystemError: E:Could not open file tcolorbox_2.51-5_all.deb - open (2: No such file or directory), E:Unable to determine the file size - fstat (9: Bad file descriptor), E:Read error - read (9: Bad file descriptor)


FATAL: all hosts have already failed -- aborting
```

To reproduce this problem simply clone this repo and run the playbook with 

``` shell
ansible-playbook -i staging -K test.yml
```
