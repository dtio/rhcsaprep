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
