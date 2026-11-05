# BE header-only 依赖组 on loongarch64

Doris BE 编译期还需要一批**纯头文件**库，不需要编译，只要放到 `installed/include/` 正确位置。
本仓库集中记录这一组（按 Doris `thirdparty/vars.sh` 的版本）。

## 成员与安装

    # parallel-hashmap 1.3.8
    cp -r parallel-hashmap-1.3.8/parallel_hashmap $P/include/
    # concurrentqueue 1.0.3
    cp concurrentqueue-1.0.3/concurrentqueue.h $P/include/
    cp concurrentqueue-1.0.3/blockingconcurrentqueue.h $P/include/
    # 其余单头文件
    cp pdqsort.h $P/include/
    cp libdivide.h $P/include/

下载统一走 `gh-proxy.com` 代理 github 的 release tarball；大文件用 `curl -C -` 续传循环并校验大小。

## 唯一需要改代码的地方

**libdivide 5.0**：源码里有一处 `other.denom` 应为 `other.div.denom`（新版结构体把字段收进了 `div`），
直接编译会报错，改掉即可。其余全部零改动。

## 验证

这批头在 Util / Common / Vec / Runtime 全量编译中全部被正确找到，`doris_be` 链接成功。
