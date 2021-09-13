# Google Forms for send SMS

คุณสามารถทดสอบ API สำหรับส่ง SMS ได้ฟรีจำนวน 50 SMS
ศึกษา API สำหรับส่ง SMS ได้ที่ https://www.taximail.com/en/support/api/sms

```js
function onFormSubmit(e) {

  var form = FormApp.openById('FORM ID'); 
  var apiLogin = JSON.parse(getSessionIDAPI());
  var session_id = apiLogin.data.session_id;

  function getSessionIDAPI() {
    var url = "https://api.taximail.com/v2/user/login";

    var data = {
      "api_key": "your api key",
      "secret_key": "your api secret_key"
    }

    var options = {
      "method": "post",
      "headers": {
          "Content-Type": "application/json",
      },
      "payload": JSON.stringify(data),
    };
    return UrlFetchApp.fetch(url, options);
  }
  
  var formResponse = e.response;
  var itemResponse = formResponse.getItemResponses();
  
  for (var i = 0; i < itemResponse.length; i++) {
    var item = itemResponse[i];
    var itemName = item.getItem().getTitle();

    if (itemName.indexOf("หมายเลขโทรศัพท์") > 0) {
      var numberPhoneForm = item.getResponse();
      var numberPhone = numberPhoneForm.replace("0", "66");
      sendSMS(numberPhone);
    }

  }


  function sendSMS(numberPhone){

    var url = "https://api.taximail.com/v2/sms";
    var data = {
      "from": "From specifies the sender name for your message.",
      "to": numberPhone,
      "text": "The message you want to send"
    }

    var options = {
        "method": "post",
        "headers": {
          "Authorization" : "Bearer " + session_id,
          "Content-Type": "application/x-www-form-urlencoded",
        },
        "payload": data
    };

    UrlFetchApp.fetch(url, options);
  }
  
 
}

```
#### Thanks

Win'ny Freedom
