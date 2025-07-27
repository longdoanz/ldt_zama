# AuraPay: Giao thức Thanh toán Kín trên FHEVM

## AuraPay là một giao thức thanh toán phi tập trung cho phép người dùng thực hiện các giao dịch một cách riêng tư trên blockchain. Bằng cách tận dụng sức mạnh của Mã hóa Hoàn toàn Đồng cấu (FHE) thông qua FHEVM, AuraPay đảm bảo rằng cả số dư và số tiền giao dịch của người dùng luôn được mã hóa và bí mật.

## Vấn đề: Sự thiếu riêng tư trên Blockchain
Các blockchain công khai như Ethereum có một đặc tính cốt lõi là sự minh bạch. Mặc dù điều này rất tốt cho việc kiểm toán, nó lại là một thảm họa cho quyền riêng tư tài chính. Bất kỳ ai cũng có thể xem số dư ví của bạn, theo dõi mọi giao dịch bạn thực hiện, và phân tích các hoạt động tài chính của bạn. Sự thiếu riêng tư này có thể dẫn đến:

- Theo dõi không mong muốn: Các đối thủ cạnh tranh, nhà quảng cáo, hoặc kẻ xấu có thể theo dõi thói quen chi tiêu của bạn.

Front-running trong DeFi: Các bot có thể thấy các giao dịch lớn đang chờ xử lý và lợi dụng chúng.

Rủi ro an ninh cá nhân: Các "cá voi" có thể trở thành mục tiêu của các cuộc tấn công lừa đảo hoặc tống tiền.

## Giải pháp: AuraPay - Thanh toán thực sự riêng tư
AuraPay giải quyết vấn đề này bằng cách xây dựng một smart contract trên FHEVM. Công nghệ này cho phép thực hiện các phép toán trực tiếp trên dữ liệu đã được mã hóa.

Với AuraPay, số dư của bạn không còn là một con số công khai. Nó là một bản mã (ciphertext). Khi bạn gửi tiền, số tiền giao dịch cũng được mã hóa. Smart contract có thể xác minh rằng bạn có đủ tiền và thực hiện giao dịch mà không bao giờ cần giải mã bất kỳ thông tin nhạy cảm nào.

## ✨ Tính năng cốt lõi
Số dư được mã hóa: Số dư tài khoản của người dùng được lưu trữ on-chain dưới dạng bản mã euint.

- Giao dịch Kín: Số tiền được chuyển trong mỗi giao dịch hoàn toàn riêng tư.

- Xác minh On-chain: Logic "kiểm tra số dư" được thực hiện hoàn toàn trên dữ liệu mã hóa, đảm bảo tính toàn vẹn mà không làm lộ thông tin.

- Giao diện tương tự ERC20: Cung cấp các hàm quen thuộc như transfer() và balanceOf() (với một chút khác biệt) để dễ dàng tích hợp.

- Chống kiểm duyệt: Vì logic chạy trên một smart contract phi tập trung, không ai có thể ngăn chặn các giao dịch của bạn.

## ⚙️ Cách hoạt động
AuraPay sử dụng các kiểu dữ liệu và toán tử được cung cấp bởi thư viện FHEVM. Luồng hoạt động của một giao dịch diễn ra như sau:

- Mã hóa phía Client: Trước khi gửi giao dịch, người dùng mã hóa số tiền họ muốn chuyển bằng khóa công khai của FHEVM.

- Gọi Smart Contract: Người dùng gọi hàm transfer(address to, bytes memory encryptedAmount) trên contract AuraPay.

- Toán tử Đồng cấu (Homomorphic Operations):

- Smart contract lấy số dư đã mã hóa của người gửi.

Nó thực hiện phép toán FHE.ge(senderBalance, encryptedAmount) để xác minh rằng số dư của người gửi lớn hơn hoặc bằng số tiền chuyển (phép so sánh này trả về một ebool - boolean đã mã hóa).

Nếu xác minh thành công, contract sẽ thực hiện:

FHE.sub(senderBalance, encryptedAmount) để trừ tiền từ người gửi.

FHE.add(receiverBalance, encryptedAmount) để cộng tiền cho người nhận.

Cập nhật Trạng thái: Các số dư đã mã hóa mới được lưu lại on-chain. Toàn bộ quá trình diễn ra mà không một nút mạng, thợ đào, hay người quan sát nào có thể biết được số tiền giao dịch là bao nhiêu.

## 🚀 Các trường hợp sử dụng
Thanh toán P2P cá nhân: Gửi tiền cho bạn bè và gia đình một cách kín đáo.

- Trả lương bí mật: Các công ty có thể trả lương cho nhân viên on-chain mà không để lộ mức lương của bất kỳ ai.

- Đầu tư DeFi ẩn danh: Gửi tiền vào các pool thanh khoản hoặc các giao thức DeFi khác mà không tiết lộ quy mô vị thế của bạn.

- Airdrop riêng tư: Phân phối token cho người dùng mà không để lộ số lượng mỗi người nhận được.

## 🛠️ Bắt đầu
### Điều kiện tiên quyết
- Node.js (v18 trở lên)

- Git

Trình quản lý gói npm hoặc yarn

Cài đặt
Clone repository:

git clone https://github.com/your-username/aurapay.git
cd aurapay

Cài đặt các dependencies:

npm install

Biên dịch contract:

npx hardhat compile

Chạy Tests
Để xác minh mọi thứ hoạt động chính xác, hãy chạy bộ kiểm thử:

npx hardhat test

## Triển khai
Tạo một file .env từ file mẫu .env.example.

Thêm PRIVATE_KEY của ví bạn muốn dùng để triển khai.

Chạy script triển khai (ví dụ cho mạng Fhenix testnet):

```npx hardhat run scripts/deploy.ts --network fhenix```

## 🤝 Đóng góp
Chúng tôi hoan nghênh các đóng góp từ cộng đồng! Vui lòng fork repository, tạo một nhánh mới cho tính năng của bạn, và gửi một Pull Request.

## 📜 Giấy phép
Dự án này được cấp phép theo Giấy phép MIT.

Tuyên bố miễn trừ trách nhiệm: Công nghệ FHE và FHEVM vẫn còn trong giai đoạn thử nghiệm. Hãy tự chịu trách nhiệm khi sử dụng trong môi trường production.