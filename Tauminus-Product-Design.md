# Tauminus — Đặc tả chức năng

Tauminus là workspace terminal dành cho lập trình với AI coding agent trên nhiều máy. Trên desktop, người dùng có thể mở shell trên máy đang dùng ngay rồi thêm host SSH khi cần. Host, project, terminal, agent, file, thay đổi mã và dịch vụ dùng chung một workspace. Mobile là client để làm việc với các phiên chạy trên máy tính hoặc host từ xa.

Các mục chức năng dưới đây mô tả hành vi sản phẩm, gồm cả các yêu cầu ngoài phạm vi bản xem trước. Mục cuối mô tả riêng những gì mockup trình duyệt thể hiện và giới hạn của mô phỏng.

## Khởi động và màn hình Start

- Desktop nhận diện máy đang dùng dưới tên **This computer**. Khi chưa có workspace đã lưu, chuẩn bị shell mặc định trong thư mục home và hiện **Start**. Người dùng có thể vào shell local ngay, không cần tài khoản dịch vụ, host SSH hay cài runtime.
- Start có bốn lối vào: **Open local terminal**, **Connect host**, **Open project** và **Resume session**. Danh sách project gần đây hiện host và đường dẫn. Danh sách phiên agent gần đây hiện host, trạng thái và thời điểm cập nhật. Chọn một mục chuyển tới đúng ngữ cảnh hoặc attach phiên còn tồn tại.
- Tùy chọn khởi động gồm **Restore last workspace** (mặc định), **New local terminal** và **Host tools**. Khôi phục workspace thành công thì bỏ qua Start. Nếu chưa có workspace đã lưu, Start vẫn hiện. Ở các lần khởi động khác, dùng thiết lập **Show at startup**. Có thể mở lại Start từ menu ứng dụng.
- Sau khi vào terminal local lần đầu trên desktop, gợi ý kết nối SSH hoặc mở project có thể bỏ qua. Host tools là cửa vào quản lý cấu hình, truyền file và các công cụ host khi người dùng cần.

## 1. Đối tượng và quan hệ

| Đối tượng | Ý nghĩa |
|---|---|
| Host | Máy local hoặc máy từ xa; có cấu hình kết nối, identity, shell mặc định, runtime và các project. |
| Project | Thư mục làm việc trên một host; chứa các tab và ngữ cảnh file, Git, dịch vụ. |
| Tab | Terminal, coding agent hoặc web preview thuộc một project. |
| Runtime | Cách tiến trình của phiên được duy trì, ví dụ shell trực tiếp, tmux hoặc Herdr Core. Host có thể có nhiều runtime. |
| Phiên khám phá được | Phiên tmux hoặc Herdr đã có trên host, có thể xuất hiện trước khi được gán vào project hay mở thành tab. |
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
- Ghim **This computer** trong cây workspace và phân biệt với các host SSH. Host local có shell và project riêng. Thao tác reconnect chỉ áp dụng cho host từ xa.
- **Host tools** dùng chung trên desktop và mobile. Chọn rõ host đích và xem trạng thái kết nối trước khi dùng **Hosts**, **SFTP**, **Keys & vaults**, **Tunnels** hoặc **Terminal tools**. Trên màn hình nhỏ có thể quay lại từ chi tiết mục về nhóm công cụ.
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

## 4. Thêm host SSH và kiểm tra môi trường

