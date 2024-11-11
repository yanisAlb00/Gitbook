# Process monitoring

Obtenir la liste des utilisateurs connectés sur le système

```
// who et w sont équivalents
who
yanis    tty2         2024-11-09 11:36 (tty2)
w
 17:36:22 up 2 days,  5:59,  1 user,  load average: 1.33, 0.81, 0.68
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
yanis    tty2     tty2             Sat11    2days  0.07s  0.06s /usr/libexec/gnome-session-binary

```

Lister tous les processus en cours sur le système

```
ps -ef
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 Nov09 ?        00:00:04 /sbin/init
root           2       0  0 Nov09 ?        00:00:00 [kthreadd]
root           3       2  0 Nov09 ?        00:00:00 [rcu_gp]
root           4       2  0 Nov09 ?        00:00:00 [rcu_par_gp]
root           5       2  0 Nov09 ?        00:00:00 [slub_flushwq]
root           6       2  0 Nov09 ?        00:00:00 [netns]
root           8       2  0 Nov09 ?        00:00:00 [kworker/0:0H-events_highpri]
root          10       2  0 Nov09 ?        00:00:00 [mm_percpu_wq]
root          11       2  0 Nov09 ?        00:00:00 [rcu_tasks_kthread]
root          12       2  0 Nov09 ?        00:00:00 [rcu_tasks_rude_kthread]
root          13       2  0 Nov09 ?        00:00:00 [rcu_tasks_trace_kthread]
root          14       2  0 Nov09 ?        00:00:03 [ksoftirqd/0]

```

Tracer l'arborescence des processus

```
ps -edf --forest
UID          PID    PPID  C STIME TTY          TIME CMD
root           2       0  0 Nov09 ?        00:00:00 [kthreadd]
root           3       2  0 Nov09 ?        00:00:00  \_ [rcu_gp]
root           4       2  0 Nov09 ?        00:00:00  \_ [rcu_par_gp]
root           5       2  0 Nov09 ?        00:00:00  \_ [slub_flushwq]
root           6       2  0 Nov09 ?        00:00:00  \_ [netns]
root           8       2  0 Nov09 ?        00:00:00  \_ [kworker/0:0H-events_highpri]
root          10       2  0 Nov09 ?        00:00:00  \_ [mm_percpu_wq]

pstree
systemd─┬─ModemManager───2*[{ModemManager}]
        ├─NetworkManager───2*[{NetworkManager}]
        ├─accounts-daemon───2*[{accounts-daemon}]
        ├─avahi-daemon───avahi-daemon
        ├─bluetoothd
        ├─colord───2*[{colord}]
        ├─containerd───14*[{containerd}]
        ├─cron
        ├─cups-browsed───2*[{cups-browsed}]
        ├─cupsd
        ├─dbus-daemon
        ├─dockerd───16*[{dockerd}]
        ├─fwupd───4*[{fwupd}]
        ├─gdm3─┬─gdm-session-wor─┬─gdm-wayland-ses─┬─gnome-session-b───3*[{gnome-session-b}]
        │      │                 │                 └─2*[{gdm-wayland-ses}]
        │      │                 └─2*[{gdm-session-wor}]
        │      └─2*[{gdm3}]
        ├─geoclue───3*[{geoclue}]
        ├─iio-sensor-prox───2*[{iio-sensor-prox}]
        ├─low-memory-moni───2*[{low-memory-moni}]
        ├─polkitd───2*[{polkitd}]
        ├─power-profiles-───2*[{power-profiles-}]
        ├─rtkit-daemon───2*[{rtkit-daemon}]
        ├─switcheroo-cont───2*[{switcheroo-cont}]
        ├─systemd─┬─(sd-pam)
        │         ├─at-spi2-registr──
```

Equivalent du gestionnaire de tâches Windows

```
top
```

Modifier la priorité associée à un processus :&#x20;

La priorité associée à un processus s'étend de -20 à 20. Plus la valeur est élevée, moins le processus est gourmand en CPU.

