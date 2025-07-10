// Import thư viện Express
const express = require('express');
const app = express();
const port = process.env.PORT || 8080; // Cổng mà Cloud Run sẽ sử dụng

// URL video của bạn đã được cập nhật vào đây
const videoUrlCuaBan = 'https://storage.googleapis.com/phim-cua-toi-processed/Phimhay/FB_VID_2156309469405291091.mp4';

// Danh sách phim
const movies = [
  { id: 1, title: 'Phim Hay Của Bạn', videoUrl: videoUrlCuaBan }
  // Bạn có thể thêm các phim khác vào đây
];

// Trang chủ hiển thị danh sách phim
app.get('/', (req, res) => {
  let movieList = '<h1>Trang Web Phim Của Tôi</h1><ul>';
  movies.forEach(movie => {
    // Tạo một liên kết đến trang phát phim
    movieList += `<li><a href="/player/${movie.id}">${movie.title}</a></li>`;
  });
  movieList += '</ul>';
  res.send(movieList);
});

// Trang phát phim
app.get('/player/:id', (req, res) => {
  const movie = movies.find(m => m.id == req.params.id);
  if (movie) {
    // Gửi về mã HTML chứa trình phát video
    res.send(`
      <!DOCTYPE html>
      <html lang="vi">
      <head>
          <meta charset="UTF-8">
          <meta name="viewport" content="width=device-width, initial-scale=1.0">
          <title>Đang phát: ${movie.title}</title>
          <style>
              body { font-family: sans-serif; background-color: #000; color: #fff; margin: 0; display: flex; justify-content: center; align-items: center; height: 100vh; }
              .container { width: 90%; max-width: 1000px; }
              video { width: 100%; height: auto; border-radius: 8px; }
              h1 { text-align: center; }
              a { color: #00aaff; display: block; text-align: center; margin-top: 20px; }
          </style>
      </head>
      <body>
          <div class="container">
              <h1>${movie.title}</h1>
              <video controls autoplay>
                <source src="${movie.videoUrl}" type="video/mp4">
                Trình duyệt của bạn không hỗ trợ thẻ video.
              </video>
              <p><a href="/">Quay lại danh sách phim</a></p>
          </div>
      </body>
      </html>
    `);
  } else {
    res.status(404).send('Không tìm thấy phim');
  }
});

// Khởi động máy chủ
app.listen(port, () => {
  console.log(`Ứng dụng đang lắng nghe trên cổng ${port}`);
});
