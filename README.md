# BAITAP4
# BTVN_BAITAP4
# bai tap 4: (sql server)
- Yêu cầu bài toán:
 - Tạo csdl cho hệ thống TKB (đã nghe giảng, đã xem cách làm)
 - Nguồn dữ liệu: TMS.tnut.edu.vn
 - Tạo các bảng tuỳ ý (3nf)
 - Tạo được query truy vấn ra thông tin gồm 4 cột: họ tên gv, môn dạy, giờ vào lớp, giờ ra.
   trả lời câu hỏi: trong khoảng thời gian từ datetime1 tới datetime2 thì có những gv nào đang bận giảng dạy.

- Các bước thực hiện:
1. Tạo github repo mới: đặt tên tuỳ ý (có liên quan đến bài tập này)
2. tạo file readme.md, edit online nó:
   paste những ảnh chụp màn hình
   gõ text mô tả cho ảnh đó
- Gợi ý:
  sử dung tms => dữ liệu thô => tiền xử lý => dữ liệu như ý (3nf)
  tạo các bảng với struct phù hợp
  insert nhiều rows từ excel vào cửa sổ edit dữ liệu 1 table (quan sát thì sẽ làm đc)

  #bài làm:
  Tạo các bảng cần thiết theo dữ liễu của TMS:
  -Bảng phòng học:

![bang phong hoc](https://github.com/user-attachments/assets/f9dfae8f-538a-4617-b205-2895c9f316a2)

  -Bảng Môn Học:
  
  ![bang mon hoc](https://github.com/user-attachments/assets/61aed777-bb53-4d78-b963-43ba0c612c3a)

  -Bảng Giáo viên
  
![bang gv (2)](https://github.com/user-attachments/assets/3f305ec6-f2bc-4a78-8aa7-6f0d2c420826)
  -Bảng Lịch học ( đây là bảng tổng hợp )
  
![bang lichoc](https://github.com/user-attachments/assets/95599711-59fe-4a1c-bdc2-00709236c0a3)

  -Bảng Lớp
![bang lop](https://github.com/user-attachments/assets/440be073-35f5-4825-8281-5edd261ef80c)

  Thêm dữ liễu vào các bảng, có thể thêm bằng cách import task hoặc edit:
   Import file Excel:
  
  ![Screenshot (17)](https://github.com/user-attachments/assets/040d456f-6e61-4bb4-ad99-789ae9900e72)
-EDIT trực tiếp:
+Bảng GV 

![EDIT GV](https://github.com/user-attachments/assets/68831779-7564-487e-86da-20610f59b261)

+Bảng Lớp

![EDIT LOP](https://github.com/user-attachments/assets/5c62944f-3d10-4352-b1ae-1660514b81b0)

+Bảng Môn Học

![EDIT MON HOC](https://github.com/user-attachments/assets/3ccaab81-1cd5-44fc-9796-0e0fe836f4bb)

+Bảng Lịch Học:
![EDIT LICHHOC](https://github.com/user-attachments/assets/aaafd1a7-7034-4d7a-9d09-22c6f3ba8c48)

Sau khi hoàn thành ta có sơ đồ như sau:

![Screenshot (22)](https://github.com/user-attachments/assets/2256010f-25bf-4912-a841-9e68c29294ab)

Truy vấn thông tin các cột và datetime:
SELECT 
   
   GV.hoten AS [Họ tên GV],
   
   MH. tenMon [Môn dạy],
  
   CASE 
       
       WHEN ld.thu = 2 THEN 'T2'
      
       WHEN ld.thu = 3 THEN 'T3'
       
       WHEN ld.thu = 4 THEN 'T4'
      
       WHEN ld.thu = 5 THEN 'T5'
      
       WHEN ld.thu = 6 THEN 'T6'
      
       WHEN ld.thu = 7 THEN 'T7'
       
       WHEN ld.thu = 8 THEN 'CN'
      
       ELSE 'N/A'
  
   END AS [Thu], 
     
FORMAT(ld.ngay, 'dd/MM/yyyy') AS [Ngay],
   
    ld.so_tiet AS [So tiet],
    
    MIN(t.gio_vao) AS [Gio vao lop],
   
    MAX(t.gio_ra) AS [Gio ra lop]

FROM 
    
    LichHoc ld
    
    INNER JOIN GiaoVien gv ON LichHoc.MaGV = gv.id
    
    INNER JOIN MonHoc mh ON LichHoc.monhoc_id = mh.id
   
    INNER JOIN TietHoc t ON t.tiet BETWEEN ld.tiet_bat_dau AND (ld.tiet_bat_dau + ld.so_tiet - 1)

WHERE 
    
    ld.giang_vien_id = 4  

GROUP BY
   
    gv.ten_gv,
  
    mh.ten_mon,
   
    ld.thu,
   
    ld.ngay,
   
    ld.so_tiet,
   
    ld.tiet_bat_dau

ORDER BY 
    
    ld.ngay, MIN(t.gio_vao);
