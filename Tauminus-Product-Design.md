# Tauminus — Đặc tả chức năng

Tauminus là workspace terminal dành cho lập trình với AI coding agent trên nhiều máy. Sản phẩm kết hợp quản lý kết nối, project, terminal, agent, file, thay đổi mã, dịch vụ và cộng tác trên desktop và mobile.

Tài liệu mô tả chức năng và quy tắc hoạt động cần có của sản phẩm; không xác nhận tình trạng triển khai.

## 1. Đối tượng và quan hệ

| Đối tượng | Ý nghĩa |
|---|---|
| Host | Máy local hoặc máy từ xa; có cấu hình kết nối, identity, shell mặc định, runtime và các project. |
| Project | Thư mục làm việc trên một host; chứa các tab và ngữ cảnh file, Git, dịch vụ. |
| Tab | Terminal, coding agent, web preview hoặc trang cấu hình liên quan; thuộc một project. |
| Pane | Một vùng làm việc trong tab, gắn với phiên hoặc tiến trình riêng. |
| Session | Phiên terminal/agent trên máy; có thể tồn tại độc lập với tab/client nếu runtime hỗ trợ. |
| Identity | Bộ thông tin đăng nhập dùng lại cho nhiều host. |
| Vault | Tập tài nguyên cá nhân/nhóm với quyền truy cập và chính sách đồng bộ riêng. |

- Tổ chức cố định **Host → Project → Tab**; cùng repository trên hai host là hai ngữ cảnh độc lập.
- Thao tác chạy lệnh, mở agent, đọc/ghi file và forwarding luôn có host/project đích xác định.
- Chuyển ngữ cảnh không tự đóng session, dừng agent hoặc hủy transfer.

## 2. Quản lý host

- Thêm, xem, sửa, đổi tên, sao chép cấu hình và xóa host đã lưu.
- Lưu tên, địa chỉ, cổng, username, giao thức, identity, tags và ghi chú.
- Nhận diện máy local; mở shell local không cần kết nối SSH.
- Tổ chức nhóm và nhóm lồng nhau; tìm kiếm, lọc theo tags, đánh dấu host thường dùng.
- Kế thừa identity, kết nối, jump host và biến môi trường từ nhóm; ghi đè riêng ở host.
- Phân biệt giá trị kế thừa với giá trị riêng; đổi nhóm không ghi đè tùy chọn riêng.
- Kiểm tra kết nối; cho lưu cấu hình khi host offline.
- Theo dõi chưa kết nối, đang kết nối, đã kết nối, reconnect và lỗi.
- Xóa host khỏi ứng dụng không xóa file trên máy; xử lý riêng các session/tác vụ đang sử dụng host.

## 3. Giao thức, kết nối và xác thực

- SSH bằng password, private key có/không passphrase và OpenSSH user certificate.
- Keyboard-interactive cho OTP, 2FA và các bước xác thực server yêu cầu.
- Dùng system SSH agent/Pageant, FIDO2 hoặc key phần cứng khi nền tảng hỗ trợ.
- Kết nối qua chuỗi jump host, ProxyJump, ProxyCommand, SOCKS5 và HTTP CONNECT.
- Cấu hình agent forwarding, environment variables, working directory, shell, timeout và keepalive theo host.
- Kiểm tra fingerprint lần đầu, lưu known hosts, đối chiếu khóa ở lần kết nối sau.
- Chặn khi host key thay đổi/bị thu hồi; yêu cầu xử lý rõ ràng, không tự thay khóa đã tin cậy.
- Hỗ trợ Mosh, Telnet và Serial khi môi trường đáp ứng; thông báo điều kiện còn thiếu.
- Serial cho chọn thiết bị, baud rate, data bits, parity, stop bits và flow control.
- Ngắt kết nối, reconnect thủ công hoặc tự động theo tùy chọn; cho hủy reconnect.
- Phân biệt lỗi mạng/DNS, timeout, xác thực, host key, proxy/jump host và thiếu quyền.

## 4. Preflight và thiết lập host

