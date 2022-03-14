```sql
use master
go
if DB_ID('VN03_1712616') is not null
drop database VN03_1712616

go
create database VN03_1712616
go

use VN03_1712616
create table GIAOVIEN
(
	MAGV char (5),
	HOTEN nvarchar(40),
	LUONG float,
	PHAI nchar(3),
	NGSINH datetime,
	DIACHI nvarchar(100),
	GVQLCM char(5),
	MABM nchar(5)
	PRIMARY KEY (MAGV)
)

create table GV_DT
(
	MAGV char(5),
	DIENTHOAI char(12),
	PRIMARY KEY (MAGV, DIENTHOAI)
)

create table BOMON
(
	MABM nchar(5),
	TENBM nvarchar(40),
	PHONG char(5),
	DIENTHOAI char(12),
	TRUONGBM char(5),
	MAKHOA char(4),
	NGAYNHANCHUC datetime,
	PRIMARY KEY (MABM)
)
create table KHOA
(
	MAKHOA char(4),
	TENKHOA nvarchar(40),
	NAMTL int,
	PHONG char(5),
	DIENTHOAI char(12),
	TRUONGKHOA char(5),
	NGAYNHANCHUC datetime,
	PRIMARY KEY (MAKHOA)
)

create table DETAI
(
	MADT char(3),
	TENDT nvarchar(100),
	CAPQL nvarchar(40),
	KINHPHI float,
	NGAYBD datetime,
	NGAYKT datetime,
	MACD nchar(4),
	GVCNDT char(5),
	PRIMARY KEY (MADT)
)

create table CHUDE
(
	MACD nchar(4),
	TENCD nvarchar(50),
	PRIMARY KEY (MACD)
)
create table CONGVIEC
(
	MADT char(3),
	SOTT int,
	TENCV nvarchar(40),
	NGAYBD datetime,
	NGAYKT datetime,
	PRIMARY KEY (MADT, SOTT),
)

create table THAMGIADT
(
	MAGV char(5),
	MADT char(3),
	STT int,
	PHUCAP float ,
	KETQUA nvarchar(40),
	PRIMARY KEY (MAGV, MADT, STT)
)

create table NGUOITHAN
(
	MAGV char(5),
	TEN nvarchar(20),
	NGSINH datetime,
	PHAI nchar(3),
	PRIMARY KEY (MAGV, TEN)
)

alter table GIAOVIEN add
	constraint FK_GIAOVIEN_BOMON foreign key (MABM) references BOMON (MABM),
	constraint FK_GIAOVIEN_GIAOVIEN foreign key (GVQLCM) references GIAOVIEN (MAGV)

alter table KHOA add
	constraint FK_KHOA_GIAOVIEN foreign key (TRUONGKHOA) references GIAOVIEN (MAGV)

alter table BOMON add
	constraint FK_BOMON_KHOA foreign key (MAKHOA) references KHOA(MAKHOA),
	constraint FK_BOMON_GIAOVIEN foreign key (TRUONGBM) references GIAOVIEN (MAGV)

alter table NGUOITHAN add
	constraint FK_NGUOITHAN_GIAOVIEN foreign key (MAGV)references GIAOVIEN (MAGV)

alter table THAMGIADT add
	constraint FK_PHANCONG_GIAOVIEN foreign key (MAGV)references GIAOVIEN (MAGV),
	constraint FK_PHANCONG_CONGVIEC foreign key (MADT, STT)references CONGVIEC(MADT, SOTT) on delete cascade

alter table DETAI add
	constraint FK_DETAI_CHUDE foreign key (MACD)references CHUDE (MACD)

alter table DETAI add
	constraint FK_DETAI_GIAOVIEN foreign key (GVCNDT)references GIAOVIEN (MAGV)

alter table GV_DT add
	constraint FK_DIENTHOAI_GIAOVIEN foreign key (MAGV)references GIAOVIEN (MAGV)

alter table CONGVIEC add
	constraint FK_CONGVIEC_DETAI foreign key (MADT)references DETAI (MADT) on delete cascade
----------------
insert into KHOA values ('CNTT',N'Công nghệ thông tin',1995,'B11','0838123456',null,'02/20/2005')
insert into KHOA values ('VL',N'Vật lý',1976,'B21','0838223223',null,'09/18/2003')
insert into KHOA values ('SH',N'Sinh học',1980,'B31','0838454545',null,'10/11/2000')
insert into KHOA values ('HH',N'Hóa học',1980,'B41','0838456456',null,'10/15/2001')
----------------
insert into BOMON values (N'HTTT',N'Hệ thống thông tin','B13','0838125125',null,'CNTT','09/20/2004')
insert into BOMON values (N'CNTT',N'Công nghệ tri thức','B15','0838126126',null, 'CNTT', null)
insert into BOMON values (N'MMT',N'Mạng máy tính','B16','0838676767 ',null,'CNTT','05/15/2005')
insert into BOMON values (N'VLĐT',N'Vật lý điện tử','B23','0838234234',null, 'VL', null)
insert into BOMON values (N'VLƯD',N'Vật lý ứng dụng','B24','0838454545',null,'VL','02/18/2006')
insert into BOMON values (N'VS',N'Vi sinh','B32','0838909090',null,'SH','01/01/2007')
insert into BOMON values (N'SH',N'Sinh hóa','B33','0838898989',null, 'SH', null)
insert into BOMON values (N'HL',N'Hóa lý','B42','0838878787',null, 'HH', null)
insert into BOMON values (N'HPT',N'Hóa phân tích','B43','0838777777',null,'HH','10/15/2007')
insert into BOMON values (N'HHC',N'Hóa hữu cơ','B44','838222222',null, 'HH', null)
----------------
insert into GIAOVIEN values ('001',N'Nguyễn Hoài An',2000,N'Nam','02/15/1973',N'25/3 Lạc Long Quân, Q.10, TP HCM', null, N'MMT')
insert into GIAOVIEN values ('002',N'Trần Trà Hương',2500,N'Nữ','06/20/1960',N'125	Trần Hưng Đạo, Q.1,TP HCM', null, N'HTTT')
insert into GIAOVIEN values ('003',N'Nguyễn Ngọc Ánh',2200,N'Nữ','05/11/1975',N'12/21	Võ Văn Ngân	Thủ Đức, TP HCM', '002',N'HTTT')
insert into GIAOVIEN values ('004',N'Trương Nam Sơn',2300,N'Nam','06/20/1959',N'215	Lý Thường Kiệt,TP Biên Hòa',null, N'VS')
insert into GIAOVIEN values ('005',N'Lý Hoàng Hà',2500,N'Nam','10/23/1954',N'22/5	Nguyễn Xí, Q.Bình Thạnh, TP HCM',null, N'VLĐT')
insert into GIAOVIEN values ('006',N'Trần Bạch Tuyết',1500,N'Nữ','05/20/1980',N'127	Hùng Vương, TP Mỹ Tho','004',N'VS')
insert into GIAOVIEN values ('007',N'Nguyễn An Trung',2100,N'Nam','06/05/1976',N'234 3/2, TP Biên Hòa',null, N'HPT')
insert into GIAOVIEN values ('008',N'Trần Trung Hiếu',1800,N'Nam','08/06/1977',N'22/11 Lý Thường Kiệt, TP Mỹ Tho','007',N'HPT')
insert into GIAOVIEN values ('009',N'Trần Hoàng Nam',2000,N'Nam','11/22/1975',N'234	Trấn Não, An Phú,TP HCM','001',N'MMT')
insert into GIAOVIEN values ('010',N'Phạm Nam Thanh',1500,N'Nam','12/12/1980',N'221	Hùng Vương, Q.5, TP HCM','007',N'HPT')
----------------
insert into GV_DT values ('001','0903123123')
insert into GV_DT values ('001','0838912112')
insert into GV_DT values ('002','0913454545')
insert into GV_DT values ('003','0903656565')
insert into GV_DT values ('003','0838121212')
insert into GV_DT values ('003','0937125125')
insert into GV_DT values ('006','0937888888')
insert into GV_DT values ('008','0913232323')
insert into GV_DT values ('008','0653717171')
----------------
insert into CHUDE values (N'QLGD',N'Quản lý giáo dục')
insert into CHUDE values (N'NCPT',N'Nghiên cứu phát triển')
insert into CHUDE values (N'ƯDCN',N'Ứng dụng công nghệ')
----------------
insert into DETAI (MADT, TENDT, KINHPHI, CAPQL, NGAYBD, NGAYKT, MACD, GVCNDT) values ('001',N'HTTT quản lý các trường ĐH',20,N'ĐHQG','10/20/2007','10/20/2008',N'QLGD','002')
insert into DETAI (MADT, TENDT, KINHPHI, CAPQL, NGAYBD, NGAYKT, MACD, GVCNDT) values ('002',N'HTTT quản lý giáo vụ cho một Khoa','20',N'Trường','10/12/2000','10/12/2001',N'QLGD','002')
insert into DETAI (MADT, TENDT, KINHPHI, CAPQL, NGAYBD, NGAYKT, MACD, GVCNDT) values ('003',N'Nghiên cứu chế tạo sợi Nanô Platin','300',N'ĐHQG','05/15/2008','05/15/2010',N'NCPT','005')
insert into DETAI (MADT, TENDT, KINHPHI, CAPQL, NGAYBD, NGAYKT, MACD, GVCNDT) values ('004',N'Tạo vật liệu sinh học bằng màng ối người','100',N'Nhà nước','01/01/2007','12/31/2009',N'NCPT','004')
insert into DETAI (MADT, TENDT, KINHPHI, CAPQL, NGAYBD, NGAYKT, MACD, GVCNDT) values ('005',N'Ứng dụng hóa học xanh','200',N'Trường','10/10/2003','12/10/2004',N'ƯDCN','007')
insert into DETAI (MADT, TENDT, KINHPHI, CAPQL, NGAYBD, NGAYKT, MACD, GVCNDT) values ('006',N'Nghiên cứu tế bào gốc','4000',N'Nhà nước','10/20/2006','10/20/2009',N'NCPT','004')
insert into DETAI (MADT, TENDT, KINHPHI, CAPQL, NGAYBD, NGAYKT, MACD, GVCNDT) values ('007',N'HTTT quản lý thư viện ở các trường ĐH','20',N'Trường','5/10/2009','05/10/2010',N'QLGD','001')
----------------
set dateformat dmy

insert into CONGVIEC values ('001',1,N'Khởi tạo và Lập kế hoạch','20/10/2007','20/12/2008')
insert into CONGVIEC values ('001',2,N'Xác định yêu cầu','21/12/2008','21/03/2008')
insert into CONGVIEC values ('001',3,N'Phân tích hệ thống','22/03/2008','22/5/2008')
insert into CONGVIEC values ('001',4,N'Thiết kế hệ thống','23/05/2008','23/06/2008')
insert into CONGVIEC values ('001',5,N'Cài đặt thử nghiệm','24/06/2008','20/10/2008')
insert into CONGVIEC values ('006',1,N'Lấy mẫu','20/10/2006','20/02/2007')
insert into CONGVIEC values ('006',2,N'Nuôi cấy','21/02/2007','21/08/2008')
insert into CONGVIEC values ('002',1,N'Khởi tạo và Lập kế hoạch','10/05/2009','10/07/2009')
insert into CONGVIEC values ('002',2,N'Xác định yêu cầu','11/07/2009','11/10/2009')
insert into CONGVIEC values ('002',3,N'Phân tích hệ thống','12/10/2009','20/12/2009')
insert into CONGVIEC values ('002',4,N'Thiết kế hệ thống','21/12/2009','22/03/2010')
insert into CONGVIEC values ('002',5,N'Cài đặt thử nghiệm','23/03/2010','10/05/2010')
set dateformat mdy
----------------
insert into THAMGIADT values ('003','001',1,1,N'Đạt')
insert into THAMGIADT values ('003','001',2,0,N'Đạt')
insert into THAMGIADT values ('002','001',4,2,N'Đạt')
insert into THAMGIADT values ('003','001',4,1,N'Đạt')
insert into THAMGIADT values ('004','006',1,0,N'Đạt')
insert into THAMGIADT values ('004','006',2,1,N'Đạt')
insert into THAMGIADT values ('006','006',2,1.5,N'Đạt')
insert into THAMGIADT values ('001','002',1,0, null)
insert into THAMGIADT values ('001','002',2,2, null)
insert into THAMGIADT values ('003','002',2,0, null)
insert into THAMGIADT values ('009','002',3,0.5, null)
insert into THAMGIADT values ('009','002',4,1.5, null)
----------------
update KHOA set TRUONGKHOA = '002' where MAKHOA='CNTT'
update KHOA set TRUONGKHOA = '005' where MAKHOA='VL'
update KHOA set TRUONGKHOA = '004' where MAKHOA='SH'
update KHOA set TRUONGKHOA = '007' where MAKHOA='HH'
----------------
update BOMON set TRUONGBM = '002' where MABM=N'HTTT'
update BOMON set TRUONGBM = '001' where MABM=N'MMT'
update BOMON set TRUONGBM = '005' where MABM=N'VLƯD'
update BOMON set TRUONGBM = '004' where MABM=N'VS'
update BOMON set TRUONGBM = '007' where MABM=N'HPT'
----------------
insert into NGUOITHAN values ('001', N'Hùng', '1/14/1990', N'Nam')
insert into NGUOITHAN values ('001', N'Thủy', '12/8/1994', N'Nữ')
insert into NGUOITHAN values ('003', N'Thu', '9/3/1998', N'Nữ')
insert into NGUOITHAN values ('003', N'Hà', '9/3/1998', N'Nữ')
insert into NGUOITHAN values ('008', N'Nam', '5/6/1991', N'Nam')
insert into NGUOITHAN values ('010', N'Nguyệt', '1/14/2006', N'Nữ')
insert into NGUOITHAN values ('007', N'Vy', '2/14/2000', N'Nữ')
insert into NGUOITHAN values ('007', N'Mai', '3/26/2003', N'Nữ')
insert into NGUOITHAN values ('009', N'An', '8/19/1996', N'Nam')

--Câu 1 Cho biết họ tên và mức lương của các giáo viên nữ.
 select HOTEN, LUONG
 from GIAOVIEN
 where PHAI=N'Nữ'

-- Câu 2 Cho biết họ tên các giáo viên và mức lương của họ sau khi tăng 10%
select HOTEN,LUONG*1.1 'LUONG'
from GIAOVIEN

--Câu 3 Cho biết mã của các giáo viên có họtên bắt đầu là “Nguyễn”và lương trên $2000 hoặc, giáo viên là trưởng bộ môn nhận chức sau năm 1995.
select distinct MAGV
from GIAOVIEN gv,BOMON bm
where ( gv.HOTEN like N'Nguyễn%' and gv.LUONG>2000)or (gv.MAGV=bm.TRUONGBM and NGAYNHANCHUC>1995)

--Câu 4 Cho biết tên những giáo viên khoa Công nghệ thông tin
select HOTEN
from GIAOVIEN gv,BOMON bm
where (gv.MABM=bm.MABM and bm.MAKHOA='CNTT')

--Câu 5 Cho biết thông tin của bộ môn cùng thông tin giảng viên làm trưởng bộ môn đó
select *
from BOMON bm, GIAOVIEN gv
where bm.TRUONGBM = gv.MAGV
--Câu 6 Với mỗi giáo viên, hãy cho biết thông tin của bộ môn mà họđang làm việc.
select *
from GIAOVIEN gv,BOMON bm
where gv.MABM=bm.MABM

--Câu 7 Cho biết tên đề tài và giáo viên chủ nhiệm đề tài.
select TENDT, HOTEN
from DETAI dt,GIAOVIEN gv
where dt.GVCNDT=gv.MAGV

--Câu 8 Với mỗi khoa cho biết thông tin trưởng khoa
select MAKHOA, gv.*
from KHOA k, GIAOVIEN gv
where k.TRUONGKHOA=gv.MAGV

--Câu 9 Cho biết các giáo viên của bộ môn “Vi sinh”có tham gia đề tài 006.
select distinct gv.HOTEN
from GIAOVIEN gv, THAMGIADT tg
where gv.MAGV=tg.MAGV and gv.MABM='VS' and tg.MADT='006'

--Câu 10:Với những đề tài thuộc cấp quản lý “Thành phố”, cho biết mã đề tài, đề tài thuộc về chủ đề nào, họ tên người chủnghiệm đề tài cùng với ngày sinh và địa chỉcủa người ấy.
select dt.MADT, dt.MACD, gv.HOTEN HOTENCNDT, gv.NGSINH,gv.DIACHI
from DETAI dt,GIAOVIEN gv
where dt.GVCNDT=gv.MAGV and dt.CAPQL=N'Thành phố'

--Câu 11:Tìm họ tên của từng giáo viên và người phụ trách chuyên môn trực tiếp của giáo viên đó.
select gv1.HOTEN, gv2.HOTEN
from GIAOVIEN gv1,GIAOVIEN gv2
where gv1.GVQLCM=gv2.MAGV

--Câu 12: Tìm họtên của những giáo viên được “Nguyễn Thanh Tùng” phụ trách trực tiếp.
select gv1.HOTEN
from GIAOVIEN gv1, GIAOVIEN gv2
where gv1.GVQLCM=gv2.MAGV and gv2.HOTEN=N'Nguyễn Thanh Tùng'

--Câu 13:Cho biết tên giáo viên là trưởng bộ môn Hệ thống thông tin
select gv.HOTEN
from BOMON bm, GIAOVIEN gv
where bm.TRUONGBM=gv.MAGV and bm.MABM='HTTT'

--Câu 14 Cho biết tên người chủ nhiệm đề tài của những đề tài thuộc chủ đề Quản lý giáo dục.

select gv.HOTEN
from DETAI dt, GIAOVIEN gv,CHUDE cd
where dt.GVCNDT=gv.MAGV and dt.MACD=cd.MACD and cd.TENCD=N'Quản lý giáo dục'
--Câu 15: Cho biết tên các công việc của đề tài HTTT quản lý các trường ĐH có thời gian bắt đầu trong tháng 3/2008.

set dateformat dmy
select cv.TENCV
from CONGVIEC cv,DETAI dt
where cv.MADT=dt.MADT and dt.TENDT like N'HTTT%' and dt.CAPQL=N'Trường' and (cv.NGAYBD between '29/2/2008' and '1/4/2008')

--Câu 16 Cho biết tên giáo viên và tên người quản lý chuyên môn của giáo viên đó
select gv1.HOTEN, gv2.HOTEN NGUOIQUANLI
from GIAOVIEN gv1,GIAOVIEN gv2
where gv1.GVQLCM=gv2.MAGV

--Câu 17 Cho các công việc bắt đầu trong khoảng từ 01/01/2007 đến 01/08/2007
select cv.MADT
from CONGVIEC cv
where cv.NGAYBD between '1/1/2007' and '01/08/2007'

--Câu 18 Cho biết họ tên các giáo viên cùng bộ môn với giáo viên “Trần Trà Hương”
select gv2.HOTEN
from GIAOVIEN gv1, GIAOVIEN gv2
where gv1.HOTEN=N'Trần Trà Hương'and gv1.MABM=gv2.MABM and gv1.HOTEN<>gv2.HOTEN

--Câu 19 Tìm những giáo viên vừa là trưởng bộ môn vừa chủ nhiệm đề tài.
select distinct bm.TRUONGBM
from BOMON bm, DETAI dt
where bm.TRUONGBM=dt.GVCNDT

--Câu 20 Cho biết tên những giáo viên vừa là trưởng khoa và vừa là trưởng bộ môn.
select gv.HOTEN
from KHOA k, BOMON bm,GIAOVIEN gv
where k.TRUONGKHOA=bm.TRUONGBM and k.TRUONGKHOA=gv.MAGV

--Câu 21 Cho biết tên những trưởng bộ môn mà vừa chủ nhiệm đề tài
select distinct gv.HOTEN
from BOMON bm, DETAI dt,GIAOVIEN gv
where bm.TRUONGBM=dt.GVCNDT and bm.TRUONGBM=gv.MAGV

--Câu 22 Cho biết mã số các trưởng khoa có chủ nhiệm đề tài
select k.TRUONGKHOA
from KHOA k, DETAI dt
where k.TRUONGKHOA=dt.GVCNDT

--Câu 23 Cho biết mã số các giáo viên thuộc bộ môn HTTT hoặc có tham gia đề tài mã 001.
select distinct tg.MAGV
from BOMON bm, THAMGIADT tg
where bm.MABM = 'HTTT' and tg.MADT='001'

--Câu 24 Chobiết giáo viên làm việc cùng bộmôn với giáo viên 002
select gv1.MAGV
from GIAOVIEN gv1, GIAOVIEN gv2
where gv1.MAGV='002' and gv1.MABM=gv2.MABM and gv1.MAGV<>gv2.MAGV

-- Câu 25 Tìm những giáo viên là trưởng bộ môn
select distinct gv.MAGV
from BOMON bm, GIAOVIEN gv
where bm.TRUONGBM=gv.MAGV

-- Câu 26 Cho biết họ tên và mức lương của các giáo viên
select gv.HOTEN, gv.LUONG
from GIAOVIEN gv
order by gv.LUONG asc

--Câu 27 Cho biết số lượng giáo viên viên và tổng lương của họ
select count(gv.MAGV) SOGV , sum(gv.LUONG) TONGLUONG
from GIAOVIEN gv

--Câu 28 Cho biết số lượng giáo viên và lương trung bình của từng bộ môn
select count (gv.MAGV) SOGV, avg(gv.LUONG) TRUNGBINH
from GIAOVIEN gv

--Câu 29 Cho biết tên chủ đề và số lượng đề tài thuộc về chủ đề đó
select cd.TENCD, count(cd.MACD)
from CHUDE cd, DETAI dt
where cd.MACD=dt.MACD
group by cd.TENCD

--Câu 30 Cho biết tên giáo viên và số lượng đề tài mà giáo viên đó tham gia
select gv.HOTEN, count(tg.MAGV ) SOLUONG
from GIAOVIEN gv left join THAMGIADT tg
on gv.MAGV=tg.MAGV
group by gv.HOTEN

--Câu 31 Cho biết tên giáo viên và số lượng đề tài mà giáo viên đó làm chủ nhiệm
select gv.HOTEN, count (dt.GVCNDT) SOLUONG
from GIAOVIEN gv left join DETAI dt
on gv.MAGV= dt.GVCNDT
group by gv.HOTEN
order by gv.HOTEN asc

-- Câu 32 Với mỗi giáo viên cho tên giáo viên và số người thân của giáo viên đó
select gv.HOTEN, count(nt.MAGV) SONGUOITHAN
from GIAOVIEN gv left join NGUOITHAN nt
on gv.MAGV=nt.MAGV
group by gv.HOTEN
order by gv.HOTEN asc

-- Câu 33 Cho biết tên những giáo viên đã tham gia từ 3 đề tài trở lên.

select gv.HOTEN, count(gv.HOTEN) SOLUONG
from GIAOVIEN gv, THAMGIADT tg
where tg.MAGV=gv.MAGV
group by gv.HOTEN
having count(gv.HOTEN)>2

-- Câu 34 cho biết số giáo viên tham gia và đề tài ứng dụng hóa học xanh
select count(distinct tg.MAGV) SOLUONG
from THAMGIADT tg, DETAI dt
where tg.MADT=dt.MADT and dt.TENDT=N'Ứng dụng hóa học xanh'
group by dt.MADT

--Câu 35 cho biết lương cao nhất của các giáo viên

select distinct gv1.LUONG
from GIAOVIEN gv1
where gv1.LUONG>= all
(
select gv2.LUONG
from GIAOVIEN gv2
)

-- Câu 36 Cho biết những giáo viên có lương cao nhất
 select gv1.MAGV
from GIAOVIEN gv1
where gv1.LUONG>= all
(
select gv2.LUONG
from GIAOVIEN gv2
)

--Câu 37 CHo biết lương cao nhất trong bộ môn HTTT
 select gv1.LUONG
 from GIAOVIEN gv1
 where gv1.MABM='HTTT' and gv1.LUONG>=all
 (select gv2.LUONG
 from GIAOVIEN gv2
 where gv2.MABM='HTTT'
 )

 --Câu 38 tên giáo viên lớn tuổi nhất trong bộ môn HTTT

select gv1.HOTEN
from GIAOVIEN gv1
where gv1.MABM='HTTT' and gv1.NGSINH<=all
(select gv2.NGSINH
from GIAOVIEN gv2
where gv2.MABM='HTTT')

--Câu 39 cho biết giáo viên nhỏ tuổi nhất trong khoa CNTT

select gv1.MAGV
from GIAOVIEN gv1, BOMON bm1
where gv1.MABM=bm1.MABM and bm1.MAKHOA='CNTT' and gv1.NGSINH>= all
(
select gv2.NGSINH
from GIAOVIEN gv2, BOMON bm2
where gv2.MABM=bm2.MABM and bm2.MAKHOA='CNTT'
)

-- Câu 40 CHo biết tên và tên khoa của những giáo viên có lương cao nhất

select gv1.HOTEN ,k.TENKHOA
from GIAOVIEN gv1, BOMON bm, KHOA k
where gv1.MABM=bm.MABM and bm.MAKHOA=k.MAKHOA and gv1.LUONG>=all
(
select gv2.LUONG
from GIAOVIEN gv2
)

--Câu 41 cho biết những giáo viên có lương cao nhất trong bộ môn của họ
select gv1.MAGV
from GIAOVIEN gv1
where gv1.LUONG>=all
(
select gv2.LUONG
from GIAOVIEN gv2
where gv1.MABM=gv2.MABM
)

-- Câu 42 cho biết tên đề tài mà giáo viên  NGUYEN HOAI AN chưa tham gia
select dt.TENDT
from  DETAI dt
except
(select distinct dt.TENDT
from THAMGIADT tg, GIAOVIEN gv,DETAI dt
where tg.MAGV=gv.MAGV and gv.HOTEN=N'Nguyễn Hoài An' and tg.MADT=dt.MADT)

--Câu 43 ho biết đề tài mà giáo viên  NGUYEN HOAI AN chưa tham gia, xuất ra ten đề tài, người chủ nhiệm đề tài

select dt.TENDT, gv.HOTEN GVCNDT
from  DETAI dt,GIAOVIEN gv
where dt.GVCNDT=gv.MAGV
and not exists
(select *
from THAMGIADT tg, GIAOVIEN gv
where tg.MAGV=gv.MAGV and gv.HOTEN=N'Nguyễn Hoài An' and tg.MADT=dt.MADT)

--Câu 44 CHo biết những giáo viên thuộc khoa CNTT mà chưa tham gia đề tài nào

select gv.MAGV
from GIAOVIEN gv, BOMON bm
where gv.MABM=bm.MABM and bm.MAKHOA='CNTT'
and not exists
(
select *
from THAMGIADT tg
where tg.MAGV=gv.MAGV
)

--Câu 45 tìm những giáo viên không tham gia bất kì đề tài nào
--Cach1
select gv.MAGV
from GIAOVIEN gv
where
not exists
(
select *
from THAMGIADT tg
where tg.MAGV=gv.MAGV)

--Cách 2
select gv.MAGV
from GIAOVIEN gv
except
(select distinct tg.MAGV
from THAMGIADT tg
)

-- Câu 46 CHo biết gv có lương lớn hơn lương của Nguyễn hoài an

select gv.MAGV
from GIAOVIEN gv
where gv.LUONG> all
( select gv1.LUONG
from GIAOVIEN gv1
where gv1.HOTEN=N'Nguyễn Hoài An')

-- Câu 47 tìm những trưởng bộ môn tham gia tối thiểu 1 đề tài
select bm.TRUONGBM GIAOVIEN
from BOMON bm
where exists
( select *
from THAMGIADT tg
where tg.MAGV=bm.TRUONGBM
)
order by GIAOVIEN asc

-- Câu 48 tìm những giáo viên cùng tên, cùng giới tính với các giáo viên khác trong cùng 1 bộ môn
select gv.MAGV
from GIAOVIEN gv
where exists
( select *
from GIAOVIEN gv1
where gv.HOTEN=gv1.HOTEN and gv.PHAI=gv1.PHAI and gv.MABM=gv.MABM and gv.MAGV<>gv1.MAGV
)

--Câu 49  Tìm những giáo viên có lương lớn hơn lương của ít nhất một giáo viên bộ môn “Công nghệ phần mềm”

select gv.MAGV
from GIAOVIEN gv
where exists
(
select *
from GIAOVIEN gv1, BOMON bm
where gv1.MABM=bm.MABM and bm.TENBM=N'Công nghệ phần mềm' and gv.LUONG>gv1.LUONG)
 -- Câu 50 Tìm những giáo viên có lương lớn hơn lương của tất cả giáo viên thuộc bộ môn “Hệthống thông tin”

 select gv.MAGV
 from GIAOVIEN gv
 where gv.LUONG> all
 (
 select gv1.LUONG
 from GIAOVIEN gv1,BOMON bm
where gv1.MABM=bm.MABM and bm.TENBM=N'Hệ thống thông tin'
)

-- Câu 51 Cho biết khoa có đông giáo viên nhất

select  k.MAKHOA, count(gv.MAGV) SOLUONG
from GIAOVIEN gv,BOMON bm, KHOA k
where gv.MABM=bm.MABM and bm.MAKHOA=k.MAKHOA
group by k.MAKHOA
having count(gv.MAGV) >=all
(
select count(gv.MAGV) SOLUONG
from GIAOVIEN gv,BOMON bm, KHOA k
where gv.MABM=bm.MABM and bm.MAKHOA=k.MAKHOA
group by k.MAKHOA
)

-- Câu 52 Cho biết họtên giáo viên chủ nhiệm nhiều đề tài nhất
select gv.HOTEN, count(dt.MADT) SOLUONG
from DETAI dt, GIAOVIEN gv
where dt.GVCNDT=gv.MAGV
group by gv.HOTEN
having count(dt.MADT)>=all
(
select count(dt1.MADT)
from DETAI dt1
group by dt1.GVCNDT
)

-- Câu 53 Cho biết mã bộ môn có nhiều giáo viên nhất
select gv.MABM
from GIAOVIEN gv
group by gv.MABM
having count(gv.MAGV)>=all
(
select count(gv1.MAGV)
from GIAOVIEN gv1
group by gv1.MABM)

--Câu 54 Cho biết tên giáo viên và tên bộ môn của giáo viên tham gia nhiều đề tài nhất

select gv.HOTEN, bm.TENBM
from GIAOVIEN gv, THAMGIADT tg, BOMON bm
where gv.MAGV=tg.MAGV and gv.MABM=bm.MABM
group by gv.HOTEN, bm.TENBM
having count(tg.MADT)>= all
(
select count(tg1.MADT)
from THAMGIADT tg1
group by tg1.MAGV
)

-- Câu 55 Cho biết tên giáo viên tham gia nhiều đề tài nhất của bộ môn HTTT
select gv.HOTEN
from THAMGIADT tg, GIAOVIEN gv
where tg.MAGV=gv.MAGV and gv.MABM='HTTT'
group by gv.HOTEN
having count(tg.MADT)>=all
(
select count(tg1.MADT)
from THAMGIADT tg1, GIAOVIEN gv1
where tg1.MAGV=gv1.MAGV and gv1.MABM='HTTT'
group by gv1.HOTEN)

-- Câu 56 Cho biết tên giáo viên và tên bộ môn của giáo viên có nhiều người thân nhất

select gv.HOTEN, bm.TENBM
from GIAOVIEN gv,NGUOITHAN nt, BOMON bm
where gv.MAGV=nt.MAGV and gv.MABM=bm.MABM
group by gv.HOTEN, bm.TENBM
having count(nt.TEN)>=all
(select count (nt1.TEN)
from GIAOVIEN gv1, NGUOITHAN nt1
where nt1.MAGV=gv1.MAGV
group by gv1.HOTEN)

-- Câu 57 Cho biết tên trưởng bộ môn mà chủ nhiệm nhiều đề tài nhất.
select gv.HOTEN
from DETAI dt, GIAOVIEN gv, BOMON bm
where dt.GVCNDT=gv.MAGV and bm.TRUONGBM=gv.MAGV
group by gv.HOTEN
having count(dt.GVCNDT)>=all
(
select count(dt1.GVCNDT)
from DETAI dt1, GIAOVIEN gv1, BOMON bm1
where dt1.GVCNDT=gv1.MAGV and bm1.TRUONGBM=gv1.MAGV
group by gv1.HOTEN)

-- Câu 58: Cho biết tên giáo viên nào mà tham gia đề tài đủ tất cả các chủ đề
select gv.HOTEN
from GIAOVIEN gv, THAMGIADT tg, DETAI dt
where tg.MAGV=gv.MAGV and tg.MADT=dt.MADT
group by gv.HOTEN
having count(distinct dt.MACD)=
(
select count(cd.MACD)
from CHUDE cd
)

--Câu 59: Cho biết tên đề tài nào mà được tất cảcác giáo viên của bộ môn HTTT tham gia.
select  dt.TENDT
from THAMGIADT tg, GIAOVIEN gv, DETAI dt
where tg.MAGV=gv.MAGV and gv.MABM='HTTT' and dt.MADT=tg.MADT
group by tg.MADT, dt.TENDT
having count(distinct gv.MAGV)=
(
select count(gv1.MAGV)
from GIAOVIEN gv1
where gv1.MABM='HTTT'
)

--Câu 60 Cho biết tên đềtài có tất cả giảng viên bộmôn “Hệthống thông tin”tham gia
select  dt.TENDT
from THAMGIADT tg, GIAOVIEN gv, DETAI dt, BOMON bm
where tg.MAGV=gv.MAGV and bm.TENBM=N'Hệ thống thông tin' and dt.MADT=tg.MADT and gv.MABM=bm.MABM
group by tg.MADT, dt.TENDT
having count(distinct gv.MAGV)=
(
select count(gv1.MAGV)
from GIAOVIEN gv1,BOMON bm1
where gv1.MABM=bm1.MABM and bm1.TENBM=N'Hệ thống thông tin'
)

--Câu 61: Cho biết giáo viên nào đã tham gia tất cảcác đềtài có mã chủđềlà QLGD.
select tg.MAGV
from THAMGIADT tg
group by tg.MAGV
having count(distinct tg.MADT)=
(
select count(distinct dt.MADT)
from DETAI dt
where dt.MACD='QLGD'
)
-- Câu 62 Cho biết tên giáo viên nào tham gia tất cảcác đề tài mà giáo viên Trần Trà Hương đã tham gia.
select gv1.HOTEN
from THAMGIADT tg1,GIAOVIEN gv1
where tg1.MAGV=gv1.MAGV and gv1.HOTEN!=N'Trần Trà Hương' and  exists
(
select *
from THAMGIADT tg,GIAOVIEN gv
where tg.MAGV=gv.MAGV and gv.HOTEN=N'Trần Trà Hương' and tg1.MADT=tg.MADT
)
group by gv1.HOTEN
having count(distinct tg1.MADT)=
(
select count(distinct tg2.MADT)
from THAMGIADT tg2,GIAOVIEN gv2
where tg2.MAGV=gv2.MAGV and gv2.HOTEN=N'Trần Trà Hương'
)

-- Câu 63 Cho biết tên đề tài nào mà được tất cả các giáo viên của bộ môn Hóa Hữu Cơ tham gia.

select dt.TENDT
from DETAI dt, THAMGIADT tg, GIAOVIEN gv, BOMON bm
where dt.MADT=tg.MADT and tg.MAGV=gv.MAGV and gv.MABM=bm.MABM and bm.TENBM=N'Hóa Hữu Cơ'
group by dt.TENDT
having count(distinct gv.MAGV)=
(
select count(distinct gv2.MAGV)
from GIAOVIEN gv2, BOMON bm2
where gv2.MABM=bm2.MABM and bm2.TENBM=N'Hóa Hữu Cơ'
)

-- Câu 64 Cho biết tên giáo viên nào mà tham gia tất cả các công việc của đề tài 006
select gv.HOTEN
from THAMGIADT tg, GIAOVIEN gv
where tg.MAGV=gv.MAGV and tg.MADT='006'
group by gv.MAGV, gv.HOTEN
having count(tg.MADT)=
(
select count(cv.MADT)
from CONGVIEC cv
where cv.MADT='006'
)

-- Câu 65 Cho biết giáo viên nào đã tham gia tất cảcác đề tài của chủ đề Ứng dụng công nghệ
select tg1.MAGV
from THAMGIADT tg1,DETAI dt1, CHUDE cd1
where tg1.MADT=dt1.MADT and dt1.MACD=cd1.MACD and cd1.TENCD=N'Ứng dụng công nghệ'
group by tg1.MAGV
having count(distinct tg1.MADT)=
(select count(dt.MADT)
from DETAI dt, CHUDE cd
where dt.MACD=cd.MACD and cd.TENCD=N'Ứng dụng công nghệ')

-- Câu 66-> 77
-- Câu 78 Xóa các đề tài thuộc chủ đề “NCPT”
delete
from DETAI
where MACD='NCPT'
-- Câu 79 Xuất ra thông tin của giáo viên (MAGV, HOTEN) và mức lương của giáo viên.
-- Mức lương được xếp theo quy tắc: Lương của giáo viên < $1800 : “THẤP” ;
--  Từ$1800 đến $2200: TRUNG BÌNH; Lương > $2200: “CAO”
select MAGV, HOTEN,
(case when LUONG<1800 then N'THAP'
	when LUONG>=1800 and LUONG<2200 then N'Trung Bình'
	else N'Cao'
end) PHANLOAILUONG
from GIAOVIEN

-- Câu 80 Xuất ra thông tin giáo viên (MAGV, HOTEN) và xếp hạng dựa vào mức lương.
-- Nếu giáo viên có lương cao nhất thì hạng là 1.

select MAGV, HOTEN, rank() over(order by LUONG desc ) XEPHANGLUONG
from GIAOVIEN

--Câu 81 Xuất ra thông tin thu nhập của giáo viên. Thu nhập của giáo viên được tính bằng LƯƠNG + PHỤCẤP.
 --Nếu giáo viên là trưởng bộ môn thì PHỤ CẤP là 300, và giáo viên là trưởng khoa thì PHỤCẤP là 600.
 select*,LUONG+
 (case when MAGV in
			( select TRUONGKHOA
				from KHOA) then 600
		else 300
	end) THUNHAP
 from GIAOVIEN
 -- Cau 82: Xuất ra năm mà giáo viên dự kiến sẽnghĩ hưu với quy định: Tuổi nghỉ hưu của Nam là 60, của Nữ là 55.
 select HOTEN, year(NGSINH)+
 (case when PHAI=N'Nam' then 60
		else 55
	end) NAMNGHIHUU
 from GIAOVIEN
```
