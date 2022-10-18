# admixer_web_sdk_guide
ADMIXER WEB SDK GUIDE (javascript)

* 하단의 코드를 복사하여 광고가 노출되길 원하는 위치에 붙여주세요.

-----------------------------------
### A. 스크립트 가이드
```html
<script type="text/javascript" src="//scr.nsmartad.com/admixer/v3/admixer.js"></script>
<script type="text/javascript">
    new admixer_ad({
        media_key : "AdMixer Media Key",
        adunit :[
            {   //기본설정
                adunit_id : "Adunit ID 1",
                display_id : "display_admixer_1" //display 영역
            },
            {   //기본설정&추가설정시
                adunit_id : "Adunit ID 2",
                display_id : "display_admixer_2",
                ad_change : true, //광고 재요청
                close_btn : true, //닫기버튼 사용여부
                passback_fn : function(){
                    //애드믹서광고 nofill인경우 패스백 처리함수 기입(패스백이 필요없는경우 기입하지 않도록 합니다)
                }
            }
        ],
        coppa : 0, //아동 대상 서비스 취급용 태그
        admixer_log : true, //로그 확인 설정
        admixer_log_display_id : "display_test_log" //로그가 표시될 영역 DIV ID
    });
</script>
<!-- Adunit이 노출될 영역에 해당 div태그를 삽입합니다. -->
<div id="display_admixer_1"></div>
<div id="display_admixer_2"></div>

<!-- 로그가 표시될 영역에 해당 div태그를 삽입합니다 -->
<div id="display_test_log"></div>
```


* __media_key__ : 사이트 등록 후 발급받은 코드값을 적용해주셔야 합니다. (필수)  
  - 해당값은 `미디어 > 미디어관리` 에서 확인 가능합니다.
* __adunit__ : 노출을 원하는 adunit 수 만큼 정보를 입력합니다. (필수)
* __adunit_id__ : 사이트 등록 후 발급받은 코드값을 적용해주셔야 합니다.  
  - 해당값은 `미디어 > 미디어관리 > 미디어정보` 에서 확인 가능합니다. (필수)
* __display_id__ : 해당 Adunit의광고를 노출하고자 하는 DIV ID (필수)
* ad_change : 페이지가 변환되지 않은 상태에서의 광고 롤링여부에 대한 설정 값입니다.
  - true:설정, false:미설정
  - 미설정시 default : false
  - false로 설정할 경우, 페이지가 전환되기 전까지 광고를 재호출하지 않습니다.
  - 해당 값을 생략하거나 true로 설정할 경우, 노출전략에서 설정한 요청주기에 따라 광고를 재호출합니다.
  - 요청주기는 `미디어 > 미디어관리 > 미디어정보`페이지의 `광고설정`버튼클릭, `광고요청주기`로 설정 가능합니다.
* close_btn : 광고 닫기버튼으로 더이상 광고 노출을 원하지 않을경우 사용합니다.
  - true:닫기버튼사용, false:미사용
  - 미설정시 default : false
  - false로 설정하거나 미입력시 닫기버튼이 노출되지 않습니다.
  - Fullscreen이 아닌 Adunit에 적용됩니다 (Fullscreen의 경우 무조건 노출됩니다.)
* passback_fn : 애드믹서광고 nofill인경우 패스백 처리함수 기입합니다.
  - 패스백이 필요 없는 경우 기입하지 않도록 합니다.
* coppa : 아동 대상 서비스 취급용 태그 (0:아동대상 미지정, 1:아동대상 지정)
  - 0:아동대상 미지정, 1:아동대상 지정
  - 미설정시 default : 0
  - 해당 항목을 미설정시 아동대상 콘텐츠가 아닌것으로 간주 합니다.
  - `아동 대상 서비스` 파라메터 값을 `해당함`으로 광고 요청 시 `패밀리정책`에 위배되는 광고를 제외하여 송출합니다.
