

ssh error

ssh 无法连接

查看`auth.log`以及`auth.log.1`，发现蹊跷的地方大概是`pam_unix(gdm-password:auth): authentication failure; logname= uid=0 euid=0 tty=/dev/tty2 ruser= rhost= user=jr`

用来检查sshd的config文件是否有错误`/usr/sbin/sshd -T`

可能跟密码相关
[曾经重置过公私钥](https://blog.csdn.net/qq_32169769/article/details/73849470?utm_medium=distribute.pc_relevant.none-task-blog-baidujs_baidulandingword-1&spm=1001.2101.3001.4242)


在主机端，`/usr/sbin/sshd -dddd -D`

结果得到

```
debug2: load_server_config: filename /etc/ssh/sshd_config
debug2: load_server_config: done config len = 276
debug2: parse_server_config: config /etc/ssh/sshd_config len 276
debug3: /etc/ssh/sshd_config:56 setting PasswordAuthentication yes
debug3: /etc/ssh/sshd_config:63 setting ChallengeResponseAuthentication no
debug3: /etc/ssh/sshd_config:91 setting X11Forwarding yes
debug3: /etc/ssh/sshd_config:95 setting PrintMotd no
debug3: /etc/ssh/sshd_config:114 setting AcceptEnv LANG LC_*
debug3: /etc/ssh/sshd_config:117 setting Subsystem sftp	/usr/lib/openssh/sftp-server
debug1: sshd version OpenSSH_7.6, OpenSSL 1.0.2n  7 Dec 2017
debug1: private host key #0: ssh-rsa SHA256:rzmlFxxr56FVOtovqQSlbGNU/YQrDSZCLw4EPUkiRTs
debug1: private host key #1: ecdsa-sha2-nistp256 SHA256:D7epphcGuNjRbHzHl0L53mWw5yUCQLdO4guA0tRIrXU
debug1: private host key #2: ssh-ed25519 SHA256:yRCqnEmmCSJOzpjpocpGqIs1sk7dfOHaiP9xjLBPJ3U
debug1: rexec_argv[0]='/usr/sbin/sshd'
debug1: rexec_argv[1]='-dddd'
debug1: rexec_argv[2]='-D'
debug3: oom_adjust_setup
debug1: Set /proc/self/oom_score_adj from 0 to -1000
debug2: fd 3 setting O_NONBLOCK
debug1: Bind to port 22 on 0.0.0.0.
Bind to port 22 on 0.0.0.0 failed: Address already in use.
debug2: fd 3 setting O_NONBLOCK
debug3: sock_set_v6only: set socket 3 IPV6_V6ONLY
debug1: Bind to port 22 on ::.
Bind to port 22 on :: failed: Address already in use.
Cannot bind any address.


```

[参考](https://serverfault.com/questions/351327/ssh-error-bind-to-port-22-failed-address-already-in-use)

[](https://www.sysgeek.cn/ubuntu-disable-ipv6/)
[final solution](https://askubuntu.com/questions/1109934/ssh-server-stops-working-after-reboot-caused-by-missing-var-run-sshd)
于是手动创建了/var/run/sshd

然而/var/log/auth.log仍然显示
```
received signal 15; terminating
```
这个后来发现是自己service stop留下来的印记 佛了

主机ip`10.112.91.210`
