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
|     shandle   | Block  | Message of successfully adding to the group that Meeti Server replys|
|     fhandle    | Block | Message of failure to add to the group that Meeti Server replys|

傳回  
```
{ "groups": [ { "group_id": "app_public_02", "group_name": "PublicGroup2", "group_type": "1", "group_members": "auid123,guid123,guid223", "group_admins": "auid123" }, { "group_id": "app_public_01", "group_name": "PublicGroup1", "group_type": "1", "group_members": "auid323,auid333,guid112", "group_admins": "guid112,auid322" } ] }
```

### Get Group
***
Get certian groups' IDs in this APP

```objective-c
-(void)getGroupWithGid:(NSArray*)groups
               shandle:(MeetiRequestHandleCallBack)shandle
               fhandle:(MeetiRequestFailHandleCallBack)fhandle;
            
```
|     Parameter Name     |  Parameter Type  |      Parameter Introduction       |
|:-----------------------------------:|:-----------------------------------:|:---------------------------------------------:|
|     groups      | NSArray:NSString  | Get a specific group|
|     shandle     | Block| Message of successfully getting the group's ID that Meeti Server replys|
|     fhandle     | Block| Message of failure to get the group's ID that Meeti Server replys|

### Adding People to Group(Add)
***
Add a specific person into a group (only admin can add people to groups)  

```objective-c
-(void)addGroupMembers:(NSArray*)UIDs
                   gid:(NSString*)gid
               shandle:(MeetiRequestHandleCallBack)shandle
               fhandle:(MeetiRequestFailHandleCallBack)fhandle
```
|     Parameter Name     |  Parameter Type  |      Parameter Introduction       |
|:-----------------------------------:|:-----------------------------------:|:---------------------------------------------:|
|     UIDs        | NSArray:NSString  | ID of those people who wants to be in the group|
|     gid         | NSString          | ID of the group wish to be added to|
|     shandle     | Block| Message of successfully adding people to the group that Meeti Server replys|
|     fhandle     | Block| Message of failure to add people to the group message that Meeti Server replys|

### Join a Group(Join)
***
Join a group (the group has to be a public group/channel)

```objective-c
-(void)joinToGroup:(NSString*)gid
           shandle:(MeetiRequestHandleCallBack)shandle
           fhandle:(MeetiRequestFailHandleCallBack)fhandle
```
|     Parameter Name     |  Parameter Type  |      Parameter Introduction       |
|:-----------------------------------:|:-----------------------------------:|:---------------------------------------------:|
|     gid         | NSString  | ID of the group wish to be added to|
|     shandle     | Block     | Message of successfully joining to the group that Meeti Server replys|
|     fhandle     | Block     | Message of failure to join the group message that Meeti Server replys|

### Leave the Group
***
Leave a group

```objective-c
-(void)leaveToGroup:(NSString*)gid
            shandle:(MeetiRequestHandleCallBack)shandle
            fhandle:(MeetiRequestFailHandleCallBack)fhandle
```
|     Parameter Name     |  Parameter Type  |      Parameter Introduction       |
|:-----------------------------------:|:-----------------------------------:|:---------------------------------------------:|
|     gid         | NSString  | ID of the group wish to leave|
|     shandle     | Block| Message of successfully leaving the group that Meeti Server replys|
|     fhandle     | Block| Message of failure to leave the group that Meeti Server replys|

### Send a text message
***
Send a message to group (has to be in the group before sending it)

```objective-c
-(void)setMessage:(NSString*)message
            group:(NSString*)gid
          shandle:(MeetiRequestHandleCallBack)shandle
          fhandle:(MeetiRequestFailHandleCallBack)fhandle
```
|     Parameter Name     |  Parameter Type  |      Parameter Introduction       |
|:-----------------------------------:|:-----------------------------------:|:---------------------------------------------:|
|     message     | NSString  | The message wish to be sent  |
|     gid        | NSString  |  The groups ID that you wish to send the message to|
|     shandle     | Block| Message of successfully sending a text message to a group that Meeti Server replys|
|     fhandle     | Block| Message of failure to send a text message to group that Meeti Server replys|


### Send a picture message
***
Send a picture message to a group (has to be in the group before sending it)

```objective-c
-(void)setImageMessage:(UIImage*)image
                 group:(NSString*)gid
               shandle:(MeetiRequestHandleCallBack)shandle
               fhandle:(MeetiRequestFailHandleCallBack)fhandle
```
|     Parameter Name     |  Parameter Type  |      Parameter Introduction       |
|:-----------------------------------:|:-----------------------------------:|:---------------------------------------------:|
|     image     | UIImage  | The image that is wish to be sent  |
|     gid        | NSString  |The group's ID that you wish to send the image to|
|     shandle     | Block| Message of successfully sending a picture to a group that Meeti Server replys|
|     fhandle     | Block| Message of failure to send a picture to a group that Meeti Server replys|

