<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Thông Tin Khách Hàng Thang Máy</title>
    <!-- Định dạng mã hóa, viewport và tiêu đề trang -->
    <link rel="stylesheet" href="https://code.jquery.com/ui/1.12.1/themes/base/jquery-ui.css">
    <!-- Thêm jQuery UI CSS cho autocomplete -->
    <style>
        body { 
            font-family: Arial, sans-serif; 
            margin: 20px; 
            padding: 0; 
            background-color: #f5f5f5;
        }
        h1, h2, h3 { 
            text-align: center; 
            color: #333;
        }
        table { 
            width: 100%; 
            border-collapse: separate;
            border-spacing: 0; 
            margin-bottom: 20px; 
            background-color: #fff; 
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
            border-radius: 8px;
            overflow: hidden;
        }
        th, td { 
            border: 1px solid #ddd; 
            padding: 10px;
            text-align: left; 
            vertical-align: middle;
        }
        th { 
            background-color: #333;
            color: #fff;
            font-weight: bold; 
        }
        .highlight { 
            background-color: #fde7e7;
            border-radius: 4px;
            padding: 5px;
            box-sizing: border-box;
        }
        .highlight-yellow { 
            background-color: #ffffcc;
            border-radius: 4px;
            padding: 5px;
            box-sizing: border-box;
        }
        .section-title { 
            font-weight: bold; 
            background-color: #444;
            color: #fff; 
            border-radius: 4px;
        }
        select, input[type="text"], input[type="number"] { 
            padding: 6px; 
            margin-left: 5px; 
            border: 1px solid #ccc; 
            border-radius: 4px;
            box-shadow: inset 0 1px 2px rgba(0,0,0,0.05);
            font-size: 14px;
        }
        input[type="number"] {
            width: 70px;
            text-align: center;
        }
        select.number-select {
            text-align: center;
            text-align-last: center;
        }
        .note { 
            font-style: italic; 
            color: #ff0512; 
            padding: 15px; 
        }
        .multi-column { 
            display: flex; 
            flex-wrap: nowrap;
            gap: 8px;
        }
        .multi-column div { 
            display: flex; 
            align-items: center;
            margin-right: 5px; 
        }
        .multi-column div input { 
            width: 60px;
        }
        .inline-label { 
            display: flex; 
            align-items: center; 
            white-space: nowrap; 
            gap: 5px; 
        }
        .inline-label select, .inline-label input { 
            margin-left: 5px; 
            margin-right: 15px; 
        }
        .wide-select { 
            width: 180px;
        }
        .extra-wide-select { 
            width: 600px;
        }
        .medium-wide-select { 
            width: 280px;
        }
        .full-width-select { 
            width: 98%; 
            box-sizing: border-box;
        }
        .no-col { 
            width: 5%;
        }
        .item-col { 
            width: 35%;
        }
        .content-col { 
            width: 60%;
        }
        .size-content-col { 
            width: 42%; 
            white-space: nowrap; 
            overflow: hidden; 
            text-overflow: ellipsis; 
        }
        .size-input-col { 
            width: 42%; 
        }
        .unit-col { 
            width: 8%; 
        }
        .full-width { 
            width: 100%;
        }
        .full-width-input { 
            width: 100%; 
            box-sizing: border-box;
        }
        .extended-input { 
            width: 100%; 
            max-width: 380px;
        }
        .aligned-input { 
            width: 100%;
            max-width: none;
        }
        .quantity-select { 
            width: 70px;
            text-align: center;
        }
        .material-label { 
            min-width: 80px;
        }
        .ui-autocomplete {
            max-height: 200px;
            overflow-y: auto;
            overflow-x: hidden;
            z-index: 1000;
            background-color: #fff;
            border: 1px solid #ccc;
            border-radius: 4px;
        }
        .ui-autocomplete .ui-menu-item {
            padding: 8px;
            cursor: pointer;
        }
        .ui-autocomplete .ui-menu-item:hover {
            background-color: #f0f0f0;
        }
        .preview-container {
            position: absolute;
            display: none;
            background-color: #fff;
            border: 1px solid #ccc;
            box-shadow: 0 2px 4px rgba(0,0,0,0.2);
            z-index: 1000;
            padding: 10px;
            border-radius: 4px;
        }
        .preview-container img {
            max-width: 200px;
            max-height: 200px;
        }
        .action-buttons {
            text-align: center;
            margin: 40px 0;
        }
        .action-buttons button {
            padding: 10px 20px;
            margin: 0 10px;
            background-color: #3641d4;
            color: #fff;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
        }
        .action-buttons button:hover {
            background-color: #555;
        }
        @media print {
            .action-buttons, .preview-container {
                display: none;
            }
            body {
                margin: 0;
                padding: 10mm;
            }
            table {
                page-break-inside: auto;
            }
            tr {
                page-break-inside: avoid;
                page-break-after: auto;
            }
        }
        @media (max-width: 600px) {
            .multi-column { 
                flex-wrap: wrap; 
            }
            .multi-column div { 
                margin-bottom: 10px; 
            }
            .wide-select, .extra-wide-select, .medium-wide-select, .full-width-select, .aligned-input, input[type="number"] { 
                width: 100%; 
                max-width: none; 
            }
        }
    </style>
