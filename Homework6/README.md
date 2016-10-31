
#MIS

SQL查询语句

----------
######1.根据Test1的用户名和用户ID以及数据库中对应的用户user表，查询对订单(order)页面中的操作权限(sys_button)
#######逻辑过程
   IF 用户名=test1 存在 then  <br>
    	do	查询ID</br>
    		根据ID和用户名查询用户对order中的sys_button的权限
    else
    	do  结束查询