- Kiểm tra OS, kiến trúc, quyền truy cập và điều kiện cần cho kết nối/session.
- Phát hiện shell thực sự có trên host, executable và shell đăng nhập hiện tại.
- Chọn shell mặc định từ các shell hợp lệ: bash, zsh, fish, pwsh, cmd hoặc shell được cấu hình.
- Lưu mặc định riêng từng host, áp dụng cho terminal mới; không đổi shell của phiên đang chạy.
- Phát hiện agent CLI đã cài, executable, phiên bản và khả năng khởi chạy với tài khoản kết nối.
- Phân biệt đã cài với đã đăng nhập/sẵn sàng sử dụng khi agent cho phép kiểm tra.
- Phát hiện tmux, Herdr, phiên bản tương thích và session có sẵn.
- Chọn dùng runtime đã có, cài tự động tmux/Herdr hoặc dùng SSH thông thường.
- Chỉ cài khi người dùng chọn; theo dõi tiến trình, xác minh kết quả, báo lỗi và cho thử lại/đổi lựa chọn.
- Không tự nâng cấp runtime, thay cấu hình hoặc kết thúc session hiện hữu.
- Chạy lại preflight khi cần; cập nhật shell/agent khả dụng sau khi môi trường thay đổi.

## 5. Quản lý project

- Thêm project bằng đường dẫn thư mục trên host; kiểm tra tồn tại và quyền truy cập.
- Đặt tên, sửa đường dẫn, tìm/mở project gần đây và gỡ khỏi workspace.
- Gỡ project không xóa thư mục hoặc repository trên host.
- Phát hiện Git root và branch; thư mục không có Git vẫn dùng được.
- Lưu đường dẫn, tab, pane, session gắn kèm và dịch vụ của project.
- Một project chạy nhiều terminal và nhiều agent đồng thời.
- Chuyển project rồi quay lại giữ nguyên tập tab và tab đang làm việc.
- Khi thư mục bị di chuyển/xóa/mất quyền, báo nguyên nhân và cho chọn lại đường dẫn.
- Mở shell ở thư mục tùy chọn trên host, không bắt buộc là repository.

## 6. Tab, pane và workspace

- Tạo, chọn, đổi tên, sắp xếp và đóng tab; mỗi tab thuộc đúng host/project.
- Chọn tab từ cây workspace hoặc danh sách tab mở cùng một nội dung/session.
- New tab chỉ mở terminal, coding agent, web preview hoặc settings liên quan; không mở file/diff thành tab tài liệu.
- Shell/agent trong launcher lấy từ preflight của host hiện tại; agent chưa cài không thể khởi chạy như agent khả dụng.
- Chọn shell mặc định hoặc shell khác cho riêng phiên mới; agent khởi chạy trong thư mục project.
- Chia pane ngang/dọc, resize, chuyển focus, phóng to/khôi phục và đóng từng pane.
- Dùng một hàng tab chung; không tạo hệ tab riêng trong mỗi pane.
- Lưu, đặt tên và khôi phục workspace gồm host/project, các tab/pane và ngữ cảnh khởi chạy.
- Khôi phục ưu tiên attach session còn tồn tại; không tự chạy thêm agent do session đang mất kết nối.
- Phân biệt đóng tab, detach session và kết thúc tiến trình; xử lý rõ tác vụ đang chạy.

## 7. Terminal

- Chạy shell local/remote, CLI, TUI và tiến trình foreground/background.
- Nhập lệnh, gửi phím điều khiển, interrupt/EOF và thay đổi kích thước terminal.
- Hỗ trợ Unicode, IME/tiếng Việt và encoding khác khi host cần.
- Chọn văn bản, copy/paste, tìm output, scrollback, xóa màn hình và mở liên kết.
- Xử lý clipboard theo quyền và tùy chọn; không mặc định cho mọi session truy cập clipboard.
- Cấu hình font, kích thước chữ, bảng màu terminal, con trỏ và phím tắt.
- Shell integration cung cấp working directory, ranh giới lệnh, thời điểm hoàn thành và exit code khi hỗ trợ.
- Đánh dấu output mới, lỗi và yêu cầu tương tác; mất kết nối không đồng nghĩa tiến trình đã dừng.
- Paste nhiều dòng hoặc gửi input vào phiên khác phải xác định rõ đích thực thi.

## 8. Coding agent

