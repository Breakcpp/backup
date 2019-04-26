---
title: JavaJDBC使用
date: 2019-04-26 17:30:29
tags: Java
---

# JDBC使用
JDBC API 允许用户访问任何形式的表格数据，尤其是存储在关系数据库中的数据。

执行流程：

连接数据源，如：数据库。

为数据库传递查询和更新指令。

处理数据库响应并返回的结果。

JDBC 驱动下载:[JDBC Driver](http://central.maven.org/maven2/mysql/mysql-connector-java/)

# 实现方法

## 连接控制器
``` java
package com.conn;

import java.sql.Connection;
import java.sql.DriverManager;

//加载数据库连接和驱动
public class ConnController {
    public static final String DB_HOST = "jdbc:mysql://localhost:8889/departments";
    public static final String DB_USERNAME = "root";
    public static final String DB_PASSWORD = "root";

    private static Connection conn = null;


    static {
        try{
            Class.forName("com.mysql.jdbc.Driver");
            conn = DriverManager.getConnection(DB_HOST,DB_USERNAME,DB_PASSWORD);
        }catch (Exception e){
            e.printStackTrace();
        }
    }

    public static Connection getConn(){
        return conn;
    }
}
```

## 实现模型实体类
``` java
package com.model;

public class AdminModel {
    public int id;
    public String admin;
    public String password;


    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getAdmin() {
        return admin;
    }

    public void setAdmin(String admin) {
        this.admin = admin;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }
}
```
## DAO操作层
``` java
package com.dao;

import com.conn.ConnController;
import com.model.AdminModel;

import java.sql.*;
import java.util.LinkedList;
import java.util.List;

public class AdminDao {

    //数据连接初始化
    Connection conn  = ConnController.getConn();

    //添加
    public void addAdmin(AdminModel model) throws Exception{

        String sql = "INSERT INTO dp_admin(username,password) values (?,?)";
        PreparedStatement ptmt = conn.prepareStatement(sql);

        ptmt.setString(1,model.getAdmin());
        ptmt.setString(2,model.getPassword());

        ptmt.execute();
    }

    //更新修改
    public void UpdateAdmin(AdminModel model) throws Exception{
        String sql = "UPDATE dp_admin set username=?,password=? where id=?";
        PreparedStatement ptmt = conn.prepareStatement(sql);

        ptmt.setString(1,model.getAdmin());
        ptmt.setString(2,model.getPassword());
        ptmt.setInt(3,model.getId());
        ptmt.execute();
        System.out.println("Update Success");
    }

    //删除
    public void deleteAdmin(int id) throws SQLException {
        String sql = "DELETE from dp_admin where id=?";
        PreparedStatement ptmt = conn.prepareStatement(sql);

        ptmt.setInt(1,id);
        ptmt.execute();
        System.out.println("Delete Success");
    }

    //查询 返回集合
    public List<AdminModel> selectAdmin() throws SQLException {
        Statement stam = conn.createStatement();
        ResultSet result = stam.executeQuery("SELECT * FROM dp_admin");
        List<AdminModel> res = new LinkedList<AdminModel>();

        while(result.next()){
            AdminModel am = new AdminModel();
            am.setId(result.getInt("id"));
            am.setAdmin(result.getString("username"));
            am.setPassword(result.getString("password"));

            res.add(am);

        }
        return res;
    }

}
```


## 测试使用
``` java
package com;

import com.conn.ConnController;
import com.dao.AdminDao;
import com.model.AdminModel;

import java.util.List;

public class JavaJDBC {
    public static void main(String args[]){
        ConnController conn = new ConnController();

        try{

            /*AdminDao dao = new AdminDao();
            AdminModel model = new AdminModel();
            model.setAdmin("test");
            model.setPassword("test");
            dao.addAdmin(model);*/
            //许多方法建立后 直接dao.所需要操作的方法(参数);
            AdminDao dao = new AdminDao();

            List<AdminModel> value =  dao.selectAdmin();

            for(AdminModel arr : value){
                System.out.println(arr.getAdmin());
                System.out.println(arr.getPassword());
            }

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```


