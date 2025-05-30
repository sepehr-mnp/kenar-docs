# ارسال پیام در چت

برنامه‌‌های کنار دیوار می‌توانند پس از [کسب اجازه از کاربر][احراز باز]، در یک چت یا چت‌های یک آگهی پیام ارسال کنند.
پیام‌های ارسالی از طرف کاربری که دسترسی به برنامه را داده و درخواست از سمتش بوده در چت ارسال می‌شود و هر دو طرف چت یک
متن پیام می‌بینند، اما امکان درج دکمهٔ متفاوت برای طرفین چت وجود دارد.\
به علاوه در قسمت بالای پیام، عنوان برنامهٔ مورد نظر نشان داده می‌شود.

| ![نمایی از یک پیام ارسال شده توسط بات](../static/img/bot-message.png) |
|:---------------------------------------------------------------------:|
|       <sub dir="rtl">نمایی از یک پیام ارسال شده توسط بات</sub>        |

> ⭐️ بهتر است پیامی که در چت ارسال می‌کنید برای هر دو طرف معنادار باشد و اطلاعات مفید در راستای خدمت دریافت شده ارائه
> دهد.

> 🛑 برای مقاصد تبلیغاتی یا معرفی امکانات و خدماتتان پیام ارسال نکنید.

## دسترسی ارسال پیام در چت

برای اینکه بتوانید در یک چت پیامی ارسال کنید، نیاز است تا اجازهٔ دسترسی در دو نقطه فراهم شده‌باشد:\
۱. برنامهٔ شما به صورت کلی دسترسی لازم را در [پنل کنار دیوار][پنل کنار] گرفته‌باشد.\
۲. [از کاربر اجازه گرفته‌باشید][احراز باز] و `access_token` ارسالی در درخواستتان این اجازه را داشته‌باشد:

- SCOPE: CONVERSATION_SEND_MESSAGE
- IDENTIFIER: CONVERSATION_ID

دو نوع دسترسی برای ارسال پیام در چت وجود دارد که برای هر کدام می‌توان جداگانه از کاربر اجازه گرفت. ارسال پیام در **یک چت
** و ارسال پیام در **چت‌های یک آگهی**
جزییات و پارامترهای لازم برای درخواست دسترسی و ایجاد `access_token` را در [صفحهٔ احراز باز][احراز باز] ببینید.

## درخواست ارسال پیام در چت

در صورتی که اجازهٔ این درخواست رو طبق توضیحات بالا و [صفحهٔ احراز باز][احراز باز] داشته‌باشید، با ارسال درخواستی مشابه
نمونهٔ زیر می‌توانید در چت مورد نظرتان پیام وارد کنید.

می‌توانید قسمت‌های فارسی را با مقادیر خودتان جایگزین کنید:

```http request
POST https://open-api.divar.ir/v2/open-platform/conversations/{{conversation_id}}/messages
Content-Type: application/json
x-api-key: {{apikey}}
Authorization: Bearer {{access_token}}

{
  "type": "TEXT",
  "message": "متن پیام",
  "sender_buttons": {
    "rows": [
      {
        "buttons": [
          {
            "action": {
              "open_direct_link": "آدرس مورد نظر برای باز شدن بعد از کلیک"
            },
            "icon": "نام آیکون",
            "caption": "متن دکمه"
          },
          {
            "action": {
              "open_server_link": {
                "data": {
                  "my_key_1": "value",
                  "my_key_2": "value2"
                }
              }
            },
            "icon": "نام آیکون",
            "caption": "متن دکمه"
          }
        ]
      }
    ]
  },
  "receiver_buttons": {
    "rows": [
      {
        "buttons": [
          {
            "action": {
              "open_direct_link": "آدرس مورد نظر برای باز شدن بعد از کلیک"
            },
            "icon": "CAR_INSPECTED",
            "caption": "متن دکمه"
          },
          {
            "action": {
              "open_server_link": {
                "data": {
                  "my_key_1": "value",
                  "my_key_2": "value2"
                }
              }
            },
            "icon": "REAL_STATE",
            "caption": "متن دکمه"
          }
        ]
      }
    ]
  }
}
```