- Launcher hỗ trợ Claude Code, Codex, OpenCode và các agent CLI khác được nhận diện/cấu hình trên host.
- Lưu executable và tham số khởi chạy; kiểm tra lại khi không tìm thấy hoặc chạy lỗi.
- Mở nhiều phiên cùng agent hoặc nhiều loại agent trong một project, không gộp các phiên.
- Gửi prompt, trả lời câu hỏi, xác nhận yêu cầu và interrupt trực tiếp trong terminal agent.
- Đăng nhập, lựa chọn model và quyền thực thi theo khả năng từng CLI; không giả định mọi agent có cùng chức năng.
- Theo dõi trạng thái có thể xác minh: khởi chạy, làm việc, cần input, hoàn tất, lỗi, mất kết nối hoặc không rõ.
- Thiếu tích hợp trạng thái vẫn dùng được như terminal; không suy đoán trạng thái từ tên tiến trình.
- Tổng hợp agent cần xử lý trên nhiều host/project và chuyển đến đúng phiên.
- Thông báo khi cần input, hoàn tất hoặc lỗi theo tùy chọn; tránh lặp cùng sự kiện.
- Mở file/thay đổi liên quan; gửi đường dẫn, đoạn mã hoặc diff cho agent khi người dùng chọn và CLI hỗ trợ.
- Tải file/ảnh lên host để cung cấp đường dẫn cho agent; xác định vị trí lưu và kết quả tải.
- Attach phiên còn tồn tại sau reconnect; phân biệt attach terminal với resume conversation riêng của agent.

## 9. Session bền vững và vòng đời ứng dụng

- Liệt kê session tmux/Herdr hiện hữu, xem thông tin và gắn vào project phù hợp.
- Tạo, attach, detach, đổi tên và kết thúc session khi runtime hỗ trợ.
- Ánh xạ session/window/pane đã có vào workspace, không nhân bản tiến trình.
- Giữ session chạy trên host khi client detach/mất kết nối nếu runtime hỗ trợ.
- Reconnect kiểm tra session còn tồn tại trước khi attach; nếu đã mất, cho tạo phiên mới.
- Tiếp tục phiên từ thiết bị khác theo quyền truy cập và khả năng đa client của runtime.
- Đóng cửa sổ desktop thu ứng dụng xuống tray; mở lại, xem phiên cần tương tác hoặc thoát hoàn toàn.
- Khi client chạy nền, giữ kết nối/tác vụ theo thiết lập; thoát hoàn toàn xử lý forwarding, transfer và phiên local đang chạy.
- Thoát client không tự kết thúc session remote bền vững; SSH thông thường và shell local có vòng đời khác.
- Không cam kết tiến trình sống qua reboot host; khôi phục workspace khác với khôi phục tiến trình.

## 10. File và editor nhẹ

- Duyệt thư mục local/remote; mở project, đường dẫn tùy chọn và làm mới nội dung.
- Phân biệt file, thư mục, symlink và mục không có quyền truy cập.
- Tìm/lọc theo tên, mở theo đường dẫn, sao chép đường dẫn tương đối/tuyệt đối.
- Tạo file/thư mục, rename, move, copy và delete với xử lý tên trùng/xung đột.
- Xem kích thước, thời gian sửa, quyền và owner khi nguồn cung cấp.
- Preview văn bản, mã nguồn, ảnh và metadata định dạng không xem trực tiếp được.
- Editor hỗ trợ syntax highlight, số dòng, tìm/thay thế, undo/redo, lưu và lưu thành file khác.
- Nhận biết encoding, kiểu xuống dòng, read-only và thay đổi chưa lưu.
- Phát hiện file bị sửa bên ngoài; cho reload hoặc giải quyết xung đột trước khi ghi đè.
- Lưu remote thất bại phải giữ nội dung đang sửa; chỉ báo thành công khi host xác nhận.
- File quá lớn/nhị phân/không hỗ trợ có lựa chọn xem thông tin hoặc tải xuống.
- File/editor mở theo ngữ cảnh, không tạo tab tài liệu; đóng nội dung quay lại đúng phiên trước đó.

## 11. Git changes và diff review

- Phát hiện repository/branch; phân biệt trạng thái Git với trạng thái session/agent.
- Liệt kê modified, added, deleted, renamed, untracked và conflicted files.
- Phân biệt staged/unstaged; làm mới khi người dùng hoặc agent sửa file.
- Xem diff theo file/hunk, phần thêm/xóa, số dòng, syntax highlight; hỗ trợ unified và so sánh hai phía.
- Xem nội dung hiện tại và phiên bản gốc; xử lý riêng file nhị phân/quá lớn.
- Chuyển file thay đổi, mở file để sửa, sao chép đoạn diff và gửi ngữ cảnh cho agent.
- Đóng review quay lại terminal, không tạo tab diff và không mất session.
- Xem diff không tự commit, merge, push hoặc giải quyết conflict.

