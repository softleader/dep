# s2i (Source-to-Image)

The [slctl](https://github.com/softleader/slctl) plugin to build source to image to SoftLeader docker swarm ecosystem

> s2i (Source-to-Image) 是一個非常針對性, 只設計給符合松凌科技開發環境 docker swarm 使用的 command, 請注意: 將來可能會因為全面轉 k8s 而廢棄使用

## Install

```sh
$ slctl plugin install github.com/softleader/s2i
```

## Usage

### prerelease, pre

![](./docs/command-prerelease.svg)

`slctl s2i prerelease` 或 `slctl s2i pre` 的目的是快速的將當前修改的異動版更到開發環境 docker swarm 中, 並且自動的在 github 上將當前的 branch 保留一個版本 (pre-release),
由於這段是在 local 進行, 因此使用前請務必確保當前專案 *local 已經跟 remote 同步過程式了*

請執行 `slctl s2i pre -h` 取得更多說明

> 此 command 僅適用於已經依照[此篇](https://github.com/softleader/softleader-microservice-wiki/wiki/Using-JIB-to-build-image)步驟調整成 jib 包版的專案

### release

![](./docs/command-release.svg)

`slctl s2i release` 的目的是快速化標準的定版程序, 如將手動去 github 下 tag 等多的步驟結合為單一 command

請執行 `slctl s2i release -h` 取得更多說明

> 2-3. 的 update service 需要專案的 Jenkinsfile 配合做些調整, 請參考 [Jenkins Hook to Update Service on Deployer](https://github.com/softleader/softleader-microservice-wiki/wiki/Jenkins-Hook-to-Update-Service-on-Deployer)

### tag

`slctl s2i tag` 的目的是快速的管理某個 repo 下得 tags 及其 releases, 可控制的 sub command 有:

#### tag delete 

`tag delete <TAG..>`, `tag del <TAG..>` 或 `tag rm <TAG..>` 可以協助你刪除不必要的 tag 以及 release, 支援傳一個或多個 `TAG`, 也可以跟其他 command 輕鬆整合, 如: 

```sh
slctl s2i tag delete $(git tag -l)
```

另外也支援了透過 regex 條件來刪除刪除符合的 TAG, 使用上需注意: 使用 regex 是針對 GitHub remote sacn 所有 tag 再過濾, 因此執行上需要較長的時間 (視 remote tag 多寡決定)

請執行 `slctl s2i tag delete -h` 取得更多說明

#### tag list 

`tag list <TAG..>` 可列出 tag 名稱, 發佈時間及發佈人員, 一樣也支援了 regex 的過濾

請執行 `slctl s2i tag list -h` 取得更多說明

## Example

Tag 跟 serviceID 都希望自動找到: 

```sh
slctl s2i pre/release -i
```

已有 serviceID `xxxxx`, 但 tag 希望自動找到:

```sh
slctl s2i pre/release --service-id xxxxx -i
```

已有 tag `v1.2.3` , 但 serviceID 希望協助找到:

```sh
slctl s2i pre/release v1.2.3 -i
```
