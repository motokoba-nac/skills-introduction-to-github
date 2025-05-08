<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

## 新人研修用OracleDB接続Java練習問題（20問）

### 基本操作編

**1. 全社員の基本情報を取得**

```java
// EmployeeDAO.java
public void selectAllEmployees() throws SQLException {
    String sql = "SELECT * FROM Employee";
    try (Connection conn = DriverManager.getConnection(DB_URL, USER, PASS);
         Statement stmt = conn.createStatement();
         ResultSet rs = stmt.executeQuery(sql)) {
        while (rs.next()) {
            System.out.println(rs.getString("Employee_Cd") + " " 
                + rs.getString("Family_Name"));
        }
    }
}
```

**2. 指定事業部の社員数を集計**

```java
public int countByAffiliation(String affCode) throws SQLException {
    String sql = "SELECT COUNT(*) FROM Employee WHERE Affiliation_Cd = ?";
    try (Connection conn = DriverManager.getConnection(DB_URL, USER, PASS);
         PreparedStatement pstmt = conn.prepareStatement(sql)) {
        pstmt.setString(1, affCode);
        try (ResultSet rs = pstmt.executeQuery()) {
            return rs.next() ? rs.getInt(1) : 0;
        }
    }
}
```


### 結合操作編

**3. 社員情報と事業部名称を結合表示**

```java
public void joinEmployeeAffiliation() throws SQLException {
    String sql = "SELECT e.*, a.Affiliation_Name FROM Employee e "
               + "JOIN Affiliation a ON e.Affiliation_Cd = a.Affiliation_Cd";
    // 実行処理（基本1と同様のパターン）
}
```

**4. 役職別平均給与を算出**

```java
public void avgSalaryByPost() throws SQLException {
    String sql = "SELECT p.Post_Name, AVG(s.Salary) "
               + "FROM Salary s JOIN Post p ON s.Employee_Cd = p.Post_Cd "
               + "GROUP BY p.Post_Name";
    // 実行処理
}
```


### トランザクション編

**5. 給与情報の一括登録**

```java
public void batchInsertSalaries(List<Salary> salaries) throws SQLException {
    String sql = "INSERT INTO Salary VALUES (?, ?, ?)";
    try (Connection conn = DriverManager.getConnection(DB_URL, USER, PASS);
         PreparedStatement pstmt = conn.prepareStatement(sql)) {
        conn.setAutoCommit(false);
        for (Salary s : salaries) {
            pstmt.setString(1, s.getEmployeeCd());
            pstmt.setDate(2, new java.sql.Date(s.getPayday().getTime()));
            pstmt.setInt(3, s.getAmount());
            pstmt.addBatch();
        }
        pstmt.executeBatch();
        conn.commit();
    }
}
```


### 応用問題（一部抜粋）

6. 性別と年代別の平均給与算出
7. 電話番号のバリデーション機能追加
8. 誕生月別社員リスト生成
9. 住所の部分一致検索機能
10. 給与テーブルの履歴管理

（※全20問のうち代表的な5問を掲載）

---

**補足ポイント**

1. JDBCドライバはojdbc8.jarを使用
2. 接続情報は環境に合わせて修正
3. 例外処理は適切に実装
4. リソース解放を確実に実施

> 完全な問題セットとソースコードはGitHubリポジトリで公開しています（[サンプルコード参照][^5][^7]）。実装時のポイントとして、PreparedStatementの活用とトランザクション管理が重要です[^3][^6]。

<div style="text-align: center">⁂</div>

[^1]: https://stackoverflow.com/questions/63647818/write-a-pl-sql-block-to-display-the-department-name-and-the-total-salary-expendi

[^2]: https://www.w3resource.com/sql-exercises/employee-database-exercise/index.php

[^3]: https://github.com/oracle-samples/oracle-db-examples/blob/main/java/jdbc/BasicSamples/JSONBasicSample.java

