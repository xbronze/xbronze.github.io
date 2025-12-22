---
title: >-
  WARNING: The requested image's platform (linux/amd64) does not match the
  detected host platform (linux/arm64/v8) and no specific platform was requested
toc: true
date: 2024-12-28 16:44:57
tags:
 - raspberrypi
 - docker
categories:
 - 折腾
---


当前我就遇到了这样的问题。我在Raspberry Pi 4B 开发板上安装了Docker，使用命令`sudo docker run --platform linux/amd64 -p 12000:80     -e PAPERMERGE__SECURITY__SECRET_KEY=abc     -e PAPERMERGE__AUTH__PASSWORD=admin     papermerge/papermerge:3.0.3`安装papermerge的应用，但是镜像在下载完成后，启动容器失败。

<!-- more -->

```
pi@raspberrypi:~ $ sudo docker run -p 12000:80 \
    -e PAPERMERGE__SECURITY__SECRET_KEY=abc \
    -e PAPERMERGE__AUTH__PASSWORD=admin \
    papermerge/papermerge:3.0.3
Unable to find image 'papermerge/papermerge:3.0.3' locally
3.0.3: Pulling from papermerge/papermerge
1b13d4e1a46e: Pull complete
1c74526957fc: Pull complete
30d855997954: Pull complete
ad5739181616: Pull complete
75e2b45cbee5: Pull complete
37a8a17eedd2: Pull complete
2697e3bb938f: Pull complete
1db75c2e70ec: Pull complete
58505172c2e6: Pull complete
0a775f2002be: Pull complete
37fb7305149c: Pull complete
02037e26ac8b: Pull complete
4f4fb700ef54: Pull complete
9653d7ced0f6: Pull complete
bd12b98793c3: Pull complete
47f6b2bc5bac: Pull complete
9f141d238019: Pull complete
602ce056b6d8: Pull complete
5e02c83b56e1: Pull complete
4fe8b167f5a3: Pull complete
ea4243bd0db9: Pull complete
389b5952531d: Pull complete
56f42d775b8f: Pull complete
2dc53ec46535: Pull complete
4a4c9786a7f0: Pull complete
5125d1a6bc0c: Pull complete
332a8c2009c8: Pull complete
21bb079de5bc: Pull complete
Digest: sha256:8849807ab74ee0dba54ea7db3dfa843da7d8fdaa7f6f5924a15f393149457925
Status: Downloaded newer image for papermerge/papermerge:3.0.3
WARNING: The requested image's platform (linux/amd64) does not match the detected host platform (linux/arm64/v8) and no specific platform was requested
exec /run.bash: exec format error
pi@raspberrypi:~ $
```


一直认为，只要保证 Docker 安装好，从Docker仓库上Pull下来的镜像，在任意机器上运行都能达到一样的效果，但是这个的前提是Docker镜像的架构和当前服务器的架构一致，以前都是 x84_64架构自然可以，但现在也有别的架构，因此 一个镜像如果只有 x86_64 架构的版本，那么是无法在 Arm 架构的服务器上运行的。