## 12. Truyền file

- Mở SFTP độc lập hoặc dùng lại host đã xác thực.
- Chọn nguồn/đích local hoặc remote, đường dẫn và chiều truyền; hỗ trợ truyền giữa các host.
- Upload/download file, thư mục và nhiều mục; kéo thả/chọn file theo khả năng thiết bị.
- Duyệt thư mục, nhập đường dẫn, lên thư mục cha, refresh và tạo thư mục đích.
- Tên trùng được xử lý bằng ghi đè, bỏ qua hoặc đổi tên; áp dụng một mục/cả nhóm theo lựa chọn.
- Sửa remote file và tải bản sửa lên đúng vị trí khi lưu.
- Xem/đổi quyền khi tài khoản cho phép; báo lỗi quyền, thiếu dung lượng hoặc đích không tồn tại.
- Giữ metadata cần thiết khi hai đầu và giao thức hỗ trợ.
- Truyền qua terminal bằng Zmodem/trzsz khi tương thích, có theo dõi kết quả tác vụ.
- Transfer tiếp tục khi chuyển terminal/project; không chặn công việc khác.

## 13. Transfer queue

- Theo dõi từng file/nhóm: tên, nguồn, đích, kích thước, tiến độ và kết quả.
- Trạng thái: chờ, đang truyền, tạm dừng, hoàn tất, lỗi, đã hủy.
- Hiển thị tốc độ/thời gian còn lại khi có đủ dữ liệu để tính.
- Hủy một tác vụ/cả nhóm; pause/resume khi giao thức hỗ trợ, nếu không thì chỉ rõ cần truyền lại.
- Retry lỗi, xem nguyên nhân và đổi cách xử lý file trùng.
- Mất kết nối không đánh dấu hoàn tất; khôi phục hoặc yêu cầu thử lại theo khả năng truyền tiếp.
- Mặc định thu gọn nhưng vẫn theo dõi tổng số tác vụ, tiến độ và lỗi.
- Mở/thu chi tiết không dừng tác vụ; tác vụ mới không ép mở hàng đợi.
- Dọn lịch sử tác vụ hoàn tất không xóa file đã truyền.

## 14. Port forwarding và tunneling

- Tạo, xem, sửa, đặt tên, sao chép, xóa quy tắc và lưu để dùng lại.
- Local forwarding: listener trên client chuyển tiếp đến dịch vụ truy cập được từ host SSH.
- Remote forwarding: listener trên host remote chuyển tiếp đến đích từ phía client.
- Dynamic forwarding: cung cấp SOCKS endpoint qua SSH.
- Cấu hình host, bind address, listen port, destination host/port và project liên quan.
- Start/stop từng quy tắc; tùy chọn tự chạy khi kết nối và phục hồi sau reconnect.
- Kiểm tra cổng không hợp lệ/đang dùng, thiếu quyền bind và server từ chối forwarding.
- Phân biệt cấu hình đã lưu, tunnel hoạt động, tunnel lỗi và dịch vụ đích không phản hồi.
- Đổi cấu hình đang chạy cần thao tác áp dụng/restart rõ ràng.
- Không tự bind ra mọi interface khi người dùng chỉ yêu cầu truy cập trên thiết bị hiện tại.
- Forward local thuộc client đang chạy; thiết bị khác phải tạo listener riêng.

## 15. Dịch vụ và web preview

- Khai báo dịch vụ/cổng của project; phát hiện khi môi trường hỗ trợ.
- Liên kết dịch vụ với forwarding, chọn cổng khác khi xảy ra xung đột.
- Mở preview từ URL hoặc dịch vụ cấu hình thành tab của project.
- Reload, điều hướng URL, copy địa chỉ và mở bằng trình duyệt ngoài.
- Phân biệt lỗi DNS, TLS, tunnel chưa chạy và dịch vụ không phản hồi.
- Đóng preview không tự dừng dev server, agent hoặc tunnel dùng chung.
- Dùng endpoint phù hợp từng thiết bị; localhost desktop không được coi là localhost điện thoại.

## 16. Snippets, autocomplete và broadcast

