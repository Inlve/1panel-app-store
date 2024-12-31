# 使用基础的Debian镜像
FROM debian:stable-slim

# 维护者信息
LABEL maintainer="inlve_jlins@outlook.com"

# 更新包并安装Postfix
RUN apt-get update && apt-get install -y \
    postfix \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# 暴露SMTP端口
EXPOSE 25

# 启动Postfix服务
CMD ["postfix", "start-fg"]
