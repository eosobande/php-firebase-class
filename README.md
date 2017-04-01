FCM -- Simple Firebase FCM Notification class

### Table of Contents
**[Initialization](#initialization)**  
**[Notifications](#notification)**  
**[Topics](#topics)**  
**[Implement theese functions](#implement-these-functions)**

### Installation
To utilize this class, simply import firebase.php into your project, and require it then make sure that following constants are defined: APP_NAME, FB_API_KEY.

```php
require_once ('firebase.php');
```

### Initialization
Simple initialization: make sure the following constants are defined APP_NAME FB_API_KEY

initialize with content-type JSON and time to live
```php
$fcm = new FCM(FCM::CONTENT_JSON, 60*60*24*5, null);
// null indicates no collapse key
```

or with content-type TEXT, time_to_live and collapse key

```php
$fcm = new FCM(FCM::CONTENT_TEXT, 60*60*24*5, 'username');
```
### Notifications

Send notifications to one registration token
```php
$token = ['ouvh;novnb:APAp[ijrvkJcLlK0p1U_RZ3-qqpMt6SoaTfsYGDuEBhfL6QpOMQpRGU09tI10xSuSwcIfmqQOvCVfpJMx_0jpovjevn;evetBW2Ro4V'];
$body = "New weather update";
$data = ['temperature'=>'10', 'humidity'=>987];

$fcm->notification($token, $body, $data);
```

or Send notifications with a different title than the APP_NAME constant value
```php
$token = ['ouvh;novnb:APAp[ijrvkJcLlK0p1U_RZ3-qqpMt6SoaTfsYGDuEBhfL6QpOMQpRGU09tI10xSuSwcIfmqQOvCVfpJMx_0jpovjevn;evetBW2Ro4V'];
$body = "New application update available";
$title = APP_NAME . ' version 3.4.5.0'

$fcm->notification($token, $body, null, $title);
```

or Send notifications with a list of tokens (multiple devices)
```php
// 8 android devices
$token = ['ouvh;novnb:APAp[ijrvkJcLlK0p1U_RZ3-qqpMt6SoaTfsYGDuEBhfL6QpOMQpRGU09tI10xSuSwcIfmqQOvCVfpJMx_0jpovjevn;evetBW2Ro4V', 'ouvh;novnb:APAp[ijrvkJcLlK0p1U_RZ3-qqpMt6SoaTfsYGDuEBhfL6QpOMQpRGU09tI10xSuSwcIfmqQOvCVfpJMx_0jpovjevn;evetBW2Ro4V', 'ouvh;novnb:APAp[ijrvkJcLlK0p1U_RZ3-qqpMt6SoaTfsYGDuEBhfL6QpOMQpRGU09tI10xSuSwcIfmqQOvCVfpJMx_0jpovjevn;evetBW2Ro4V', 'ouvh;novnb:APAp[ijrvkJcLlK0p1U_RZ3-qqpMt6SoaTfsYGDuEBhfL6QpOMQpRGU09tI10xSuSwcIfmqQOvCVfpJMx_0jpovjevn;evetBW2Ro4V', 'ouvh;novnb:APAp[ijrvkJcLlK0p1U_RZ3-qqpMt6SoaTfsYGDuEBhfL6QpOMQpRGU09tI10xSuSwcIfmqQOvCVfpJMx_0jpovjevn;evetBW2Ro4V', 'ouvh;novnb:APAp[ijrvkJcLlK0p1U_RZ3-qqpMt6SoaTfsYGDuEBhfL6QpOMQpRGU09tI10xSuSwcIfmqQOvCVfpJMx_0jpovjevn;evetBW2Ro4V', 'ouvh;novnb:APAp[ijrvkJcLlK0p1U_RZ3-qqpMt6SoaTfsYGDuEBhfL6QpOMQpRGU09tI10xSuSwcIfmqQOvCVfpJMx_0jpovjevn;evetBW2Ro4V', 'ouvh;novnb:APAp[ijrvkJcLlK0p1U_RZ3-qqpMt6SoaTfsYGDuEBhfL6QpOMQpRGU09tI10xSuSwcIfmqQOvCVfpJMx_0jpovjevn;evetBW2Ro4V'];

$body = "New weather update";
$data = ['temperature'=>'10', 'humidity'=>987];

$fcm->notification($token, $body, $data);
```

### Topics

Send notifications to devices subscribed to a certain topic
```php
// null indicates no condition for the topic
$data = ['temperature'=>5, 'humidity'=234]
$fcm->topics('weather', null, 'Lagos State is going to experiance a sunny day tommorow', $data);
```

or Send filtered topic notifications with conditions
```php
// null indicates no condition for the topic
$data = ['temperature'=>5, 'humidity'=234];
$condition = "'TopicA' in topics && ('TopicB' in topics || 'TopicC' in topics)";
$body = 'Lagos State is going to experiance a sunny day tommorow';

$fcm->topics(null, $condition, $body, $data);
```
### Implement these functions
The following functions are void and need to be implemented by you to suit your needs

1. FCM::remove_registration_id()
    This is automatically called when one or more of the provided tokens is now unregistered, therefore it needs to be deleted from your database/storage since its basically of no use. It takes the token as the single parameter
```php
$fcm->remove_registration_id($token);
```
2. FCM::update_registration_id()
    This is automatically called when one or more of the provided tokens has changed to a new one and your records need to be updated. It takes the old token as the first parameter and the new token as the second parameter
```php
$fcm->remove_registration_id($old, $new);
```
