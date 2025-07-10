# Sử dụng phiên bản Node.js 18
FROM node:18-slim

# Tạo thư mục làm việc trong container
WORKDIR /usr/src/app

# Sao chép tệp package.json để cài đặt thư viện trước
COPY package*.json ./

# Chạy lệnh cài đặt
RUN npm install

# Sao chép toàn bộ mã nguồn vào
COPY . .

# Mở cổng 8080
EXPOSE 8080

# Lệnh để khởi động ứng dụng
CMD [ "node", "index.js" ]