### Get messages
***
Get messages from a specific group at a specific time

```objective-c
-(void)getMessageWithGid:(NSString*)gid
          loadAllMessage:(BOOL)loadAll
                 shandle:(MeetiRequestHandleCallBack)shandle
                 fhandle:(MeetiRequestFailHandleCallBack)fhandle
```
|     Parameter Name     |  Parameter Type  |      Parameter Introduction       |
|:-----------------------------------:|:-----------------------------------:|:---------------------------------------------:|
|     gid        | NSString  |  Get the messages from a specific group|
|     loadAll     | BOOL  | Whether or not to get all the messages from the group  |
|     shandle     | Block| Message of successfully getting the messages from a specific group that Meeti Server replys|
|     fhandle     | Block| Message of failure to get the messages from a specific group that Meeti Server replys|

傳回:  
```
{ "messages": [ { "msg_content": "world!!", "msg_senderid": "auid123", "msg_type": "1", "msg_groupid": "app_public_02", "msg_time": 1407837653982 }, { "msg_content": "hello~~", "msg_senderid": "auid113", "msg_type": "1", "msg_groupid": "app_public_02", "msg_time": 1407843940276 } ] }
```


### Set Personal Profile
***
Setting the personal information (You can get the profile by using the function "getprofile") 

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
|     name        | NSString  |  Name|
|     phone        | NSString  |  Phone|
|     company        | NSString  |  Company's name|
|     companyPhone        | NSString  |  Company's phone number|
|     description        | NSString  |  Personal detail information|
|     searchKeywords        | NSArray:NSString  |  keywords that can be referred to|
|     shandle     | Block| Message of successfully setting the personal profile information that Meeti Server replys|
|     fhandle     | Block| Message of failure to set the personal profile information that Meeti Server replys|

### Get Personal Information (User ID)
***
Get a specific personal information by searching the ID

```objective-c
-(void)getProfileByUIDs:(NSArray *)UIDs
                shandle:(MeetiRequestHandleCallBack)shandle
                fhandle:(MeetiRequestFailHandleCallBack)fhandle
```
|     Parameter Name     |  Parameter Type  |      Parameter Introduction       |
|:-----------------------------------:|:-----------------------------------:|:---------------------------------------------:|
|     UIDs        | NSArray:NSString  |  Get the user ID|
|     shandle     | Block| Message of successfully getting the personal information by searching the ID that Meeti Server replys|
|     fhandle     | Block| Message of failure to get the personal information by searching the ID that Meeti Server replys|

Return  
```
{ "profiles": [ { "person_company": "", "person_notify": 1, "person_companyphone": "", "person_email": "auid123", "person_name": "hard_working_actor", "person_keyword": "", "person_searchable": "0", "person_phone": "" }, { "person_company": "", "person_notify": 1, "person_companyphone": "", "person_email": "auid222", "person_name": "People_That_Lives_in_Taipei_Who_Likes_Bagel", "person_keyword": "", "person_searchable": "0", "person_phone": "" } ] }
```

### Get Personal Information (Keyword)
***
Get a specific personal information by using keyword search

```objective-c
-(void)getProfileByKeywords:(NSArray *)keywords
                shandle:(MeetiRequestHandleCallBack)shandle
                fhandle:(MeetiRequestFailHandleCallBack)fhandle
```
|    Parameter Name     |  Parameter Type  |      Parameter Introduction       |
|:-----------------------------------:|:-----------------------------------:|:---------------------------------------------:|
|     keywords        | NSArray:NSString  | The keyword wish to be searched |
|     shandle     | Block| Message of successfully getting personal information by searching the keywords that Meeti Server replys|
|     fhandle     | Block| Message of failure to get the personal information by searching the keywords that Meeti Server replys|

傳回  
```
{ "profiles": [ { "person_company": "", "person_notify": 1, "person_companyphone": "", "person_email": "auid123", "person_name": "hard_working_actor", "person_keyword": "", "person_searchable": "0", "person_phone": "" }, { "person_company": "", "person_notify": 1, "person_companyphone": "", "person_email": "auid222", "person_name": "People_That_Lives_in_Taipei_Who_Likes_Bagel", "person_keyword": "", "person_searchable": "0", "person_phone": "" } ] }
```

### Set Personal Profile Picture
***
Set a personal profile picture

```objective-c
-(void)setProfilePicture:(UIImage*)pic
                 shandle:(MeetiRequestHandleCallBack)shandle
                 fhandle:(MeetiRequestFailHandleCallBack)fhandle
```

