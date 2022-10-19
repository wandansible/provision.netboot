Ansible role: Provision - Netboot
=================================

Provisions a host with an unattended install via PXE. This is
done by configuring the necessary files on a netboot server to
allow the machine to be netbooted and a debian preseed file to
be passed to the OS installer. The provision user is
automatically created when the host is installed and the
username is set to provision_user_username and the password is
set to provision_user_password.

Requirements
------------

This role depends on the [base provision role](https://github.com/wandansible/provision).

To use this role, the python package `netaddr` must be installed on the host running ansible.

Role Variables
--------------

```
ENTRY POINT: main - Provision preseeded host via PXE

        Provisions a host with an unattended install via PXE. This is
        done by configuring the necessary files on a netboot server to
        allow the machine to be netbooted and a debian preseed file to
        be passed to the OS installer. The provision user is
        automatically created when the host is installed and the
        username is set to provision_user_username and the password is
        set to provision_user_password.

OPTIONS (= is mandatory):

- debian_mirror
        Debian apt mirror URL
        [Default: http://deb.debian.org/debian/]
        type: str

- desktop_env
        Name of desktop environment to install
        [Default: (null)]
        type: str

- netboot_disk
        Disk to use as the root drive, leave as an empty string to use
        the first non-usb disk drive as the root drive
        [Default: (null)]
        type: str

- netboot_distribution
        Distribution being installed, e.g. "ubuntu" or "debian"
        [Default: (null)]
        type: str

- netboot_domain
        DNS search domain configured during installation, by default
        this is set to everything after the first "." in
        inventory_hostname
        [Default: (null)]
        type: str

- netboot_enable_unattended_upgrades
        If true, enable unattended upgrades during installation
        [Default: False]
        type: bool

- netboot_extra_boot_options
        Text for extra grub boot configuration
        [Default: (null)]
        type: str

- netboot_extra_packages
        List of extra packages to install
        [Default: (null)]
        elements: str
        type: list

- netboot_extra_preseed_lines
        Text for extra debian preseed configuration
        [Default: (null)]
        type: str

- netboot_extra_tasks
        List of extra tasks to install
        [Default: (null)]
        elements: str
        type: list

- netboot_groups
        List of unix groups to add during installation
        [Default: (null)]
        elements: dict
        type: list

        OPTIONS:

        = gid
            ID for the group

            type: int

        = name
            Name for the group

            type: str

= netboot_image
        Name of the image to boot and install

        type: str

- netboot_locale
        Locale configured during installation
        [Default: en_US.UTF-8]
        type: str

= netboot_mac_addr
        MAC address for the interface used for netbooting

        type: str

- netboot_mirror
        URL for apt mirror configured during installation
        [Default: (null)]
        type: str

- netboot_network_interface
        Network interface static configuration. The value of this is
        compatible with https://github.com/wandansible/networking, and
        by default the first interface from network_interfaces is
        selected if it contains static configuration. When this is set
        to an empty dict then DHCP or autoconfiguration is used.
        [Default: (null)]
        type: dict

- netboot_ntp_server
        Domain or address for NTP server configured during
        installation
        [Default: pool.ntp.org]
        type: str

- netboot_packages
        List of packages to install
        [Default: ['openssh-server', 'lsb-release', 'sudo']]
        elements: str
        type: list

- netboot_partition
        Partition configuration
        [Default: {'method': 'regular', 'recipe': 'atomic'}]
        type: dict

        OPTIONS:

        - lvm_guided_size
            If method = lvm, set the size of the logical volume(s)
            based on this value. This may be given as a percentage of
            the resulting volume group, as a size of the combined
            logical volume(s), or as "max" to use the entire volume
            group.
            [Default: max]
            type: str

        - method
            Partition type or method of installation
            (Choices: regular, lvm, ask-during-install)[Default:
            regular]
            type: str

        - recipe
            Partition structure for filesystem(s):
              * atomic: all files in one partition
              * home: separate /home partition
              * multi: separate /home, /var, and /tmp partitions
            (Choices: atomic, home, multi)[Default: atomic]
            type: str

- netboot_preseed_dir
        Directory, on the remote netboot server, for the debian
        preseed files
        [Default: /srv/https/preseed]
        type: str

- netboot_preseed_url
        URL for the hosts debian preseed file
        [Default: (null)]
        type: str

= netboot_server
        Hostname of the remote netboot server to configure, this must
        match the inventory_hostname of a netboot server in your
        ansible inventory

        type: str

- netboot_tasks
        List of tasks to install
        [Default: (null)]
        elements: str
        type: list

- netboot_tftp_dir
        Directory, on the remote netboot server, containing the files
        served over TFTP
        [Default: /srv/tftp]
        type: str

- netboot_timezone
        Timezone configured during installation
        [Default: Etc/UTC]
        type: str

- netboot_wait
        Netboot wait times
        [Default: {'boot': {'retries': 100, 'delay': 10, 'post_delay':
        120}, 'ssh': {'delay': 0, 'timeout': 3600}}]
        type: dict

        OPTIONS:

        = boot
            Wait times during initial PXE boot

            type: dict

            OPTIONS:

            = delay
                Time in seconds to wait between checks

                type: int

            = post_delay
                Time in seconds to wait after the host is reachable
                before removing the grub and preseed configuration
                from the remote netboot server

                type: int

            = retries
                Number of attempts to check if the host is reachable

                type: int

        = ssh
            Wait times while checking for SSH connection

            type: dict

            OPTIONS:

            = delay
                Time in seconds to wait before starting to poll

                type: int

            = timeout
                Time in seconds to wait for an SSH connection

                type: int

- provision_user_fullname
        Fullname for provision user
        [Default: ansible]
        type: str

- provision_user_groups
        List of groups to add the provision user to
        [Default: ['sudo']]
        elements: str
        type: list

- provision_user_uid
        UID for the provision user, leave this set to 0 to
        automatically select a UID
        [Default: (null)]
        type: int

- root_password
        Password for root
        [Default: (null)]
        type: str

- ubuntu_mirror
        Ubuntu apt mirror URL
        [Default: http://archive.ubuntu.com/ubuntu/]
        type: str
```

Installation
------------

This role can either be installed manually with the ansible-galaxy CLI tool:

    ansible-galaxy install git+https://github.com/wandansible/provision,main,wandansible.provision
    ansible-galaxy install git+https://github.com/wandansible/provision.netboot,main,wandansible.provision.netboot
     
Or, by adding the following to `requirements.yml`:

    - name: wandansible.provision
      src: https://github.com/wandansible/provision
    - name: wandansible.provision.netboot
      src: https://github.com/wandansible/provision.netboot

Roles listed in `requirements.yml` can be installed with the following ansible-galaxy command:

    ansible-galaxy install -r requirements.yml

Example Playbook
----------------

    - hosts: all
      roles:
         - role: wandansible.provision.netboot
           become: true
           vars:
             netboot_image: debian-11-x86_64
             netboot_server: pxe.example.org
             netboot_locale: en_NZ.UTF-8
             netboot_timezone: "Etc/UTC"
             netboot_domain: example.org
             root_password: "hunter2"
