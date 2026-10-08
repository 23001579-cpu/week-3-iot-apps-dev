# week-3-iot-apps-dev
# OneButton Library Demo

Ví dụ sử dụng thư viện [OneButton](https://github.com/mathertel/OneButton) để
đọc nút nhấn và điều khiển LED tích hợp trên các board ESP32. Chương trình
không dùng `delay()` để nháy LED; vòng lặp liên tục cập nhật LED và trạng thái
nút, nhờ đó các thao tác được xử lý không chặn.

## Chức năng

- **Single click:** đảo trạng thái LED (đang tắt thì bật, đang bật thì tắt).
- **Double click:** chuyển LED sang chế độ nhấp nháy với khoảng thời gian
  200 ms giữa các lần đổi trạng thái.
- Nhấn giữ không còn kích hoạt chế độ nhấp nháy.
- OneButton xử lý nút và khử rung phím.

Sau khi double-click, LED tiếp tục nhấp nháy cho đến khi có single click; theo
cách hoạt động của `LED::flip()`, single click khi LED đang nhấp nháy sẽ đưa
LED về trạng thái tắt.

## Phần cứng và cấu hình chân

Các chân và mức tác động được khai báo theo từng môi trường trong
[`platformio.ini`](platformio.ini):

| Môi trường PlatformIO | Board | Nút nhấn | Mức tác động nút | LED | Mức tác động LED |
| --- | --- | ---: | --- | ---: | --- |
| `esp32doit-devkit-v1` | ESP32 DOIT DevKit V1 | GPIO0 | LOW | GPIO2 | HIGH |
| `esp32c3_super_mini` | ESP32-C3 Super Mini | GPIO9 | LOW | GPIO8 | LOW |

Chương trình điều khiển LED tích hợp trên board. Nút nhấn phải được nối phù hợp
với mạch/board tương ứng và mức tác động được cấu hình. `OneButton` được khởi
tạo với mức tác động nút đảo từ `BTN_ACT`.

## Cấu trúc thư mục

```text
OneButton_Lib_Demo/
├── lib/
│   └── LED/
│       └── LED.h           # Lớp điều khiển LED: on, off, flip, blink
├── src/
│   └── main.cpp            # Khởi tạo nút và gắn callback OneButton
└── platformio.ini          # Thư viện, board và cấu hình chân
```

## Cách chương trình hoạt động

1. `setup()` tắt LED ban đầu, sau đó gắn `btnPush()` với sự kiện single click
   và `btnDoubleClick()` với sự kiện double click.
2. `loop()` gọi `led.loop()` để cập nhật chế độ nháy không chặn và gọi
   `button.tick()` để OneButton nhận biết các thao tác nút.
3. `btnPush()` gọi `led.flip()` để đảo trạng thái LED.
4. `btnDoubleClick()` gọi `led.blink(200)` để bắt đầu nháy với thời gian
   200 ms. Sự kiện nháy được bắt đầu sau khi OneButton nhận biết double-click
   trong khoảng thời gian phân loại click của thư viện.

## Build và nạp chương trình

1. Mở thư mục `OneButton_Lib_Demo` bằng VS Code đã cài PlatformIO IDE.
2. Chọn môi trường tương ứng trong `platformio.ini`:
   - ESP32 DOIT DevKit V1: `env:esp32doit-devkit-v1`
   - ESP32-C3 Super Mini: `env:esp32c3_super_mini`
3. Build, kết nối board qua USB và dùng lệnh **Upload** của PlatformIO.
4. Mở Serial Monitor ở tốc độ 115200 nếu cần theo dõi cổng serial.

PlatformIO tải thư viện `mathertel/OneButton` phiên bản tương thích `^2.6.1`
theo khai báo `lib_deps`.

## Thư viện và tham khảo

- [OneButton - Mathertel](https://github.com/mathertel/OneButton): đọc thao tác
  nút, nhận diện click và khử rung.
- `lib/LED/LED.h`: lớp LED tự viết với các thao tác bật, tắt, đảo trạng thái và
  nhấp nháy.
