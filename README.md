# CentOS_Linux에 제큐어 Mysql 설치 후  이클립스 연동시 썼던 eclipse script
```
package test;


import java.sql.*;


public class test {

	static final String JDBC_DRIVER = "com.mysql.cj.jdbc.Driver";
	static final String DB_URL = "jdbc:mysql://..../mysql?useSSL=false";
	static final String USERNAME = "user";
	static final String PASSWORD = "1234";
	public static String getEncryptedValue() {
		Connection conn = null;
		Statement stmt = null;
		ResultSet rs = null;
		String encryptedValue = null;
		
		try {
			Class.forName(JDBC_DRIVER);
			conn = DriverManager.getConnection(DB_URL, USERNAME, PASSWORD);
			stmt = conn.createStatement();
			String sql1 = " from dual;";
			rs = stmt.executeQuery(sql1);
			rs.next();
			encryptedValue = rs.getString(1);
			System.out.print("enc= " + encryptedValue);
		}catch(SQLException se1) {
			se1.printStackTrace();
		}catch(Exception ex) {
			ex.printStackTrace();
		}finally {
            try {
                if (rs != null) rs.close();
                if (stmt != null) stmt.close();
                if (conn != null) conn.close();
            } catch (SQLException se2) {
                se2.printStackTrace();
            }
		}
		return encryptedValue;

	}
	
	public static void main(String[] args) {
		getEncryptedValue();
	}

}


```
사용자 생성
```
CREATE USER '새로운사용자이름'@'%' IDENTIFIED BY '새로운비밀번호';

```
모든 권한 부여
```
ALTER USER 'username'@'%' IDENTIFIED WITH 'mysql_native_password' BY 'password';
FLUSH PRIVILEGES;
GRANT ALL PRIVILEGES ON mysql.* TO 'user'@'%';
FLUSH PRIVILEGES;
```
톰캣 연동 코드
```
<%@ page import="패키지.클래스명" %>

<% String encryptedValue = test.getEncryptedValue(); %>

<% request.setAttribute("encryptedValue", encryptedValue); %>

<p>Encrypted Value: <%= encryptedValue %></p>
```
