MIS
=====
原型介绍
-------
#1.主页
##1.设备查看
![]( https://github.com/CUMTElite2014/Sun/blob/master/%E4%BF%9D%E5%85%BB%E8%AE%B0%E5%BD%95.png ) 
##2.设备录入
![]( https://github.com/CUMTElite2014/Sun/blob/master/%E8%AE%BE%E5%A4%87%E5%BD%95%E5%85%A5.png) 
##3.保养记录
![](https://github.com/CUMTElite2014/Sun/blob/master/%E4%BF%9D%E5%85%BB%E8%AE%B0%E5%BD%95.png) 
##4.材料消耗
![](https://github.com/CUMTElite2014/Sun/blob/master/%E6%9D%90%E6%96%99%E6%B6%88%E8%80%97.png ) 
#2.数据库关系（ER图）
![](https://github.com/CUMTElite2014/Sun/blob/master/ER%E5%9B%BE.png) 
#3.sql语句
1.type表（设备类别表)
  id
  name（设备类型名字)
2.content表（设备修理内容表）
  id  设备ID
  content 设备修理内容
  Equiment表（设备表）

Eid  自增长主键
Enumber 设备型号
ET_id（设备类别表ID）
4.maintenance表（保修详情表）
id  自增长主键
group 小组名
person 检修人
type  检查类型（年检）
time  检查时间
explain 说明
Eid         （设备表）外键
circle       年检之类的时间（年检填365）
SQL语句
1.查询保修信息
*根据设备号查询保修信息*/
/*（详情图1）*/
select * from equipment,maintenance,type,consume
where 
    equipment.Eid=(SELECT Eid from equipment where equipment.Enumber="1001")        
and
    check_detail.Eid = (SELECT Eid from equipment where equipment.Enumber="1001")
and
    check_type.Eid = (SELECT Eid from equipment where equipment.Enumber="1001")
and
    check_consume.Eid = (SELECT Eid from equipment where equipment.Enumber="1001") 
and
    check_detail.CD_id=check_type.CD_id  /*详情表和内容表ID一致（详情表ID）*/
/*（详情图2）*/     
/* 查看消耗品 内容表和消耗表ID一致（详情表ID） 删除中间下方的注释即可*/          
/*  
and
    check_detail.CD_id=check_consume.CD_id  
*/  
/*详情表和消耗表ID一致（详情表ID）*/ 
/*（详情图3）*/ 
/* 查看某一时间的保修详情  删除下方中间的注释即可*/           
/*
and
    CD_time='2015-10-11' 
*/
详情图1
数据库 详情图2
数据库 详情图3 数据库

2.信息录入

/*保养信息录入 需要修改内容，否则重复*/
/*填写保养人员等详细*/
INSERT INTO `cart`.`check_detail` (`CD_group`, `CD_person`, `CD_type`, `CD_time`, `Eid`) VALUES ('风组', '小小风', '半年检', '2016-       09-10', '5');
/*填写保养内容*/
INSERT INTO `cart`.`check_type` (`CT_content`, `CT_situaction`, `Eid`, `CD_id`) VALUES ('检查6000V接线盒内瓷瓶、端子；', '正常', '1',     '5');
INSERT INTO `cart`.`check_type` (`CT_content`, `CT_situaction`, `CT_note`, `Eid`, `CD_id`) VALUES ('接线盒内卫生清洁；', '异常', '更换    接线盒', '1', '5');
INSERT INTO `cart`.`check_type` (`CT_content`, `CT_situaction`, `Eid`, `CD_id`) VALUES ('电机上是否有杂物，多要通知工艺处理。', '正常',     '2', '6');
INSERT INTO `cart`.`check_type` (`CT_content`, `CT_situaction`, `Eid`, `CD_id`) VALUES ('电机接地线连接是否可靠；', '正常', '2', '6');
数据库

3.预警信息

/*预警信息*/
select *
from equipment,(select check_detail.Eid,check_detail.CD_circle,max(check_detail.CD_time) as timee from check_detail group by             check_detail.Eid) AS T
where T.Eid=equipment.Eid 
and 
    (T.CD_circle - DATEDIFF(now(),T.timee)) <4    
group by equipment.Eid
