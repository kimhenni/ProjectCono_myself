## 전체 회원 정보


##### AdminDAO()
```java
// 전체 회원 정보
	public ArrayList<String[]> selectAllMember(int pageNum, int listLimit) {
		ArrayList<String[]> memberList = new ArrayList<String[]>();
		
		int startRow =(pageNum -  1)  * listLimit;
		
		try {
			String sql = "SELECT * FROM member ORDER BY member_date DESC LIMIT ?,?";
			pstmt = con.prepareStatement(sql);
			pstmt.setInt(1, startRow);
			pstmt.setInt(2, listLimit);
			
			rs = pstmt.executeQuery();
			
			while(rs.next()) {
				System.out.println("1");
				ResultSet rs2 = null;
				ResultSet rs3 = null;
				
				String sql2 = "SELECT shop_idx FROM shop WHERE member_id=?";
				pstmt2 = con.prepareStatement(sql2);
				System.out.println(rs.getString("member_id"));
				pstmt2.setString(1, rs.getString("member_id"));
				rs2 = pstmt2.executeQuery();
				
				if(rs2.next()) {
					System.out.println("2");

					String sql3 = "SELECT coin_total FROM coin WHERE member_id=?";
					pstmt3 = con.prepareStatement(sql3);
					pstmt3.setString(1, rs.getString("member_id"));
					rs3 = pstmt3.executeQuery();
					if(rs3.next()) {

						String[] member = new String[9];
						member[0] = (rs.getString("member_id"));
						member[1] = (rs.getString("member_nick"));
						member[2] = (rs.getString("member_email"));
						member[3] = (rs2.getString("shop_idx"));
						member[4] = (rs3.getString("coin_total"));
						member[5] = (rs.getString("member_phone"));
						member[6] = (rs.getString("member_birth"));
						member[7] = (rs.getString("member_date"));
						member[8] = (rs.getString("member_status"));
						
						memberList.add(member);
						System.out.println("3");
//				close(rs2);
//				close(rs3);
//				close(pstmt2);
//				close(pstmt3);
						
						
					}
					
					
				}
			}
			
		} catch (SQLException e) {
			System.out.println("SQL 구문오류 - selectAllMember");
			e.printStackTrace();
		} finally {
			close(pstmt);
			close(rs);
		}
		
		return memberList;
	}
  
  ```

##### MemberListService
```java
	public ArrayList<String[]> getMemberList(int pageNum, int listLimit) {
		System.out.println("MemberListService - getMemberList()");
		
		ArrayList<String[]> memberList  = null;
		
		Connection con = getConnection();
		
		AdminDAO  adminDAO = AdminDAO.getInstance();
		
		adminDAO.setConnection(con);
		
		// 게시물 목록 조회
		memberList = adminDAO.selectAllMember(pageNum, listLimit);
		
		close(con);
		
		return memberList;
	}
	```
	
	
