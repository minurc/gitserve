你好！
很冒昧用这样的方式来和你沟通，如有打扰请忽略我的提交哈。我是光年实验室（gnlab.com）的HR，在招Golang开发工程师，我们是一个技术型团队，技术氛围非常好。全职和兼职都可以，不过最好是全职，工作地点杭州。
我们公司是做流量增长的，Golang负责开发SAAS平台的应用，我们做的很多应用是全新的，工作非常有挑战也很有意思，是国内很多大厂的顾问。
如果有兴趣的话加我微信：13515810775  ，也可以访问 https://gnlab.com/，联系客服转发给HR。
gitserve
=========

A restricted SSH server and library for supporting controlled Git repository
access and code submission.

This library comes with two tools:
 * `git-submitd`: A service that supports one-way pushes of full repos, along
      with submission hooks for inspecting and accepting those repos.
 * `git-hostd`: A service that hosts a git repo or a folder of git repos to
      users that (optionally) have ssh keys in a specific whitelist.

### git-hostd sample interaction

Make a repo and start the server:
```shell
~$ go get github.com/jtolds/gitserve/cmd/git-hostd
~$ mkdir -p myrepo && cd myrepo
~/myrepo$ git init
Initialized empty Git repository in /home/jt/myrepo/.git/
~/myrepo$ touch newfile1
~/myrepo$ git add .
~/myrepo$ git commit -m 'first commit!'
[master (root-commit) 2266e76] first commit!
 0 files changed
 create mode 100644 newfile1
~/myrepo$ git hostd
2014/08/16 02:11:07 NOTE - listening on [::]:7022
```

Clone your repo from somewhere else, make a change, and push:
```shell
~$ git clone ssh://localhost:7022/ myrepo2
Cloning into 'myrepo2'...
Welcome to the gitserve git-hostd code hosting tool!
Please see https://github.com/jtolds/gitserve for more info.

remote: Counting objects: 3, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (3/3), done.
~$ cd myrepo2
~/myrepo2$ touch newfile2
~/myrepo2$ git add newfile2
~/myrepo2$ git commit -m 'second commit!'
[master 043fcab] second commit!
 0 files changed
 create mode 100644 newfile2
~/myrepo2$ git push origin HEAD:refs/heads/mybranch
Welcome to the gitserve git-hostd code hosting tool!
Please see https://github.com/jtolds/gitserve for more info.

Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 230 bytes, done.
Total 2 (delta 0), reused 0 (delta 0)
To ssh://localhost:7022/
 * [new branch]      HEAD -> mybranch
~/myrepo2$
```

### git-submitd sample interaction

Start the server:
```shell
~$ go get github.com/jtolds/gitserve/cmd/git-submitd
~$ ssh-keygen -N '' -qf git-submitd-key
~$ git-submitd --addr :7022 --private_key git-submitd-key \
       --inspect $GOPATH/src/github.com/jtolds/gitserve/cmd/git-submitd/submission-trigger.py
2014/08/16 02:11:07 NOTE - listening on [::]:7022
```

Push a git repo:
```shell
~$ mkdir myrepo && cd myrepo
~/myrepo$ git init
Initialized empty Git repository in /home/jt/myrepo/.git/
~/myrepo$ git remote add git-submitd ssh://localhost:7022/myrepo
~/myrepo$ touch newfile{1,2}
~/myrepo$ git add .
~/myrepo$ git commit -m 'first commit!'
[master (root-commit) 2266e76] first commit!
 0 files changed
 create mode 100644 newfile1
 create mode 100644 newfile2
~/myrepo$ git push git-submitd master
Welcome to the gitserve git-submitd code repo submission tool!
Please see https://github.com/jtolds/gitserve for more info.

Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 218 bytes, done.
Total 3 (delta 0), reused 0 (delta 0)

Thanks for pushing some code!
===============================================================
You are user: jt
You pushed repo: /tmp/submission-907291030
You came from: [::1]:39059
The repo name is: /myrepo
Your public key is: ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDB...

You pushed:
/tmp/tmpRM4PbC
/tmp/tmpRM4PbC/newfile1
/tmp/tmpRM4PbC/newfile2

To ssh://localhost:7022/myrepo
 * [new branch]      master -> master
~/myrepo$
```

Make sure to check out `submission-trigger.py` to see how to customize
git-submitd for your own ends!


#### License

```plain
The MIT License (MIT)

Copyright (c) 2014 JT Olds

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
