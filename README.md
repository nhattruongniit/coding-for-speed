
# Coding for spped: Optimize performance website

[Presentation coding for speed](https://docs.google.com/presentation/d/1oEMGluswl7dYmJuE9YjHA6rDiBjt0C-qsyL3D3YQGpc/edit#slide=id.p)

# Có 3 giới hạn về tốc độ load của 1 website

- 0.1 second: đây là giới hạn cao nhất khi user tương tác và cảm thấy ứng dụng response ngay tức thì. Ví dụ: touch event trên web...

- 1.0 second: đây là giới hạn cao nhất khi user cảm thấy ứng dụng luôn luôn làm việc. Sẽ ko thấy ứng dụng chạy chậm. User sẽ để ý đến việc có khoảng dừng 1s, nhưng sẽ ko làm cho họ mất tập trung.

- 10 second: đây là giới hạn cao nhất khi user tập trung 1 vấn đề nào đó. Nếu cảm thấy 1 method/task sẽ lâu hơn 10 giây, nên có 1 progress indicator là task vẫn đang được thực thi hoặc 1 cách nào đó để User cancel task hiện tại

=> Đây là ngưỡng nhận thức của con người đã được nghiên cứu qua nhiều thập kỷ.

# Người dùng rất thích các website có tốc độ load nhanh.

- 74% người sử dụng mobile sẽ bỏ qua website của bạn khi chờ quá 5s.
- 69% người sử dụng table, desktop mong đợi website được load trong khoảng 2s or có thể ít hơn.

# How slow is the web?

- Trung bình 1 page load trên mobile chỉ tầm 7s.
- Thông thường cac website sẽ load từ 1 -> 7s trên device. Chỉ có khoảng 10% load khoảng or thấp hơn 1s trên device.
- Từ năm 2010, Google bắt đầu sử dụng thời gian load page như là 1 trong cac yếu tố để họ xếp hạng các website.

# Slow equals less revenu

- Amazon reported that every 100ms decrease in page load time increased revenue by 1%.


# The journey of a packet request  


- we have to do a DNS lookup to translate the URL to an IP address.


# Some definitions

- bandwidth: băng thông - lượng dữ liệu được truyền mỗi giây.
- latency:  độ trễ - chậm trễ trong thời gian truyền dữ liệu, được chỉ định là lượng thời gian từ viễc gửi yêu cầu dữ liệu và nhận phản hồi (thời gian chuyến đi khứ hồi)  
- render
- parse

# Rendering steps

- Cả hai DOM and CSSDOM phải được load xong để hiển thị các elements trước khi render tree có thể dc lên cấu trúc.

- Render tree này có thể bị dừng hiển thị bởi DOM, CSSDOM or javascript.

- Điều kiện tiên quyêt để xây dựng render tree: HTML parsed, CSS parsed, DOM complete, CSSOM complete, sync javascript complete.



# Cache HTTP Header  

- When a visitor comes to your site for the first time, everything has to be loaded from the
your server. All images, content, scripts, style sheets, etc. But much of that can be stored
in the browser’s cache, so when the user returns, much of the site can be loaded from the
cache, which is much faster

- The two primary tools to control how your site is cached are the HTTP headers Expires and Cache- Control.

## Last-Modified

- Khi Server gửi nội dung về Browser thì kèm theo thuộc tính Last-Modified để xác định thời điểm sửa file. Ví dụ: Last-modified: Wed, 16 Oct 2013 05:00:25 GMT+7.

- Lúc này, Browser biết được file inet.png được tạo ra ngày 16/10/2013. Lần sau khi cần tải lại file này, nó sẽ gửi yêu cầu đến Server kèm theo header  Last-Modifed. Server chỉ trả về nội dung nếu file đã được cập nhật.

- Phương thức Last-Modieifed có vấn đề là vẫn phải kết nối nhiều lần đến server, tại server phải xử lý thời gian cập nhật file.  Nếu đồng hồ trên server bị sai lệch thì nội dung file vẫn được tải xuống Browser.

## ETag

- Last-Modified còn nhiều vấn đề: Vẫn phải kết nối nhiều lần đến server, tại server phải xử lý thời gian cập nhật file, (2) Nếu đồng hồ trên server bị sai lệch thì nội dung file vẫn được tải xuống Browser.

-  ETag với giá trị là chuỗi số duy nhất cho tất cả các file trên server. Nó có thể là một mã Hash hay Footprint: mỗi file đều có footprint (dấu chân) duy nhất, nếu bạn thay đổi nội dung file (dù chỉ là 1 byte) thì footprint sẽ thay đổi.   

- Thay vì gửi cho Browser thời gian cập nhật lần cuối, Server sẽ gửi mã ETag. Browser sẽ lưu lại file kèm theo mã ETag. Lần sau khi cần tải lại file này, nó sẽ gửi yêu cầu đến Server kèm theo header ETag. 
Server chỉ trả về nội dung nếu file đã được cập nhật.

## Expires

- Cache theo cách #2 thì Browser vẫn phải kết nối nhiều lần đến Server để kiểm tra xem file có thay đổi nội dung. 
- Thay vì để Browser hỏi nhiều lần, khi gửi file, Server sẽ gửi kèm header expires để thông báo rằng nội dung file sẽ không thay đổi cho đến thời điểm expires
- Không có kết nối Browser - Server. Browser tự kiểm tra nếu file còn thời hạn thì sử dụng luôn nội dung đã được cache. Điều này sẽ làm giảm bớt công việc của Server - dành thời gian làm việc khác. 
- Ví dụ: Expires: Wed, 16 Oct 2013 05:00:25 GMT+7.


## Max-Age

- Thông tin expires (ngày hết hạn) rất tuyệt vời, nhưng Browser phải liên tục tính toán thời gian. Giải pháp #4 là sử dụng header max-age - "file inet.png sẽ hết hạn trong 1 tuần kể từ hôm nay", đơn giản hơn so với việc sử dụng chính xác thời điểm hết hạn.

## Cache-Control: public | private | no-cache, max-age = n 

- Giá trị max-age được tính bằng giây,  1 giờ = 3600,  1 ngày = 86400. 
- Public: file có thể được cache bởi proxy hoặc các máy chủ trung gian
- Private: file có giá trị khác nhau cho từng người sử dụng. Browser có thể cache, nhưng các proxy không được cache.
- No-cache: Browser và Proxy không được cache file này.

## MỘT SỐ TIP KHÁC GIÚP TĂNG TỐC ĐỘ TẢI TRANG WEB

1. Bật chế độ GZIP để nén dữ liệu từ trao đổi giữa Server - Browser
2. Nén file css, js, ảnh trước khi đưa lên server
3. Giảm số lượng connections tới server:
    - href="http://unica.vn/css/abc.css"
    - href="http://unica.vn/css/xyz.css"

--> Sửa thành: href="http://unica.vn/css/abc.css, xyz.css"

## Setting Cache-Control header in HTML

- meta http- equiv="Cache- Control" content="public". When both Expires and Cache- Control headers are present, the Cache- Control settings take priority.

# Font rendering

- Web font rất là phổ biến. Chúng ta nên nhúng font bằng Google Font. Ưu điểm của Google Font là rất nhanh và đảm bảo. Là host của Google nên tốc độ rất nhanh, ổn định và giảm băng thông cho host.

- Nếu bạn xài quá nhiều font, thì sẽ ảnh hưởng tới việc load nếu như các font có kích thước nặng.


# Actions for speed: Images

- Images có lẽ là yếu tố quan trọng nhất trong quá trình tải trang của bạn. 

- Vì vậy sẽ có các kỹ thuật trong việc xử lý hình ảnh.

## Image compression

Nén hình ảnh sẽ làm giảm kích thươc của chúng và giúp tăng tốc độ web load.

## File type overview

1/ GIF: 
- Có 256 color. 
- Chủ yếu sử dụng cho các hình như button or web animation, hình động, các icon.

2/ JPG: 
- Độ màu từ 24 tới 16 triệu màu
- Là chuẩn thông dụng nhất áp dụng cho các hình.
- Thích hợp tạo ra các file ảnh với dung lượng nhỏ.
- Thích hợp với những hình ảnh hơn 256  màu
- Nếu hình ảnh bạn có văn bản, text, hình shapes, or các khu vực có màu thì ko nên xài jpg.

3/ PNG:
- Định dạng PNG được tạo ra như một cải tiến , thay thế cho Graphics Interchange Format (GIF), và nó là định dạng ảnh nén không bị suy giảm chất lượng, được sử dụng rất nhiều trên internet.
- Dung lượng file lớn hon JPG

4/ WebP: 
- là hình ảnh được phát triển bởi google. Nó có dung lượng nhẹ hơn png và jpg và còn hỗ trợ tranparent. Đã áp dụng trên google play, youtube.
- Hiện tại chỉ có google chrome support.

5/ SVG:
- dung lượng nhỏ.
- không bị vỡ hình khi phóng to, thu nhỏ.
- thích hợp cho các logo, icon.

## Replace images with CSS/CSS3 gradients.

- Ví dụ: các hình arrow, border radius, box shadow

## CSS Sprites

- Giảm yêu cầu request lên server.
- Là kỹ thuật gom các hình ảnh nhỏ thành 1 tấm hình lớn.
- Giới hạn size < 500kb cho mỗi tấm.
- Client gửi tối đa tầm 5 or 6 request, nên việc load 1 tấm hình sprite sẽ giảm dc yêu cầu request phía client cho server.

## Use preloading appropriately

- Load ngầm các hình ảnh cho các page sau

## Use lazyload images

## Avoid scaling images

- Đừng bao h chỉnh lại size hình so vs ảnh gốc. img height="500px", width="100px"

# Actions for speed: Page Technique

## DOM Architecture

- Tất cả các file như styesheets, script phải được download, parse trước khi browser render ra web page. Nên điều này sẽ làm chậm việc load web, đặc biệt đối với mobile.

- Nếu chúng ta cấu trúc tốt các file, thì sẽ tăng đáng kễ tốc độ load web.


### Sequence matters

- Tải các file javascript sẽ làm cho quá trình xử lý page dừng lại cho đến khi file script dc load xong. Điều này làm chậm việc load page.

- Các file css cho phép được tải đồng thời với nhau. Nếu chúng ta thay đổi các tập tin được tải, sẽ làm giảm đáng kể thời gian tải trang.

- Lưu ý: Hầu hết các trình duyệt hiện đại sẽ tải xuống các tập lệnh bên ngoài song song, nhưng chúng sẽ chỉ phân tích cú pháp và thực thi mã theo trình tự. Điều này là cần thiết để giữ lại bất kỳ phụ thuộc mã nào
giữa các tập lệnh.

### Identify critical CSS

- Nó là thuật ngữ nói về above-the-fold và below-the-fold.
- Above-the-fold là nói về phần nội dung hiển thị nằm ở trên đầu của 1 website.


# Actions for speed: CSS Technique
 
- Luôn luôn đặt file css nằm trong tag head.
- Tránh @import: khi gặp @import, việc load file css sẽ stop lại cho tới khi @import dc load xong. Thay vào đó hãy sử dụng nhiều tag links.
- Luôn luôn sử dụng external CSS và JS. Vì chúng sẽ được cache trong browse.
- Minify file CSS, JS lại. Nếu được minify luon file html.

# Actions for speed: JS Technique

- Như chúng ta đã biết scripts sẽ chặn việc load của CSSOM or DOM. Nên:
- Hãy bỏ qua việc thay đổi các thành phần CSS thông qua script như: elem.style.*
- Bỏ qua các đoạn script như document.write, nó sẽ chặn hiển thị cấu trúc DOM.
- Minimize javascript

### Minimize javascrippt

### Load script asynchronously

- Hãy sử dụng load bất đồng bộ cho script ở bất cứ nơi nào mà nó load.
- Ex: script src="/js/functions.js" async

### Defer script loading

- Có thể sử dụng defer script loading. Nó sẽ chỉ được load khi page đã load xong.
- Load trước event DOMContentLoaded.
- Ex: script src="/js/functions.js" defer

# Actions for speed: DOM Technique

### Avoid Flash

### Page size

- Các file css nên có kích thước ít nhất là 14kb. Đây là số lượng dữ liệu tối đa có thể được chuyển trong một đơn yêu cầu HTTP (giả sử không có chuyển hướng).
- Điều này có thể lớn tới ~ 40 kB nếu sử dụng nén như gzip. 

### User preloading

- Bạn sẽ tải trước các file của các page sau.
- rel="prefetch": Nạp trước Link   
- rel="dns-prefetch": Nạp trước DNS
- rel="prerender"

### Share the heavy load

- Nếu site có nhiều hình ảnh, video. Thì hãy xem xét việc chia ra các hosting # để up chúng.
- Hình ảnh có thể up lên vimeo. Video có thể up lên youtube

### Use web font 

### Remove unnecessary DOM element

# Actions for speed: Server Technique

### Enable text compression

- Text nén sẽ giảm đáng kể lượng respon http
- Browse hỗ trợ nén trong http request là gzip and deflate. 