- Tạo, sửa, xóa, tìm kiếm snippets; tổ chức bằng thư mục/nhóm.
- Lưu nội dung, mô tả và biến đầu vào; điền đủ biến trước khi chạy.
- Cho dán vào input để sửa hoặc thực thi theo thao tác chủ động.
- Chạy snippet trên một/nhiều phiên, kết quả phân biệt từng đích.
- Cấu hình startup snippet; không chạy lại trên mỗi reconnect nếu không được yêu cầu.
- Chia sẻ snippets qua vault với quyền chỉnh sửa tương ứng.
- Autocomplete theo shell/host từ lệnh, tham số, đường dẫn, history và snippets; cho tắt.
- Gợi ý credential tại lời nhắc phù hợp cần người dùng chọn; không ghi bí mật vào output/history.
- Broadcast chỉ chạy khi được bật và có danh sách phiên nhận xác định.
- Thêm/bỏ đích, dừng broadcast, gửi interrupt đến phiên đã chọn.
- Không broadcast credential/lời nhắc xác thực hoặc input vượt quyền chia sẻ session.

## 17. History, session logs và recording

- Lưu/tìm lịch sử lệnh theo host, project, session, thời gian và kết quả khi có dữ liệu.
- Copy hoặc đưa lệnh cũ vào input; tìm kiếm không tự chạy lại lệnh.
- Tắt ghi history, xóa một phần/toàn bộ và đặt thời gian lưu.
- Start/stop recording, nhận biết phiên đang được ghi.
- Replay, pause, seek, đổi tốc độ; giữ thứ tự output và thay đổi kích thước terminal.
- Tìm nội dung log/recording; bookmark và ghi chú.
- Import/export log hoặc recording theo định dạng hỗ trợ.
- Phân biệt dữ liệu cá nhân với log nhóm; quyền xem, ghi chú, xuất và xóa theo chính sách.
- Không ghi input xác thực bí mật vào history; cho kiểm soát nội dung lưu/chia sẻ.

## 18. Hoạt động và thông báo

- Tổng hợp session đang chạy, output chưa đọc, cần input, lỗi và mất kết nối.
- Chuyển từ sự kiện/thông báo đến đúng host/project/tab.
- Thông báo kết quả agent, transfer, reconnect và cài runtime theo tùy chọn.
- Mute theo phiên/host; tùy chọn âm thanh và thông báo hệ điều hành.
- Pattern/regex để highlight output và nhận biết sự kiện người dùng quan tâm.
- Trigger chỉ thực hiện hành động đã cấu hình; chạy lệnh theo output cần được bật rõ ràng.

## 19. Keychain và identities

- Tạo, import, đổi tên, xem metadata và xóa SSH keys tương thích.
- Quản lý passphrase, private/public key và OpenSSH certificates.
- Copy/export public key; export private key qua thao tác rõ ràng và xác thực cần thiết.
- Cài public key vào tài khoản đã xác thực trên host, không ghi đè key khác ngoài yêu cầu.
- Tạo identity gồm username/phương thức xác thực; dùng cho host/nhóm và cho ghi đè.
- Dùng ssh-agent, FIDO2/security key, key gắn với thiết bị; xử lý touch/PIN/biometric khi hỗ trợ.
- Quản lý known hosts: fingerprint, nguồn tin cậy, thay đổi khóa và thu hồi khóa.
- Lấy credential từ password manager/secret provider được người dùng cấu hình; không bắt buộc sao chép bí mật vào vault.
- Thiếu credential, chứng chỉ hết hạn hoặc thiếu thiết bị xác thực phải yêu cầu bổ sung; không tự đổi identity.

## 20. Vault, đồng bộ và sao lưu

- Tạo/quản lý vault cá nhân cho hosts, identities, keys, snippets và cấu hình liên quan.
- Khóa/mở ứng dụng hoặc vault bằng phương thức nền tảng hỗ trợ; tự khóa theo thời gian/chính sách.
- Khóa truy cập dữ liệu và kết thúc session là hai hành vi riêng.
- Dùng chức năng local không bắt buộc tài khoản dịch vụ.
- Bật/tắt sync và chọn nơi lưu do người dùng quản lý: thư mục, Git hoặc S3.
- Mã hóa đầu cuối; chọn dữ liệu sync, loại trừ credentials/keys khi muốn.
- Tách cấu hình chung khỏi dữ liệu riêng thiết bị như đường dẫn local và listener forwarding.
- Trạng thái sync gồm đang chạy, đã cập nhật, offline, lỗi và xung đột; cho retry và giải quyết xung đột.
- Giữ thay đổi offline để sync sau; xóa dữ liệu có quy tắc lan truyền rõ ràng.
- Backup/restore cấu hình và vault; bản sao chứa bí mật phải được bảo vệ.
- Đồng bộ session metadata không di chuyển tiến trình đang chạy sang máy khác.

