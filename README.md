# develop-manual
## 分支管理
### 主要分支
* master 生产环境分支，通常情况下该分支为默认保护分支，开发者无法向 master 分支直接推送代码
* release 预发布分支，用于上线前的预览、调试

### 协助分支 

* feature /<功能需求> - 功能模块分支，用于常规功能开发，例 feature/userAuthentication 代表用户验证模块开发分支
* hotfix /<bug描述> - 热修复分支，用于修复生成环境中的bug（拉取自 master），例 hotfix/saveRecordFailed 代表该分支用于 修复保存记录失败

### [commit的使用规范](https://github.com/ppn029012/fgg-dev-manual/blob/master/Git%E4%BD%BF%E7%94%A8%E8%A7%84%E8%8C%83/%E5%A6%82%E4%BD%95%E5%86%99%20commit%20%E5%86%85%E5%AE%B9%20.md)

#### <p align="center">gitflow 流程图（由于削减了develop分支所以将develop分支与release合并即可）</p>
![image](https://github.com/HuasionWu/develop-manual/raw/master/images/clipboard.jpg)

## 流程介绍
* 不同feature在不同feature分支上单独开发(或测试)。
* 确定版本号和此版本将要发布的功能后，将相应featustre分支统一向release分支合并，进行预发布。
* release分支作为持续集成关注的分支，修复bug。
* 待release分支测试验收通过后，统一向master分支合并，并在master分支打tag。
* 若线上发现严重bug，需走hotfix流程。
* 基于master分支拉出hotfix分支修复线上问题。
* bug修复完成统一向master和release分支合并。
* master分支打上新的tag，发布新版本。

### [git flow工作流指令学习指南](https://juejin.im/post/6844903997589946382)

## Merge加上参数（合并分支）
merge的四种状态 --ff（默认）即 **fast-ward** 方式、**--squash**、**--no-ff**、**--rebase**。

### fast-forward
Git 合并两个分支时，如果顺着一个分支走下去可以到达另一个分支的话，那么 Git 在合并两者时，只会简单地把指针右移，叫做“快进”（fast-forward）不过这种情况如果删除分支，则会丢失merge分支信息。
### –squash
把一些不必要commit进行压缩，比如说，你的feature在开发的时候写的commit很乱，那么我们合并的时候不希望把这些历史commit带过来，于是使用–squash进行合并，此时文件已经同合并后一样了，但不移动HEAD，不提交。需要进行一次额外的commit来“总结”一下，然后完成最终的合并。
### –no-ff
关闭fast-forward模式，在提交的时候，会创建一个merge的commit信息，然后合并的和master分支merge的不同行为，向后看，其实最终都会将代码合并到master分支，而区别仅仅只是分支上的简洁清晰的问题；然后向前看，也就是我们使用reset的时候，就会发现，不同的行为就带来了不同的影响。

通常我们把 master 作为主分支，上面存放的都是比较稳定的代码，提交频率也很低，而 release 是用来开发特性的，上面会存在许多零碎的提交，快进式合并会把 release 的提交历史混入到 master 中，搅乱 master 的提交历史。
