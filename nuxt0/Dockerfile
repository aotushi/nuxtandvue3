#Dockerfile
#制定node镜像的版本
FROM node:20-alpine

#移动当前目录下面的文件到app目录下
WORKDIR /app

# 只安装 OpenSSL
RUN apk add --no-cache openssl

# 设置 npm 镜像
RUN npm config set registry https://registry.npmmirror.com

# 安装依赖
COPY package*.json ./
RUN npm install

# 复制所有文件
COPY . .

# 先构建应用
RUN npm run build

EXPOSE 3000

CMD ["npm", "start"]