- **Add host** cho tạo kết nối hoặc nhập các mục đã chọn từ cấu hình OpenSSH của máy tính. Trong chế độ nhập, xem alias, địa chỉ, cổng và IdentityFile. Mục đã có được đánh dấu. Mục nhập và mục chọn **Save host** xuất hiện dưới trạng thái chưa kết nối, chờ kiểm tra môi trường.
- **Quick connect** điền biểu mẫu từ `user@host[:port]` hoặc địa chỉ host, gồm IPv6 đặt trong ngoặc vuông. Có thể sửa tên, địa chỉ, cổng, username, phương thức xác thực và identity. Phần nâng cao có jump host/proxy, keepalive, timeout và tùy chọn SSH. Xác thực biểu mẫu tại trường lỗi.
- Luồng kết nối đi qua **Connection → Verify identity → Preflight → Choose environment → Ready**. Giữ dữ liệu đã nhập khi quay lại hoặc khi kết nối thất bại. Khi sai xác thực, cho chọn identity khác và thử lại. Khi fingerprint thay đổi, hiện khóa đã lưu và khóa server đưa ra trước khi người dùng quyết định thay thế.
- Kiểm tra OS, kiến trúc, quyền truy cập và điều kiện cần cho kết nối/phiên.
- Phát hiện shell thực sự có trên host, executable và shell đăng nhập hiện tại.
- Chọn shell mặc định từ các shell hợp lệ: bash, zsh, fish, pwsh, cmd hoặc shell được cấu hình.
- Lưu mặc định riêng từng host, áp dụng cho terminal mới; không đổi shell của phiên đang chạy.
- Phát hiện agent CLI đã cài, executable, phiên bản và khả năng khởi chạy với tài khoản kết nối.
- Phân biệt đã cài với đã đăng nhập/sẵn sàng sử dụng khi agent cho phép kiểm tra.
- Phát hiện tmux, Herdr, phiên bản tương thích và session có sẵn.
- Chọn dùng runtime đã có, cài tmux hoặc Herdr Core theo yêu cầu, hoặc dùng SSH thông thường. Nêu rõ khả năng attach lại khi mất kết nối trước khi xác nhận.
- Chỉ cài sau thao tác chủ động. Hiện tiến trình và log, kiểm tra lại phiên bản, báo thiếu quyền hoặc lỗi cài đặt. Cho thử lại, dùng runtime khác đã có hoặc tiếp tục với SSH thông thường.
- **Ready** tóm tắt đích kết nối, shell, runtime và các agent tìm được. Chỉ mở terminal trên host sau khi hoàn tất xác minh và preflight.
- Không tự nâng cấp runtime, thay cấu hình hoặc kết thúc session hiện hữu.
- Chạy lại preflight khi cần; cập nhật shell/agent khả dụng sau khi môi trường thay đổi.

## 5. Quản lý project

- Thêm project bằng đường dẫn thư mục trên host; kiểm tra tồn tại và quyền truy cập.
- Đặt tên, sửa đường dẫn, tìm/mở project gần đây và gỡ khỏi workspace.
- Gỡ project không xóa thư mục hoặc repository trên host.
- Phát hiện Git root và branch. Thư mục không có Git vẫn dùng được và mở Inspector ở **Files**.
- Hiện nguồn project **Added manually** hoặc **Discovered via Herdr**. Người dùng cũng có thể tạo project từ đường dẫn đã xem trong **Assign session**. Việc tìm thấy runtime hay phiên riêng lẻ không tự tạo project.
- Project chưa có tab hiện trạng thái trống, tên host và đường dẫn, cùng thao tác mở terminal hoặc **New tab**.
- Lưu đường dẫn, tab, pane, session gắn kèm và dịch vụ của project.
- Một project chạy nhiều terminal và nhiều agent đồng thời.
- Chuyển project rồi quay lại giữ nguyên tập tab và tab đang làm việc.
- Khi thư mục bị di chuyển/xóa/mất quyền, báo nguyên nhân và cho chọn lại đường dẫn.
- Mở shell ở thư mục tùy chọn trên host, không bắt buộc là repository.

## 6. Tab, pane và workspace

