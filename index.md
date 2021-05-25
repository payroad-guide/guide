---
layout: default
title: LINK型連動
nav_order: 1
description: "LINK型連動方式のガイド"
permalink: /
---

# LINK型(画面)の連動ガイド

リンク型は、決済代行会社がご提供する決済画面を呼び出して、決済処理をする接続方式です。<br>
加盟店さまが決済画面を構築することなく、簡単かつスピーディーにご導入いただけます。

---

### ■連動フロー
＜図の「?」アイコンをクリックし、情報確認が可能＞
<img src="{{ site.baseurl }}/assets/images/link_flow.png" id="image_screen" width="90%">

1. FROMの基本構造

```markdown
<form action="https://stbfep.sps-system.com/xxxx/xxxxxxxx" method="POST">
  <input type="hidden" name="タグエレメント名1" value="設定内容1">
  <input type="hidden" name="タグエレメント名2" value="設定内容2">
  <input type="submit" value="submit">
</form>
```
<ul>
  <li><small>文字コード（キャラクタセット）はShift_JIS</small></li>
  <li><small>使用可能Portは、「443」となります。</small></li>
  <li><small>未設定項目については空文字の設定をお願いします。ただし、明細行繰返項目のみ、タグエレメントの削除が可能です。</small></li>
  <li><small>FORMタグのaction属性に指定する接続先は、管理画面から確認してください。</small></li>
</ul>

---

### ■連動サンプル（連動フローの番号と連動します。）

### ② LINK型「FORM」要求
```json
## Header
Authorization: 統合決済APIのアクセストークン
Content-Type: コンテンツタイプ
PayRoad-Api-Version: APIバージョン
PayRoad-Payment-Method: 決済手段コード

## Body(Json)
{
  "ipAddress": "string",
  "language": "ja",
  "paymentInfo": {
    "free1": "string",
    "free2": "string",
    "productPackageId": "string",
    "productPackageName": "string",
    "products": [
      {
        "count": 0,
        "id": "string",
        "name": "string",
        "subtotalAmount": 0,
        "subtotalTax": 0
      }
    ],
    "totalAmount": 0,
    "totalTax": 0
  },
  "serviceId": 0,
  "serviceInfo": {
    "cancelUrl": "string",
    "currency": "JPY",
    "errorUrl": "string",
    "region": "string",
    "returnUrl": "string",
    "serviceNotifyUrl": "string",
    "userKey": "string"
  },
  "serviceOrderId": "string",
  "testFlag": "0"
}
```
### ⑤ LINK型「FORM」返却
```markdown
返却値（JSON）
{
  "isSuccessful": true,
  "resultCode": "string",
  "resultInfo": {
    "formTag": "
              <form action="https://stbfep.sps-system.com/xxxx/xxxxxxxx" method="POST">
                  <input type="hidden" name="pay_method" value="credit">
                  <input type="hidden" name="merchant_id" value="20053">
                  <input type="hidden" name="service_id" value="001">
                  <input type="hidden" name="cust_code" value="XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX">
                  <input type="hidden" name="sps_cust_no" value="">
                  <input type="hidden" name="sps_payment_no" value="">
                  <input type="hidden" name="order_id" value="YYYYYYYY-YYYY-YYYY-YYYY-YYYYYYYYYYYY">
                  <input type="hidden" name="item_id" value="1">
                  <input type="hidden" name="free_csv" value="">
                  <!--商品明細行繰り返し start -->
                  <!—明細を使用しない場合は丸ごと省略可能です -->
                    <input type="hidden" name="dtl_rowno" value="1">
                    <input type="hidden" name="dtl_item_id" value="SBPS-001">
                    <input type="hidden" name="dtl_item_name" value="テスト明細商品名1">
                    <input type="hidden" name="dtl_item_count" value="1">
                    <input type="hidden" name="dtl_tax" value="50">
                    <input type="hidden" name="dtl_amount" value="1000">
                    <input type="hidden" name="dtl_rowno" value="2">
                    <input type="hidden" name="dtl_item_id value="SBPS-002">
                    <input type="hidden" name="dtl_item_name" value="テスト明細商品名2">
                    <input type="hidden" name="dtl_item_count" value="1">
                    <input type="hidden" name="dtl_tax" value="100">
                    <input type="hidden" name="dtl_amount" value="2000">
                  <!--//商品明細行繰り返し end -->
                  <input type="hidden" name="request_date" value="20081015214001">
                  <input type="hidden" name="limit_second" value="600">
                  <input type="hidden" name="sps_hashcode" value=" ab8be32d9602530076219c3424122db77d5202bb"> 
                <input type="submit" value="submit">
              </form>
               "
    "orderId": "string",
    "serviceOrderId": "string"
  },
  "resultMessage": "string"
}
```
### ⑩ 決済結果
```json
## LINK型「FORM」要求時に設定したURL（serviceNotifyUrl）に通知されます。
{
  "isSuccessful": true,
  "resultCode": "PAY-COMMON-0000",
  "resultInfo": {
    "amount": "string",
    "completedDate": "2020-08-15 08:15:00",
    "currency": "JPY",
    "orderId": "string",
    "region": "JP",
    "serviceOrderId": "string"
  },
  "resultMessage": "string"
}

```
### ⑭ リダイレクト
```text
LINK型「FORM」要求時に設定したURL（returnUrl）にリダイレクトされます。
```
<script type="text/javascript" src="https://code.jquery.com/jquery-3.2.1.min.js"></script>
<script type="text/javascript" src="https://code.jquery.com/ui/1.12.1/jquery-ui.min.js"></script>
<script type="text/javascript" src="https://unpkg.com/leaflet@1.0.3/dist/leaflet.js"></script>
<script type="text/javascript" src="{{ '/assets/js/vendor/imgViewer2.js' | relative_url }}"></script>
<script src="https://unpkg.com/leaflet-responsive-popup@0.2.0/leaflet.responsive.popup.js"></script>
<script type="text/javascript" src="{{ '/assets/js/vendor/leaflet-beautify-marker-icon.js' | relative_url }}"></script>
  
