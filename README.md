# 1panel-app-store

## 计划任务

```shell
#!/bin/bash

# 定义变量
REPO_URL="https://github.com/Inlve/1panel-app-store.git"
BRANCH="main"
LOCAL_DIR="app-store"
DEST_DIR="/opt/1panel/resource/apps/local"
PREFIX="1panel_"

# 创建临时目录用于克隆
mkdir -p "$LOCAL_DIR"

# 克隆仓库指定分支到本地目录
if git clone --depth 1 --branch "$BRANCH" "$REPO_URL" "$LOCAL_DIR"; then
    echo "克隆仓库成功。"
else
    echo "克隆仓库失败。"
    exit 1
fi

# 清理目标目录中已有的 app_ 前缀的文件夹
if [ -d "$DEST_DIR" ]; then
    echo "清理已有的 $PREFIX 文件夹..."
    find "$DEST_DIR" -type d -name "$PREFIX*" -exec rm -rf {} +
fi

# 确保目标目录存在
mkdir -p "$DEST_DIR"

# 移动并重命名子目录
if [ -d "$LOCAL_DIR/apps" ]; then
    for subdir in "$LOCAL_DIR/apps"/*; do
        if [ -d "$subdir" ]; then
            subdir_name=$(basename "$subdir")
            mv "$subdir" "$DEST_DIR/$PREFIX$subdir_name"
        fi
    done
    echo "子目录已成功移动并重命名。"
else
    echo "$LOCAL_DIR/apps 目录不存在。"
    exit 1
fi

# 删除本地目录
rm -rf "$LOCAL_DIR"

echo "本地目录 $LOCAL_DIR 已删除。"

# 提示下次更新的说明
echo "脚本完成。下次更新时将自动清理并重新克隆和移动。"
```