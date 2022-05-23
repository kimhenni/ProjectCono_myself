## 전체회원 정보 조회(join절 사용)
##### 성공코드

```java
	
	// 전체 회원 정보
		public ArrayList<String[]> selectAllMember(int pageNum, int listLimit) {
			ArrayList<String[]> memberList = null;
			
			int startRow =(pageNum -  1)  * listLimit;
			
			try {
				String sql = "select m.member_id, s.shop_idx, c.coin_total,  m.member_nick, m.member_email, m.member_phone, m.member_birth, m.member_date, m.member_status "
						+ "from member m left outer join shop s "
						+ "on m.member_id = s.member_id "
						+ "left outer join coin c "
						+ "on s.member_id = c.member_id "
						+ "ORDER BY m.member_date DESC LIMIT ?,?";
				pstmt = con.prepareStatement(sql);
				pstmt.setInt(1, startRow);
				pstmt.setInt(2, listLimit);
				
				rs = pstmt.executeQuery();
				
				// 전체 게시물을 저장하는 객체 생성
				memberList = new ArrayList<String[]>();
				
				while(rs.next()) {
					String[] member = new String[9];
					member[0] = (rs.getString("member_id"));
					member[1] = (rs.getString("member_nick"));
					member[2] = (rs.getString("member_email"));
					member[3] = (rs.getString("shop_idx"));
					member[4] = (rs.getString("coin_total"));
					member[5] = (rs.getString("member_phone"));
					member[6] = (rs.getString("member_birth"));
					member[7] = (rs.getString("member_date"));
					member[8] = (rs.getString("member_status"));
					
					memberList.add(member);
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
  
  ### ArrayList<String[]> 이기때문에 EL사용할 때도 String[]로 받아야함
  
  ```jsp
  <c:if test="${not empty memberList and pageInfo.getListCount() > 0}">
					<c:forEach var="member" items="${memberList }">
						<tr> 
							<td><input type="checkbox" name="chk"></td> 
							<td>${member[0] }</td>
							<td>${member[1] }</td>
							<td>${member[2] }</td>
							<td>${member[3] }</td>
							<td>${member[4] }</td>
		 					<td>${member[5] }</td>
							<td>${member[6] }</td>
							<td>${member[7] }</td>
							<td>
								<input type="button" value="탈퇴" onclick="confirmExit('${member[0] }', '${pageInfo.getPageNum() }' )">
							</td> 
						</tr> 
					</c:forEach>
				</c:if>
```