|     Parameter Name     |  Parameter Type  |      Parameter Introduction       |
|:-----------------------------------:|:-----------------------------------:|:---------------------------------------------:|
|     pic        | UIImage  | The image that wish to be set as profile picture |
|     shandle     | Block| Message of succesfully setting the personal profile picture that Meeti Server replys|
|     fhandle     | Block| Message of failure to set the personal profile picture that Meeti Server replys|


### Get Specific Person Profile Picture
***

```objective-c
-(void)getProfilePictureWithUID:(NSString*)UID
                            toPath:(NSString*)path
                           shandle:(MeetiRequestHandleCallBack)shandle
                           fhandle:(MeetiRequestFailHandleCallBack)fhandle
```

|     Parameter Name     |  Parameter Type  |      Parameter Introduction       |
|:-----------------------------------:|:-----------------------------------:|:---------------------------------------------:|
|     UID        | NSString  | The user ID whom you wish to get their profile picture |
|     path        | NSString  | The path that you wish to save the image to |
|     shandle     | Block| Message of successfully getting the profile picture of the specific person by their ID that Meeti Server replys|
|     fhandle     | Block| Message of failure to get the profile picture of the specific person by their ID that Meeti Server replys|


### Get Your Friend List 
***

```objective-c
-(void)getFriendsWithProfile:(BOOL)withProfile
                     shandle:(MeetiRequestHandleCallBack)shandle
                     fhandle:(MeetiRequestFailHandleCallBack)fhandle
```

|     Parameter Name     |  Parameter Type  |      Parameter Introduction       |
|:-----------------------------------:|:-----------------------------------:|:---------------------------------------------:|
|     withProfile        | BOOL  |  Whether or not to include the profile's information |
|     shandle     | Block| Message of successfully getiing your friend list that Meeti Server replys|
|     fhandle     | Block| Message of failure to get your friend list that Meeti Server replys|

### Add Friends
***
```objective-c
-(void)addFriendByUIDs:(NSArray *)uids
                  shandle:(MeetiRequestHandleCallBack)shandle
                  fhandle:(MeetiRequestFailHandleCallBack)fhandle
```

|     Parameter Name     |  Parameter Type  |      Parameter Introduction       |
|:-----------------------------------:|:-----------------------------------:|:---------------------------------------------:|
|     uids        | NSArray:NSString  |  The user ID that you wish to add as a friend |
|     shandle     | Block| Message of successfully adding the friend that Meeti Server replys|
|     fhandle     | Block| Message of failure to add the friend taht Meeti Server replys|

### Follow a Specific User
***
```objective-c
-(void)follow:(NSString*)uid
      shandle:(MeetiRequestHandleCallBack)shandle
      fhandle:(MeetiRequestFailHandleCallBack)fhandle
```

|     Parameter Name     |  Parameter Type  |      Parameter Introduction       |
|:-----------------------------------:|:-----------------------------------:|:---------------------------------------------:|
|     uid        |  NSString  |  Follow specific user ID |
|     shandle     | Block| Message of successfully following the user that Meeti Server replys|
|     fhandle     | Block| Message of failure to follow the user that Meeti Server replys|

### Unfollow a Specific User
***
```objective-c
-(void)unfollow:(NSString*)uid
        shandle:(MeetiRequestHandleCallBack)shandle
        fhandle:(MeetiRequestFailHandleCallBack)fhandle
```

|     Parameter Name     |  Parameter Type  |      Parameter Introduction       |
|:-----------------------------------:|:-----------------------------------:|:---------------------------------------------:|
|     uid        |  NSString  |  Unfollow specific user ID |
|     shandle     | Block| Message of successfully unfollowing the user that Meeti Server replys|
|     fhandle     | Block| Message of failure to unfollow the user that Meeti Server replys|


### Get the list of the people who you are following
***
```objective-c
-(void)getFollow:(MeetiRequestHandleCallBack)shandle
         fhandle:(MeetiRequestFailHandleCallBack)fhandle
```

|     Parameter Name     |  Parameter Type  |      Parameter Introduction       |
|:-----------------------------------:|:-----------------------------------:|:---------------------------------------------:|
|     shandle     | Block| Message of successfully getting the list of people you are following that Meeti Server replys|
|     fhandle     | Block| Message of failure to get the list of people you are following that Meeti Server replys|
Return:   
```
{ "followers": [ "auid112", "auid222", "guid123" ], "following": [ "auid133", "auid222", "guid133" ] }
```

### Setting of Push Notification's token  
***
Not supported at the moment. 


### 新增待辦事項
***
Not supported at the moment. 


### 取得待辦事項的列表
***
Not supported at the moment. 