* admixer_log : 로그 확인을 설정할 수 있습니다.
  - true:설정, false:미설정
  - 미설정시 default : false
  - admixer_log 설정하면 페이지에 다음의 2가지가 표시되어 좀 더 편하게 테스트를 하실 수 있습니다.
    1) `Next Ad`버튼: 요청주기와 무관 다음 순서의 광고를 강제 호출할 수 있습니다.
    2) 로그: 광고호출, 수신, 수신실패 등의 횟수가 표시됩니다.
* admixer_log_display_id : 로그가 표시될 영역의 Div ID.
  - admixer_log(로그확인설정) true일 경우 기입합니다. 


-------------------------------
## B. 안드로이드 앱내 웹뷰를 통해 WEBSDK 사용시

* 웹뷰에서 javascript가 동작 할수 있도록 설정 합니다.
* 웹뷰에서 로컬스토리지 기능을 사용 할수 있도록 설정 하셔야 합니다.
* 웹뷰 영역에서 새창을 띄우는 경우(_blank 처리) 광고영역 내에서 랜딩되는 것 방지 합니다.
* 클릭 랜딩시 마켓(market://)을 비롯해 앱으로 연결되는 앱 링크가 동작하지 않을 수 있기 때문에 별도 처리가 필요합니다.

```java
[웹뷰셋팅].setJavaScriptEnabled(true);
[웹뷰셋팅].setDomStorageEnabled(true);
[웹뷰셋팅].setCacheMode(WebSettings.LOAD_NO_CACHE);
[웹뷰셋팅].setSupportMultipleWindows(true);

// _blank 처리(브라우져 새창 띄우기)
[웹뷰셋팅].setWebChromeClient(new WebChromeClient() {
    @Override
    public boolean onCreateWindow(WebView view, boolean dialog, boolean userGesture, Message resultMsg) {

        WebView newWebView = new WebView(view.getContext());

        WebView.WebViewTransport transport = (WebView.WebViewTransport)resultMsg.obj;
        transport.setWebView(newWebView);
        resultMsg.sendToTarget();

        return true;
    }
});

// 마켓(market://) 및 앱 링크 처리"
[웹뷰셋팅].setWebViewClient(new WebViewClient(){
    @Override
    public boolean shouldOverrideUrlLoading(WebView view, String url) {
        if ( view == null || url == null) {
            // 처리하지 못함
            return false;
        }

        if ( url.startsWith("http:") || url.startsWith("https:") ) {
            // HTTP/HTTPS 요청은 내부에서 처리한다.
            view.loadUrl(url);
        } else {
            Intent intent;
            try {
                intent = Intent.parseUri(url, Intent.URI_INTENT_SCHEME);
            } catch (URISyntaxException e) {
                // 처리하지 못함
                return false;
            }

            try {
                view.getContext().startActivity(intent);
            } catch (ActivityNotFoundException e) {
                // Intent Scheme인 경우, 앱이 설치되어 있지 않으면 Market으로 연결
                if ( url.startsWith("intent:") && intent.getPackage() != null) {
                    url = "market://details?id=" + intent.getPackage();
                        view.getContext().startActivity(new Intent(Intent.ACTION_VIEW,Uri.parse(url) ));
                        return true;
                } else {
                    // 처리하지 못함
                    return false;
                }
            }
        }
        return true;
    }
});        
```

* 뒤로 가기 버튼 클릭시 이전 페이지 이동 처리가 필요한 경우 아래와 같이 설정 합니다.
```java
public boolean onKeyDown (int keyCode, KeyEvent event) {
    // 뒤로가기 처리
    if (keyCode == KeyEvent.KEYCODE_BACK) {
        // 이전페이지가 있으면 이전페이지로 이동
        if ([웹뷰].canGoBack()){
            [웹뷰].goBack();
            return false;
        }
    }

    return super.onKeyDown(keyCode, event);
}
```
