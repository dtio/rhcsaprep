# File redirection

    Redirect stdout, stderr while appending
    >> file 2>&1
    &>> file

    Redirect stdout, stderr and replacing
    > file 2>&1
    &> file

    Write to file with tee

    command |& tee filename.log
    command 2>&1 tee filename.log

    to append use tee -a
    command |& tee -a filename.log
    command 2>&1 tee -a filename.log


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

# firewalld

    # firewall-cmd --permanent --zone=block --add-source=x.x.x.x/32
    # firewall-cmd --permanent --zone=public --add-service=mysql
    # filreall-cmd --permanent --zone=public --add-port=8080/tcp
    # firewall-cmd --reload 

# podman

    .config/containers/registries.conf

    unqualified-search-registries == ['registry.lab.example.com']

    [[registry]]
    location = "registry.lab.example.com"
    insecure = true
    blocked = false

    $ podman login registry.lab.example.com
    
    $ podman run -d --name webapp \
    -p 8090:8080 \
    -v ~/webcontent:/var/www:Z \
    registry.lab.example.com/rhel9/httpd-24
    
    $ podman run -d --name db-app01 \
    -e MYSQL_USER=developer \
    -e MYSQL_PASSWORD=redhat \
    -e MYSQL_DATABASE=inventory \
    -e MYSQL_ROOT_PASSWORD=redhat \
    -p 13306:3306 \
    -v /home/podmgr/storage/database:/var/lib/mysql/data:Z \
    registry.lab.example.com/rhel9/mariadb-105
    
    $ mkdir -p ~/.config/systemd/user/
    $ cd ~/.config/systemd/user
    $ podman generate systemd --name webapp --new --files 
    $ podman stop webapp
    $ podman rm webapp
    $ systemctl --user daemon-reload
    $ systemctl --user enable --now container-webapp
    $ loginctl enable-linger

    In case permission issue
    $ podman exec -it db-app01 grep mysql /etc/passwd
    $ podman unshare chown 27:27 /home/user/db_data

# find command

    # find / -iname filename
    # find / -user username1 -group group1 -perm 640 
    # find / -type f -size 100c 