### درخواست

| Field Name       | Field Type                | Description                                      |
|------------------|---------------------------|--------------------------------------------------|
| type             | String                    | نوع محتوا را مشخص می‌کند، در حال حاضر فقط "TEXT" |
| message          | String                    | محتوای متن پیام                                  |
| sender_buttons   | [ButtonGrid](#buttongrid) | شامل پیکربندی دکمه‌ها برای فرستنده               |
| receiver_buttons | [ButtonGrid](#buttongrid) | شامل پیکربندی دکمه‌ها برای گیرنده                |

### ButtonGrid

| Field Name | Field Type                     | Description                             |
|------------|--------------------------------|-----------------------------------------|
| rows       | Array[[ButtonRow](#buttonrow)] | آرایه‌ای از ردیف دکمه‌ها. حداکثر ۳ ردیف |

### ButtonRow

| Field Name | Field Type               | Description                                   |
|------------|--------------------------|-----------------------------------------------|
| buttons    | Array[[Button](#button)] | آرایه‌ای از دکمه‌ها. حداکثر ۳ دکمه در هر ردیف |

### Button

| Field Name | Field Type                                                                                                      | Description                                     |
|------------|-----------------------------------------------------------------------------------------------------------------|-------------------------------------------------|
| action     | [Action][Action]                                                                                                | اکشنی که پس از کلیک کاربر بر روی دکمه رخ می‌دهد |
| icon       | [Icon](https://www.figma.com/design/ZhhSihwKTjiER1VUDX4ovh/%F0%9F%93%92-Kenar-Docs-(WIP)?node-id=1501-2225&p=f) | نام آیکون نمایش داده‌شده بر روی دکمه            |
| caption    | String                                                                                                          | متنی که بر روی دکمه نمایش داده می‌شود           |

### خطاها

ممکن است در پاسخ این درخواست خطا به صورت زیر دریافت شود:

```HTTP
HTTP/1.1  412
{
    "code": 9,
    "message": "message delivery failed due to recipient's restriction"
}
```

این خطا به چند دلیل می ‌تواند دریافت شود:

- کاربر توسط کاربر مقابل در چت مسدود شده باشد
- کاربر یا کاربر مقابل در بلک لیست باشند.
- ...

## دسترسی ارسال پیام در تمام مکالمات یک آگهی

دسترسی قبلی تنها مربوط به یک مکالمه می‌باشد، اما با داشتن این دسترسی می‌توانید در تمام مکالمات مربوط به یک آگهی از طرف
آگهی گذار پیام بفرستید:

- SCOPE: CHAT_POST_CONVERSATIONS_MESSAGE_SEND
- IDENTIFIER: POST_TOKEN

## دسترسی ارسال پیام در تمام مکالمات تمام آگهی‌های آگهی گذار

در صورتی که نیاز دارید در تمام چت‌هایی که کاربر سمت آگهی گذار می‌باشد، پیام ارسال کنید می‌توانید از اسکوپ زیر استفاده که
`RESOURCE_ID` یا `IDENTIFIER` ندارد:

- SCOPE: CHAT_SUPPLIER_ALL_CONVERSATIONS_MESSAGE_SEND

## کلیک کاربر روی دکمهٔ درج شده زیر پیام

پس از کلیک کاربر بر روی دکمه‌های پیام، با توجه به [اکشنی][Action] که بر روی دکمه تعریف شده است،
کاربر به سمت برنامه‌ی شما هدایت می‌شود.

<br />

[احراز باز]: ../oauth

[API key]: ../management/api-keys.md

[Action]: ../widgets/actions

[پنل کنار]: ../management


[بازشدن برنامه]: #کلیک-کاربر-روی-دکمهٔ-درج-شده-زیر-پیام


<br /><br />

<div align="center">

![Wire Puzzle](../static/img/wire-puzzle.svg)

</div>

<br /><br />
