# 푸쉬메시지 실행

푸쉬 보내는 api<br>
FCM_KEY => 파이어베이스 계정 로그인하여 cloud message의 키값 복사하여 사용<br>
(registration_ids, to), title, message(필수)<br>
registration_ids =>  보내고자하는 디바이스 토큰 여러개를 배열($token_list)값으로 전달 (1:다수) <br>
to =>  푸쉬를 보내고자 하는 디바이스 토큰($token_list)을 전달 (1:1)<br>
data => title 푸쉬제목<br>
data => message 푸쉬내용<br>

```PHP
function send_notification($token_list, $title, $message, $clickAction="", $content_idx="") {
    //FCM 인증키
    $FCM_KEY = 'AAAAh3UhYdw:APA91bGvgDN7DydoFfojdVo2D1yYto4jgdoIr-70rD7nm8XdDhSeCjWUYkmVCebpRaTXajHPAooNkFtpzti_LjqsEM1rl4M4qAjk4zShxdiEjCn1cutOJaVr6ewqeg9Ksw6SUEhufhZX';
    //FCM 전송 URL
    $FCM_URL = 'https://fcm.googleapis.com/fcm/send';

    //전송 데이터
    $fields = array (
       'registration_ids' => $token_list,
       'data' => array (
          'title' => $title,
          'message' => $message,
          'intent' => $clickAction,
          'content_idx' => $content_idx,
       ),
       'notification' => array (
          'title' => $title,
          'body' => $message,
          'content_idx' => $content_idx,
          'badge' => 1,
       ),
    );


    //설정
    $headers = array( 'Authorization:key='. $FCM_KEY, 'Content-Type:application/json' );

    $ch = curl_init();
    curl_setopt($ch, CURLOPT_URL, $FCM_URL);
    curl_setopt($ch, CURLOPT_POST, true);
    curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, 0);
    curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, true);
    curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode($fields));
    $result = curl_exec($ch);
    if($result === false) {
       die('Curl failed: ' . curl_error($ch));
    }
    curl_close($ch);
    $obj = json_decode($result);

    return $obj;
}
```