- Tạo, chọn, đổi tên, sắp xếp và đóng tab; mỗi tab thuộc đúng host/project.
- Chọn tab từ cây workspace hoặc danh sách tab mở cùng một nội dung/session.
- Launcher **New tab** mở terminal, coding agent hoặc web preview theo host/project hiện tại. Mở lại preview chọn tab preview đã có của project. Mục **Host settings** trong launcher dẫn tới Settings theo phạm vi host. File và diff mở trong vùng làm việc của project, ngoài hàng tab phiên.
- **Ctrl+T** trên Windows hoặc **⌘T** trên macOS mở terminal local. Nếu đang ở project local, dùng thư mục project. Nếu đang ở project SSH, dùng project Home trên **This computer**. Để mở shell trên host đang chọn, dùng **New tab**.
- Launcher lấy shell và agent từ môi trường của host hiện tại. Agent chưa cài bị vô hiệu hóa và có lý do.
- Chọn shell mặc định hoặc shell khác cho riêng phiên mới; agent khởi chạy trong thư mục project.
- Chia pane ngang/dọc, resize, chuyển focus, phóng to/khôi phục và đóng từng pane.
- Dùng một hàng tab chung. Desktop có thể phóng to pane đang chọn và khôi phục tỷ lệ chia đã lưu. Mobile có trình chọn tab và pane để chuyển vùng hiển thị.
- Desktop có cây **Hosts → Projects → Tabs**, vùng **Agents** có thể thu gọn/ẩn và Inspector theo ngữ cảnh với **Files**, **Changes**, **Ports**. Có thể chuyển sidebar thành rail và mở nhanh host hoặc Agents từ rail. Lệnh **Reveal active tab** mở đúng nhánh chứa tab; đóng nhánh không ảnh hưởng phiên.
- Lưu kích thước sidebar và Inspector theo thiết bị, tỷ lệ chia theo tab và trạng thái mở nhánh. **Reset layout** trả kích thước về mặc định và giữ các tab hiện có.
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
- Gửi prompt, trả lời câu hỏi, xác nhận yêu cầu và interrupt trực tiếp trong terminal agent. Sau khi agent nhận câu trả lời, cập nhật phiên và bộ đếm Needs input.
- Đăng nhập, lựa chọn model và quyền thực thi theo khả năng từng CLI; không giả định mọi agent có cùng chức năng.
- Dùng chung các trạng thái công việc **Starting**, **Working**, **Needs input**, **Idle**, **Completed**, **Cancelling…**, **Cancelled**, **Failed** và **Unknown** trên danh sách Agents, tab và pane. **Completed** xác nhận công việc gần nhất đã xong và agent có thể vẫn đang chạy. Chỉ thấy tiến trình thì dùng **Unknown**. Host thiếu nguồn trạng thái vẫn có thể dùng agent qua terminal khi CLI khả dụng.
- Theo dõi kết nối host riêng với trạng thái công việc. Khi mất kết nối, giữ hàng phiên và hiện kết quả biết gần nhất. **Needs input** dùng cùng một bộ đếm ở các màn hình. Lỗi kết nối hiển thị riêng.
- **Agents** tổng hợp các phiên đang attach và đã detach trên mọi host. Tìm theo tên phiên, agent, host hoặc project. **All active** liệt kê phiên chưa hoàn tất/hủy, **Needs input** lọc phiên chờ trả lời, **Recent** liệt kê phiên đã hoàn tất/hủy. Đưa yêu cầu nhập và lỗi lên đầu. Chọn một hàng để focus pane hiện có hoặc attach phiên đã detach, giữ nguyên định danh phiên.
- Khi người dùng yêu cầu hủy một công việc có thể đang chạy, hiển thị **Cancelling…** trong lúc chờ. Chỉ hiện **Cancelled** khi có xác nhận. Nếu không xác nhận được, trả về trạng thái trước và nêu lý do.
- Thông báo khi cần input, hoàn tất hoặc lỗi theo tùy chọn; tránh lặp cùng sự kiện.
- Mở file/thay đổi liên quan; gửi đường dẫn, đoạn mã hoặc diff cho agent khi người dùng chọn và CLI hỗ trợ.
- Tải file/ảnh lên host để cung cấp đường dẫn cho agent; xác định vị trí lưu và kết quả tải.
- Attach phiên còn tồn tại sau reconnect; phân biệt attach terminal với resume conversation riêng của agent.

## 9. Khám phá phiên, duy trì phiên và vòng đời ứng dụng

