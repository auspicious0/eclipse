# CentOS_Linux에 제큐어 Mysql 설치 후  이클립스 연동시 썼던 eclipse script
```
package test;


import java.sql.*;


public class test {

	static final String JDBC_DRIVER = "com.mysql.cj.jdbc.Driver";
	static final String DB_URL = "jdbc:mysql://192.168.1.93/mysql?useSSL=false";
	static final String USERNAME = "user2";
	static final String PASSWORD = "Dkfvk!@234";
	public static void main(String[] args) throws SQLException {
		Connection conn = null;
		Statement stmt = null;
		ResultSet rs = null;
		
		try {
			Class.forName(JDBC_DRIVER);
			conn = DriverManager.getConnection(DB_URL, USERNAME, PASSWORD);
			stmt = conn.createStatement();
			String sql1 = "select xdb_enc('normal', '123123123123') from dual;";
			rs = stmt.executeQuery(sql1);
			rs.next();
			String sql = rs.getString(1);
			System.out.print("enc= " + sql);
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
	}

}
```
사용자 생성
```
CREATE USER '새로운사용자이름'@'%' IDENTIFIED BY '새로운비밀번호';

```
모든 권한 부여
```
ALTER USER 'username'@'%' IDENTIFIED WITH 'mysql_native_password' BY 'Dkfvk!@234';
FLUSH PRIVILEGES;
GRANT ALL PRIVILEGES ON mysql.* TO 'user2'@'%';
FLUSH PRIVILEGES;
```
