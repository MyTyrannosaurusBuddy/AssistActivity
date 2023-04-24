# AssistActivity
com.tencent.connect.common.AssistActivity给诸多应用带来了安全风险。  
AndroidManifest.xml有如下声明的应用都面临数据泄露和权限被利用的问题：
```
<activity android:theme="@android:style/Theme.Translucent.NoTitleBar" android:name="com.tencent.connect.common.AssistActivity" android:exported="true" android:configChanges="keyboardHidden|orientation|screenLayout|screenSize|smallestScreenSize"/>  
```
AssistActivity代码当中存在明显的Intent重定向漏洞：
```
@Override // android.app.Activity
    public void onCreate(Bundle bundle) {
        super.onCreate(bundle);

        Intent intent = (Intent) getIntent().getParcelableExtra(EXTRA_INTENT); // 获取
        int intExtra = intent == null ? 0 : intent.getIntExtra(Constants.KEY_REQUEST_CODE, 0);
        this.mAppId = intent == null ? "" : intent.getStringExtra("appid");
        if (intent != null) {
            startActivityForResult(intent, intExtra); // 使用
        } 
    }
 ```
 对比微博的分享Activity，早期虽然也有重定向漏洞，但是后来做了入参净化，避免了集成sdk应用无意识导出产生的安全风险。  
 建议微信修复一下，不然这个锅总是甩不到合适的地方。
 