- Inventory theo từng host liệt kê runtime, phiên tmux/Herdr, phiên bản và kết quả quét gần nhất. **Rescan** cập nhật danh sách trên host đang kết nối. Khi lỗi, hiện nguyên nhân và cho thử lại. Dùng host, runtime và ID phiên từ runtime làm định danh ổn định, kể cả khi phiên đổi tên.
- Gợi ý project khi tất cả cửa sổ của phiên trỏ về cùng một project trên cùng host. So theo ranh giới thư mục và chọn project có đường dẫn sâu nhất. Windows chấp nhận cả dấu gạch xuôi và ngược, đồng thời so không phân biệt hoa thường. Linux phân biệt hoa thường. Dùng home của host để giải `~`, nhưng project Home không nhận phiên theo kiểu mặc định.
- Phiên có working directory thiếu, ngoài các project, hoặc trải trên nhiều project xuất hiện trong **Unassigned** kèm lý do. **Assign session** hiện từng window và working directory để người dùng chọn project cùng host hoặc tạo project theo đường dẫn tìm được. Lưu liên kết đã xác nhận qua các lần mở lại. Việc chạy `cd` sau đó không tự chuyển phiên sang project khác.
- Tạo, attach, detach, đổi tên và kết thúc phiên khi runtime hỗ trợ. Attach lại định danh đã mở thì chọn tab tương ứng. Mockup hiện tối đa bốn window cùng lúc dưới dạng pane, hoặc dùng trình chọn window khi phiên có nhiều hơn bốn, giữ riêng nội dung từng window khi chuyển.
- **Session info** hiển thị host, project và nguồn project, runtime/phiên bản, ID phiên và window khi có, trạng thái attach, working directory từng pane, cùng hệ quả của việc ngắt kết nối.
- Phiên remote qua tmux/Herdr có thể tiếp tục trên host khi client detach hoặc mất kết nối. Khi đóng tab, cho chọn **Detach** hoặc **End process** nếu runtime hỗ trợ. Shell local và SSH thông thường có vòng đời theo tiến trình hoặc kết nối tương ứng. Sau reconnect, xác minh phiên còn tồn tại trước khi attach; nếu phiên đã mất, cho mở phiên mới.
- Tiếp tục phiên từ thiết bị khác theo quyền của người dùng và khả năng đa client của runtime. Khôi phục layout và tab lưu trên thiết bị là thao tác riêng với việc xác minh, attach hoặc khởi chạy tiến trình.
- Desktop mặc định đóng cửa sổ xuống tray, có thể đặt nút đóng thành thoát có xác nhận. Nút thu nhỏ có thể gửi cửa sổ xuống taskbar hoặc tray. Khi ở tray, ứng dụng vẫn giữ kết nối, agent, transfer và forwarding. Khôi phục cửa sổ giữ các tab và pane.
- Khi thoát hoàn toàn, liệt kê terminal/agent local, forwarding, transfer và phiên SSH thường bị ảnh hưởng. Phiên tmux/Herdr còn tồn tại được attach lại khi mở ứng dụng. Với host đang mất kết nối, chỉ hiển thị trạng thái đã biết lần cuối cho đến khi kiểm tra lại. Shell local cần tạo tiến trình mới. Agent local phải được người dùng khởi chạy lại. Khả năng sống qua reboot host tùy runtime và cấu hình host.

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
- File/editor mở trong vùng làm việc của project. Đóng nội dung đưa người dùng về đúng phiên trước đó. Khi có nội dung chưa lưu, yêu cầu quyết định trước khi đóng.
- Cây file hỗ trợ hiện file ẩn, giữ nhánh đã mở theo project và chỉ báo số file thay đổi dưới từng thư mục. Hiện riêng trạng thái đang tải, lỗi quyền hoặc thư mục trống.

## 11. Git changes và diff review

- Phát hiện repository/branch; phân biệt trạng thái Git với trạng thái session/agent.
- Liệt kê modified, added, deleted, renamed, untracked và conflicted files.
- Phân biệt staged/unstaged; làm mới khi người dùng hoặc agent sửa file.
- Xem diff theo file/hunk, phần thêm/xóa, số dòng, syntax highlight; hỗ trợ unified và so sánh hai phía.
- Xem nội dung hiện tại và phiên bản gốc; xử lý riêng file nhị phân/quá lớn.
- Chuyển file thay đổi, mở file để sửa, sao chép đoạn diff và gửi ngữ cảnh cho agent.
- Đóng review quay lại terminal, không tạo tab diff và không mất session.
- Xem diff không tự thay đổi repository. Cho stage/unstage và commit bằng thao tác riêng. Commit cần file đã stage và thông điệp. Giữ merge, push và giải quyết conflict là các thao tác riêng với review.

## 12. Truyền file

