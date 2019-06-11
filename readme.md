# EveryCalendar
## 1. 패키지 분류
### 1.1 - res
어플리케이션에 사용되는 리소스들이 들어있는 패키지.

주로 Activity, Fragment, View를 구성하는 Layout 파일들이며,
각종 데이터들(String, Color 등)을 포함한다.

### 1.2 - com.skku.everycalendar

`edu.skku.everycalendar.activities` : Activity들과 MainActivity를 구성하는 Fragment들이 있는 패키지 


`edu.skku.everycalendar.dataType` : 어플리케이션에서 사용되는 Data를 저장, 사용하기 위한 구조체들이 있는 패키지


`edu.skku.everycalendar.everytime` : Everytime 관련 기능들을 담당하는 Class들이 있는 패키지


`edu.skku.everycalendar.friends` : 어플리케이션의 친구 관련 기능, 레이아웃 등에 사용되는 Class들이 있는 패키지


`edu.skku.everycalendar.googleCalendar` : 구글 캘린더 API 관련 기능들을 담당하는 Class들이 있는 패키지


`edu.skku.everycalendar.monthItems` : 어플리케이션에서 날짜 선택을 위한 달력 구현에 사용되는 패키지


`edu.skku.everycalendar.service` : 어플리케이션에서 조율 신청을 받기 위한 **Always-On Service** 를 구현하는 패키지


`edu.skku.everycalendar.table` : 어플리케이션에서 사용되는 TimeTable Custom View 구현에 사용되는 패키지


`edu.skku.everycalendar.functions` : 어플리케이션에서 사용되는 기타 함수들이 구현된 Class들이 있는 패키지

---

## 2. 화면 구성

`edu.skku.everycalendar.activities`가 담당하는 영역이다

### 2.1 - LoginActivity
로그인 기능을 담당하는 로그인 화면을 총괄하는 객체이다.

`edu.skku.everycalendar.everytime`의 `LoginReqest`를 통해 로그인을 진행한다.

로그인에 성공하면 자동 로그인 체크박스를 확인하여 SharedPreferences로 자동 로그인을 세팅한다.

### 2.2 - MainActivity
대부분의 기능들을 담당하는 메인 화면을 총괄하는 객체이다

각각의 기능에 대한 화면을 구성하기 위해

- `TableFragment`
- `FriendsFragment`
- `GoogleCalFragment`
- `AdjustFragment`

로 화면을 나누었다.

### 2.3 - TableFragment
사용자의 한 주 일정을 테이블로 보여주는 객체이다.

`edu.skku.everycalendar.table`를 사용하여 `Timetable`을 표시한다

테이블 뷰 위에 Button을 사용하여 일정을 표시하며, 일정을 선택하면 Toast로 일정을 출력한다.

### 2.4 - FriendsFragment
친구 목록을 Listview로 보여주는 객체이다

`edu.skku.everycalendar.AddFriendRequest`를 사용하여 에브리타임에서 친구 목록을 받아온다.

친구를 선택하면 `FriendsActivity`로 화면을 전환하여 친구의 시간표를 출력한다.

### 2.5 - GoogleCalFragment
구글 캘린더 관리 화면을 담당하는 객체이다

기본적으로 한 주의 일정을 ListView로 보여주며

`edu.skku.everycalendar.googleCalendar.GoogleCalRequest`를 사용하여

일정의 추가, 제거, 변경 작업을 수행한다.

### 2.6 - AdjustFragment
친구들과의 일정을 조율하는 화면을 담당하는 객체이다.

조율에 필요한 요소들을 모두 선택한 후 조율 버튼을 누르면

`edu.skku.everycalendar.functions.JoinSchedule`

`edu.skku.everycalendar.functions.JoinSchedulReq`

을 사용하여 조율 작업을 수행한다.

수행 결과는 `AdjustResultActivity`로 전환하여 출력한다.

### 2.7 - FriendsActivity
친구 시간표를 출력하는 화면을 담당하는 객체이다.

