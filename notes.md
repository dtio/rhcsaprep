# VIM Shortcuts

    v - characters selection
    V - line selection
    CTRL-v - block selection

# systemd-tmpfiles

    Config sample

    d /run/test 0700 root root 30s

    Create the directory
    # systemd-tmpfiles --create

    Cleanup after expiry
    # systemd-tmpfiles --clean

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