- Mở SFTP độc lập hoặc dùng lại host đã xác thực.
- Chọn nguồn và đích local hoặc remote, đường dẫn và chiều truyền. Hỗ trợ truyền giữa các host. Desktop hiển thị hai đầu cạnh nhau. Màn hình hẹp đi theo các bước chọn nguồn, chọn đích rồi gửi.
- Upload/download file, thư mục và nhiều mục; kéo thả/chọn file theo khả năng thiết bị.
- Duyệt thư mục, nhập đường dẫn, lên thư mục cha, refresh và tạo thư mục đích.
- Tên trùng được xử lý bằng **Replace**, **Skip** hoặc **Keep both**. Có thể áp dụng từng file hoặc cả nhóm theo lựa chọn. Mục chưa có quyết định tiếp tục chờ.
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
- Kiểm tra cổng không hợp lệ/đang dùng, thiếu quyền bind và server từ chối forwarding. Khi cổng bận, báo tiến trình đang chiếm cổng nếu xác định được, gợi ý cổng rảnh hoặc cho nhập cổng khác.
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
- Mời chia sẻ live terminal theo người tham gia hoặc qua liên kết có thời hạn. Liên kết mặc định chỉ cho xem. Vai trò có thể yêu cầu quyền điều khiển chỉ được gửi input sau khi chủ phiên cấp quyền.
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

## 23. Desktop và mobile

- Desktop và mobile dùng cùng cấu trúc host/project/phiên và hỗ trợ truy cập agent, file/editor/diff, transfer, forwarding, vault và cộng tác theo khả năng thiết bị. Chức năng phụ thuộc phần cứng hoặc hệ điều hành phải nêu điều kiện sử dụng.
- Mobile là client cho phiên chạy trên máy tính hoặc host từ xa, không có shell native trên điện thoại. Thanh điều hướng có **Terminal**, **Files**, **Changes**, **Agents** và **More**. Ports/forwarding, transfer, preview, Settings, thêm host và chia sẻ phiên đi qua More. Thanh trên cùng mở cây host, trình chọn pane và launcher.
- Mobile có phím terminal bổ trợ và vùng soạn prompt dài. Vùng soạn hiển thị host và project đích trước khi gửi vào pane đang chọn. Hỗ trợ copy/paste, chọn file/ảnh và voice input khi thiết bị hỗ trợ.
- Khi mobile bị hệ điều hành đình chỉ, giữ trạng thái và reconnect/reattach khi quay lại. Phân biệt tiến trình trên host với các tác vụ cần client hoạt động như truyền file từ thiết bị và forward local.
- Command palette tìm host, project, tab, agent, thao tác và cấu hình. Khi chưa nhập, ưu tiên mục gần đây nếu có. Kết quả hiện host và đường dẫn riêng với tên project, nêu lý do khi lệnh không khả dụng.

## 24. Settings và cập nhật