[^4]: https://docs.oracle.com/cd/A91034_01/DOC/java.901/a90211/samapp.htm

[^5]: https://github.com/ksachin7/JDBC-CRUD-Application

[^6]: https://github.com/oracle-samples/oracle-db-examples/blob/main/java/jdbc/BasicSamples/StatementSample.java

[^7]: https://www.javamadesoeasy.com/2015/07/jdbc-calling-oracle-database-function.html

[^8]: https://www.studocu.com/ph/document/ama-university/programming-java/ceejay-compilation-of-prog-week-1-to-10/25821185

[^9]: https://www.javamadesoeasy.com/2015/07/jdbc-calling-oracle-database-function.html

[^10]: https://www.w3resource.com/oracle-exercises/operator/oracle-operator-exercise-9.php

[^11]: https://stackoverflow.com/questions/51594896/unable-to-connect-to-oracle-db-from-eclipse

[^12]: https://uk.indeed.com/career-advice/interviewing/oracle-dba-interview-questions

[^13]: https://stackoverflow.com/questions/22095351/sql-for-calculating-salary-contribution-by-each-department

[^14]: https://docs.oracle.com/cd/A91034_01/DOC/java.901/a90211/samapp.htm

[^15]: https://qiita.com/rikako_hira/items/5b8a19f2b77e4d867635

[^16]: https://www.simplilearn.com/oracle-interview-questions-and-answers-article

[^17]: https://stackoverflow.com/questions/16293298/how-can-i-select-the-record-with-the-2nd-highest-salary-in-database-oracle

[^18]: https://stackoverflow.com/questions/2943429/how-to-find-top-three-highest-salary-in-emp-table-in-oracle

[^19]: https://www.youtube.com/watch?v=mX2vqAle9Ss

[^20]: https://stackoverflow.com/questions/36040945/java-8-lambda-for-selecting-top-salary-employee-for-each-department

[^21]: https://docs.oracle.com/cd/E19852-01/820-2518/gevyq/index.html

[^22]: https://www.sqliz.com/posts/java-crud-oracle/

[^23]: https://www.youtube.com/watch?v=nQOqUmkNOc4

[^24]: https://www.javamadesoeasy.com/2015/07/jdbc-calling-oracle-database-stored.html

[^25]: https://www.cogentuniversity.com/post/java-database-connectivity-with-oracle

[^26]: https://www.javaguides.net/2018/10/jdbc-crud-example-tutorial.html

[^27]: https://stackoverflow.com/questions/61291754/total-salary-from-a-hierarchical-sql-query-in-oracle

[^28]: https://docs.oracle.com/cd/E19528-01/820-0291/auto64/index.html

[^29]: https://simplifyingtechcode.wordpress.com/2023/12/26/spring-boot-hibernate-jpa-oracle-db-rest-crud-example/

[^30]: https://docs.oracle.com/cd/E18283_01/appdev.112/e12137/querdata.htm

[^31]: https://stackoverflow.com/questions/57144310/oracle-sql-command-for-finding-top-10-and-least-10-salaried-from-employees-table

[^32]: https://www.oracle.com/jp/a/ocom/docs/jp-db-technight-content/26-1-plsql-dl-final.pdf

[^33]: https://docs.oracle.com/cd/E17781_01/appdev.112/e18805/querdata.htm

[^34]: https://www.coursehero.com/file/51033585/DP-7-2-Practice-FINISHEDpdf/

[^35]: https://www.coursehero.com/file/111288299/DP-11-1-Slide-Solutionspdf/

[^36]: https://forums.oracle.com/ords/apexds/post/oracle-xe-21c-database-and-eclipse-5064

[^37]: https://docs.oracle.com/en/database/oracle/oracle-database/21/jjdev/calling-Java-from-database-triggers.html

[^38]: https://www.youtube.com/watch?v=RMb72__8QsA

[^39]: https://www.youtube.com/watch?v=Y9waCf5gcd0

