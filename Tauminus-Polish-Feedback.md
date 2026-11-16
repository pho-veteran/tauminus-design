# Tauminus — Góp ý hoàn thiện mockup và bổ sung đặc tả

Mục tiêu: đưa mockup từ bố trí chức năng sang trải nghiệm sản phẩm hoàn chỉnh, có thể bàn giao để thiết kế và triển khai. Giữ terminal và coding agent là trung tâm; hoàn thiện cả hình thức, tương tác và ý nghĩa dữ liệu. Không chia giai đoạn phát hành; toàn bộ phạm vi sản phẩm cần được preview.

Nguồn đối chiếu: [mockup Lovable hiện tại](https://remote-workbench-orbit.lovable.app/), [đặc tả chức năng](./Tauminus-Product-Design.md), yêu cầu và ảnh tham khảo do chủ sản phẩm cung cấp. File này là yêu cầu sửa mockup và đặc tả, không xác nhận các thay đổi đã được triển khai.

## 1. Định hướng chỉnh sửa

- Giữ **Host → Project → Tab**. Project là ngữ cảnh làm việc trên một host; tab là terminal, coding agent hoặc web preview. Không thêm chế độ project-oriented.
- Giữ một hàng tab chung; pane nằm trong tab. Resize không đồng nghĩa bổ sung docking tự do hoặc hàng tab riêng cho từng pane.
- File, editor nhẹ và diff là nội dung theo ngữ cảnh; không đưa chúng vào New tab.
- Dùng bản mẫu cũ để hiểu nghiệp vụ, không coi tỷ lệ, khoảng cách, viền, vị trí toolbar hay hình dạng modal của nó là ràng buộc thiết kế.
- Cho phép thiết kế lại composition, mật độ, typography, elevation, trạng thái và micro-interaction để các màn hình có chất lượng đồng nhất. Tránh chỉ thay màu hoặc bo góc trên bố cục cũ.
- Phong cách cần yên tĩnh, sắc nét, dễ đọc; sự tối giản không được làm mất phân cấp hoặc khiến mọi thành phần đều mờ nhạt. Màu nhấn dành cho focus, lựa chọn và trạng thái có ý nghĩa.
- Desktop và mobile dùng cùng mô hình chức năng, nhưng cách bố trí phải phù hợp thiết bị; không thu nhỏ nguyên màn desktop xuống điện thoại.

## 2. Những điểm đã quan sát trực tiếp

- Add host hiện có một ô SSH address và các gợi ý SSH config, đặt gần phía trên màn hình; thiếu phân cấp và luồng nhập đầy đủ ngay tại điểm vào.
- Command palette lệch sang phải, danh sách lệnh/project nối tiếp nhau không có tiêu đề nhóm; có shortcut `⌘` trong khi đang xem trên Windows.
- Host tools hiện mở như một màn phủ toàn workspace trên desktop.
- Thu sidebar làm cây điều hướng biến mất hoàn toàn, không giữ rail icon.
- Có ký hiệu `?` cạnh project và tab agent; phần nội dung agent cho biết Needs input nhưng ký hiệu trong điều hướng không tự giải thích được.
- File tree đã có icon, thụt lề và ký hiệu Git M/A; vẫn thiếu đường dẫn thị giác rõ để đọc quan hệ cha–con. Cần hoàn thiện cấu trúc, không làm lại từ giả định chưa có tree.
- Thanh trên hiện có nút minimize; chưa đủ ba điều khiển cửa sổ theo yêu cầu. Nút chuyển Prototype Desktop/Mobile nằm trong chrome sản phẩm.
- Mobile đã có composition riêng với các mục Terminal, Files, Changes, Ports và More; cần hoàn thiện các luồng chi tiết thay vì chỉ thay đổi chiều rộng.

Các mục tiếp theo bao gồm cả yêu cầu đã được chủ sản phẩm nêu và đề xuất bổ sung. Resize, toàn bộ trạng thái lỗi, keyboard navigation và mọi màn Host tools chưa được kiểm thử đầy đủ trong lần xem này; không coi các mục chưa kiểm thử là lỗi đã được chứng minh.

## 3. Resize và độ ổn định của workspace

- Cho kéo thay đổi chiều rộng sidebar trái, inspector phải và tỷ lệ giữa các pane ngang/dọc.
- Divider có cursor và hover phù hợp; vùng kéo đủ dễ bắt nhưng nét hiển thị vẫn nhẹ. Có thao tác bàn phím cho separator, không chỉ hỗ trợ chuột.
- Đặt kích thước tối thiểu/tối đa theo nội dung để không làm mất nút đóng, tab đang chọn hoặc nội dung thiết yếu. Khi không đủ chỗ, chuyển composition thay vì ép tất cả cột nhỏ lại.
- Lưu chiều rộng sidebar/inspector theo thiết bị và tỷ lệ pane theo workspace/tab. Có Reset layout; không đồng bộ kích thước desktop nguyên xi sang mobile.
- Khi kéo tới ngưỡng nhỏ nhất, sidebar chuyển sang rail icon; không biến mất và không gây bật/tắt liên tục quanh ngưỡng.
- Resize terminal phải cập nhật viewport terminal. Trong mockup, ít nhất nội dung và scrollbar phải thích nghi, không tràn khỏi pane.
- Phóng to một pane rồi khôi phục phải giữ tỷ lệ cũ, tab đang chọn, nội dung và focus.

**Nghiệm thu:** kéo các divider, thu/giãn cửa sổ và đổi tab không làm mất session, che nút hoặc reset bố cục; Reset layout có kết quả rõ ràng.

## 4. Sidebar: collapse cây và rail icon

- Phân biệt ba thao tác: thu một host, thu một project và thu toàn sidebar. Mỗi thao tác có icon/tooltip riêng, không dùng một nút cho nhiều nghĩa.
- Bổ sung Collapse all trong header Hosts; menu phụ có Expand all và Reveal active tab. Thu cây không đóng hay detach bất kỳ session nào.
- Giữ trạng thái mở/đóng của host và project khi chuyển màn. Khi đi từ Agents hoặc tìm kiếm đến một tab, mở đúng nhánh cần thiết để nhìn thấy tab đó.
- Rail desktop đề xuất rộng khoảng 48–56 px, giữ nút mở rộng sidebar, tìm kiếm, các host thường dùng, Agents, Host tools, Settings và Add host. Chỉ số này là điểm khởi đầu, cần cân chỉnh cùng tổng thể.
- Host đang chọn có dấu nhận diện rõ; host cùng loại vẫn phân biệt được bằng tooltip tên và trạng thái. Với nhiều host, dùng vùng cuộn/overflow thay vì nhồi icon xuống dưới màn hình.
- Chọn icon host trong rail mở flyout cây project/tab của host đó; chọn tab xong giữ rail thu gọn. Có thể ghim mở lại sidebar rộng.
- Badge cần chú ý vẫn hiện khi thu gọn. Tooltip và nhãn cho screen reader không phụ thuộc vào màu hoặc tên viết tắt khó đoán.
- Hosts và Agents là hai vùng có thể thu gọn, có giới hạn chiều cao và cuộn phù hợp. Settings/Add host không bị đẩy mất khỏi vùng thao tác khi danh sách dài.

**Nghiệm thu:** ở trạng thái rail, người dùng vẫn tìm và mở được mọi host/project/tab, biết phiên nào cần phản hồi, mở Settings và thêm host mà không phải đoán icon.

## 5. Danh sách Agents toàn bộ máy

- Đặt section **Agents** dưới cây Hosts, tổng hợp các phiên agent đã được phát hiện trên những host người dùng có quyền truy cập, không giới hạn trong project đang chọn.
- Mỗi dòng gồm icon agent, tên phiên/tác vụ, host/project, trạng thái và thời điểm cập nhật khi cần. Cùng một agent chạy nhiều phiên phải là nhiều dòng riêng.
- Có bộ lọc All active, Needs input và Recent; tìm theo tên phiên, agent, host hoặc project. Các phiên hoàn tất/hủy gần đây vẫn xem được nhưng không lấn át phiên đang hoạt động.
- Ưu tiên Needs input và Failed; sau đó Working và Idle. Giữ thứ tự ổn định trong cùng nhóm, không liên tục nhảy dòng theo từng output.
- Click một dòng mở đúng host/project/tab/pane và focus phiên đó. Nếu chưa attach, thực hiện attach; không khởi chạy thêm một agent mới.
- Khi thu sidebar, icon Agents có số phiên cần chú ý và mở danh sách dạng flyout.
- Không hứa liệt kê mọi tiến trình AI trên máy nếu không có dữ liệu phát hiện. Host offline hiển thị dữ liệu gần nhất kèm stale/disconnected; host chưa hỗ trợ báo phạm vi theo dõi.

| Trạng thái tác vụ | Ý nghĩa cần thể hiện |
|---|---|
| Starting | Đã yêu cầu khởi chạy, chưa xác nhận sẵn sàng. |
| Working | Agent xác nhận đang xử lý. |
| Needs input | Có câu hỏi, yêu cầu quyền hoặc tương tác đang chờ. |
| Idle | Phiên còn hoạt động nhưng đang chờ tác vụ mới; không suy ra chỉ vì output im lặng. |
| Completed | Tác vụ gần nhất đã hoàn tất; không nhất thiết tiến trình agent đã thoát. |
| Cancelled | Tác vụ đã được xác nhận hủy; khác với gửi yêu cầu hủy đang chờ. |
| Failed | Có lỗi được xác nhận, kèm nguyên nhân và hành động phù hợp. |
| Unknown | Không có dữ liệu đủ tin cậy để kết luận. |

- Trạng thái kết nối Connected / Reconnecting / Disconnected và độ mới dữ liệu là thông tin riêng, không thay thế trạng thái tác vụ.
- Dùng cùng từ vựng và icon ở Agents, cây project, tab, pane và thông báo. Tổng số Needs input phải khớp giữa các nơi.
- Không suy luận Completed từ việc đóng tab, Cancelled từ mất mạng, hoặc Working chỉ từ việc tiến trình tồn tại.

**Nghiệm thu:** một phiên được biểu diễn nhất quán ở mọi nơi; đi tới phiên không nhân bản tiến trình; mất kết nối không làm phiên biến mất hoặc được báo hoàn tất giả.

## 6. Nguồn project/session và phát hiện tmux/Herdr

- Hiểu yêu cầu “masking” là **đánh dấu nguồn và runtime**, không phải che tên project. Tách nguồn project như Added manually / Discovered via Herdr khỏi runtime của từng session như Plain terminal / tmux / Herdr Core.
- Project có thể chứa nhiều loại session. Không gán toàn bộ project là tmux chỉ vì một terminal của project dùng tmux.
- Dùng badge/icon nhỏ có tooltip trên project/session, ví dụ `Discovered via Herdr` ở project và `tmux · session: build` ở tab. Nhãn chi tiết xuất hiện khi xem thông tin, tránh phủ nhiều badge lên mọi dòng.
- Preflight và Rescan sessions phát hiện runtime, phiên bản và session có thể truy cập. Hiển thị nguồn, session ID/tên, trạng thái attach, working directory của từng pane khi có dữ liệu.
- Đối chiếu trong **cùng host** bằng đường dẫn đã chuẩn hóa theo hệ điều hành; không gộp theo tên project hoặc tên repository giữa hai host.
- CWD trùng project root: đề xuất gắn session vào project đó. CWD nằm trong project: chọn root khớp sâu nhất theo ranh giới thư mục, không so khớp prefix chuỗi đơn giản.
- Tôn trọng mapping người dùng đã xác nhận. CWD của shell thay đổi sau một lệnh `cd` không tự di chuyển tab sang project khác.
- Với session tmux chỉ có một ngữ cảnh project rõ ràng, attach thành tab terminal có badge tmux trong project tương ứng; bảo toàn cấu trúc window/pane của session khi runtime cho phép.
- Với tmux nhiều window/pane thuộc nhiều project hoặc không xác định được CWD, đặt trong nhóm **Discovered sessions** dưới host và cho Assign to project. Đây là nhóm phiên chưa gán, không phải repository giả tên “tmux”.
- Có thể tạo project từ đường dẫn được phát hiện sau khi người dùng xem/xác nhận. Không tự tạo project rỗng chỉ vì runtime đã được cài.
- Dùng định danh host/runtime/session ổn định để tránh hiển thị trùng qua nhiều nguồn. Tên phiên đổi không được tạo thêm một bản sao; attach lại không chạy lại tác vụ.
- Với Plain terminal, ghi đúng khả năng tồn tại và reconnect; không ngụ ý persistence như tmux/Herdr.
- Demo đủ bốn trường hợp: project từ Herdr, tmux khớp project, tmux chưa gán hoặc nhiều CWD, terminal thường trong project; thêm trường hợp không đọc được metadata.

**Nghiệm thu:** nhìn cây biết project/session đến từ đâu; phiên khớp path xuất hiện đúng project; phiên mơ hồ không bị gắn sai và vẫn có đường attach thủ công.

## 7. Entry modal và luồng Add host

- Trong bản góp ý này, **entry modal được đề xuất là điểm bắt đầu workspace**, không đồng nghĩa Add host. Hiện khi chưa có workspace hoặc lần mở đầu; có mục Open start screen để mở lại và tùy chọn không hiện lại.
- Entry cung cấp bốn đường rõ: Open local terminal, Connect host, Open project và Resume recent session/workspace. Không yêu cầu tài khoản hoặc SSH để dùng local terminal.
- Mỗi đường có mô tả ngắn, icon và thông tin gần đây vừa đủ; không biến thành dashboard nhiều card thống kê. Mở app lần sau ưu tiên workspace trước nếu người dùng đã bật khôi phục.
- Add host cần header, mô tả mục đích, nội dung có nhóm, footer hành động và feedback; không chỉ một input đứng riêng trong hộp trống.
- Giữ Quick connect bằng `user@host`, đồng thời có form đầy đủ: tên, hostname/IP, port, username, phương thức xác thực và identity. Giá trị phân tích từ Quick connect phải xem/sửa được.
- Tách rõ Create connection và Import from SSH config. SSH config gợi ý cần thể hiện alias và đích kết nối, không chỉ các chữ rời cạnh nhau.
- Advanced chứa jump host/proxy, keepalive, timeout và tùy chọn liên quan; thông tin cốt lõi không bị giấu trong Advanced.
- Có Save host khi chưa thể kết nối và Connect & check environment. Validation tại đúng trường, không dùng toast chung cho mọi lỗi.
- Luồng đầy đủ: Connection → Verify identity/fingerprint khi cần → Preflight → Choose environment → Ready. Hiển thị tiến độ và cho quay lại mà giữ dữ liệu.
- Preflight hiển thị shell/agent/runtime đã phát hiện, trạng thái không hỗ trợ hoặc kiểm tra lỗi. Chọn default shell riêng cho host; không giả định máy nào cũng có bash hoặc pwsh.
- Runtime cho chọn Reuse installed, Install tmux, Install Herdr Core hoặc Continue without persistence; cài đặt chỉ bắt đầu từ thao tác chủ động và có kết quả kiểm tra lại.
- Thiết kế đầy đủ kết nối thành công, host offline, auth failure, fingerprint thay đổi, quyền cài thiếu, cài lỗi và retry. Giữ dữ liệu nhập để sửa tiếp.

**Nghiệm thu:** từ màn trống có thể vào local terminal hoặc hoàn thành một luồng host có preflight; không có bước cụt, mất dữ liệu hoặc báo “Ready” dù kiểm tra thất bại.

## 8. Hệ modal và Host tools

- Tất cả modal desktop căn giữa theo cả hai trục trong viewport ứng dụng, không căn theo một pane hoặc bị lệch bởi chiều rộng sidebar. Command palette cũng tuân theo yêu cầu này.
- Dùng kích thước theo nhiệm vụ: xác nhận nhỏ, form vừa, Host tools/Settings lớn. Có max-height và khoảng cách an toàn với mép cửa sổ; nội dung dài cuộn bên trong, header/footer vẫn truy cập được.
- Với màn hình hẹp hoặc chiều cao không đủ, chuyển thành màn full-screen có Back/Close và safe area. Trên mobile, bàn phím mở không được che input hay nút tiếp tục.
- Overlay, border, shadow, padding, tiêu đề, mô tả, nút đóng và footer phải thống nhất. Tránh mọi modal có cùng một hộp nhỏ bất kể nội dung.
- Focus vào vùng phù hợp khi mở, giữ focus trong modal, trả về nơi gọi khi đóng; hỗ trợ Escape khi an toàn. Nội dung chưa lưu có xử lý rõ khi đóng.
- Menu/popover được neo vào nút gọi, không bị áp quy tắc căn giữa của modal. Phân biệt menu, dialog và full-screen route trong thiết kế.
- Host tools trên desktop mở trong modal lớn, giữ workspace phía sau; có navigation Hosts, SFTP, Keys & vaults, Tunnels, Terminal tools và selector host rõ ràng.
- Trên màn nhỏ, Host tools là full-screen với back stack hợp lý. Đóng công cụ phải quay lại đúng tab, pane và vị trí đang làm việc.
- Khi cần Add/Edit từ Host tools, ưu tiên chuyển nội dung hoặc bước trong cùng dialog; tránh chồng nhiều modal và nhiều lớp backdrop.
- Đóng Host tools không dừng transfer, tunnel hoặc agent. Hành động đang chạy vẫn có nơi theo dõi và mở lại.

**Nghiệm thu:** mở Add host, Ctrl+K, Host tools, Settings và dialog xác nhận ở sidebar rộng/rail đều căn đúng; màn nhỏ không cắt nội dung hoặc làm mất đường quay lại.

## 9. Command palette và New tab

- Ctrl+K chia nhóm có tiêu đề: Recent, Navigate, Terminal & agents, Project & sessions, Host tools, Settings. Khi có tìm kiếm, kết quả vẫn giữ ngữ cảnh nhóm và sắp xếp theo độ phù hợp.
- Mỗi dòng có icon, tên hành động, host/project đích nếu cần và shortcut. Không trộn địa chỉ project vào tên lệnh dài khó quét.
- Shortcut phải đúng nền tảng: Ctrl trên Windows/Linux, Command trên macOS. Nhãn hiển thị và thao tác thực tế phải khớp.
- Hỗ trợ điều hướng bằng Up/Down, Enter, Escape; thể hiện selection rõ; empty result có hướng dẫn cụ thể. Lệnh không khả dụng phải có lý do, không giả vờ thực thi thành công.
- New tab tiếp tục là menu nhanh, không thành command palette thứ hai. Liệt kê ngay các shell và coding agent được preflight phát hiện trên host hiện tại, không submenu terminal.
- Chia nhóm ngắn Terminal / Coding agents / Preview & settings; shell mặc định được nhận biết. Agent chưa cài có trạng thái và đường đến cài đặt phù hợp, không chạy giả như đã có.
- Settings từ launcher mở đúng nhóm cấu hình và giữ host context. Không tạo thêm một bản Settings không đồng bộ với phần Settings chung.
- Không thêm Open file hoặc Open diff vào New tab; vẫn có thể truy cập file từ file tree hoặc lệnh điều hướng phù hợp.

**Nghiệm thu:** người mới tìm được đúng thao tác qua nhóm; người quen mở terminal/agent bằng bàn phím mà không qua nhiều tầng menu.

## 10. File tree, editor nhẹ và Changes

- Thể hiện cấu trúc bằng chevron expand/collapse nhất quán, indent theo cấp, đường guide tinh tế và icon thư mục mở/đóng. Không chỉ xếp chữ thụt lề.
- Chọn dòng, hover và keyboard focus phải khác nhau; file đang xem có trạng thái rõ và có Reveal in tree.
- Icon file theo loại, nét và kích thước đồng nhất; có thể dùng màu tiết chế cho loại file nhưng không tạo một cột màu gây nhiễu.
- Giữ Git badge M/A/D/R/conflict, có tooltip chữ đầy đủ; trạng thái thư mục có thay đổi được tổng hợp vừa đủ. Không chỉ dựa vào màu để đọc thay đổi.
- Header tree cho biết project và root/path, có refresh, collapse all, search/filter và toggle hidden files. Action ít dùng nằm trong context menu.
- Tên dài dùng ellipsis và tooltip đầy đủ; status badge không đẩy tên biến mất. Thư mục lớn có cuộn, loading, empty, permission denied và retry riêng.
- Editor/diff có breadcrumb, syntax highlight, trạng thái chưa lưu/read-only, lỗi lưu, thao tác đóng/quay lại dễ thấy. Mở rồi đóng phải trả lại terminal đang làm việc.
- Diff có phân biệt added/deleted, số dòng, chọn file thay đổi và bố cục thích nghi khi hẹp; tránh chỉ tô nguyên khối màu không đọc được code.

**Nghiệm thu:** với tree 4–5 cấp và nhiều tên dài vẫn đọc được quan hệ cha–con; file chọn và thay đổi Git rõ ở cả Light/Dark; review không sinh tab tài liệu.

## 11. Settings: cấu trúc và chức năng cần preview

- Có entry Settings cố định ở cuối sidebar và trong rail; truy cập thêm từ command palette. Desktop dùng dialog lớn, mobile dùng màn riêng có điều hướng quay lại.
- Settings có tìm kiếm, nhóm rõ, mô tả ngắn và reset theo nhóm. Tách **App/This device**, **Host: tên máy** và các giá trị inherited; không để người dùng sửa nhầm cấu hình toàn app khi chỉ muốn đổi một host.
- Tùy chọn tức thời có preview; cấu hình cần Save/Apply phải hiển thị dirty state. Nếu cần reconnect/restart, giải thích trước khi áp dụng. Không có Save cho hình thức nhưng bấm không đổi gì.

| Nhóm | Nội dung phải thiết kế và mô phỏng |
|---|---|
| Appearance & preferences | Theme System / Dark / Light; System theo thay đổi của OS. Font giao diện, cỡ chữ, mật độ hiển thị, giảm chuyển động, ngôn ngữ. Preview trực tiếp và reset. Font UI tách khỏi terminal/code. |
| Workspace & navigation | Restore workspace, hiện entry khi mở app, kích thước/khôi phục layout, trạng thái cây, cách hiển thị rail và Agents, hành vi đóng cửa sổ/minimize to tray. |
| Terminal | Font monospace, cỡ chữ, line-height, ligatures, bảng màu, cursor, scrollback, copy/paste, bell và shell integration; default shell riêng từng host. |
| Connections & hosts | Mặc định kết nối, reconnect, timeout, keepalive, jump host/proxy, identity kế thừa; nút đến cấu hình host cụ thể và chạy lại preflight. |
| Agents & sessions | Agent CLI phát hiện được, executable/launch arguments, runtime mặc định theo host, rescan, attach/restore preferences, nguồn trạng thái và thông báo; không thêm điều phối agent tự động. |
| Files & transfers | Hidden files, hành vi mở/preview, editor preferences, xác nhận file trùng, thư mục đích mặc định, số transfer đồng thời và lịch sử queue. |
| Notifications | Needs input, completed, failed, transfer, connection; âm thanh, OS notifications, mute theo host/session và quiet mode. |
| Keyboard shortcuts | Tìm lệnh, xem/sửa shortcut, phát hiện trùng, reset; phân biệt app shortcut và phím phải chuyển vào terminal. |
| Security, vault & sync | Auto-lock, identities/known hosts, nguồn secrets, phạm vi sync, trạng thái sync và conflict; dẫn tới công cụ quản lý tương ứng, không nhân bản form khác nghĩa. |
| History & recordings | Bật/tắt lưu, retention, nơi lưu khi được hỗ trợ, export và xóa có xác định phạm vi. |
| Updates & dependencies | Phiên bản app, kiểm tra cập nhật, release notes, tải/cài/restart; inventory và cập nhật runtime theo từng host như mô tả bên dưới. |
| About & diagnostics | Phiên bản/build, môi trường, liên kết hỗ trợ; xem/copy diagnostics đã loại bí mật. Không tự gửi log. |

- Light mode phải thiết kế đầy đủ surface, border, text, selection, trạng thái, overlay, editor và terminal palette; không chỉ đảo màu nền hoặc thay một biến theme.
- Preferences có thể được đồng bộ nếu phù hợp; font cài trên thiết bị, layout, shell path và runtime trên host có phạm vi riêng. Font không có trên thiết bị khác dùng fallback và báo rõ.

**Nghiệm thu:** Settings là luồng hoạt động được, không phải danh sách placeholder; đổi theme/font nhìn thấy ngay, reset khôi phục đúng, đổi default shell chỉ áp dụng phiên mới của host đã chọn.

## 12. Updates và dependency trên host

- Tách **Application update** trên thiết bị đang dùng khỏi **Host dependencies**. Không dùng một nút Update all không rõ sẽ thay đổi máy nào.
- App update có current/latest version, release notes, Check for updates, download progress, Ready to restart, up-to-date, offline và failed/retry.
- Cho trì hoãn restart; trước khi restart hiển thị tác vụ client bị ảnh hưởng như local terminal, transfer và forwarding. Không nói mọi thứ đều an toàn chỉ vì remote session có tmux.
- Host dependencies chọn host trước, liệt kê tmux/Herdr Core: installed version, phiên bản có thể cập nhật khi nguồn hỗ trợ, compatibility, installation source và quyền cần có.
- Dùng trạng thái Not installed, Up to date, Update available, Unsupported, Unknown và Check failed. Không suy ra thiếu runtime khi kiểm tra lỗi.
- Preflight phát hiện và Settings quản lý cùng một inventory. Rescan hoặc cập nhật xong phải cập nhật launcher và session capabilities, không để mỗi màn giữ một kết quả khác nhau.
- Cài/nâng cấp chỉ sau lựa chọn cụ thể của người dùng, qua cách phù hợp OS và nguồn cài đã biết. Không tự thay bản cài thủ công bằng một cơ chế khác.
- tmux/Herdr đang có session: hiển thị tác động dự kiến và cho hoãn; không tự kill server hoặc restart runtime để hoàn thành update. Không cam kết cập nhật nóng khi chưa xác minh.
- Hiển thị tiến trình, log đủ chẩn đoán, kết quả xác minh phiên bản và retry. Khi không có quyền/không hỗ trợ, cung cấp hướng dẫn phù hợp; không làm nút Update trông như đã khả dụng.
- Không hứa rollback mọi dependency. Chỉ đưa hành động khôi phục khi cách cài thực sự hỗ trợ.
- Agent CLI được liệt kê với trạng thái và cách quản lý; chỉ có Update trực tiếp khi app nhận diện được cơ chế cập nhật an toàn. Các công cụ khác có Open instructions.

**Nghiệm thu:** mô phỏng thành công, không có update, host offline, thiếu quyền, đang có session và update lỗi; mọi thao tác chỉ rõ thiết bị/host đích.

## 13. Icons, typography và hoàn thiện hình thức

- Thay ký tự placeholder `?`, ký hiệu giả nút hoặc emoji chắp vá trong chrome của app bằng icon SVG từ một hệ thống nhất quán. `?` hiện biểu thị Needs input nên dùng icon thông báo/yêu cầu tương tác kèm tooltip, không đổi thành icon trợ giúp chung.
- Phân biệt application chrome với output terminal: dấu `$`, `>`, dấu hỏi trong câu và ký tự do CLI render vẫn giữ nguyên. Không “làm đẹp” bằng cách sửa nội dung terminal thực.
- Dùng icon có nhận diện riêng cho shell, agent và web preview; logo agent chính xác nếu dùng. Đồng nhất optical size, baseline và độ dày nét, không trộn nhiều phong cách.
- Chuẩn hóa type scale, chiều cao dòng, padding, khoảng cách giữa nhóm, radius và chiều cao control. Nút, input, select, tab và menu phải thuộc cùng một hệ.
- Làm rõ hierarchy giữa host, project, tab; tiêu đề, metadata và trạng thái không cùng một độ đậm/độ mờ. Tên và nội dung chính phải đọc thoải mái, nhất là Light mode.
- Active tab, selected tree item, focused pane và hover dùng dấu hiệu khác nhau nhưng phối hợp được. Border/outline của mọi pane không cùng sáng gây tranh focus.
- Rút bớt header và thông tin lặp lại giữa breadcrumb, tab, pane và status bar; vẫn luôn biết host đích khi nhập lệnh.
- Toolbar ưu tiên thao tác thường dùng, chuyển thao tác hiếm vào overflow có nhãn. Icon button có tooltip, accessible name và vùng bấm thoải mái.
- Hoàn thiện hover, pressed, focus-visible, disabled, loading, success và error. Disabled vẫn đọc được và có lý do khi cần; spinner không làm nút đổi kích thước.
- Animation ngắn, có mục đích cho collapse, modal, resize/focus; tôn trọng Reduced motion. Không thêm hiệu ứng nhấp nháy để tạo cảm giác agent “đang hoạt động”.
- Empty state phải gắn với hành động tiếp theo: no hosts → Add host; no project → Open folder; no session → New terminal/Attach; no changes → trạng thái sạch; lỗi truy cập → Retry hoặc sửa quyền.
- Đưa bộ điều khiển demo Desktop/Mobile và chọn scenario ra ngoài chrome sản phẩm hoặc vào chế độ preview riêng. Bản trình bày sạch không để thanh Prototype hay công cụ biên tập che action của app.

**Nghiệm thu:** xem toàn bộ màn và modal cùng lúc thấy một sản phẩm thống nhất; không còn ký tự giả icon trong chrome, thành phần mờ khó đọc hoặc màn phụ có độ hoàn thiện thấp hơn workspace.

## 14. Điều khiển cửa sổ và vòng đời

- Mô phỏng đủ ba nút góc phải trên desktop: Minimize, Maximize/Restore, Close; có hover/pressed/tooltip và vùng click riêng. Close có trạng thái hover phù hợp nền tảng.
- Maximize/Restore thay đổi khung preview để thấy được bố cục đáp ứng. Minimize và Close có kết quả mô phỏng rõ, có cách mở lại; không gọi đóng tab trình duyệt thật.
- Phân biệt Minimize xuống taskbar với hành vi Close to tray đã chọn cho sản phẩm. Tooltip/nội dung hướng dẫn không gọi cả hai hành vi là “minimize” một cách mơ hồ.
- Trong mock, Close to tray hiển thị trạng thái app đang chạy nền và affordance Restore; Quit app là hành động riêng.
- Trên mobile không hiển thị bộ điều khiển cửa sổ desktop. Khi preview nền tảng khác, dùng chrome/shortcut phù hợp nền tảng đó.

**Nghiệm thu:** cả ba nút có tác dụng mô phỏng và đường quay lại; đóng cửa sổ không bị thể hiện thành việc hủy mọi remote session.

## 15. Hoàn thiện các luồng phụ và mobile

- SFTP: browser nguồn/đích, đường dẫn, chọn file, upload/download và xử lý conflict là vùng chính. Transfer queue mặc định thu gọn, có tóm tắt tiến độ/lỗi; thêm job không tự bung queue.
- Host tools modal vẫn đủ chỗ cho truyền file hai phía trên desktop. Mobile chọn nguồn/đích theo từng bước; không ép hai bảng nhỏ cạnh nhau.
- Ports/tunnels: phân biệt cấu hình đã lưu, tunnel đang chạy và dịch vụ đích có phản hồi; demo port conflict, restart, copy URL và mở preview. Đóng preview không dừng tunnel dùng chung.
- Keys/vaults: có trạng thái locked/unlocked, chọn identity, empty/error; dữ liệu bí mật trong mock là dữ liệu giả và được che đúng chỗ.
- Collaboration: preview mời người xem, yêu cầu/cấp/thu hồi điều khiển, mất kết nối và kết thúc chia sẻ; không chỉ một nút Share mở toast.
- Mobile: Agents toàn máy, Settings, entry, preflight, runtime/session discovery, transfers và Host tools đều có đường truy cập. Sidebar desktop trở thành điều hướng phù hợp touch, không giữ rail icon chật chỉ để giống desktop.
- Touch target đủ lớn, không có chức năng chỉ xuất hiện khi hover; xử lý safe area, màn ngang, bàn phím ảo và nội dung dài.
- Giữ nội dung prompt/dữ liệu chưa lưu khi đổi view hoặc mở dialog. Quay lại đúng ngữ cảnh, không reset session để minh họa màn tiếp theo.

**Nghiệm thu:** các luồng chính và phụ đều đi tới kết quả hoặc trạng thái lỗi có đường xử lý; không có control trông hoạt động nhưng không phản hồi.

## 16. Các bổ sung cần đưa vào đặc tả chức năng

Khi sửa `Tauminus-Product-Design.md`, giữ tài liệu đó thuần chức năng/nghiệp vụ. Chi tiết màu, khoảng cách, icon size và composition ở mockup; chỉ đồng bộ hành vi, đối tượng và quy tắc sau:

| Phần hiện có | Nội dung cần bổ sung/làm rõ |
|---|---|
| 1. Đối tượng và quan hệ | Project discovery source khác session runtime; session có định danh ổn định, association tới project và trạng thái kết nối riêng. Discovered sessions là nhóm chưa gán, không phải project bắt buộc. |
| 2, 5, 6. Hosts, projects, workspace | Thu cây/rail không đóng phiên; lưu trạng thái navigation/layout theo phạm vi; Reveal active tab; entry và restore; giữ một hàng tab và split trong tab. |
| 4, 9. Preflight và persistence | Quy tắc match CWD cùng host, trường hợp nhiều project/không có metadata, xác nhận mapping, attach không nhân bản; rescan dùng cùng inventory. |
| 8, 18. Agents và hoạt động | Agents tổng hợp nhiều host; định nghĩa Starting/Working/Needs input/Idle/Completed/Cancelled/Failed/Unknown; freshness và kết nối riêng; không suy luận từ output im lặng. |
| 10, 11. Files và changes | Reveal in tree, tìm/lọc/hidden files, giữ selection/ngữ cảnh, trạng thái đọc/lưu lỗi; file/diff không thành tab trong New tab. |
| 12–15. Transfer, queue, tunnels, preview | Đóng dialog không dừng tác vụ; queue thu gọn không bị tự mở; giữ phân biệt tunnel/service/client và tính liên tục khi chuyển view. |
| 19–22. Keys, vault, cộng tác, import | Giữ đầy đủ phạm vi đã có; bổ sung đường vào từ Host tools/Settings và hành vi quay lại/giữ dữ liệu, tránh form trùng nhưng kết quả khác nhau. |
| 23. Thiết bị và preferences | Phân nhóm Settings, theme System/Dark/Light, font UI khác terminal, phạm vi global/device/host, reset, save/preview, keyboard shortcuts, accessibility và reduced motion. |
| 23, 9. Updates và app lifecycle | App update khác dependency update theo host; quyền/nguồn cài/compatibility, phiên đang chạy, defer/retry/verify; Minimize, Close to tray và Quit là ba nghĩa riêng. |
| 24. Quy tắc chung | Modal không thay đổi vòng đời tác vụ; trạng thái phải có dữ liệu xác nhận; mọi thao tác xác định đúng host và phạm vi, giữ nội dung chưa lưu khi thất bại. |

- Không xóa chức năng cũ chỉ vì mockup chưa có màn đẹp để biểu diễn. Chọn cách trình bày phù hợp và bổ sung trạng thái tương ứng.
- Không ghi các góp ý này như chức năng đã triển khai. Đặc tả là hành vi yêu cầu; mockup chỉ chứng minh tương tác mô phỏng.
- Không thêm ADR, lịch sử ngày tháng, phân kỳ phát hành hoặc giải thích kiến trúc triển khai vào đặc tả chức năng.

## 17. Bộ cảnh và tiêu chí bàn giao

- Workspace có nhiều host/project, tên dài, nhiều tab và 2–4 pane; sidebar rộng, rail, cây thu gọn, Agents có nhiều trạng thái.
- Entry lần đầu, restore lần sau, local-only và chưa có project/session.
- Add host từ đầu tới preflight, chọn shell/runtime, thành công và các lỗi; project/session Herdr, tmux khớp path và tmux chưa gán.
- Ctrl+K có nhóm, tìm được/không được, keyboard navigation; New tab hiện ngay shells/agents theo host.
- Host tools và Settings dạng modal trên desktop, dạng full-screen trên màn nhỏ; đóng/quay lại giữ workspace.
- Settings Light/Dark/System, font preview/reset, cấu hình per-host, app update và dependency update với trạng thái lỗi/đang có session.
- File tree nhiều cấp, tên dài, Git changes, editor chưa lưu/lưu lỗi và diff; transfer queue đóng/mở; tunnel/preview và collaboration có kết quả mô phỏng.
- Window controls, tray/restore và dữ liệu tác vụ sau khi mở lại.
- Kiểm tra tối thiểu desktop 1440×900, laptop 1280×720, tablet 768×1024, mobile 390×844 và màn hẹp 320 px; kiểm tra thêm zoom chữ/UI và bàn phím ảo. Kích thước này là điểm kiểm tra, không phải các layout cứng.
- Duyệt cả Light và Dark cho mọi thành phần dùng chung, không chỉ workspace. Kiểm tra focus, tooltip, disabled, loading, empty và error.
- Bản mock cuối dùng dữ liệu nhất quán giữa sidebar, Agents, tab, modal, inspector và status bar. Scenario demo có thể reset, nhưng thao tác bình thường không tự reset dữ liệu.
- Người nhận bàn giao cung cấp mockup cập nhật, đặc tả chức năng đã đồng bộ và kết quả kiểm tra các cảnh trên. Mục chưa hỗ trợ phải có mô tả/hành vi rõ, không để placeholder giả hoàn chỉnh.

Hoàn thành khi người dùng có thể khám phá và thao tác liên tục như một sản phẩm được thiết kế có chủ đích: hiểu mình đang ở đâu, hành động tác động lên máy/phiên nào, điều gì đang diễn ra và bước tiếp theo là gì; đồng thời mọi màn có chất lượng hình thức đồng đều, không bị gò theo layout mẫu ban đầu.