- **Settings** mở từ menu ứng dụng, command palette, phím tắt hoặc mục cấu hình host. Nút **Host settings** của launcher mở nhóm Terminal theo phạm vi host hiện tại trong cùng hộp Settings. Host tools có phần quản lý cấu hình kết nối đã lưu.
- Tìm theo tên thiết lập, từ khóa hoặc nhóm. Các nhóm gồm giao diện, workspace, terminal, kết nối, agent/phiên, file/transfer, thông báo, phím tắt, bảo mật/vault/sync, history/recordings, cập nhật và chẩn đoán. **System** theo chế độ sáng/tối của hệ điều hành. Font giao diện và terminal có tùy chọn riêng; báo khi thiết bị phải dùng font dự phòng.
- Phạm vi **App** là giá trị chung. Thiết lập hỗ trợ phạm vi **Host** kế thừa giá trị App hoặc cho chọn **Override**; bỏ ghi đè sẽ dùng lại giá trị App. Chỉ rõ giá trị lấy từ đâu và thiết lập nào chỉ áp dụng toàn ứng dụng hoặc theo thiết bị. Khôi phục mặc định theo nhóm và phạm vi đang chọn, giữ tùy chọn ngoài phạm vi đó.
- Nêu thời điểm có hiệu lực: ngay, sau **Apply**, ở phiên mới, sau reconnect hoặc sau restart. Hủy thao tác khi còn chỉnh sửa chờ áp dụng phải giữ hoặc bỏ theo lựa chọn của người dùng. Phím tắt theo nền tảng, cho sửa, khôi phục mặc định và báo tên lệnh bị trùng.
- Cập nhật ứng dụng và phần phụ thuộc trên host là hai thao tác riêng. Với ứng dụng, cho kiểm tra phiên bản, xem ghi chú, tải, thấy tiến độ/lỗi và thử lại. Sau tải, cho hoãn hoặc xác nhận khởi động lại với danh sách phiên và tác vụ bị ảnh hưởng.
- Với host, hiển thị trạng thái, phiên bản, thời điểm kiểm tra, nguồn, yêu cầu quyền và khả năng tương thích cho tmux/Herdr. Lỗi kiểm tra giữ dữ liệu biết gần nhất và khác với chưa cài. Cài/cập nhật chỉ chạy theo yêu cầu ở host được chọn. Phiên đang chạy cần hiện tác động và lựa chọn hoãn hoặc tiếp tục. Hiện log, kiểm tra kết quả và chỉ dẫn cài thủ công khi nguồn hoặc quyền yêu cầu. Khi ứng dụng không quản lý cách cài agent CLI, nêu cách cập nhật phù hợp với nguồn cài hiện tại.
- Chẩn đoán hiện phiên bản, môi trường và thông tin kết nối cần cho hỗ trợ. Lọc địa chỉ, username và secrets khỏi dữ liệu chia sẻ theo mặc định.

## 25. Quy tắc xử lý chung

- Cấu hình đã lưu, kết nối client, tiến trình remote và tác vụ đang chạy có trạng thái riêng.
- Ghi đè/xóa dữ liệu, kết thúc tiến trình, đổi quyền hoặc chạy lệnh nhiều đích phải xác định rõ phạm vi tác động. Khi nhận diện lệnh có tác động lớn trên host production, hiện lệnh và project đích rồi yêu cầu xác nhận bằng tên host.
- Mất mạng/quyền phải giữ dữ liệu chưa lưu và báo nguyên nhân; không báo thao tác lỗi là đã hoàn tất.
- Xem file/diff, đổi project, đóng preview và thu gọn queue không tự chạy lệnh hay sửa dữ liệu host.
- Không tự cấp thêm quyền cho agent, điều phối agent hoặc quản lý Git worktree thay người dùng.

## Phạm vi bản xem trước trong trình duyệt

- Bản xem trước dùng dữ liệu mẫu và mô hình trạng thái trong trình duyệt. Phần terminal và phản hồi agent là mô phỏng. Các trạng thái tray, attach, reconnect và quit minh họa vòng đời ứng dụng native. Workspace, một số thiết lập, layout và liên kết phiên đã xác nhận được lưu bằng bộ nhớ trình duyệt với luồng lưu workspace trên desktop. Việc nạp lại dữ liệu này chỉ khôi phục trạng thái giao diện.
- Add host, kiểm tra fingerprint/preflight, nhập SSH config, khám phá runtime và cài/cập nhật dùng kịch bản mẫu. Biểu mẫu cho thấy các lựa chọn kết nối. Mockup chỉ chuyển một phần trường vào bản ghi host và danh sách SSH config là dữ liệu mẫu. Một số tùy chọn Settings mới có giao diện hoặc lưu giá trị.
- Đọc/ghi file, stage/commit Git, truyền SFTP, forwarding, web preview và cập nhật ứng dụng minh họa thao tác, tiến độ và lỗi với dữ liệu mẫu. Keys/vault mô phỏng mở khóa và trạng thái đồng bộ. Giao diện bảo mật mô tả private key nằm trong vault và được loại khỏi sync. Chia sẻ phiên mô phỏng lời mời, người tham gia, quyền nhập và reconnect. Các luồng này chưa xác minh hoạt động với hệ thống bên ngoài.
- Mobile là giao diện cảm ứng dùng chung dữ liệu mẫu. Thanh **Prototype** nằm ngoài màn hình ứng dụng để đổi thiết bị, nền tảng, theme, kịch bản và đặt lại dữ liệu. `This computer` trong mô phỏng mobile đại diện cho máy tính của người dùng.
