# RunC 源码解读

解读RunC源码，RunC GitHub地址：https://github.com/opencontainers/runc  

## 参考文章  
* [Docker背后的标准化容器执行引擎——runC](http://www.infoq.com/cn/articles/docker-standard-container-execution-engine-runc)
* [Docker背后的容器管理——Libcontainer深度解析](http://www.infoq.com/cn/articles/docker-container-management-libcontainer-depth-analysis?utm_source=infoq&utm_campaign=user_page&utm_medium=link)
* [Docker背后的内核知识——cgroups资源限制](http://www.infoq.com/cn/articles/docker-kernel-knowledge-cgroups-resource-isolation?utm_source=infoq&utm_campaign=user_page&utm_medium=link)
* [Docker背后的内核知识——Namespace资源隔离](http://www.infoq.com/cn/articles/docker-kernel-knowledge-namespace-resource-isolation?utm_source=infoq&utm_campaign=user_page&utm_medium=link)
* [DOCKER基础技术：LINUX NAMESPACE（上）](https://coolshell.cn/articles/17010.html)
* [DOCKER基础技术：LINUX NAMESPACE（下）](https://coolshell.cn/articles/17029.html)
* [DOCKER基础技术：LINUX CGROUP](https://coolshell.cn/articles/17049.html)
* [DOCKER基础技术：AUFS](https://coolshell.cn/articles/17061.html)
* [DOCKER基础技术：DEVICEMAPPER](https://coolshell.cn/articles/17200.html)
* [runC总体调用逻辑](http://blog.csdn.net/waltonwang/article/details/53892790)  
* [zhonglinzhang博客](http://blog.csdn.net/zhonglinzhang/article/category/3271199)

## [返回目录](https://github.com/MulticsYin/MulticsDevOps#dockerkubernetes)  

目录树如下：  
工程有点庞大，每天下班后花2~4小时分析，欢迎参与。  

```
.:
checkpoint.go    create.go   events.go  kill.go       list.go      MAINTAINERS_GUIDE.md  NOTICE            PRINCIPLES.md  restore.go       script      start.go  tty.go     utils_linux.go  VERSION
contrib          delete.go   exec.go    libcontainer  main.go      Makefile              notify_socket.go  ps.go          rlimit_linux.go  signals.go  state.go  update.go  vendor
CONTRIBUTING.md  Dockerfile  init.go    LICENSE       MAINTAINERS  man                   pause.go          README.md      run.go           spec.go     tests     utils.go   vendor.conf

./contrib:
cmd  completions

./contrib/cmd:
recvtty

./contrib/cmd/recvtty:
recvtty.go

./contrib/completions:
bash

./contrib/completions/bash:
runc

./libcontainer:
apparmor               console_linux.go         container_windows.go  factory.go             keys                  process_linux.go      setns_init_linux.go     stats_freebsd.go  tags
capabilities_linux.go  console_solaris.go       criu_opts_linux.go    factory_linux.go       message_linux.go      README.md             specconv                stats.go          user
cgroups                console_windows.go       criu_opts_windows.go  factory_linux_test.go  network_linux.go      restored_process.go   SPEC.md                 stats_linux.go    utils
compat_1.5_linux.go    container.go             criurpc               generic_error.go       notify_linux.go       rootfs_linux.go       stacktrace              stats_solaris.go  xattr
configs                container_linux.go       devices               generic_error_test.go  notify_linux_test.go  rootfs_linux_test.go  standard_init_linux.go  stats_windows.go
console_freebsd.go     container_linux_test.go  error.go              init_linux.go          nsenter               seccomp               state_linux.go          sync.go
console.go             container_solaris.go     error_test.go         integration            process.go            setgroups_linux.go    state_linux_test.go     system

./libcontainer/apparmor:
apparmor_disabled.go  apparmor.go

./libcontainer/cgroups:
cgroups.go  cgroups_test.go  cgroups_unsupported.go  fs  rootless  stats.go  systemd  utils.go  utils_test.go

./libcontainer/cgroups/fs:
apply_raw.go       blkio_test.go  cpuset.go       devices.go       freezer_test.go    hugetlb_test.go  name.go          net_prio.go       pids.go             utils.go
apply_raw_test.go  cpuacct.go     cpuset_test.go  devices_test.go  fs_unsupported.go  memory.go        net_cls.go       net_prio_test.go  pids_test.go        utils_test.go
blkio.go           cpu.go         cpu_test.go     freezer.go       hugetlb.go         memory_test.go   net_cls_test.go  perf_event.go     stats_util_test.go  util_test.go

./libcontainer/cgroups/rootless:
rootless.go

./libcontainer/cgroups/systemd:
apply_nosystemd.go  apply_systemd.go

./libcontainer/configs:
blkio_device.go        cgroup_windows.go  config_linux_test.go    device_defaults.go  interface_priority_map.go  namespaces_linux.go                namespaces_unsupported.go
cgroup_linux.go        config.go          config_test.go          device.go           mount.go                   namespaces_syscall.go              network.go
cgroup_unsupported.go  config_linux.go    config_windows_test.go  hugepage_limit.go   namespaces.go              namespaces_syscall_unsupported.go  validate

./libcontainer/configs/validate:
rootless.go  rootless_test.go  validator.go  validator_test.go

./libcontainer/criurpc:
criurpc.pb.go  criurpc.proto  Makefile

./libcontainer/devices:
devices_linux.go  devices_test.go  devices_unsupported.go  number.go

./libcontainer/integration:
checkpoint_test.go  doc.go  execin_test.go  exec_test.go  init_test.go  seccomp_test.go  template_test.go  utils_test.go

./libcontainer/keys:
keyctl.go

./libcontainer/nsenter:
namespace.h  nsenter_gccgo.go  nsenter.go  nsenter_test.go  nsenter_unsupported.go  nsexec.c  README.md

./libcontainer/seccomp:
config.go  fixtures  seccomp_linux.go  seccomp_linux_test.go  seccomp_unsupported.go

./libcontainer/seccomp/fixtures:
proc_self_status

./libcontainer/specconv:
example.go  spec_linux.go  spec_linux_test.go

./libcontainer/stacktrace:
capture.go  capture_test.go  frame.go  frame_test.go  stacktrace.go

./libcontainer/system:
linux.go  proc.go  proc_test.go  syscall_linux_386.go  syscall_linux_64.go  syscall_linux_arm.go  sysconfig.go  sysconfig_notcgo.go  unsupported.go  xattrs_linux.go

./libcontainer/user:
lookup.go  lookup_unix.go  lookup_unsupported.go  MAINTAINERS  user.go  user_test.go

./libcontainer/utils:
cmsg.go  utils.go  utils_test.go  utils_unix.go

./libcontainer/xattr:
errors.go  xattr_linux.go  xattr_test.go  xattr_unsupported.go

./man:
md2man-all.sh  runc.8.md             runc-create.8.md  runc-events.8.md  runc-kill.8.md  runc-pause.8.md  runc-restore.8.md  runc-run.8.md   runc-start.8.md  runc-update.8.md
README.md      runc-checkpoint.8.md  runc-delete.8.md  runc-exec.8.md    runc-list.8.md  runc-ps.8.md     runc-resume.8.md   runc-spec.8.md  runc-state.8.md

./script:
check-config.sh  release.sh  tmpmount  validate-gofmt

./tests:
integration

./tests/integration:
cgroups.bats     create.bats  delete.bats  exec.bats  helpers.bash  list.bats  pause.bats  README.md  spec.bats   start_detached.bats  state.bats  tty.bats     version.bats
checkpoint.bats  debug.bats   events.bats  help.bats  kill.bats     mask.bats  ps.bats     root.bats  start.bats  start_hello.bats     testdata    update.bats

./tests/integration/testdata:
hello-world.tar

./vendor:
github.com  golang.org

./vendor/github.com:
coreos  docker  godbus  golang  mrunalp  opencontainers  seccomp  sirupsen  syndtr  urfave  vishvananda

./vendor/github.com/coreos:
go-systemd  pkg

./vendor/github.com/coreos/go-systemd:
activation  dbus  LICENSE  README.md  util

./vendor/github.com/coreos/go-systemd/activation:
files.go  listeners.go  packetconns.go

./vendor/github.com/coreos/go-systemd/dbus:
dbus.go  methods.go  properties.go  set.go  subscription.go  subscription_set.go

./vendor/github.com/coreos/go-systemd/util:
util_cgo.go  util.go  util_stub.go

./vendor/github.com/coreos/pkg:
dlopen  LICENSE  NOTICE  README.md

./vendor/github.com/coreos/pkg/dlopen:
dlopen_example.go  dlopen.go

./vendor/github.com/docker:
docker  go-units

./vendor/github.com/docker/docker:
LICENSE  NOTICE  pkg  README.md

./vendor/github.com/docker/docker/pkg:
mount  README.md  symlink  term

./vendor/github.com/docker/docker/pkg/mount:
flags_freebsd.go  flags_linux.go        mounter_freebsd.go  mounter_unsupported.go  mountinfo_freebsd.go  mountinfo_linux.go        sharedsubtree_linux.go
flags.go          flags_unsupported.go  mounter_linux.go    mount.go                mountinfo.go          mountinfo_unsupported.go

./vendor/github.com/docker/docker/pkg/symlink:
fs.go  LICENSE.APACHE  LICENSE.BSD  README.md

./vendor/github.com/docker/docker/pkg/term:
tc_linux_cgo.go  tc_other.go  term.go  termios_darwin.go  termios_freebsd.go  termios_linux.go  term_windows.go  winconsole

./vendor/github.com/docker/docker/pkg/term/winconsole:
console_windows.go  term_emulator.go

./vendor/github.com/docker/go-units:
duration.go  LICENSE  README.md  size.go

./vendor/github.com/godbus:
dbus

./vendor/github.com/godbus/dbus:
auth_external.go  call.go         conn_other.go  doc.go      homedir_dynamic.go  LICENSE     README.markdown      transport_generic.go             transport_unix.go  variant_parser.go
auth.go           conn_darwin.go  dbus.go        encoder.go  homedir.go          message.go  sig.go               transport_unixcred_dragonfly.go  variant.go
auth_sha1.go      conn.go         decoder.go     export.go   homedir_static.go   object.go   transport_darwin.go  transport_unixcred_linux.go      variant_lexer.go

./vendor/github.com/golang:
protobuf

./vendor/github.com/golang/protobuf:
LICENSE  proto  README.md

./vendor/github.com/golang/protobuf/proto:
clone.go  decode.go  encode.go  equal.go  extensions.go  lib.go  message_set.go  pointer_reflect.go  pointer_unsafe.go  properties.go  text.go  text_parser.go

./vendor/github.com/mrunalp:
fileutils

./vendor/github.com/mrunalp/fileutils:
fileutils.go  idtools.go  LICENSE  README.md

./vendor/github.com/opencontainers:
runtime-spec  selinux

./vendor/github.com/opencontainers/runtime-spec:
LICENSE  README.md  specs-go

./vendor/github.com/opencontainers/runtime-spec/specs-go:
config.go  state.go  version.go

./vendor/github.com/opencontainers/selinux:
go-selinux  LICENSE  README.md

./vendor/github.com/opencontainers/selinux/go-selinux:
label  selinux.go  xattrs.go

./vendor/github.com/opencontainers/selinux/go-selinux/label:
label.go  label_selinux.go

./vendor/github.com/seccomp:
libseccomp-golang

./vendor/github.com/seccomp/libseccomp-golang:
LICENSE  README  seccomp.go  seccomp_internal.go

./vendor/github.com/sirupsen:
logrus

./vendor/github.com/sirupsen/logrus:
alt_exit.go   doc.go    exported.go   hooks.go           LICENSE    logrus.go  terminal_appengine.go  terminal_linux.go       terminal_solaris.go  text_formatter.go
CHANGELOG.md  entry.go  formatter.go  json_formatter.go  logger.go  README.md  terminal_bsd.go        terminal_notwindows.go  terminal_windows.go  writer.go

./vendor/github.com/syndtr:
gocapability

./vendor/github.com/syndtr/gocapability:
capability  LICENSE

./vendor/github.com/syndtr/gocapability/capability:
capability.go  capability_linux.go  capability_noop.go  enum_gen.go  enum.go  syscall_linux.go

./vendor/github.com/urfave:
cli

./vendor/github.com/urfave/cli:
app.go  category.go  cli.go  command.go  context.go  errors.go  flag_generated.go  flag.go  funcs.go  help.go  LICENSE  README.md

./vendor/github.com/vishvananda:
netlink

./vendor/github.com/vishvananda/netlink:
addr.go        filter.go        LICENSE  link_linux.go  neigh_linux.go  netlink_unspecified.go  protinfo.go        qdisc.go        README.md  route_linux.go  xfrm_policy.go        xfrm_state.go
addr_linux.go  filter_linux.go  link.go  neigh.go       netlink.go      nl                      protinfo_linux.go  qdisc_linux.go  route.go   xfrm.go         xfrm_policy_linux.go  xfrm_state_linux.go

./vendor/github.com/vishvananda/netlink/nl:
addr_linux.go  link_linux.go  nl_linux.go  route_linux.go  tc_linux.go  xfrm_linux.go  xfrm_policy_linux.go  xfrm_state_linux.go

./vendor/golang.org:
x

./vendor/golang.org/x:
sys

./vendor/golang.org/x/sys:
LICENSE  PATENTS  README  unix

./vendor/golang.org/x/sys/unix:
asm_darwin_386.s       env_unix.go                 syscall.go                 zerrors_darwin_arm.go       zsyscall_dragonfly_amd64.go  zsysnum_dragonfly_amd64.go  ztypes_freebsd_386.go
asm_darwin_amd64.s     env_unset.go                syscall_linux_386.go       zerrors_dragonfly_amd64.go  zsyscall_freebsd_386.go      zsysnum_freebsd_386.go      ztypes_freebsd_amd64.go
asm_darwin_arm64.s     flock.go                    syscall_linux_amd64_gc.go  zerrors_freebsd_386.go      zsyscall_freebsd_amd64.go    zsysnum_freebsd_amd64.go    ztypes_freebsd_arm.go
asm_darwin_arm.s       flock_linux_32bit.go        syscall_linux_amd64.go     zerrors_freebsd_amd64.go    zsyscall_freebsd_arm.go      zsysnum_freebsd_arm.go      ztypes_linux_386.go
asm_dragonfly_amd64.s  gccgo_c.c                   syscall_linux_arm64.go     zerrors_freebsd_arm.go      zsyscall_linux_386.go        zsysnum_linux_386.go        ztypes_linux_amd64.go
asm_freebsd_386.s      gccgo.go                    syscall_linux_arm.go       zerrors_linux_386.go        zsyscall_linux_amd64.go      zsysnum_linux_amd64.go      ztypes_linux_arm64.go
asm_freebsd_amd64.s    gccgo_linux_amd64.go        syscall_linux.go           zerrors_linux_amd64.go      zsyscall_linux_arm64.go      zsysnum_linux_arm64.go      ztypes_linux_arm.go
asm_freebsd_arm.s      gccgo_linux_sparc64.go      syscall_linux_mips64x.go   zerrors_linux_arm64.go      zsyscall_linux_arm.go        zsysnum_linux_arm.go        ztypes_linux_mips64.go
asm_linux_386.s        openbsd_pledge.go           syscall_linux_mipsx.go     zerrors_linux_arm.go        zsyscall_linux_mips64.go     zsysnum_linux_mips64.go     ztypes_linux_mips64le.go
asm_linux_amd64.s      race0.go                    syscall_linux_ppc64x.go    zerrors_linux_mips64.go     zsyscall_linux_mips64le.go   zsysnum_linux_mips64le.go   ztypes_linux_mips.go
asm_linux_arm64.s      race.go                     syscall_linux_s390x.go     zerrors_linux_mips64le.go   zsyscall_linux_mips.go       zsysnum_linux_mips.go       ztypes_linux_mipsle.go
asm_linux_arm.s        README.md                   syscall_linux_sparc64.go   zerrors_linux_mips.go       zsyscall_linux_mipsle.go     zsysnum_linux_mipsle.go     ztypes_linux_ppc64.go
asm_linux_mips64x.s    sockcmsg_linux.go           syscall_netbsd_386.go      zerrors_linux_mipsle.go     zsyscall_linux_ppc64.go      zsysnum_linux_ppc64.go      ztypes_linux_ppc64le.go
asm_linux_mipsx.s      sockcmsg_unix.go            syscall_netbsd_amd64.go    zerrors_linux_ppc64.go      zsyscall_linux_ppc64le.go    zsysnum_linux_ppc64le.go    ztypes_linux_s390x.go
asm_linux_ppc64x.s     str.go                      syscall_netbsd_arm.go      zerrors_linux_ppc64le.go    zsyscall_linux_s390x.go      zsysnum_linux_s390x.go      ztypes_linux_sparc64.go
asm_linux_s390x.s      syscall_bsd.go              syscall_netbsd.go          zerrors_linux_s390x.go      zsyscall_linux_sparc64.go    zsysnum_linux_sparc64.go    ztypes_netbsd_386.go
asm_netbsd_386.s       syscall_darwin_386.go       syscall_no_getwd.go        zerrors_linux_sparc64.go    zsyscall_netbsd_386.go       zsysnum_netbsd_386.go       ztypes_netbsd_amd64.go
asm_netbsd_amd64.s     syscall_darwin_amd64.go     syscall_openbsd_386.go     zerrors_netbsd_386.go       zsyscall_netbsd_amd64.go     zsysnum_netbsd_amd64.go     ztypes_netbsd_arm.go
asm_netbsd_arm.s       syscall_darwin_arm64.go     syscall_openbsd_amd64.go   zerrors_netbsd_amd64.go     zsyscall_netbsd_arm.go       zsysnum_netbsd_arm.go       ztypes_openbsd_386.go
asm_openbsd_386.s      syscall_darwin_arm.go       syscall_openbsd.go         zerrors_netbsd_arm.go       zsyscall_openbsd_386.go      zsysnum_openbsd_386.go      ztypes_openbsd_amd64.go
asm_openbsd_amd64.s    syscall_darwin.go           syscall_solaris_amd64.go   zerrors_openbsd_386.go      zsyscall_openbsd_amd64.go    zsysnum_openbsd_amd64.go    ztypes_solaris_amd64.go
asm_solaris_amd64.s    syscall_dragonfly_amd64.go  syscall_solaris.go         zerrors_openbsd_amd64.go    zsyscall_solaris_amd64.go    zsysnum_solaris_amd64.go
bluetooth_linux.go     syscall_dragonfly.go        syscall_unix_gc.go         zerrors_solaris_amd64.go    zsysctl_openbsd.go           ztypes_darwin_386.go
constants.go           syscall_freebsd_386.go      syscall_unix.go            zsyscall_darwin_386.go      zsysnum_darwin_386.go        ztypes_darwin_amd64.go
dirent.go              syscall_freebsd_amd64.go    zerrors_darwin_386.go      zsyscall_darwin_amd64.go    zsysnum_darwin_amd64.go      ztypes_darwin_arm64.go
endian_big.go          syscall_freebsd_arm.go      zerrors_darwin_amd64.go    zsyscall_darwin_arm64.go    zsysnum_darwin_arm64.go      ztypes_darwin_arm.go
endian_little.go       syscall_freebsd.go          zerrors_darwin_arm64.go    zsyscall_darwin_arm.go      zsysnum_darwin_arm.go        ztypes_dragonfly_amd64.go

```
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
├── vendor.conf
└── VERSION
```

## [返回目录](https://github.com/MulticsYin/MulticsDevOps#dockerkubernetes)
