# 使用官方Directus镜像
FROM directus/directus:latest

# 设置工作目录
WORKDIR /directus

# 复制模板文件
COPY template ./template

# 创建必要的目录
RUN mkdir -p /directus/uploads /directus/extensions

# 安装PostgreSQL客户端工具
RUN apt-get update && apt-get install -y postgresql-client && rm -rf /var/lib/apt/lists/*

# 设置环境变量
ENV NODE_ENV=production
ENV DB_CLIENT=pg
ENV CACHE_ENABLED=false
ENV WEBSOCKETS_ENABLED=true
ENV EXTENSIONS_PATH=/directus/extensions
ENV EXTENSIONS_AUTO_RELOAD=true

# 创建简单的初始化脚本
RUN echo '#!/bin/bash\n\
# 等待数据库连接\n\
echo "Waiting for database connection..."\n\
while ! pg_isready -h "$DB_HOST" -p "$DB_PORT" -U "$DB_USER" -d "$DB_DATABASE" 2>/dev/null; do\n\
  echo "Database not ready, waiting..."\n\
  sleep 3\n\
done\n\
echo "Database connected successfully!"\n\
\n\
# 启动Directus\n\
echo "Starting Directus..."\n\
exec npx directus start' > /directus/start.sh && chmod +x /directus/start.sh

# 暴露端口
EXPOSE 8055

# 启动命令
CMD ["/directus/start.sh"] 
