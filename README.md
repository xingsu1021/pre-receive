# pre-receive
A simple gitlab commit message check hook.

https://mritd.me/2018/05/11/add-commit-message-style-check-to-your-gitlab/

查阅了相关资料得出，在进行 push 时，GitLab 会调用这个钩子文件，这个钩子文件必须放在 /var/opt/gitlab/git-data/repositories/<group>/<project>.git/custom_hooks 目录中，当然具体路径也可能是 /home/git/repositories/<group>/<project>.git/custom_hooks；custom_hooks 目录需要自己创建，具体可以参阅文档的 Setup；

在进行 push 操作时，GitLab 会调用这个钩子文件，并且从 stdin 输入三个参数，分别为 之前的版本 commit ID、push 的版本 commit ID 和 push 的分支；根据 commit ID 我们就可以很轻松的获取到提交信息，从而实现进一步检测动作；根据 GitLab 的文档说明，当这个 hook 执行后以非 0 状态退出则认为执行失败，从而拒绝 push；同时会将 stderr 信息返回给 client 端；说了这么多，下面就可以直接上代码了，为了方便我就直接用 go 造了一个 pre-receive，官方文档说明了不限制语言



把以上代码编译后生成的 pre-receive 文件复制到对应项目的钩子目录即可；要注意的是文件名必须为 pre-receive，同时 custom_hooks 目录需要自建；custom_hooks 目录以及 pre-receive 文件用户组必须为 git:git；在删除分支时 commit ID 为 0000000000000000000000000000000000000000，此时不需要检测提交信息，否则可能导致无法删除分支/tag