`edu.skku.everycalendar.table`를 사용하여 `Timetable`을 표시한다.

### 2.8 - AdjustResultActivity
조율 결과를 출력하는 화면을 담당하는 객체이다

`edu.skku.everycalendar.table`를 사용하여 `Timetable`을 표시한다.

출력 결과는 사진으로 저장하거나, 공유할 수 있다.

---

## 3. 주요 기능들

### 3.1 - Everytime 로그인

```JAVA
import edu.skku.everycalendar.everytime.LoginRequest;

LoginRequest loginRequest = new LoginRequest(id, pw);
loginRequest.JWRequest();

if(loginRequest.getLogined()){
    // Login!!
} 
else{
    // Login Failed
}                       
```

### 3.2 - Everytime 시간표 불러오기

```JAVA
import edu.skku.everycalendar.everytime.MyTimeTableReq;
MyTimeTableReq myTimeTableReq = new MyTimeTableReq(cookie);

myTimeTableReq.makeTimeTable();
ArrayList<TimetableData> classList = myTimeTableReq.getClassList();
```

### 3.3 - Everytime 친구 리스트 불러오기

```JAVA
import edu.skku.everycalendar.everytime.FriendsListRequest;
FriendsListRequest friendsListRequest = new FriendsListRequest(cookie);

friendsListRequest.makeFriendList();
Map<String, FriendInfoData> friendList = friendsListRequest.getFriendList();
```

### 3.4 - Everytime 친구 시간표 불러오기

```JAVA
import edu.skku.everycalendar.everytime.FriendTimetableReq;
FriendTimetableReq friendTimeTableReq = new FriendTimetableReq(cookie, key);

friendTimeTableReq.makeTimeTable();
ArrayList<TimetableData> classList = friendTimeTableReq.getClassList();
```

### 3.5 - Everytime 친구 추가 요청하기

```JAVA
import edu.skku.everycalendar.everytime.AddFriendRequest;
AddFriendRequest addFriendRequest = new AddFriendRequest(cookie);

String result = addFriendRequest.addFriend(id);
```

### 3.6 - 같은 수업을 듣는 친구 추천

```JAVA
import edu.skku.everycalendar.functions.RecommendFriend;
RecommendFriend recommendFriend = new RecommendFriend();

recommendFriend.recommend();
HashMap<String, String> result = recommendFriend.getFriendList();
```

### 3.7 - 에브리타임 친구가 EveryCalendar 사용자인지 확인

```JAVA
import edu.skku.everycalendar.functions.CheckOurUser;
CheckOurUser chkOurUser = new CheckOurUser();

chkOurUser.getUserList();
if(chkOurUser.chkUser(FriendInfoData)){
    //Our User!
}
else{
    //Not User
}
```

### 3.8 - 구글 캘린더에서 일정 받아오기

```JAVA
import edu.skku.everycalendar.googleCalendar.GoogleCalRequest;
GoogleCalRequest googleCalRequest = new GoogleCalRequest(Context, Activity, accountName);

googleCalRequest.setModeGet(StartDate, EndDate);
googleCalRequest.getCalendarData();
ArrayList<TimetableData> events = googleCalRequest.getEvents();
```

### 3.8 - 구글 캘린더에서 일정 추가하기

```JAVA
import edu.skku.everycalendar.googleCalendar.GoogleCalRequest;
GoogleCalRequest googleCalRequest = new GoogleCalRequest(Context, Activity, accountName);

googleCalRequest.setModeAdd(Summary, Location, Description, StartDateTime, EndDateTime);
googleCalRequest.addEventToCalendar();
```

### 3.8 - 시간표 조율하기

```JAVA
import edu.skku.everycalendar.functionsJoinSchedule;
import edu.skku.everycalendar.functionsJoinSchedulReq;

JoinSchedule joinSchedule = new JoinSchedule(startHour, endHour);
JoinSchedulReq joinSchedulReq = new JoinSchedulReq();

joinSchedulReq.joinRequest(startDate, endDate, friendList, joinSchedule)
```