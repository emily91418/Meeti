MeetiFramework
==============

MeetiFramework provides developers to use Meeti API create service with cloud APP  


Currently, we've provide two examples to developer to use as a reference  
1. [instant chatroom online](https://github.com/MOBAGEL/Meeti-Chatroom-example-iOS)    
2. [remote car controller](https://github.com/MOBAGEL/Meeti-Remote-control-car-example-iOS)  


## Step One  

- Download MeetiFramework and add it to your existing iphone project
- Apply for App_ID and APP_SecretKey (http://meeti.ex.mobagel.com/admin/login.php ) and add it to your existing iphone project
- Apply for a testing account. You are welcome to email us to ask how


## Things to Pay Attention for

- If you get Link framework error message while compiling, please check if Building setting -> framework search path is corresponding to framework's file path.  
- User for Xcode6 and the following error message appear

```
dyld: Library not loaded: @rpath/MeetiFramework.framework/MeetiFramework  
```

please see  
![alt tag](https://cloud.githubusercontent.com/assets/1070832/5391832/a4d863bc-815a-11e4-870b-71bed7f95506.png)  
## Contact Us  

- If you need any help, feel free to contact us (us@mobagel.com)  
- If you find any new bug, you can create a new issue
- If you want more functions, you can create a new issue
  
## API Return Value 

Each API will all have the following return value. Exceptions will be explained individually later.
-  ok : API execute correctly  
-  input_error : enter the wrong parameter  
-  auth_error : authentication error (token invalid; ex: trying to be another person that isn't you)  
-  auth2_error : authentication error (ex: trying to send message to group A while you aren't in that group)
-  timeout : exceed time limit  
  
## Get Notification (NSNotification)

Since we aren't sure when we will get message, we will need to set NSNotification to get data/information  

Usage of NSNotification as the following    
[NSNotification](http://stackoverflow.com/questions/2191594/send-and-receive-messages-through-nsnotificationcenter-in-objective-c)

You can see sample code as a reference to use Meeti
https://github.com/MOBAGEL/Meeti-Chatroom-example-iOS  


Related Commands
*   MeetiFriendProfileChangeNotification:

    > Get notification when a friend's information has been updated
*   MeetiFriendImageChangeNotification:

    > Get notification when a friend's profile picture has been updated
*   MeetiGetMessageNotification:

    > Get notification when a group get text message
*   MeetiGetImageNotification:

    > Get notification when a group get picture message

*   MeetiGetGroupNotification:

    > Get notification when a group has been updated
*   MeetiFriendAddme:

    > Get notification when a friend request is revieved

# How to Use  

### Initialize MeetiServer  (App_ID && APP_Key)
***
MeetiServer Provides functions for development environment and connection with Meeti server．  

First time to use the object function of Meeti Server in a project, you need to initialize with the following code:  

```objective-c
-(void)initWithAPPID:(NSString*)appID secretKey:(NSString*)secretKey  
```

| Parameter Name |  Parameter Type | Parameter Introduction  |
|:-----------------------------------:|:-----------------------------------:|:---------------------------------------------:|
|     appID       | NSString | Provides product ID to developers|
|     secretKey   | NSString |  Provide product key to developers|

### Initialize MeetiServer  (Userid && Password)
***
MeetiServer Provides functions of development environemnt and connection of Meeti server．  

Function to initialize a user account    

```objective-c
-(void)updateUserID:(NSString*)userid password:(NSString*)password
```

| Parameter Name |  Parameter Type  | Parameter Introduction  |
|:-----------------------------------:|:-----------------------------------:|:---------------------------------------------:|
|     userid       | NSString |  Provides product ID to developers|
|     password   | NSString |   Provide product key to developers|



### Add New Group
***
When creating a new group in a APPID, people who initialize and the admins are all founder of the group  

```objective-c
-(void)setGroup:(NSString*)name
           type:(NSString*)type
        shandle:(MeetiRequestHandleCallBack)shandle
        fhandle:(MeetiRequestFailHandleCallBack)fhandle
            
```
|     Parameter Name    |  Parameter Type  |      Parameter Introduction       |
|:-----------------------------------:|:-----------------------------------:|:---------------------------------------------:|
|     name       | NSString | Group's Name |
|     type       | NSString | Group's type. Currently we provide public and private channels|
|     shandle   | Block  | Meeti Server新增群組回傳訊息|
|     fhandle    | Block | Meeti Server新增群組失敗訊息|

傳回  
```
{ "groups": [ { "group_id": "app_public_02", "group_name": "PublicGroup2", "group_type": "1", "group_members": "auid123,guid123,guid223", "group_admins": "auid123" }, { "group_id": "app_public_01", "group_name": "PublicGroup1", "group_type": "1", "group_members": "auid323,auid333,guid112", "group_admins": "guid112,auid322" } ] }
```

### 取得群組
***
取得這個APP ID 內某些特定群組

```objective-c
-(void)getGroupWithGid:(NSArray*)groups
               shandle:(MeetiRequestHandleCallBack)shandle
               fhandle:(MeetiRequestFailHandleCallBack)fhandle;
            
```
|     Parameter Name     |  Parameter Type  |      Parameter Introduction       |
|:-----------------------------------:|:-----------------------------------:|:---------------------------------------------:|
|     groups      | NSArray:NSString  | 取得特定群組|
|     shandle     | Block| Meeti Server取得群組回傳訊息|
|     fhandle     | Block| Meeti Server取得群組失敗訊息|

### 加入群組(Add)
***
將某個特定人加進群組  (加人必須要有管理員權限)

```objective-c
-(void)addGroupMembers:(NSArray*)UIDs
                   gid:(NSString*)gid
               shandle:(MeetiRequestHandleCallBack)shandle
               fhandle:(MeetiRequestFailHandleCallBack)fhandle
```
|     Parameter Name     |  Parameter Type  |      Parameter Introduction       |
|:-----------------------------------:|:-----------------------------------:|:---------------------------------------------:|
|     UIDs        | NSArray:NSString  | ID of those people who wants to be in the group|
|     gid         | NSString          | 需要加進的群組id|
|     shandle     | Block| Meeti Server加入群組回傳訊息|
|     fhandle     | Block| Meeti Server加入群組失敗訊息|

### 加入群組(Join)
***
加入群組  (加入的群組必須為公開群組 or 頻道)

```objective-c
-(void)joinToGroup:(NSString*)gid
           shandle:(MeetiRequestHandleCallBack)shandle
           fhandle:(MeetiRequestFailHandleCallBack)fhandle
```
|     Parameter Name     |  Parameter Type  |      Parameter Introduction       |
|:-----------------------------------:|:-----------------------------------:|:---------------------------------------------:|
|     gid         | NSString  | 要加進的群組id|
|     shandle     | Block     | Meeti Server加入群組回傳訊息|
|     fhandle     | Block     | Meeti Server加入群組失敗訊息|

### 離開群組
***
離開群組

```objective-c
-(void)leaveToGroup:(NSString*)gid
            shandle:(MeetiRequestHandleCallBack)shandle
            fhandle:(MeetiRequestFailHandleCallBack)fhandle
```
|     Parameter Name     |  Parameter Type  |      Parameter Introduction       |
|:-----------------------------------:|:-----------------------------------:|:---------------------------------------------:|
|     gid         | NSString  | 要加進的群組id|
|     shandle     | Block| Meeti Server離開群組回傳訊息|
|     fhandle     | Block| Meeti Server離開群組失敗訊息|

### 發送文字訊息
***
傳送訊息至群組（發送訊息前必須要在群組內）

```objective-c
-(void)setMessage:(NSString*)message
            group:(NSString*)gid
          shandle:(MeetiRequestHandleCallBack)shandle
          fhandle:(MeetiRequestFailHandleCallBack)fhandle
```
|     Parameter Name     |  Parameter Type  |      Parameter Introduction       |
|:-----------------------------------:|:-----------------------------------:|:---------------------------------------------:|
|     message     | NSString  |將要發送的訊息  |
|     gid        | NSString  | 要將訊息發送到某群組|
|     shandle     | Block| Meeti Server發送訊息回傳訊息|
|     fhandle     | Block| Meeti Server發送訊息失敗訊息|


### 發送圖片訊息
***
傳送圖片至群組（發送圖片前必須要在群組內）

```objective-c
-(void)setImageMessage:(UIImage*)image
                 group:(NSString*)gid
               shandle:(MeetiRequestHandleCallBack)shandle
               fhandle:(MeetiRequestFailHandleCallBack)fhandle
```
|     Parameter Name     |  Parameter Type  |      Parameter Introduction       |
|:-----------------------------------:|:-----------------------------------:|:---------------------------------------------:|
|     image     | UIImage  |將要發送的圖片  |
|     gid        | NSString  | 要將圖片發送到某群組|
|     shandle     | Block| Meeti Server發送訊息回傳訊息|
|     fhandle     | Block| Meeti Server發送訊息失敗訊息|

### 取得訊息
***
從特定群組中取得某個時間點的訊息

```objective-c
-(void)getMessageWithGid:(NSString*)gid
          loadAllMessage:(BOOL)loadAll
                 shandle:(MeetiRequestHandleCallBack)shandle
                 fhandle:(MeetiRequestFailHandleCallBack)fhandle
```
|     Parameter Name     |  Parameter Type  |      Parameter Introduction       |
|:-----------------------------------:|:-----------------------------------:|:---------------------------------------------:|
|     gid        | NSString  |  從特定群組取得訊息|
|     loadAll     | BOOL  | 是否要取得這個群組內所有訊息  |
|     shandle     | Block| Meeti Server取得訊息回傳訊息|
|     fhandle     | Block| Meeti Server取得訊息失敗訊息|

傳回:  
```
{ "messages": [ { "msg_content": "world!!", "msg_senderid": "auid123", "msg_type": "1", "msg_groupid": "app_public_02", "msg_time": 1407837653982 }, { "msg_content": "hello~~", "msg_senderid": "auid113", "msg_type": "1", "msg_groupid": "app_public_02", "msg_time": 1407843940276 } ] }
```


### 設定個人資訊
***
設定個人資訊，要取得可以用getprofile函示拿到

```objective-c
-(void)setProfile:(NSString*)name
            phone:(NSString*)phone
          company:(NSString*)company
     companyPhone:(NSString*)companyPhone
      description:(NSString*)description
         keywords:(NSArray*)searchKeywords
          shandle:(MeetiRequestHandleCallBack)shandle
          fhandle:(MeetiRequestFailHandleCallBack)fhandle
```
|     Parameter Name     |  Parameter Type  |      Parameter Introduction       |
|:-----------------------------------:|:-----------------------------------:|:---------------------------------------------:|
|     name        | NSString  |  個人資訊名稱|
|     phone        | NSString  |  個人資訊電話|
|     company        | NSString  |  個人資訊公司名稱|
|     companyPhone        | NSString  |  個人資訊公司電話|
|     description        | NSString  |  個人詳細資訊|
|     searchKeywords        | NSArray:NSString  |  可以被搜尋找到的keywords|
|     shandle     | Block| Meeti Server設定個人資訊回傳訊息|
|     fhandle     | Block| Meeti Server設定個人資訊失敗訊息|

### 取得個人資訊(使用者id)
***
取得特定人員的資訊，使用使用者id搜尋

```objective-c
-(void)getProfileByUIDs:(NSArray *)UIDs
                shandle:(MeetiRequestHandleCallBack)shandle
                fhandle:(MeetiRequestFailHandleCallBack)fhandle
```
|     Parameter Name     |  Parameter Type  |      Parameter Introduction       |
|:-----------------------------------:|:-----------------------------------:|:---------------------------------------------:|
|     UIDs        | NSArray:NSString  |  要取得的使用者id|
|     shandle     | Block| Meeti Server取得個人資訊回傳訊息|
|     fhandle     | Block| Meeti Server取得個人資訊失敗訊息|

傳回  
```
{ "profiles": [ { "person_company": "", "person_notify": 1, "person_companyphone": "", "person_email": "auid123", "person_name": "勤奮的演員", "person_keyword": "", "person_searchable": "0", "person_phone": "" }, { "person_company": "", "person_notify": 1, "person_companyphone": "", "person_email": "auid222", "person_name": "喜歡貝果的臺北人", "person_keyword": "", "person_searchable": "0", "person_phone": "" } ] }
```

### 取得個人資訊(關鍵字)
***
取得特定人員的資訊，使用關鍵字搜尋

```objective-c
-(void)getProfileByKeywords:(NSArray *)keywords
                shandle:(MeetiRequestHandleCallBack)shandle
                fhandle:(MeetiRequestFailHandleCallBack)fhandle
```
|    Parameter Name     |  Parameter Type  |      Parameter Introduction       |
|:-----------------------------------:|:-----------------------------------:|:---------------------------------------------:|
|     keywords        | NSArray:NSString  | 要搜尋的關鍵字名稱 |
|     shandle     | Block| Meeti Server取得個人資訊回傳訊息|
|     fhandle     | Block| Meeti Server取得個人資訊失敗訊息|

傳回  
```
{ "profiles": [ { "person_company": "", "person_notify": 1, "person_companyphone": "", "person_email": "auid123", "person_name": "勤奮的演員", "person_keyword": "", "person_searchable": "0", "person_phone": "" }, { "person_company": "", "person_notify": 1, "person_companyphone": "", "person_email": "auid222", "person_name": "喜歡貝果的臺北人", "person_keyword": "", "person_searchable": "0", "person_phone": "" } ] }
```

### 設定個人的大頭圖
***
取得特定人員的資訊，使用關鍵字搜尋

```objective-c
-(void)setProfilePicture:(UIImage*)pic
                 shandle:(MeetiRequestHandleCallBack)shandle
                 fhandle:(MeetiRequestFailHandleCallBack)fhandle
```

|     Parameter Name     |  Parameter Type  |      Parameter Introduction       |
|:-----------------------------------:|:-----------------------------------:|:---------------------------------------------:|
|     pic        | UIImage  | 要設定的大頭圖圖片 |
|     shandle     | Block| Meeti Server設定個人圖片資訊回傳訊息|
|     fhandle     | Block| Meeti Server設定個人圖片資訊失敗訊息|


### 取得特定人員的大頭圖
***

```objective-c
-(void)getProfilePictureWithUID:(NSString*)UID
                            toPath:(NSString*)path
                           shandle:(MeetiRequestHandleCallBack)shandle
                           fhandle:(MeetiRequestFailHandleCallBack)fhandle
```

|     Parameter Name     |  Parameter Type  |      Parameter Introduction       |
|:-----------------------------------:|:-----------------------------------:|:---------------------------------------------:|
|     UID        | NSString  | 要取得大頭圖圖片的使用者 |
|     path        | NSString  | 要儲存在哪個路徑 |
|     shandle     | Block| Meeti Server取得圖片資訊回傳訊息|
|     fhandle     | Block| Meeti Server取得圖片資訊失敗訊息|


### 取得自己的朋友
***

```objective-c
-(void)getFriendsWithProfile:(BOOL)withProfile
                     shandle:(MeetiRequestHandleCallBack)shandle
                     fhandle:(MeetiRequestFailHandleCallBack)fhandle
```

|     Parameter Name     |  Parameter Type  |      Parameter Introduction       |
|:-----------------------------------:|:-----------------------------------:|:---------------------------------------------:|
|     withProfile        | BOOL  |  是否要包含Profile資訊 |
|     shandle     | Block| Meeti Server取得朋友資訊回傳訊息|
|     fhandle     | Block| Meeti Server取得朋友資訊失敗訊息|

### 加入朋友
***
```objective-c
-(void)addFriendByUIDs:(NSArray *)uids
                  shandle:(MeetiRequestHandleCallBack)shandle
                  fhandle:(MeetiRequestFailHandleCallBack)fhandle
```

|     Parameter Name     |  Parameter Type  |      Parameter Introduction       |
|:-----------------------------------:|:-----------------------------------:|:---------------------------------------------:|
|     uids        | NSArray:NSString  |  將要加入的使用者id |
|     shandle     | Block| Meeti Server加入朋友回成功傳訊息|
|     fhandle     | Block| Meeti Server加入朋友失敗訊息|

### 追蹤某個使用者
***
```objective-c
-(void)follow:(NSString*)uid
      shandle:(MeetiRequestHandleCallBack)shandle
      fhandle:(MeetiRequestFailHandleCallBack)fhandle
```

|     Parameter Name     |  Parameter Type  |      Parameter Introduction       |
|:-----------------------------------:|:-----------------------------------:|:---------------------------------------------:|
|     uid        |  NSString  |  要追蹤的使用者id |
|     shandle     | Block| Meeti Server追蹤成功回傳訊息|
|     fhandle     | Block| Meeti Server追蹤失敗訊息|

### 取消追蹤某個使用者
***
```objective-c
-(void)unfollow:(NSString*)uid
        shandle:(MeetiRequestHandleCallBack)shandle
        fhandle:(MeetiRequestFailHandleCallBack)fhandle
```

|     Parameter Name     |  Parameter Type  |      Parameter Introduction       |
|:-----------------------------------:|:-----------------------------------:|:---------------------------------------------:|
|     uid        |  NSString  |  要取消追蹤的使用者id |
|     shandle     | Block| Meeti Server取消追蹤成功回傳訊息|
|     fhandle     | Block| Meeti Server取消追蹤失敗訊息|


### 取得追蹤的人列表
***
```objective-c
-(void)getFollow:(MeetiRequestHandleCallBack)shandle
         fhandle:(MeetiRequestFailHandleCallBack)fhandle
```

|     Parameter Name     |  Parameter Type  |      Parameter Introduction       |
|:-----------------------------------:|:-----------------------------------:|:---------------------------------------------:|
|     shandle     | Block| Meeti Server取得列表回傳訊息|
|     fhandle     | Block| Meeti Server取得列表失敗訊息|
傳回：  
```
{ "followers": [ "auid112", "auid222", "guid123" ], "following": [ "auid133", "auid222", "guid133" ] }
```

### 設定推播的token  
***
目前不支援


### 新增待辦事項
***
目前不支援


### 取得待辦事項的列表
***
目前不支援
