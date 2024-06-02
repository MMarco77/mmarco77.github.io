```
▛▀▖      ▐  ▜    ▛▀▖      
▙▄▘▞▀▖▞▀▖▜▀ ▐ ▞▀▖▙▄▘▌ ▌▞▀▌
▌ ▌▛▀ ▛▀ ▐ ▖▐ ▛▀ ▌ ▌▌ ▌▚▄▌
▀▀ ▝▀▘▝▀▘ ▀  ▘▝▀▘▀▀ ▝▀▘▗▄▘
```

Training APK. Contains multilevel training.

# Hardcoded Secrets

## Flag 1 - Hardcoding Sensitive Data

Goal: To unlock secret folder => Find strings

Use [ApkLab](src/tools/android/ApkLab/index.md) to open apk files. Activate options:
1. decompile Java
2. Find "Enter PIN" => res/layout/activity_embedded_secret_strings.xml
    1. Ref a resource "editTextSecretPin". looking for `findViewById(*editTextSecretPin*) => `java_src/app/beetlebug/ctf/EmbeddedSecretStrings.java`

```java
embeddedSecretStrings.pin = (EditText) embeddedSecretStrings.findViewById(C0670R.C0673id.editTextSecretPin);
String s1 = EmbeddedSecretStrings.this.pin.getText().toString();
if (s1.equals(EmbeddedSecretStrings.this.getString(C0670R.string.V98bFQrpGkDJ))) {
    SharedPreferences.Editor editor = EmbeddedSecretStrings.this.sharedPreferences.edit();
    editor.putFloat("ctf_score_secret_string", 6.25f);
```

Looking for `string.V98bFQrpGkDJ` into resource file `string.xml` => `res/values/strings.xml`

```xml
  <string name="V98bFQrpGkDJ">7432580</string>
```

## Flag 2 - Hardcoding Secrets

Looking for `Enter Promo mode`
=> res/layout/activity_embedded_secret_source_code.xml

```java
...
public String beetle_bug_shop_promo_code = "beetle1759";
...
 EmbeddedSecretSourceCode embeddedSecretSourceCode = EmbeddedSecretSourceCode.this;
                embeddedSecretSourceCode.m_price = (TextView) embeddedSecretSourceCode.findViewById(C0670R.C0673id.textViewPrice);
                if (EmbeddedSecretSourceCode.this.m_promo.getText().toString().equals(EmbeddedSecretSourceCode.this.beetle_bug_shop_promo_code)) {
```
# Data Storage

## Flag 3 - Shared Preferences

```shell
$ adb shell
$# cd /data/data/app.beetlebug/shared_prefs/
$# ls -liash
$# ls
flag_scores.xml       # Current Flag value
preferences.xml       # 
shared_pref_flag.xml
user_info.xml
walkthrough.xml
$# cat preferences.xml
<?xml version='1.0' encoding='utf-8' standalone='yes' ?>
<map>
    <string name="5_sqlite">MHgxMTcyYzA0</string>
    <string name="11_firebase">MHgzMzY1QTEw</string>
    <string name="13_xss">MHg2NnI5MjE0</string>
    <string name="4_ext_store">MHgzOTgyYyU0</string>
    <string name="8_service">MHgyMjIxMDNB</string>
    <string name="7_content">MHg3MzM0MjFN</string>
    <string name="9_log">MHg1NTU0MWQz</string>
    <string name="16_patch">MHgzM2U5JGU=</string>
    <string name="10_sqli">MHg5MTMzNFox</string>
    <string name="15_fingerprint">MHg0M0oxMjMm</string>
    <string name="6_activity">MHgzMzRmMjIx</string>
    <string name="3_pref">MHgxNDQyYzA0</string>
    <string name="12_url">MHgzM2YzMzQx</string>
    <string name="14_clip">MHgxMTMyYzQh</string>
</map>
$# cat shared_pref_flag.xml
<?xml version='1.0' encoding='utf-8' standalone='yes' ?>
<map>
    <string name="password">    &lt;string name=&quot;flag&quot;&gt;0x32f6641&lt;/string&gt;&#10;    </string>
    <string name="flag 3">0x1442c04</string>
    <string name="username">test</string>
</map>
$# # cat user_info.xml
<?xml version='1.0' encoding='utf-8' standalone='yes' ?>
<map>
    <boolean name="is_logged_in" value="true" />
    <string name="flag">0x32f6641</string>
    <string name="user_token">ca9eada3-d132-46d3-8c43-333f3890f6eb</string>
    <string name="user">test</string>
</map>
```

Pour shared_pref

`&lt;string name=&quot;flag&quot;&gt;0x32f6641&lt;/string&gt;&#10;` => `<string name="flag">0x32f6641</string>`

But we had to find "Enter Flags'

1. Find activity for `Shared PReferences` => `res/layout/activity_insecure_storage_sharedpref.xml`
2. Rechercher `setContentView(C0670R.layout.activity_insecure_storage_sharedpref);` => `java_src/app/beetlebug/ctf/InsecureStorageSharedPref.java`

There are a code `String pref_result = this.preferences.getString("3_pref", "");` => Ref. dans `preferences.xml`

```xml
<string name="3_pref">MHgxNDQyYzA0</string>
```

```python
from base64 import b64decode;print(b64decode("MHgxNDQyYzA0").decode('utf8'))
```

=> `0x1442c04`

## Flag 4 - External Storage

When register new user, a new file will be create:

> /storage/emulated/0/Documents

```shell
$ cat /storage/emulated/0/Documents/user.txt
Pass: 1234
Email: johndoe@antispam.com
flag : 0x3982c%4
```
Flag string reference `4_ext_store`

```xml
<string name="4_ext_store">MHgzOTgyYyU0</string>
```

```python
from base64 import b64decode;print(b64decode("MHgzOTgyYyU0").decode('utf8'))
```

`0x3982c%4`

## Flag 5 - Sqlite Database

Looking for database

> /data/data/app.beetlebug/databases 

`user.db` => `1	qwer1234!@#$SDFSDF	0x1172c04`

Flag => `0x1172c04`.

# WebViews

## Flag 6 - Load Arbitrary URL

- Get info with `objection`

```shell
$ objection app.beetlebug explore
$ env
$ android hooking get current_activity
Activity: app.beetlebug.ctf.WebViewURLActivity
```

- Launch activity with parameters

```shell 
$ adb shell am start -n app.beetlebug/.ctf.WebViewURLActivity --es reg_url "file:///android_asset/pwn.html"
# same as 
$ adb shell am start -n app.beetlebug/app.beetlebug.ctf.WebViewURLActivity --es reg_url "file:///android_asset/pwn.html"
```

**NOTE**

Classic command:

> adb shell am start -n <PACKAGE_NAME/ACTIVITY_CLASS> [EXTRA_DATA]

Here are the available activities:

```text
$ [Objection] android hooking list activities
app.beetlebug.FlagsOverview
app.beetlebug.MainActivity
app.beetlebug.MainActivity$1
app.beetlebug.Walkthrough
app.beetlebug.adapter.SlideViewPagerAdapter
app.beetlebug.adapter.SlideViewPagerAdapter$1
app.beetlebug.adapter.SlideViewPagerAdapter$2
app.beetlebug.ctf.BinaryPatchActivity
app.beetlebug.ctf.WebViewURLActivity
app.beetlebug.ctf.WebViewXSSActivity
app.beetlebug.handlers.VulnerableContentProvider
app.beetlebug.handlers.VulnerableContentProvider$DatabaseHelper
app.beetlebug.home.AndroidComponentsHome
app.beetlebug.home.BiometricFragmentHome
app.beetlebug.home.DatabaseFragmentHome
app.beetlebug.home.InsecureStorageFragmentHome
app.beetlebug.home.SecretsFragmentHome
app.beetlebug.home.SensitiveDataFragmentHome
app.beetlebug.home.WebViewFragmentHome
app.beetlebug.home.WebViewFragmentHome$1
app.beetlebug.home.WebViewFragmentHome$2
app.beetlebug.home.WebViewFragmentHome$3
app.beetlebug.user.PlayerStats
app.beetlebug.user.UserSignUp
```

The app name is `app.beetlebug`, the rest is the activities name:

```text
.FlagsOverview
.MainActivity
.MainActivity$1
.Walkthrough
.adapter.SlideViewPagerAdapter
.adapter.SlideViewPagerAdapter$1
.adapter.SlideViewPagerAdapter$2
.ctf.BinaryPatchActivity
.ctf.WebViewURLActivity
.ctf.WebViewXSSActivity
.handlers.VulnerableContentProvider
.handlers.VulnerableContentProvider$DatabaseHelper
.home.AndroidComponentsHome
.home.BiometricFragmentHome
.home.DatabaseFragmentHome
.home.InsecureStorageFragmentHome
.home.SecretsFragmentHome
.home.SensitiveDataFragmentHome
.home.WebViewFragmentHome
.home.WebViewFragmentHome$1
.home.WebViewFragmentHome$2
.home.WebViewFragmentHome$3
.user.PlayerStats
.user.UserSignUp
```

**NOTE** `adb shell am start -n app.beetlebug/app.beetlebug.ctf.WebViewURLActivity --es reg_url "file:///android_asset/pwn.html"` cannot be display. Into decompiled APK, `assets/pwn.html`, there are `assets/pwn.png`

=> `adb shell am start -n app.beetlebug/app.beetlebug.ctf.WebViewURLActivity --es reg_url "file:///android_asset/pwn.png`

=> `0x33f3341`

## Flag 7 - JavaScript Code Injection (XSS)

Write into payload the classic

```js
<script>alert("Hello Worlds")</script>
```

The flag appears behind the send `0x66r9214`