<!-- <script type="text/javascript">
  (function($) {
    $(window).on("load", function() {
      var $img = $("#image_screen").imgViewer2(
        {
          onReady: function() {
            this.setZoom(2);
            this.setZoom(1);
          }
        }
      );
    });
  })(jQuery);
</script> -->
<script type="text/javascript">
  (function($) {
    $.widget("wgm.imgNotes2", $.wgm.imgViewer2, {
      options: {
  /*
  *	Defaault action for addNote callback
  */
        addNote: function(data) {
          var map = this.map,
            loc = this.relposToLatLng(data.x, data.y);
          var popup = L.responsivePopup({ hasTip: true, autoPan: false}).setContent(data.note);
          var icon = L.BeautifyIcon.icon({icon: 'question',
                          borderColor: '#e94c4c',
                          textColor: '#2869e6',
                          backgroundColor: 'transparent'
          });
          L.marker(loc, {icon: icon}).addTo(map).bindPopup(popup);
        }
        
      },
      
  /*
  *	Add notes from a javascript array
  */
      import: function(notes) {
        if (this.ready) {
          var self = this;
          $.each(notes, function() {
            self.options.addNote.call(self, this);
          });	
        }
      }
    });
    $(document).ready( function() {
      var $img = $("#image_screen").imgNotes2( {
              onReady: function() {
                var notes = [	
                        {x: "0.737", y:"0.045", note: '\
                        POST（Body：JSON）要求<br>\
                        <a href="#-link型form要求">サンプル確認</a> '
                        },
                        {x: "0.737", y:"0.115", note: '\
                        返却値はJSONで「formTag」から生成FORMを取得可能<br>\
                        <a href="#-link型form返却">サンプル確認</a> '
                        },
                        {x: "0.700", y:"0.655", note: '\
                        LINK型「FORM」要求時に設定したURL(serviceNotifyUrl)に通知されます。<br>\
                        <a href="#-決済結果">サンプル確認</a> '
                        },
                        {x: "0.215", y:"0.793", note: '\
                        LINK型「FORM」要求時に設定したURL（returnUrl）にリダイレクトされます。例えば、購入完了画面へリダイレクト<br>\
                        <a href="#-リダイレクト">サンプル確認</a> '
                        },
                        ];
                this.import(notes);
              }
      });
    });
  })(jQuery);
</script>