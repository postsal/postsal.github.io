---
layout: post
title: MyBatis小问题(1)-Mapper中错误No constructor found
categories: Mybatis
description: MyBatis小问题(1)-Mapper中错误No constructor found
keywords: No constructor found
---
javabean代码：

	package com.github.postsal.bean;
	
	public class User {
	
	    private int id;
	    private String name;
	    private int age;
	    
	    public User(int id, String name, int age) {
	        super();
	        this.id = id;
	        this.name = name;
	        this.age = age;
	    }
	
	    public int getId() {
	        return id;
	    }
	
	    public void setId(int id) {
	        this.id = id;
	    }
	
	    public String getName() {
	        return name;
	    }
	
	    public void setName(String name) {
	        this.name = name;
	    }
	
	    public int getAge() {
	        return age;
	    }
	
	    public void setAge(int age) {
	        this.age = age;
	    }
	
	    @Override
	    public String toString() {
	        return "User [id=" + id + ", name=" + name + ", age=" + age + "]";
	    }
	}

在JUnit中写了一个测试程序，用来查询。

	@Test
	public void testSelect() {
	    SqlSessionFactory factory = MyBatisUtils.getFactory();
	    SqlSession openSession = factory.openSession();
	    UserMapper mapper = openSession.getMapper(UserMapper.class);
	    User selectUser = mapper.selectUser(1);
	    System.out.println(selectUser);
	}

运行结果报错：

	org.apache.ibatis.exceptions.PersistenceException: 
	
	### Error querying database. Cause: org.apache.ibatis.executor.ExecutorException: No constructor found in com.github.postsal.bean.User matching [java.lang.Integer, java.lang.String, java.lang.Integer]
	### The error may exist in com/tszhao/mapper/UserMapper.java (best guess)
	### The error may involve com.github.postsal.mapper.UserMapper.selectUser
	### The error occurred while handling results
	### SQL: select * from users where id=?
	### Cause: org.apache.ibatis.executor.ExecutorException: No constructor found in com.github.postsal.bean.User matching [java.lang.Integer, java.lang.String, java.lang.Integer]
	at org.apache.ibatis.exceptions.ExceptionFactory.wrapException(ExceptionFactory.java:30)
	...

看样子，应该跟构造函数相关。找不到与User相关的构造函数。试着在User中增加了一个默认的构造函数，通过。。。