</head>
<body>
    <form id="elevator-form">
        <h1>THÔNG TIN KHÁCH HÀNG THANG MÁY</h1>
        <p style="text-align: center;">Ngày <span id="day"></span> Tháng <span id="month"></span> Năm <span id="year"></span></p>
        <div class="action-buttons">
            <button type="button" id="print-form">In biểu mẫu</button>
        </div>
        <div style="text-align: left; margin-bottom: 20px;">
            <p>Tên Khách hàng: <input type="text" class="highlight full-width" required></p>
            <p>Tên Công trình: <input type="text" class="highlight full-width" required></p>
            <p>Địa chỉ lắp đặt: <input type="text" id="address" class="highlight full-width" placeholder="Nhập địa chỉ..." required></p>
        </div>

        <table>
            <tr>
                <th style="width: 20%;">Chuyên Viên Kinh doanh</th>
                <td style="width: 30%;">
                    <select class="extra-wide-select">
                        <option>Vui lòng chọn</option>
                        <option>Lê Hải Lưu</option>
                        <option>Đỗ Việt Anh</option>
                        <option>Lê Đức Anh</option>
                        <option>Mai Quý Dương</option>
                        <option>Lê Thành Trung</option>
                        <option>Nguyễn Duy Quyết</option>
                        <option>Vũ Thanh Quang</option>
                        <option>Hoàng Văn Tuấn</option>
                        <option>Quách Phúc Hải</option>
                        <option>Vũ Hoài Nam</option>
                        <option>Nguyễn Thị Thanh Tâm</option>
                        <option>Nguyễn Đức Tuyển</option>
                        <option>Trần Trọng Đạt</option>
                        <option>Nguyễn Chí Hạnh</option>
                        <option>Huỳnh Xuân Thành</option>
                        <option>Nguyễn Huy Lai</option>
                        <option>Nguyễn Thị Phi Yến</option>
                        <option>Nguyễn Quốc Anh</option>
                        <option>Sầm Đức Thắng</option>
                        <option>Bùi Quang Đạt</option>
                        <option>Trần Văn Cường</option>
                        <option>Đỗ Đức Hưng</option>
                        <option>Nguyễn Sơn Dương</option>
                        <option>Đào Nguyễn Minh Vũ</option>
                        <option>Hoàng Văn Hải</option>
                        <option>Nguyễn Đoàn Nguyên</option>
                    </select>
                </td>
                <td class="note" style="width: 50%;" rowspan="3">
                    Lưu ý quan trọng: BP Thiết kế chỉ tiếp nhận khi các ô màu đỏ được tick đầy đủ. Trường hợp không tick đủ Hồ sơ sẽ bị trả lại để bổ sung<br>
                    Ghi chú:<br>
                    - Nếu dự án không có Hồ sơ mời thầu (BOQ), bộ phận Kinh doanh phải điền đầy đủ toàn bộ thông tin theo mẫu này.<br>
                    - Nếu dự án đã có Hồ sơ mời thầu (BOQ), BP Thiết kế lên cấu hình dựa trên theo nội dung BOQ đính kèm
                </td>
            </tr>
            <tr>
                <th style="width: 20%;">Nội dung hỗ trợ</th>
                <td style="width: 30%;">
                    <select class="extra-wide-select">
                        <option>Vui lòng chọn</option>
                        <option>Khảo sát</option>
                        <option>Tư vấn</option>
                        <option>Thiết kế sơ bộ</option>
                        <option>Thiết kế tổng thể</option>
                        <option>Khảo sát + tư vấn</option>
                        <option>Khảo sát + tư vấn + thiết kế sơ bộ</option>
                        <option>Khảo sát + tư vấn + thiết kế tổng thể</option>
                    </select>
                </td>
            </tr>
            <tr>
                <th style="width: 20%;">Hồ sơ mời thầu (BOQ)</th>
                <td style="width: 30%;">
                    <select class="extra-wide-select">
                        <option>Vui lòng chọn</option>
                        <option>Đã có BOQ đính kèm</option>
                        <option>Chưa có BOQ</option>
                    </select>
                </td>
            </tr>
        </table>

        <h2>THÔNG SỐ KỸ THUẬT CƠ BẢN</h2>
        <table>
            <tr><th class="no-col">No.</th><th class="item-col">HẠNG MỤC</th><th class="content-col">NỘI DUNG</th></tr>
            <tr>
                <td class="no-col">1</td>
                <td class="item-col">Loại thang máy</td>
                <td class="content-col">
                    <div class="inline-label">
                        <select class="extra-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>Theo thiết kế tư vấn</option>
                            <option>Thang máy tải khách</option>
                            <option>Thang máy tải hàng kèm người</option>
                            <option>Thang máy tải hàng không kèm người (tải trọng <=300kg)</option>
                            <option>Thang máy băng ca- Bệnh viện</option>
                            <option>Thang Homelift</option>
                            <option>Thang tải thực phẩm</option>
                            <option>Thang Homelift không đối trọng</option>
                            <option>Thang PCCC (EN 81-72)</option>
                            <option>Thang máy có chức năng phục vụ lực lượng PCCC</option>
                            <option>Thang chở Ôtô</option>
                            <option>Thang máy tròn</option>
                            <option>Thang quan sát</option>
                        </select>
                    </div>
                </td>
            </tr>
            <tr>
                <td class="no-col">2</td>
                <td class="item-col">Cấu hình thang máy</td>
                <td class="content-col">
                    <div class="inline-label">
                        <select class="extra-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>Thiết kế tư vấn</option>
                            <option>Theo yêu cầu Chủ đầu tư</option>
                        </select>
                    </div>
                </td>
            </tr>
            <tr>
                <td class="no-col">3</td>
                <td class="item-col">Nhà cung cấp</td>
                <td class="content-col">
                    <div class="inline-label">
                        <select class="extra-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>ALPEC</option>
                            <option>NIDEC</option>
                            <option>SJEC</option>
                            <option>YIDA</option>
                        </select>
                    </div>
                </td>
            </tr>
            <tr>
                <td class="no-col">4</td>
                <td class="item-col">Số thang</td>
                <td class="content-col">
                    <div class="inline-label">
                        <input type="number" min="1" max="4" step="1" value="1" class="quantity-select">
                    </div>
                </td>
            </tr>
            <tr>
                <td class="no-col">5</td>
                <td class="item-col">Tải trọng [kg]</td>
                <td class="content-col">
                    <div class="inline-label">
                        <select class="extra-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>300</option>
                            <option>350</option>
                            <option>450</option>
                            <option>550</option>
                            <option>630</option>
                            <option>700</option>
                            <option>750</option>
                            <option>800</option>
                            <option>900</option>
                            <option>1000</option>
                            <option>1150</option>
                            <option>1350</option>
                            <option>1600</option>
                            <option>2000</option>
                            <option>3000</option>
                            <option>5000</option>
                            <option>10000</option>
                            <option>Theo thiết kế tư vấn</option>
                        </select>
                    </div>
                </td>
            </tr>
            <tr>
                <td class="no-col">6</td>
                <td class="item-col">Loại cửa</td>
                <td class="content-col">
                    <div class="inline-label">
                        <select class="extra-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>CO</option>
                            <option>2CO</option>
                            <option>3CO</option>
                            <option>SO</option>
                            <option>MO</option>
                            <option>ATO</option>
                            <option>ATO-2P</option>
                            <option>2U</option>
                            <option>UD</option>
                            <option>RD</option>
                            <option>FD</option>
                            <option>N/A</option>
                        </select>
                    </div>
                </td>
            </tr>
            <tr>
                <td class="no-col">7</td>
                <td class="item-col">Loại mở cửa</td>
                <td class="content-col">
                    <div class="inline-label">
                        <select class="extra-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>Cabin 1 cửa</option>
                            <option>Cabin 2 cửa thông nhau</option>
                            <option>Cabin 2 cửa vuông góc</option>
                        </select>
                    </div>
                </td>
            </tr>
            <tr>
                <td class="no-col">8</td>
                <td class="item-col">Tốc độ [m/s]</td>
                <td class="content-col">
                    <div class="inline-label">
                        <select class="extra-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>0.3</option>
                            <option>0.4</option>
                            <option>0.5</option>
                            <option>1.0</option>
                            <option>1.5</option>
                            <option>1.75</option>
                            <option>2.0</option>
                            <option>2.5</option>
                            <option>3.0</option>
                        </select>
                    </div>
                </td>
            </tr>
            <tr>
                <td class="no-col">9</td>
                <td class="item-col">Loại động cơ</td>
                <td class="content-col">
                    <div class="inline-label">
                        <select class="extra-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>FUJI-JAPAN (nhập khẩu)</option>
                            <option>FUJI-Nhật Bản (mua trong nước)</option>
                            <option>FUJI-THAILAN</option>
                            <option>NIDEC</option>
                            <option>TORIN_CHINA</option>
                            <option>FUJI-KOREA</option>
                            <option>ZIEHL -ABEGG</option>
                            <option>Theo tiêu chuẩn của hãng</option>
                        </select>
                    </div>
                </td>
            </tr>
            <tr>
                <td class="no-col">10</td>
                <td class="item-col">Vị trí thang máy</td>
                <td class="content-col">
                    <div class="inline-label">
                        <select class="extra-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>Trong nhà</option>
                            <option>Ngoài trời</option>
                        </select>
                    </div>
                </td>
            </tr>
            <tr>
                <td class="no-col">11</td>
                <td class="item-col">Vị trí đối trọng</td>
                <td class="content-col">
                    <div class="inline-label">
                        <select class="extra-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>Phía sau Carbin</option>
                            <option>Bên cạnh Carbin</option>
                            <option>Thiết kế tư vấn</option>
                        </select>
                    </div>
                </td>
            </tr>
            <tr>
                <td class="no-col">12</td>
                <td class="item-col">Vị trí phòng máy</td>
                <td class="content-col">
                    <div class="inline-label">
                        <select class="extra-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>CÓ PHÒNG MÁY</option>
                            <option>KHÔNG PHÒNG MÁY</option>
                            <option>Thiết kế tư vấn</option>
                        </select>
                    </div>
                </td>
            </tr>
            <tr>
                <td class="no-col">13</td>
                <td class="item-col">Kết cấu hố thang</td>
                <td class="content-col">
                    <div class="inline-label">
                        <select class="extra-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>Bê tông tường gạch [tiêu chuẩn]</option>
                            <option>Bê tông cốt thép toàn bộ giếng thang</option>
                            <option>Khung thép [Fujialpha cấp]</option>
                            <option>Khung thép [Chủ đầu tư cấp]</option>
                            <option>Khung Nhôm định hình [Fujialpha cấp]</option>
                            <option>Khung Nhôm định hình [Chủ đầu tư cấp]</option>
                            <option>Thiết kế tư vấn</option>
                        </select>
                    </div>
                </td>
            </tr>
            <tr>
                <td class="no-col">14</td>
                <td class="item-col">Ốp bao che xung quanh giếng thang (nếu có)</td>
                <td class="content-col">
                    <div class="inline-label">
                        <select class="extra-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>Không</option>
                            <option>Ốp kính khuôn tranh xung quanh giếng thang</option>
                            <option>Ốp kính phủ xung quanh giếng thang</option>
                            <option>Ốp bao che bằng vât liệu khác theo hình ảnh đính kèm</option>
                            <option>Chủ đầu tư thực hiện</option>
                        </select>
                    </div>
                </td>
            </tr>
            <tr>
                <td class="no-col">15</td>
                <td class="item-col">Số tầng</td>
                <td class="content-col">
                    <div class="inline-label">
                        <input type="number" min="1" max="100" step="1" value="1" class="quantity-select">
                    </div>
                </td>
            </tr>
            <tr>
                <td class="no-col">16</td>
                <td class="item-col">Số điểm dừng</td>
                <td class="content-col">
                    <div class="inline-label">
                        <input type="number" min="1" max="100" step="1" value="1" class="quantity-select">
                    </div>
                </td>
            </tr>
            <tr>
                <td class="no-col">17</td>
                <td class="item-col">Số cửa tầng</td>
                <td class="content-col">
                    <div class="inline-label">
                        <input type="number" min="1" max="100" step="1" value="1" class="quantity-select">
                    </div>
                </td>
            </tr>
            <tr>
                <td class="no-col">18</td>
                <td class="item-col">Ký hiệu tầng</td>
                <td class="content-col">
                    <div class="inline-label">
                        <input type="text" class="highlight full-width-input">
                    </div>
                </td>
            </tr>
            <tr>
                <td class="no-col">19</td>
                <td class="item-col">Hồ sơ bản vẽ thiết kế xây dựng [kiến trúc + kết cấu]</td>
                <td class="content-col">
                    <div class="inline-label">
                        <select class="extra-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>Có (file đính kèm)</option>
                            <option>Không</option>
                        </select>
                    </div>
                </td>
            </tr>
            <tr>
                <td class="no-col">20</td>
                <td class="item-col">Hiện trạng thi công đến tầng thang máy</td>
                <td class="content-col">
                    <div class="inline-label">
                        <input type="text" class="highlight-yellow full-width-input">
                    </div>
                </td>
            </tr>
        </table>

        <h2>THÔNG SỐ KÍCH THUỚC</h2>
        <table>
            <tr><th class="no-col">No.</th><th class="size-content-col">NỘI DUNG</th><th class="size-input-col">KÍCH THƯỚC</th><th class="unit-col">ĐƠN VỊ</th></tr>
            <tr>
                <td class="no-col">21</td>
                <td class="size-content-col">Kích thước giếng thang chờ thô</td>
                <td class="size-input-col"><div class="inline-label">W-Rộng: <input type="text" class="highlight"> x D-Sâu: <input type="text" class="highlight"> </div></td>
                <td class="unit-col">(mm)</td>
            </tr>
            <tr>
                <td class="no-col">22</td>
                <td class="size-content-col">Kích thước cabin</td>
                <td class="size-input-col"><div class="inline-label">W-Rộng: <input type="text" class="highlight"> x D-Sâu: <input type="text" class="highlight"> x C-Cao: <input type="text" class="highlight"> </div></td>
                <td class="unit-col">(mm)</td>
            </tr>
            <tr>
                <td class="no-col">23</td>
                <td class="size-content-col">Kích thước cửa</td>
                <td class="size-input-col"><div class="inline-label">W-Rộng: <input type="text" class="highlight"> x C-Cao: <input type="text" class="highlight"> </div></td>
                <td class="unit-col">(mm)</td>
            </tr>
            <tr>
                <td class="no-col">24</td>
                <td class="size-content-col">Chiều cao đỉnh giếng thang (OH)</td>
                <td class="size-input-col"><div class="inline-label"><input type="text" class="highlight extended-input"> </div></td>
                <td class="unit-col">(mm)</td>
            </tr>
            <tr>
                <td class="no-col">25</td>
                <td class="size-content-col">Độ sâu Hố Pít (PD)</td>
                <td class="size-input-col"><div class="inline-label"><input type="text" class="highlight extended-input"> </div></td>
                <td class="unit-col">(mm)</td>
            </tr>
            <tr>
                <td class="no-col">26</td>
                <td class="size-content-col">Chiều cao Phòng máy</td>
                <td class="size-input-col"><div class="inline-label"><input type="text" class="highlight extended-input"> </div></td>
                <td class="unit-col">(mm)</td>
            </tr>
            <tr>
                <td class="no-col">27</td>
                <td class="size-content-col">Chiều cao tầng (không có thì điền chiều cao tầng trung bình)</td>
                <td class="size-input-col">
                    <div class="multi-column">
                        <div>Tầng 1 <input type="text" class="highlight"></div>
                        <div>Tầng 2 <input type="text" class="highlight"></div>
                        <div>Tầng 3 <input type="text" class="highlight"></div>
                        <div>Tầng 4 <input type="text" class="highlight"></div>
                        <div>Tầng 5 <input type="text" class="highlight"></div>
                        <div>Tầng …n <input type="text" class="highlight"></div>
                    </div>
                </td>
                <td class="unit-col">(mm)</td>
            </tr>
        </table>

        <h2>ĐẶC TÍNH CABIN-CỬA THANG MÁY- KHUNG BAO, COP, LOP</h2>
        <table>
            <tr><td class="no-col" rowspan="8">28</td><td colspan="2" class="section-title">Cabin</td></tr>
            <tr>
                <td class="item-col">MODEL</td>
                <td class="content-col">
                    <div class="inline-label">
                        <select class="medium-wide-select" id="cabin-model">
                            <option value="" data-image="">Vui lòng chọn</option>
                            <option value="FJA-C01-TC" data-image="https://ibb.co/QFrX0N2m">FJA-C01-TC</option>
                            <option value="FJA-C02-TC" data-image="https://ibb.co/d0KyQ50h">FJA-C02-TC</option>
                            <option value="FJA-C03-TC" data-image="https://ibb.co/HTLYb1SW">FJA-C03-TC</option>
                            <option value="FJA-ECO01" data-image="https://via.placeholder.com/150?text=FJA-ECO01">FJA-ECO01</option>
                            <option value="FJA-ECO02" data-image="https://ibb.co/Tq2N3PVN">FJA-ECO02</option>
                            <option value="FJA-ECO03" data-image="https://ibb.co/0Vqz5mbN">FJA-ECO03</option>
                            <option value="FJA-C08-LC" data-image="https://via.placeholder.com/150?text=FJA-C08-LC">FJA-C08-LC</option>
                            <option value="FJA-C11-LC" data-image="https://via.placeholder.com/150?text=FJA-C11-LC">FJA-C11-LC</option>
                            <option value="FJA-C15-LC" data-image="https://via.placeholder.com/150?text=FJA-C15-LC">FJA-C15-LC</option>
                            <option value="FJA-C16-LC" data-image="https://via.placeholder.com/150?text=FJA-C16-LC">FJA-C16-LC</option>
                            <option value="FJA-C17-LC" data-image="https://via.placeholder.com/150?text=FJA-C17-LC">FJA-C17-LC</option>
                            <option value="FJA-C30-LC" data-image="https://via.placeholder.com/150?text=FJA-C30-LC">FJA-C30-LC</option>
                            <option value="FJA-C44-LC" data-image="https://via.placeholder.com/150?text=FJA-C44-LC">FJA-C44-LC</option>
                            <option value="FJA-C46-LC" data-image="https://via.placeholder.com/150?text=FJA-C46-LC">FJA-C46-LC</option>
                            <option value="FJA-C49-LC" data-image="https://via.placeholder.com/150?text=FJA-C49-LC">FJA-C49-LC</option>
                            <option value="FJA-C51-LC" data-image="https://via.placeholder.com/150?text=FJA-C51-LC">FJA-C51-LC</option>
                            <option value="FJA-C52-LC" data-image="https://via.placeholder.com/150?text=FJA-C52-LC">FJA-C52-LC</option>
                            <option value="FJA-C19-LC" data-image="https://via.placeholder.com/150?text=FJA-C19-LC">FJA-C19-LC</option>
                            <option value="FJA-C20-LC" data-image="https://via.placeholder.com/150?text=FJA-C20-LC">FJA-C20-LC</option>
                            <option value="FJA-C21-LC" data-image="https://via.placeholder.com/150?text=FJA-C21-LC">FJA-C21-LC</option>
                            <option value="FJA-HL01-TC" data-image="https://via.placeholder.com/150?text=FJA-HL01-TC">FJA-HL01-TC</option>
                            <option value="FJA-HL02-TC" data-image="https://via.placeholder.com/150?text=FJA-HL02-TC">FJA-HL02-TC</option>
                            <option value="FJA-HL01-LC" data-image="https://via.placeholder.com/150?text=FJA-HL01-LC">FJA-HL01-LC</option>
                            <option value="FJA-HL02-LC" data-image="https://via.placeholder.com/150?text=FJA-HL02-LC">FJA-HL02-LC</option>
                            <option value="FJA-HL03-LC" data-image="https://via.placeholder.com/150?text=FJA-HL03-LC">FJA-HL03-LC</option>
                            <option value="FJA-QS01" data-image="https://via.placeholder.com/150?text=FJA-QS01">FJA-QS01</option>
                            <option value="FJA-QS02" data-image="https://via.placeholder.com/150?text=FJA-QS02">FJA-QS02</option>
                            <option value="FJA-QS03" data-image="https://via.placeholder.com/150?text=FJA-QS03">FJA-QS03</option>
                            <option value="FJA-QS04" data-image="https://via.placeholder.com/150?text=FJA-QS04">FJA-QS04</option>
                            <option value="Thang máy bệnh viện" data-image="https://via.placeholder.com/150?text=Hospital">Thang máy bệnh viện (Catalog trang 72)</option>
                            <option value="Thang máy tải hàng" data-image="https://via.placeholder.com/150?text=Goods">Thang máy tải hàng (Catalog trang 74)</option>
                            <option value="Thang máy tải ô tô" data-image="https://via.placeholder.com/150?text=Car">Thang máy tải ô tô (Catalog trang 76)</option>
                            <option value="Thang máy tải hàng loại nhỏ" data-image="https://via.placeholder.com/150?text=Small">Thang máy tải hàng loại nhỏ (Catalog trang 78)</option>
                            <option value="FJA-CT01" data-image="https://via.placeholder.com/150?text=FJA-CT01">FJA-CT01</option>
                            <option value="FJA-CT02" data-image="https://via.placeholder.com/150?text=FJA-CT02">FJA-CT02</option>
                            <option value="FJA-CT03" data-image="https://via.placeholder.com/150?text=FJA-CT03">FJA-CT03</option>
                            <option value="FJA-CT04" data-image="https://via.placeholder.com/150?text=FJA-CT04">FJA-CT04</option>
                            <option value="FJA-CT05" data-image="https://via.placeholder.com/150?text=FJA-CT05">FJA-CT05</option>
                            <option value="Theo tiêu chuẩn của hãng" data-image="">Theo tiêu chuẩn của hãng</option>
                            <option value="Model khác hình ảnh đính kèm" data-image="">Model khác hình ảnh đính kèm</option>
                        </select>
                        <div id="preview-container" class="preview-container">
                            <img id="preview-image" src="" alt="Preview Image">
                        </div>
                    </div>
                </td>
            </tr>
            <tr><td class="item-col" rowspan="3">Vách cabin</td><td class="content-col"><div class="inline-label">Vách trước: <input type="text" class="highlight aligned-input"> </div></td></tr>
            <tr><td class="content-col"><div class="inline-label">Vách sau: <input type="text" class="highlight aligned-input"> </div></td></tr>
            <tr><td class="content-col"><div class="inline-label">Hai vách bên: <input type="text" class="highlight aligned-input"> </div></td></tr>
            <tr>
                <td class="item-col">Sàn cabin</td>
                <td class="content-col">
                    <div class="inline-label">
                        <select class="medium-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>Fuji cung cấp đồng bộ theo model cabin</option>
                            <option>Fuji cung cấp theo model khác theo hình ảnh đính kèm</option>
                            <option>Do chủ đầu tư cung cấp</option>
                        </select>
                    </div>
                </td>
            </tr>
            <tr>
                <td class="item-col">Trần cabin (Trần giả)</td>
                <td class="content-col">
                    <div class="inline-label">
                        <select class="medium-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>Đồng bộ theo model cabin</option>
                            <option>FJA-T01-TC</option>
                            <option>FJA-T02-TC</option>
                            <option>FJA-T03-TC</option>
                            <option>FJA-T06-TC</option>
                            <option>FJA-T07-TC</option>
                            <option>FJA-T08-TC</option>
                            <option>FJA-T09-TC</option>
                            <option>FJA-T10-TC</option>
                            <option>Theo tiêu chuẩn của hãng</option>
                            <option>Model khác hình ảnh đính kèm</option>
                        </select>
                    </div>
                </td>
            </tr>
            <tr>
                <td class="item-col">Tay vịn</td>
                <td class="content-col">
                    <div class="inline-label">
                        <select class="medium-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>FJA-TV01-TC</option>
                            <option>FJA-TV02-TC</option>
                            <option>FJA-TV03-TC</option>
                            <option>FJA-TV04-TC</option>
                            <option>FJA-TV05-TC</option>
                            <option>FJA-TV06-TC</option>
                            <option>FJA-TV07-TC</option>
                            <option>FJA-TV08-TC</option>
                            <option>Theo tiêu chuẩn của hãng</option>
                            <option>Model khác hình ảnh đính kèm</option>
                        </select>
                        Số lượng: 
                        <input type="number" min="0" step="1" value="0" class="quantity-select">
                    </div>
                </td>
            </tr>
            <tr><td class="no-col" rowspan="5">29</td><td colspan="2" class="section-title">Cửa</td></tr>
            <tr>
                <td class="item-col">Cửa Cabin</td>
                <td class="content-col">
                    <div class="inline-label">
                        Model: 
                        <select class="medium-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>FJA-CC01-LC</option>
                            <option>FJA-CC02-LC</option>
                            <option>FJA-CC03-LC</option>
                            <option>FJA-CC04-LC</option>
                            <option>FJA-CC05-LC</option>
                            <option>FJA-CC06-LC</option>
                            <option>Đồng bộ theo Model cabin</option>
                            <option>Theo tiêu chuẩn của hãng</option>
                            <option>Model khác hình ảnh đính kèm</option>
                        </select>
                        Vật liệu: <input type="text" class="highlight full-width-input">
                    </div>
                </td>
            </tr>
            <tr>
                <td class="item-col">Cửa Tầng chính</td>
                <td class="content-col">
                    <div class="inline-label">
                        Model: 
                        <select class="medium-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>FJA-CC01-LC</option>
                            <option>FJA-CC02-LC</option>
                            <option>FJA-CC03-LC</option>
                            <option>FJA-CC04-LC</option>
                            <option>FJA-CC05-LC</option>
                            <option>FJA-CC06-LC</option>
                            <option>Đồng bộ theo Model cabin</option>
                            <option>Theo tiêu chuẩn của hãng</option>
                            <option>Model khác hình ảnh đính kèm</option>
                        </select>
                        Vật liệu: <input type="text" class="highlight full-width-input">
                    </div>
                </td>
            </tr>
            <tr>
                <td class="item-col">Cửa Tầng khác</td>
                <td class="content-col">
                    <div class="inline-label">
                        Model: 
                        <select class="medium-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>FJA-CC01-LC</option>
                            <option>FJA-CC02-LC</option>
                            <option>FJA-CC03-LC</option>
                            <option>FJA-CC04-LC</option>
                            <option>FJA-CC05-LC</option>
                            <option>FJA-CC06-LC</option>
                            <option>Đồng bộ theo Model cabin</option>
                            <option>Theo tiêu chuẩn của hãng</option>
                            <option>Model khác hình ảnh đính kèm</option>
                        </select>
                        Vật liệu: <input type="text" class="highlight full-width-input">
                    </div>
                </td>
            </tr>
            <tr>
                <td class="item-col">Cửa chống cháy</td>
                <td class="content-col">
                    <div class="inline-label">
                        <select class="medium-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>Không có</option>
                            <option>E30</option>
                            <option>E45</option>
                            <option>E60</option>
                            <option>E90</option>
                            <option>E120</option>
                            <option>EI30</option>
                            <option>EI60</option>
                            <option>EI70</option>
                            <option>EI90</option>
                        </select>
                    </div>
                </td>
            </tr>
            <tr><td class="no-col" rowspan="3">30</td><td colspan="2" class="section-title">Khung bao cửa tầng</td></tr>
            <tr>
                <td class="item-col">Khung bao cửa tầng chính</td>
                <td class="content-col">
                    <div class="inline-label">
                        Loại: 
                        <select class="medium-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>Bản hẹp</option>
                            <option>Bản hẹp mở rộng</option>
                            <option>Bản rộng</option>
                        </select>
                        Vật liệu: <input type="text" class="highlight full-width-input">
                    </div>
                </td>
            </tr>
            <tr>
                <td class="item-col">Khung bao cửa tầng khác</td>
                <td class="content-col">
                    <div class="inline-label">
                        Loại: 
                        <select class="medium-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>Bản hẹp</option>
                            <option>Bản hẹp mở rộng</option>
                            <option>Bản rộng</option>
                        </select>
                        Vật liệu: <input type="text" class="highlight full-width-input">
                    </div>
                </td>
            </tr>
            <tr>
                <td class="no-col">31</td>
                <td class="item-col">COP</td>
                <td class="content-col">
                    <div class="inline-label">
                        Model: 
                        <select class="medium-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>FJA-ĐK01-TC</option>
                            <option>FJA-ĐK01-LC</option>
                            <option>FJA-ĐK06-LC</option>
                            <option>Đồng bộ theo Model cabin</option>
                            <option>Theo tiêu chuẩn của hãng</option>
                            <option>Model khác hình ảnh đính kèm</option>
                        </select>
                        Vật liệu: 
                        <select class="medium-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>Inox</option>
                            <option>Vật liệu khác hình ảnh đính kèm</option>
                        </select>
                        Số lượng: <input type="number" min="0" step="1" value="0" class="quantity-select">
                    </div>
                </td>
            </tr>
            <tr>
                <td class="no-col">32</td>
                <td class="item-col">LOP</td>
                <td class="content-col">
                    <div class="inline-label">
                        Model: 
                        <select class="medium-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>FJA-ĐK02-TC</option>
                            <option>FJA-ĐK03-TC</option>
                            <option>FJA-ĐK04-TC</option>
                            <option>FJA-ĐK02-LC</option>
                            <option>FJA-ĐK03-LC</option>
                            <option>FJA-ĐK04-LC</option>
                            <option>FJA-ĐK05-LC</option>
                            <option>FJA-ĐK07-LC</option>
                            <option>FJA-ĐK08-LC</option>
                            <option>FJA-ĐK09-LC</option>
                            <option>Theo tiêu chuẩn của hãng</option>
                            <option>Model khác hình ảnh đính kèm</option>
                        </select>
                        Vật liệu: 
                        <select class="medium-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>Inox</option>
                            <option>Vật liệu khác hình ảnh đính kèm</option>
                        </select>
                        Số lượng: <input type="number" min="0" step="1" value="0" class="quantity-select">
                    </div>
                </td>
            </tr>
            <tr><td class="no-col" rowspan="3">33</td><td colspan="2" class="section-title">Hiển thị ngang</td></tr>
            <tr>
                <td class="item-col">Cửa tầng chính</td>
                <td class="content-col">
                    <div class="inline-label">
                        <select class="medium-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>Có cung cấp</option>
                            <option>Không cung cấp</option>
                        </select>
                        Model: 
                        <select class="medium-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>Không</option>
                            <option>FJA-ĐK05-LC</option>
                            <option>FJA-ĐK09-LC</option>
                            <option>Theo tiêu chuẩn của hãng</option>
                            <option>Model khác hình ảnh đính kèm</option>
                        </select>
                    </div>
                </td>
            </tr>
            <tr>
                <td class="item-col">Cửa tầng khác</td>
                <td class="content-col">
                    <div class="inline-label">
                        <select class="medium-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>Có cung cấp</option>
                            <option>Không cung cấp</option>
                        </select>
                        Model: 
                        <select class="medium-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>Không</option>
                            <option>FJA-ĐK05-LC</option>
                            <option>FJA-ĐK09-LC</option>
                            <option>Theo tiêu chuẩn của hãng</option>
                            <option>Model khác hình ảnh đính kèm</option>
                        </select>
                    </div>
                </td>
            </tr>
            <tr><td class="no-col" rowspan="3">34</td><td colspan="2" class="section-title">Ốp bao che thẩm mỹ</td></tr>
            <tr>
                <td class="item-col">Cabin</td>
                <td class="content-col">
                    <div class="inline-label">
                        <select class="medium-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>Không</option>
                            <option>Ốp bao che 4 mặt bằng vật liệu Inox</option>
                            <option>Ốp bao che 4 mặt bằng vật liệu Tôn sơn</option>
                        </select>
                    </div>
                </td>
            </tr>
            <tr>
                <td class="item-col">Đối trọng</td>
                <td class="content-col">
                    <div class="inline-label">
                        <select class="medium-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>Không</option>
                            <option>Ốp bao che 4 mặt bằng vật liệu Inox</option>
                            <option>Ốp bao che 4 mặt bằng vật liệu Tôn sơn</option>
                        </select>
                    </div>
                </td>
            </tr>
        </table>

        <h2>CHỨC NĂNG LỰA CHỌN</h2>
        <table>
            <tr><th class="no-col">No.</th><th class="item-col">HẠNG MỤC</th><th class="content-col">NỘI DUNG</th></tr>
            <tr>
                <td class="no-col">35</td>
                <td class="item-col">Điều khiển thang đôi</td>
                <td class="content-col">
                    <div class="inline-label">
                        <select class="extra-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>Có</option>
                            <option>Không yêu cầu</option>
                        </select>
                    </div>
                </td>
            </tr>
            <tr>
                <td class="no-col">36</td>
                <td class="item-col">Điều khiển thang nhóm</td>
                <td class="content-col">
                    <div class="inline-label">
                        <select class="extra-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>Có</option>
                            <option>Không yêu cầu</option>
                        </select>
                    </div>
                </td>
            </tr>
            <tr>
                <td class="no-col">37</td>
                <td class="item-col">Bảng điều khiển cabin cho người tàn tật</td>
                <td class="content-col">
                    <div class="inline-label">
                        <select class="extra-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>Có</option>
                            <option>Không yêu cầu</option>
                        </select>
                    </div>
                </td>
            </tr>
            <tr>
                <td class="no-col">38</td>
                <td class="item-col">Tính năng báo cháy (Fire Switch)</td>
                <td class="content-col">
                    <div class="inline-label">
                        <select class="extra-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>Có</option>
                            <option>Không yêu cầu</option>
                        </select>
                    </div>
                </td>
            </tr>
            <tr>
                <td class="no-col">39</td>
                <td class="item-col">Tính

            </tr>
            <tr>
                <td class="no-col">39</td>
                <td class="item-col">Tính năng cứu hỏa (Fireman Switch)</td>
                <td class="content-col">
                    <div class="inline-label">
                        <select class="extra-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>Có</option>
                            <option>Không yêu cầu</option>
                        </select>
                    </div>
                </td>
            </tr>
            <tr>
                <td class="no-col">40</td>
                <td class="item-col">Đèn báo tầng đến</td>
                <td class="content-col">
                    <div class="inline-label">
                        <select class="extra-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>Có</option>
                            <option>Không yêu cầu</option>
                        </select>
                    </div>
                </td>
            </tr>
            <tr>
                <td class="no-col">41</td>
                <td class="item-col">Đầu đọc thẻ</td>
                <td class="content-col">
                    <div class="inline-label">
                        <select class="extra-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>Có</option>
                            <option>Không yêu cầu</option>
                        </select>
                    </div>
                </td>
            </tr>
            <tr>
                <td class="no-col">42</td>
                <td class="item-col">Phục vụ tầng VIP</td>
                <td class="content-col">
                    <div class="inline-label">
                        <select class="extra-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>Có</option>
                            <option>Không yêu cầu</option>
                        </select>
                    </div>
                </td>
            </tr>
            <tr>
                <td class="no-col">43</td>
                <td class="item-col">Camera trong cabin</td>
                <td class="content-col">
                    <div class="inline-label">
                        <select class="extra-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>Có</option>
                            <option>Không yêu cầu</option>
                        </select>
                    </div>
                </td>
            </tr>
            <tr>
                <td class="no-col">44</td>
                <td class="item-col">Kết nối phần mềm giám sát</td>
                <td class="content-col">
                    <div class="inline-label">
                        <select class="extra-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>Có</option>
                            <option>Không yêu cầu</option>
                        </select>
                    </div>
                </td>
            </tr>
            <tr>
                <td class="no-col">45</td>
                <td class="item-col">Phục vụ giờ cao điểm (tầng trên, tầng dưới)</td>
                <td class="content-col">
                    <div class="inline-label">
                        <select class="extra-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>Có</option>
                            <option>Không yêu cầu</option>
                        </select>
                    </div>
                </td>
            </tr>
            <tr>
                <td class="no-col">46</td>
                <td class="item-col">Giảm chấn cabin, đối trọng</td>
                <td class="content-col">
                    <div class="inline-label">
                        <select class="extra-wide-select">
                            <option>Vui lòng chọn</option>
                            <option>Tiêu chuẩn</option>
                            <option>Thủy lực</option>
                            <option>Cao su</option>
                            <option>Model khác hình ảnh đính kèm</option>
                        </select>
                    </div>
                </td>
            </tr>
            <tr>
                <td class="no-col">47</td>
                <td class="item-col">Đối với các chức năng khác, vui lòng liên hệ với BP Thiết kế để được tư vấn thêm</td>
                <td class="content-col">
                    <div class="inline-label">
                        <input type="text" class="highlight full-width-input">
                    </div>
                </td>
            </tr>
        </table>

        <p style="text-align: center;">Trang 2/2</p>

        <div class="action-buttons">
            <button type="button" id="print-form">In biểu mẫu</button>
        </div>
    </form>

    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <script src="https://code.jquery.com/ui/1.12.1/jquery-ui.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
    <script>
        // Automatically set the current date
        const today = new Date();
        const day = String(today.getDate()).padStart(2, '0');
        const month = String(today.getMonth() + 1).padStart(2, '0');
        const year = today.getFullYear();
        document.getElementById('day').textContent = day;
        document.getElementById('month').textContent = month;
        document.getElementById('year').textContent = year;

        // Address autocomplete with Nominatim
        $(document).ready(function() {
            $("#address").autocomplete({
                source: function(request, response) {
                    $.ajax({
                        url: "https://nominatim.openstreetmap.org/search",
                        dataType: "json",
                        data: {
                            q: request.term,
                            format: "json",
                            addressdetails: 1,
                            limit: 10,
                            countrycodes: "vn"
                        },
                        headers: {
                            "User-Agent": "ElevatorForm/1.0 (contact: your-email@example.com)"
                        },
                        success: function(data) {
                            response($.map(data, function(item) {
                                return {
                                    label: item.display_name,
                                    value: item.display_name
                                };
                            }));
                        },
                        error: function() {
                            alert("Không thể tải gợi ý địa chỉ. Vui lòng thử lại sau.");
                        }
                    });
                },
                minLength: 3,
                select: function(event, ui) {
                    console.log("Địa chỉ được chọn: " + ui.item.value);
                }
            });

            // Cabin model preview
            const $select = $("#cabin-model");
            const $previewContainer = $("#preview-container");
            const $previewImage = $("#preview-image");

            $select.on("change mouseover", function() {
                const selectedOption = $(this).find("option:selected");
                const imageUrl = selectedOption.data("image");

                if (imageUrl) {
                    $previewImage.attr("src", imageUrl);
                    $previewContainer.css({
                        display: "block",
                        left: $select.offset().left + $select.outerWidth() + 10,
                        top: $select.offset().top
                    });
                } else {
                    $previewContainer.hide();
                }
            });

            $select.on("mouseout", function() {
                $previewContainer.hide();
            });

            // Print form
            $("#print-form").on("click", function() {
                window.print();
            });
        });
    </script>
</body>
</html>
