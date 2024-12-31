# 1panel-app-store

## 计划任务

```shell
#!/bin/bash

# 定义变量
REPO_URL="https://github.com/Inlve/1panel-app-store.git"
BRANCH="main"
LOCAL_DIR="app-store"
SOURCE_DIR="$LOCAL_DIR/apps"
TARGET_DIR="/opt/1panel/resource/apps/local"

# 克隆仓库函数
clone_repo() {
    echo "正在克隆仓库..."
    git clone -b "$BRANCH" "$REPO_URL" "$LOCAL_DIR" || {
        echo "克隆失败，请检查网络或仓库地址。"
        exit 1
    }
    echo "克隆完成。"
}

# 对比更新函数
incremental_update() {
    echo "开始增量更新..."

    # 创建目标目录（如果不存在）
    mkdir -p "$TARGET_DIR"

    # 删除在目标目录中存在但不在源目录中的文件夹
    for target_subdir in "$TARGET_DIR"/*; do
        if [ -d "$target_subdir" ]; then
            subdir_name=$(basename "$target_subdir")
            if [ ! -d "$SOURCE_DIR/$subdir_name" ]; then
                echo "删除旧的文件夹：$subdir_name"
                rm -rf "$target_subdir"
            fi
        fi
    done

    # 添加或更新文件夹
    for source_subdir in "$SOURCE_DIR"/*; do
        if [ -d "$source_subdir" ]; then
            subdir_name=$(basename "$source_subdir")
            target_subdir="$TARGET_DIR/$subdir_name"
            if [ ! -d "$target_subdir" ]; then
                echo "添加新文件夹：$subdir_name"
                cp -r "$source_subdir" "$target_subdir"
            else
                echo "更新文件夹：$subdir_name"
                rsync -a --delete "$source_subdir/" "$target_subdir/"
            fi
        fi
    done

    echo "增量更新完成。"
}

# 删除本地目录函数
delete_local_dir() {
    echo "删除本地目录 $LOCAL_DIR..."
    rm -rf "$LOCAL_DIR"
    echo "本地目录已删除。"
}

# 执行流程
echo "开始执行脚本..."

# 克隆仓库
clone_repo

# 进行增量更新
incremental_update

# 删除本地目录
delete_local_dir

echo "脚本执行完成。"
```