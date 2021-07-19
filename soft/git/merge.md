# 合并学习

## 说明
git使用中除了提交和拉取，最常用的使用方式应该是合并了

## 合并场景
1. 本地多分支合并  
    这种情况多是发生在多分支合并，如debug和dev合并。
    ```
    git checkout dev
    git merge debug
    ```
2. 远程单分支合并
    这种情况多是发生在多设备操作，一边提交了新的分支或提交，本地要合并此分支再提交的情况
    ```
    git fetch origin dev
    git merge dev
    ```