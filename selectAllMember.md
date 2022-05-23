## 전체 회원 정보


> dao에서 배열로 보냈을 때

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
	
	##### MemberListAction
	```java
	public class MemberListAction implements Action {

	@Override
	public ActionForward execute(HttpServletRequest request, HttpServletResponse response) throws Exception {
		System.out.println("MemberListAction");
		
		ActionForward forward = null;
		
		// 페이징 처리
		int pageNum = 1;
		int listLimit = 8;
		int pageLimit = 5;
		
		if(request.getParameter("page") != null) {
			pageNum = Integer.parseInt(request.getParameter("page"));
		}
		
		// 총 게시물 수 조회
		MemberListService service = new MemberListService();
		int listCount = service.getListCount();
		
		// 게시물 목록 가져오기
		ArrayList<String[]> memberList = service.getMemberList(pageNum, listLimit);
		
		//페이징 처리 계산
		int maxPage = (int)Math.ceil((double)listCount / listLimit);

		// 2. 현재 페이지에서 보여줄 시작 페이지 번호(1, 11, 21 등의 시작 번호) 계산
		int startPage = ((int)((double)pageNum / pageLimit + 0.9) - 1) * pageLimit + 1;

		// 3. 현재 페이지에서 보여줄 끝 페이지 번호(10, 20, 30 등의 끝 번호) 계산
		int endPage = startPage + pageLimit - 1;

		// 4. 만약, 끝 페이지(endPage)가 현재 페이지에서 표시할 총 페이지 수(maxPage)보다 클 경우
		//    끝 페이지 번호를 총 페이지 수로 대체
		if(endPage > maxPage) {
			endPage = maxPage;
		}
		
		// 페이징 처리 정보를 PageInfo 객체에 저장
		PageInfo pageInfo = new PageInfo(pageNum, maxPage, startPage, endPage, listCount);
		
		// 뷰페이지로 전달
		request.setAttribute("pageInfo", pageInfo);
		request.setAttribute("memberList", memberList);
		
		forward = new ActionForward();
		forward.setPath("../admin/admin_member.jsp");
		forward.setRedirect(false);
		
		return forward;
	}

}
```
