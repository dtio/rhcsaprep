# File redirection

    Redirect stdout, stderr while appending
    >> file 2>&1
    &>> file

    Redirect stdout, stderr and replacing
    > file 2>&1
    &> file

# VIM Shortcuts

    v - characters selection
    V - line selection
    CTRL-v - block selection

# Network creation

    nmcli con add con-name customname type ethernet ifname ethX

    File will be created in
    /etc/NetworkManager/system-connections/

    nmcli con show customname

# rsyslog

    Configure additional logging
    /etc/rsyslog.d/debug.conf
    *.debug /var/log/messages-debug

    Testing logging
    # logger -p user.debug "Test message"

# Repo format

    [errata]
    name=Red Hat Updates
    baseurl=http://content.example.com/rhel9.3/x86_64/rhcsa-practice/errata
    enabled=1
    gpgcheck=0

# Grep

    Exclude empty lines
    grep -v ^$

    Include multiple pattern in grep search
    grep -e abc -e def file

# Timezone stuffs

    timedatectl set-timezone Asia/Singapore
    timezonectl set-ntp true
    chronyc sources -v

# systemd-tmpfiles

    Config sample /etc/tmpfiles.d/momentary.conf

    d /run/test 0700 root root 30s

    Create the directory
    # systemd-tmpfiles --create /etc/tmpfiles.d/momentary.conf

    Cleanup after expiry
    # systemd-tmpfiles --clean /etc/tmpfiles.d/momentary.conf

# autofs direct mount

    /etc/auto.master.d/direct.autofs

        /-  /etc/auto.direct

    /etc/auto.direct

        /direct -rw,sync,nfs4   server:/share/direct

# autofs indirect mount
    
    /etc/auto.master.d/indirect.autofs

        /mainshare  /etc/auto.indirect

    /etc/auto.indirect

        *   -rw,sync,nfs4   server:/share/&

# tuned

    # tuned-adm list
    # tuned-adm active
    # tuned-adm profile profilename

# nice

    The lower number the higher is the priority
    # renice -n -10 pidnumber

# Boot Issues

    remove console from linux line
    add rd.break

    mount -o remount,rw /sysroot
    chroot /sysroot
    passwd root
    touch .autorelabel
    exit
    exit

    Or add systemd.unit=emergency.target

# Selinux Stuffs

    # chcon -t httpd_sys_content_t /directory
    or
    # semanage fcontext -a -t http_sys_content_t '/directory(/.*)?'
    # restorecon -Rv /directory

    # semanage port -a -t http_port_t -p tcp 82

    # getsebool -a
    # setsebool -P httpd_enable_homedirs on
    # semanage boolean -l

    # grep sealert /var/log/messages
    # sealert -l lookupid
    # sealert -a /var/log/audit/audit.log

# parted

    # parted /dev/vdb mklabel gpt
    # parted /dev/vdb mkpart userdata xfs 2048s 1000MB
    After partition creation
    # udevadm settle
    After updating /etc/fstab
    # systemctl daemon-reload

# stratis

    # dnf install stratisd stratis-cli
    # stratis pool create pool1 /dev/vdb
    # stratis pool add-data pool1 /dev/vdc
    # stratis pool list
    # stratis blockdev list pool1
    # stratis filesystem create pool1 fs1
    # stratis filesystem list
    # stratis filesystem snapshot pool1 fs1 snapshot1

    /etc/fstab entry
    UUID=XXXX /dir xfs defaults,x-systemd.requires=stratisd.service 0 0

# lvm

    # parted /dev/vdb mkpart primary 514MiB 1026MiB
    # parted /dev/vdb set 2 lvm on
    # udevadm settle

    