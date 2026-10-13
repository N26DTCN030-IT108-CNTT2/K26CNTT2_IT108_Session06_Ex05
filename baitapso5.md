\# Báo cáo Phân tích: Xây dựng Công cụ Báo cáo Log Tự động (`auto\_report.ps1`)



\## Phần 1: Thiết kế Kiến trúc Script PowerShell

Cấu trúc kịch bản tự động hóa được chia làm 3 khối chính:

1\. \*\*Khối Header \& Giao diện:\*\* Sử dụng lệnh `Write-Host` kèm tham số màu sắc `-ForegroundColor` để xuất thông báo trạng thái chuyên nghiệp ra màn hình Terminal.

2\. \*\*Khối Khởi tạo Tiêu đề Báo cáo:\*\* Tự động lấy mốc thời gian hệ thống thông qua `Get-Date` để định dạng dòng tiêu đề file báo cáo: `=== LOG REPORT DATE \[Ngày hôm nay] ===`.

3\. \*\*Khối Xử lý \& Nối dữ liệu Logic:\*\* Đọc file `access.log`, lọc các dòng chứa lỗi `404`, và dùng `Add-Content` để \*nối thêm\* vào tệp kết quả mà không làm mất dữ liệu cũ.



\---



\## Phần 2: Tệp Mã nguồn `auto\_report.ps1` Hoàn chỉnh



```powershell

\# ==========================================

\# Script: auto\_report.ps1

\# Mô tả: Tự động lọc lỗi 404 từ access.log và ghi báo cáo

\# ==========================================



\# 1. Hiển thị giao diện thông báo khởi chạy

Write-Host "=========================================" -ForegroundColor Cyan

Write-Host "   BAT DAU XU LY VA PHAN TICH LOG 404     " -ForegroundColor Yellow

Write-Host "=========================================" -ForegroundColor Cyan



\# 2. Định nghĩa đường dẫn file

$logFile = ".\\access.log"

$reportFile = ".\\report\_404.csv"



\# 3. Tạo dòng tiêu đề tự động kèm ngày hiện tại

$currentDate = Get-Date -Format "yyyy-MM-dd HH:mm:ss"

$headerTitle = "=== LOG REPORT DATE \[$currentDate] ==="



\# Ghi tiêu đề vào file báo cáo mới/nối thêm

Add-Content -Path $reportFile -Value$headerTitle



\# 4. Đọc file log, lọc lỗi 404 và nối thêm vào báo cáo

Get-Content -Path $logFile \\vert{} Select-String "404" \\vert{} Add-Content -Path $reportFile



\# 5. Thông báo hoàn thành giao diện

Write-Host "\[SUCCESS] Export generated successfully!" -ForegroundColor Green

Write-Host "File: $reportFile" -ForegroundColor White

Write-Host "Time: \~0.5 seconds" -ForegroundColor Gray

