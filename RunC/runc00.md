# RunC 源码解读

解读RunC源码，RunC GitHub地址：https://github.com/opencontainers/runc  

目录树如下：  
工程有点庞大，每天下班后花2~4小时分析，欢迎参与。  

## [返回目录](https://github.com/MulticsYin/MulticsDevOps#dockerkubernetes)  


```
.
├── checkpoint.go
├── contrib
│   ├── cmd
│   │   └── recvtty
│   │       └── recvtty.go
│   └── completions
│       └── bash
│           └── runc
├── CONTRIBUTING.md
├── create.go
├── delete.go
├── Dockerfile
├── events.go
├── exec.go
├── init.go
├── kill.go
├── libcontainer
│   ├── apparmor
│   │   ├── apparmor_disabled.go
│   │   └── apparmor.go
│   ├── capabilities_linux.go
│   ├── cgroups
│   │   ├── cgroups.go
│   │   ├── cgroups_test.go
│   │   ├── cgroups_unsupported.go
│   │   ├── fs
│   │   │   ├── apply_raw.go
│   │   │   ├── apply_raw_test.go
│   │   │   ├── blkio.go
│   │   │   ├── blkio_test.go
│   │   │   ├── cpuacct.go
│   │   │   ├── cpu.go
│   │   │   ├── cpuset.go
│   │   │   ├── cpuset_test.go
│   │   │   ├── cpu_test.go
│   │   │   ├── devices.go
│   │   │   ├── devices_test.go
│   │   │   ├── freezer.go
│   │   │   ├── freezer_test.go
│   │   │   ├── fs_unsupported.go
│   │   │   ├── hugetlb.go
│   │   │   ├── hugetlb_test.go
│   │   │   ├── memory.go
│   │   │   ├── memory_test.go
│   │   │   ├── name.go
│   │   │   ├── net_cls.go
│   │   │   ├── net_cls_test.go
│   │   │   ├── net_prio.go
│   │   │   ├── net_prio_test.go
│   │   │   ├── perf_event.go
│   │   │   ├── pids.go
│   │   │   ├── pids_test.go
│   │   │   ├── stats_util_test.go
│   │   │   ├── utils.go
│   │   │   ├── utils_test.go
│   │   │   └── util_test.go
│   │   ├── rootless
│   │   │   └── rootless.go
│   │   ├── stats.go
│   │   ├── systemd
│   │   │   ├── apply_nosystemd.go
│   │   │   └── apply_systemd.go
│   │   ├── utils.go
│   │   └── utils_test.go
│   ├── compat_1.5_linux.go
│   ├── configs
│   │   ├── blkio_device.go
│   │   ├── cgroup_linux.go
│   │   ├── cgroup_unsupported.go
│   │   ├── cgroup_windows.go
│   │   ├── config.go
│   │   ├── config_linux.go
│   │   ├── config_linux_test.go
│   │   ├── config_test.go
│   │   ├── config_windows_test.go
│   │   ├── device_defaults.go
│   │   ├── device.go
│   │   ├── hugepage_limit.go
│   │   ├── interface_priority_map.go
│   │   ├── mount.go
│   │   ├── namespaces.go
│   │   ├── namespaces_linux.go
│   │   ├── namespaces_syscall.go
│   │   ├── namespaces_syscall_unsupported.go
│   │   ├── namespaces_unsupported.go
│   │   ├── network.go
│   │   └── validate
│   │       ├── rootless.go
│   │       ├── rootless_test.go
│   │       ├── validator.go
│   │       └── validator_test.go
│   ├── console_freebsd.go
│   ├── console.go
│   ├── console_linux.go
│   ├── console_solaris.go
│   ├── console_windows.go
│   ├── container.go
│   ├── container_linux.go
│   ├── container_linux_test.go
│   ├── container_solaris.go
│   ├── container_windows.go
│   ├── criu_opts_linux.go
│   ├── criu_opts_windows.go
│   ├── criurpc
│   │   ├── criurpc.pb.go
│   │   ├── criurpc.proto
│   │   └── Makefile
│   ├── devices
│   │   ├── devices_linux.go
│   │   ├── devices_test.go
│   │   ├── devices_unsupported.go
│   │   └── number.go
│   ├── error.go
│   ├── error_test.go
│   ├── factory.go
│   ├── factory_linux.go
│   ├── factory_linux_test.go
│   ├── generic_error.go
│   ├── generic_error_test.go
│   ├── init_linux.go
│   ├── integration
│   │   ├── checkpoint_test.go
│   │   ├── doc.go
│   │   ├── execin_test.go
│   │   ├── exec_test.go
│   │   ├── init_test.go
│   │   ├── seccomp_test.go
│   │   ├── template_test.go
│   │   └── utils_test.go
│   ├── keys
│   │   └── keyctl.go
│   ├── message_linux.go
│   ├── network_linux.go
│   ├── notify_linux.go
│   ├── notify_linux_test.go
│   ├── nsenter
│   │   ├── namespace.h
│   │   ├── nsenter_gccgo.go
│   │   ├── nsenter.go
│   │   ├── nsenter_test.go
│   │   ├── nsenter_unsupported.go
│   │   ├── nsexec.c
│   │   └── README.md
│   ├── process.go
│   ├── process_linux.go
│   ├── README.md
│   ├── restored_process.go
│   ├── rootfs_linux.go
│   ├── rootfs_linux_test.go
│   ├── seccomp
│   │   ├── config.go
│   │   ├── fixtures
│   │   │   └── proc_self_status
│   │   ├── seccomp_linux.go
│   │   ├── seccomp_linux_test.go
│   │   └── seccomp_unsupported.go
│   ├── setgroups_linux.go
│   ├── setns_init_linux.go
│   ├── specconv
│   │   ├── example.go
│   │   ├── spec_linux.go
│   │   └── spec_linux_test.go
│   ├── SPEC.md
│   ├── stacktrace
│   │   ├── capture.go
│   │   ├── capture_test.go
│   │   ├── frame.go
│   │   ├── frame_test.go
│   │   └── stacktrace.go
│   ├── standard_init_linux.go
│   ├── state_linux.go
│   ├── state_linux_test.go
│   ├── stats_freebsd.go
│   ├── stats.go
│   ├── stats_linux.go
│   ├── stats_solaris.go
│   ├── stats_windows.go
│   ├── sync.go
│   ├── system
│   │   ├── linux.go
│   │   ├── proc.go
│   │   ├── proc_test.go
│   │   ├── syscall_linux_386.go
│   │   ├── syscall_linux_64.go
│   │   ├── syscall_linux_arm.go
│   │   ├── sysconfig.go
│   │   ├── sysconfig_notcgo.go
│   │   ├── unsupported.go
│   │   └── xattrs_linux.go
│   ├── tags
│   ├── user
│   │   ├── lookup.go
│   │   ├── lookup_unix.go
│   │   ├── lookup_unsupported.go
│   │   ├── MAINTAINERS
│   │   ├── user.go
│   │   └── user_test.go
│   ├── utils
│   │   ├── cmsg.go
│   │   ├── utils.go
│   │   ├── utils_test.go
│   │   └── utils_unix.go
│   └── xattr
│       ├── errors.go
│       ├── xattr_linux.go
│       ├── xattr_test.go
│       └── xattr_unsupported.go
├── LICENSE
├── list.go
├── main.go
├── MAINTAINERS
├── MAINTAINERS_GUIDE.md
├── Makefile
├── man
│   ├── md2man-all.sh
│   ├── README.md
│   ├── runc.8.md
│   ├── runc-checkpoint.8.md
│   ├── runc-create.8.md
│   ├── runc-delete.8.md
│   ├── runc-events.8.md
│   ├── runc-exec.8.md
│   ├── runc-kill.8.md
│   ├── runc-list.8.md
│   ├── runc-pause.8.md
│   ├── runc-ps.8.md
│   ├── runc-restore.8.md
│   ├── runc-resume.8.md
│   ├── runc-run.8.md
│   ├── runc-spec.8.md
│   ├── runc-start.8.md
│   ├── runc-state.8.md
│   └── runc-update.8.md
├── NOTICE
├── notify_socket.go
├── pause.go
├── PRINCIPLES.md
├── ps.go
├── README.md
├── restore.go
├── rlimit_linux.go
├── run.go
├── script
│   ├── check-config.sh
│   ├── release.sh
│   ├── tmpmount
│   └── validate-gofmt
├── signals.go
├── spec.go
├── start.go
├── state.go
├── tests
│   └── integration
│       ├── cgroups.bats
│       ├── checkpoint.bats
│       ├── create.bats
│       ├── debug.bats
│       ├── delete.bats
│       ├── events.bats
│       ├── exec.bats
│       ├── help.bats
│       ├── helpers.bash
│       ├── kill.bats
│       ├── list.bats
│       ├── mask.bats
│       ├── pause.bats
│       ├── ps.bats
│       ├── README.md
│       ├── root.bats
│       ├── spec.bats
│       ├── start.bats
│       ├── start_detached.bats
│       ├── start_hello.bats
│       ├── state.bats
│       ├── testdata
│       │   └── hello-world.tar
│       ├── tty.bats
│       ├── update.bats
│       └── version.bats
├── tty.go
├── update.go
├── utils.go
├── utils_linux.go
├── vendor
│   ├── github.com
│   │   ├── coreos
│   │   │   ├── go-systemd
│   │   │   │   ├── activation
│   │   │   │   │   ├── files.go
│   │   │   │   │   ├── listeners.go
│   │   │   │   │   └── packetconns.go
│   │   │   │   ├── dbus
│   │   │   │   │   ├── dbus.go
│   │   │   │   │   ├── methods.go
│   │   │   │   │   ├── properties.go
│   │   │   │   │   ├── set.go
│   │   │   │   │   ├── subscription.go
│   │   │   │   │   └── subscription_set.go
│   │   │   │   ├── LICENSE
│   │   │   │   ├── README.md
│   │   │   │   └── util
│   │   │   │       ├── util_cgo.go
│   │   │   │       ├── util.go
│   │   │   │       └── util_stub.go
│   │   │   └── pkg
│   │   │       ├── dlopen
│   │   │       │   ├── dlopen_example.go
│   │   │       │   └── dlopen.go
│   │   │       ├── LICENSE
│   │   │       ├── NOTICE
│   │   │       └── README.md
│   │   ├── docker
│   │   │   ├── docker
│   │   │   │   ├── LICENSE
│   │   │   │   ├── NOTICE
│   │   │   │   ├── pkg
│   │   │   │   │   ├── mount
│   │   │   │   │   │   ├── flags_freebsd.go
│   │   │   │   │   │   ├── flags.go
│   │   │   │   │   │   ├── flags_linux.go
│   │   │   │   │   │   ├── flags_unsupported.go
│   │   │   │   │   │   ├── mounter_freebsd.go
│   │   │   │   │   │   ├── mounter_linux.go
│   │   │   │   │   │   ├── mounter_unsupported.go
│   │   │   │   │   │   ├── mount.go
│   │   │   │   │   │   ├── mountinfo_freebsd.go
│   │   │   │   │   │   ├── mountinfo.go
│   │   │   │   │   │   ├── mountinfo_linux.go
│   │   │   │   │   │   ├── mountinfo_unsupported.go
│   │   │   │   │   │   └── sharedsubtree_linux.go
│   │   │   │   │   ├── README.md
│   │   │   │   │   ├── symlink
│   │   │   │   │   │   ├── fs.go
│   │   │   │   │   │   ├── LICENSE.APACHE
│   │   │   │   │   │   ├── LICENSE.BSD
│   │   │   │   │   │   └── README.md
│   │   │   │   │   └── term
│   │   │   │   │       ├── tc_linux_cgo.go
│   │   │   │   │       ├── tc_other.go
│   │   │   │   │       ├── term.go
│   │   │   │   │       ├── termios_darwin.go
│   │   │   │   │       ├── termios_freebsd.go
│   │   │   │   │       ├── termios_linux.go
│   │   │   │   │       ├── term_windows.go
│   │   │   │   │       └── winconsole
│   │   │   │   │           ├── console_windows.go
│   │   │   │   │           └── term_emulator.go
│   │   │   │   └── README.md
│   │   │   └── go-units
│   │   │       ├── duration.go
│   │   │       ├── LICENSE
│   │   │       ├── README.md
│   │   │       └── size.go
│   │   ├── godbus
│   │   │   └── dbus
│   │   │       ├── auth_external.go
│   │   │       ├── auth.go
│   │   │       ├── auth_sha1.go
│   │   │       ├── call.go
│   │   │       ├── conn_darwin.go
│   │   │       ├── conn.go
│   │   │       ├── conn_other.go
│   │   │       ├── dbus.go
│   │   │       ├── decoder.go
│   │   │       ├── doc.go
│   │   │       ├── encoder.go
│   │   │       ├── export.go
│   │   │       ├── homedir_dynamic.go
│   │   │       ├── homedir.go
│   │   │       ├── homedir_static.go
│   │   │       ├── LICENSE
│   │   │       ├── message.go
│   │   │       ├── object.go
│   │   │       ├── README.markdown
│   │   │       ├── sig.go
│   │   │       ├── transport_darwin.go
│   │   │       ├── transport_generic.go
│   │   │       ├── transport_unixcred_dragonfly.go
│   │   │       ├── transport_unixcred_linux.go
│   │   │       ├── transport_unix.go
│   │   │       ├── variant.go
│   │   │       ├── variant_lexer.go
│   │   │       └── variant_parser.go
│   │   ├── golang
│   │   │   └── protobuf
│   │   │       ├── LICENSE
│   │   │       ├── proto
│   │   │       │   ├── clone.go
│   │   │       │   ├── decode.go
│   │   │       │   ├── encode.go
│   │   │       │   ├── equal.go
│   │   │       │   ├── extensions.go
│   │   │       │   ├── lib.go
│   │   │       │   ├── message_set.go
│   │   │       │   ├── pointer_reflect.go
│   │   │       │   ├── pointer_unsafe.go
│   │   │       │   ├── properties.go
│   │   │       │   ├── text.go
│   │   │       │   └── text_parser.go
│   │   │       └── README.md
│   │   ├── mrunalp
│   │   │   └── fileutils
│   │   │       ├── fileutils.go
│   │   │       ├── idtools.go
│   │   │       ├── LICENSE
│   │   │       └── README.md
│   │   ├── opencontainers
│   │   │   ├── runtime-spec
│   │   │   │   ├── LICENSE
│   │   │   │   ├── README.md
│   │   │   │   └── specs-go
│   │   │   │       ├── config.go
│   │   │   │       ├── state.go
│   │   │   │       └── version.go
│   │   │   └── selinux
│   │   │       ├── go-selinux
│   │   │       │   ├── label
│   │   │       │   │   ├── label.go
│   │   │       │   │   └── label_selinux.go
│   │   │       │   ├── selinux.go
│   │   │       │   └── xattrs.go
│   │   │       ├── LICENSE
│   │   │       └── README.md
│   │   ├── seccomp
│   │   │   └── libseccomp-golang
│   │   │       ├── LICENSE
│   │   │       ├── README
│   │   │       ├── seccomp.go
│   │   │       └── seccomp_internal.go
│   │   ├── sirupsen
│   │   │   └── logrus
│   │   │       ├── alt_exit.go
│   │   │       ├── CHANGELOG.md
│   │   │       ├── doc.go
│   │   │       ├── entry.go
│   │   │       ├── exported.go
│   │   │       ├── formatter.go
│   │   │       ├── hooks.go
│   │   │       ├── json_formatter.go
│   │   │       ├── LICENSE
│   │   │       ├── logger.go
│   │   │       ├── logrus.go
│   │   │       ├── README.md
│   │   │       ├── terminal_appengine.go
│   │   │       ├── terminal_bsd.go
│   │   │       ├── terminal_linux.go
│   │   │       ├── terminal_notwindows.go
│   │   │       ├── terminal_solaris.go
│   │   │       ├── terminal_windows.go
│   │   │       ├── text_formatter.go
│   │   │       └── writer.go
│   │   ├── syndtr
│   │   │   └── gocapability
│   │   │       ├── capability
│   │   │       │   ├── capability.go
│   │   │       │   ├── capability_linux.go
│   │   │       │   ├── capability_noop.go
│   │   │       │   ├── enum_gen.go
│   │   │       │   ├── enum.go
│   │   │       │   └── syscall_linux.go
│   │   │       └── LICENSE
│   │   ├── urfave
│   │   │   └── cli
│   │   │       ├── app.go
│   │   │       ├── category.go
│   │   │       ├── cli.go
│   │   │       ├── command.go
│   │   │       ├── context.go
│   │   │       ├── errors.go
│   │   │       ├── flag_generated.go
│   │   │       ├── flag.go
│   │   │       ├── funcs.go
│   │   │       ├── help.go
│   │   │       ├── LICENSE
│   │   │       └── README.md
│   │   └── vishvananda
│   │       └── netlink
│   │           ├── addr.go
│   │           ├── addr_linux.go
│   │           ├── filter.go
│   │           ├── filter_linux.go
│   │           ├── LICENSE
│   │           ├── link.go
│   │           ├── link_linux.go
│   │           ├── neigh.go
│   │           ├── neigh_linux.go
│   │           ├── netlink.go
│   │           ├── netlink_unspecified.go
│   │           ├── nl
│   │           │   ├── addr_linux.go
│   │           │   ├── link_linux.go
│   │           │   ├── nl_linux.go
│   │           │   ├── route_linux.go
│   │           │   ├── tc_linux.go
│   │           │   ├── xfrm_linux.go
│   │           │   ├── xfrm_policy_linux.go
│   │           │   └── xfrm_state_linux.go
│   │           ├── protinfo.go
│   │           ├── protinfo_linux.go
│   │           ├── qdisc.go
│   │           ├── qdisc_linux.go
│   │           ├── README.md
│   │           ├── route.go
│   │           ├── route_linux.go
│   │           ├── xfrm.go
│   │           ├── xfrm_policy.go
│   │           ├── xfrm_policy_linux.go
│   │           ├── xfrm_state.go
│   │           └── xfrm_state_linux.go
│   └── golang.org
│       └── x
│           └── sys
│               ├── LICENSE
│               ├── PATENTS
│               ├── README
│               └── unix
│                   ├── asm_darwin_386.s
│                   ├── asm_darwin_amd64.s
│                   ├── asm_darwin_arm64.s
│                   ├── asm_darwin_arm.s
│                   ├── asm_dragonfly_amd64.s
│                   ├── asm_freebsd_386.s
│                   ├── asm_freebsd_amd64.s
│                   ├── asm_freebsd_arm.s
│                   ├── asm_linux_386.s
│                   ├── asm_linux_amd64.s
│                   ├── asm_linux_arm64.s
│                   ├── asm_linux_arm.s
│                   ├── asm_linux_mips64x.s
│                   ├── asm_linux_mipsx.s
│                   ├── asm_linux_ppc64x.s
│                   ├── asm_linux_s390x.s
│                   ├── asm_netbsd_386.s
│                   ├── asm_netbsd_amd64.s
│                   ├── asm_netbsd_arm.s
│                   ├── asm_openbsd_386.s
│                   ├── asm_openbsd_amd64.s
│                   ├── asm_solaris_amd64.s
│                   ├── bluetooth_linux.go
│                   ├── constants.go
│                   ├── dirent.go
│                   ├── endian_big.go
│                   ├── endian_little.go
│                   ├── env_unix.go
│                   ├── env_unset.go
│                   ├── flock.go
│                   ├── flock_linux_32bit.go
│                   ├── gccgo_c.c
│                   ├── gccgo.go
│                   ├── gccgo_linux_amd64.go
│                   ├── gccgo_linux_sparc64.go
│                   ├── openbsd_pledge.go
│                   ├── race0.go
│                   ├── race.go
│                   ├── README.md
│                   ├── sockcmsg_linux.go
│                   ├── sockcmsg_unix.go
│                   ├── str.go
│                   ├── syscall_bsd.go
│                   ├── syscall_darwin_386.go
│                   ├── syscall_darwin_amd64.go
│                   ├── syscall_darwin_arm64.go
│                   ├── syscall_darwin_arm.go
│                   ├── syscall_darwin.go
│                   ├── syscall_dragonfly_amd64.go
│                   ├── syscall_dragonfly.go
│                   ├── syscall_freebsd_386.go
│                   ├── syscall_freebsd_amd64.go
│                   ├── syscall_freebsd_arm.go
│                   ├── syscall_freebsd.go
│                   ├── syscall.go
│                   ├── syscall_linux_386.go
│                   ├── syscall_linux_amd64_gc.go
│                   ├── syscall_linux_amd64.go
│                   ├── syscall_linux_arm64.go
│                   ├── syscall_linux_arm.go
│                   ├── syscall_linux.go
│                   ├── syscall_linux_mips64x.go
│                   ├── syscall_linux_mipsx.go
│                   ├── syscall_linux_ppc64x.go
│                   ├── syscall_linux_s390x.go
│                   ├── syscall_linux_sparc64.go
│                   ├── syscall_netbsd_386.go
│                   ├── syscall_netbsd_amd64.go
│                   ├── syscall_netbsd_arm.go
│                   ├── syscall_netbsd.go
│                   ├── syscall_no_getwd.go
│                   ├── syscall_openbsd_386.go
│                   ├── syscall_openbsd_amd64.go
│                   ├── syscall_openbsd.go
│                   ├── syscall_solaris_amd64.go
│                   ├── syscall_solaris.go
│                   ├── syscall_unix_gc.go
│                   ├── syscall_unix.go
│                   ├── zerrors_darwin_386.go
│                   ├── zerrors_darwin_amd64.go
│                   ├── zerrors_darwin_arm64.go
│                   ├── zerrors_darwin_arm.go
│                   ├── zerrors_dragonfly_amd64.go
│                   ├── zerrors_freebsd_386.go
│                   ├── zerrors_freebsd_amd64.go
│                   ├── zerrors_freebsd_arm.go
│                   ├── zerrors_linux_386.go
│                   ├── zerrors_linux_amd64.go
│                   ├── zerrors_linux_arm64.go
│                   ├── zerrors_linux_arm.go
│                   ├── zerrors_linux_mips64.go
│                   ├── zerrors_linux_mips64le.go
│                   ├── zerrors_linux_mips.go
│                   ├── zerrors_linux_mipsle.go
│                   ├── zerrors_linux_ppc64.go
│                   ├── zerrors_linux_ppc64le.go
│                   ├── zerrors_linux_s390x.go
│                   ├── zerrors_linux_sparc64.go
│                   ├── zerrors_netbsd_386.go
│                   ├── zerrors_netbsd_amd64.go
│                   ├── zerrors_netbsd_arm.go
│                   ├── zerrors_openbsd_386.go
│                   ├── zerrors_openbsd_amd64.go
│                   ├── zerrors_solaris_amd64.go
│                   ├── zsyscall_darwin_386.go
│                   ├── zsyscall_darwin_amd64.go
│                   ├── zsyscall_darwin_arm64.go
│                   ├── zsyscall_darwin_arm.go
│                   ├── zsyscall_dragonfly_amd64.go
│                   ├── zsyscall_freebsd_386.go
│                   ├── zsyscall_freebsd_amd64.go
│                   ├── zsyscall_freebsd_arm.go
│                   ├── zsyscall_linux_386.go
│                   ├── zsyscall_linux_amd64.go
│                   ├── zsyscall_linux_arm64.go
│                   ├── zsyscall_linux_arm.go
│                   ├── zsyscall_linux_mips64.go
│                   ├── zsyscall_linux_mips64le.go
│                   ├── zsyscall_linux_mips.go
│                   ├── zsyscall_linux_mipsle.go
│                   ├── zsyscall_linux_ppc64.go
│                   ├── zsyscall_linux_ppc64le.go
│                   ├── zsyscall_linux_s390x.go
│                   ├── zsyscall_linux_sparc64.go
│                   ├── zsyscall_netbsd_386.go
│                   ├── zsyscall_netbsd_amd64.go
│                   ├── zsyscall_netbsd_arm.go
│                   ├── zsyscall_openbsd_386.go
│                   ├── zsyscall_openbsd_amd64.go
│                   ├── zsyscall_solaris_amd64.go
│                   ├── zsysctl_openbsd.go
│                   ├── zsysnum_darwin_386.go
│                   ├── zsysnum_darwin_amd64.go
│                   ├── zsysnum_darwin_arm64.go
│                   ├── zsysnum_darwin_arm.go
│                   ├── zsysnum_dragonfly_amd64.go
│                   ├── zsysnum_freebsd_386.go
│                   ├── zsysnum_freebsd_amd64.go
│                   ├── zsysnum_freebsd_arm.go
│                   ├── zsysnum_linux_386.go
│                   ├── zsysnum_linux_amd64.go
│                   ├── zsysnum_linux_arm64.go
│                   ├── zsysnum_linux_arm.go
│                   ├── zsysnum_linux_mips64.go
│                   ├── zsysnum_linux_mips64le.go
│                   ├── zsysnum_linux_mips.go
│                   ├── zsysnum_linux_mipsle.go
│                   ├── zsysnum_linux_ppc64.go
│                   ├── zsysnum_linux_ppc64le.go
│                   ├── zsysnum_linux_s390x.go
│                   ├── zsysnum_linux_sparc64.go
│                   ├── zsysnum_netbsd_386.go
│                   ├── zsysnum_netbsd_amd64.go
│                   ├── zsysnum_netbsd_arm.go
│                   ├── zsysnum_openbsd_386.go
│                   ├── zsysnum_openbsd_amd64.go
│                   ├── zsysnum_solaris_amd64.go
│                   ├── ztypes_darwin_386.go
│                   ├── ztypes_darwin_amd64.go
│                   ├── ztypes_darwin_arm64.go
│                   ├── ztypes_darwin_arm.go
│                   ├── ztypes_dragonfly_amd64.go
│                   ├── ztypes_freebsd_386.go
│                   ├── ztypes_freebsd_amd64.go
│                   ├── ztypes_freebsd_arm.go
│                   ├── ztypes_linux_386.go
│                   ├── ztypes_linux_amd64.go
│                   ├── ztypes_linux_arm64.go
│                   ├── ztypes_linux_arm.go
│                   ├── ztypes_linux_mips64.go
│                   ├── ztypes_linux_mips64le.go
│                   ├── ztypes_linux_mips.go
│                   ├── ztypes_linux_mipsle.go
│                   ├── ztypes_linux_ppc64.go
│                   ├── ztypes_linux_ppc64le.go
│                   ├── ztypes_linux_s390x.go
│                   ├── ztypes_linux_sparc64.go
│                   ├── ztypes_netbsd_386.go
│                   ├── ztypes_netbsd_amd64.go
│                   ├── ztypes_netbsd_arm.go
│                   ├── ztypes_openbsd_386.go
│                   ├── ztypes_openbsd_amd64.go
│                   └── ztypes_solaris_amd64.go
├── vendor.conf
└── VERSION
```

## [返回目录](https://github.com/MulticsYin/MulticsDevOps#dockerkubernetes)