## 21. Cộng tác và quản trị nhóm

- Tạo nhiều team vault; mời thành viên, cấp/đổi/thu hồi quyền.
- Phân quyền quản trị, chỉnh sửa và sử dụng/xem theo vault.
- Chia sẻ host, cấu hình và snippets có/không credential theo chính sách.
- Thu hồi truy cập ngăn sử dụng mới và xử lý phiên chia sẻ; không hứa thu hồi dữ liệu đã sao chép ra ngoài.
- Mời chia sẻ live terminal với thời hạn và quyền xem/điều khiển.
- Theo dõi người tham gia, kết nối và người giữ quyền nhập.
- Một người điều khiển tại một thời điểm; yêu cầu, cấp, chuyển và thu hồi quyền nhập.
- Chủ phiên lấy lại quyền, ngắt người tham gia hoặc dừng chia sẻ.
- Người chỉ xem không gửi input, chạy snippet hay broadcast vào phiên.
- Mất kết nối/reconnect phải cập nhật quyền điều khiển và trạng thái người tham gia.
- Quản lý logs nhóm, quyền xem/xuất và thời gian lưu.
- Chính sách xác thực nhóm, 2FA, đăng nhập tập trung và vòng đời thành viên khi tổ chức sử dụng.
- Tra cứu hoạt động quản trị, thay quyền và chia sẻ theo quyền; không lưu bí mật trong nhật ký quản trị.

## 22. Import, export và inventory cloud

- Import OpenSSH config, CSV và dữ liệu SSH client được hỗ trợ như PuTTY, MobaXterm, SecureCRT.
- Preview mục nhập, ánh xạ trường, chọn mục và giải quyết trùng trước khi áp dụng.
- Báo trường/mục bị bỏ qua hoặc không hỗ trợ; không coi nhập một phần là thành công toàn bộ.
- Lấy inventory từ tài khoản cloud đã kết nối: AWS, Azure, DigitalOcean, GCP theo provider hỗ trợ.
- Chọn tài khoản, project/subscription, region; refresh không tự xóa tùy chỉnh host đã lưu.
- Export host, nhóm, snippets và workspace; không mặc định xuất credential dạng rõ.
- Import/export không tự kết nối, chạy lệnh hoặc cài phần mềm lên host vừa nhập.

## 23. Desktop, mobile và tùy chọn ứng dụng

- Cùng phạm vi chức năng: host/project/session, agent, file/editor/diff, transfer, forwarding, vault và cộng tác.
- Chức năng phụ thuộc phần cứng/OS phải kiểm tra khả dụng và thông báo giới hạn cụ thể.
- Mobile có phím terminal bổ trợ, copy/paste, nhập prompt dài, chọn file/ảnh và voice input khi thiết bị hỗ trợ.
- Khi mobile bị hệ điều hành đình chỉ, giữ trạng thái và reconnect/reattach khi quay lại; không giả định socket luôn chạy nền.
- Phân biệt tác vụ tiếp tục trên host với tác vụ cần client hoạt động, như truyền file từ thiết bị và forward local.
- Tìm kiếm nhanh và command palette cho host/project/tab, thao tác và cấu hình.
- Cấu hình phím tắt, terminal preferences, thông báo, mặc định kết nối, lưu history/logs và hành vi đóng ứng dụng.
- Kiểm tra/cài cập nhật theo lựa chọn; xử lý tác vụ đang chạy trước khi restart ứng dụng.

## 24. Quy tắc xử lý chung

- Cấu hình đã lưu, kết nối client, tiến trình remote và tác vụ đang chạy có trạng thái riêng.
- Ghi đè/xóa dữ liệu, kết thúc tiến trình, đổi quyền hoặc chạy lệnh nhiều đích phải xác định rõ phạm vi tác động.
- Mất mạng/quyền phải giữ dữ liệu chưa lưu và báo nguyên nhân; không báo thao tác lỗi là đã hoàn tất.
- Xem file/diff, đổi project, đóng preview và thu gọn queue không tự chạy lệnh hay sửa dữ liệu host.
- Không tự cấp thêm quyền cho agent, điều phối agent hoặc quản lý Git worktree thay người dùng.
