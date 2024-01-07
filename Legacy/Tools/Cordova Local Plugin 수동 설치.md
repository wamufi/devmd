---
boostnote:
  createdAt: '2021-02-22T06:37:01.026Z'
  updatedAt: '2021-02-22T06:57:35.771Z'
  type: MARKDOWN_NOTE
  folder: ec2c5e1a46e8f816e4bb
  title: Cordova Local Plugin 수동 설치
  tags:
    - Cordova
  linesHighlighted: []
  isStarred: false
  key: d30a4287-8166-4f5c-983f-bcc0d3230d18
  storage: d112f8ec1e85e056a09d
---

Cordova Local Plugin 수동 설치
---
`CordovaError: Could not determine package name from output:
up to date, audited 111 packages in 656ms`

[CordovaError: Could not determine package name from output:](:note:c41848a0-3da7-48ce-bd7d-8c1f60f3f618)

이거로도 해결이 안 될 때, `npm audit fix`도 안 될 때 아무튼 로컬 플러그인 설치가 안 될 때!
아주 험난했다. 흑흑

Android 기준이다.
찾기가 힘들다면 플러그인 아무거나 설치해서 거기 추가되는 걸 따라가보자. 나는 flashlight 플러그인을 이용했다.

플러그인 폴더 이름은 `PrintPlugIn`, 패키지 이름은 `com.goodstream.printplugin`, 버전은 `0.9.0`

platforms/android/app/src/main/res/xml/config.xml
```xml
<feature name="PrintPlugin">
    <param name="android-package" value="com.goodstream.printplugin.PrintPlugin" />
</feature>
```

platforms/android/android.json
```json
"config_munge": {
    "files": {
        "res/xml/config.xml": {
            "parents": {
                "/*": [
                    {
                        "xml": "<feature name=\"PrintPlugin\"><param name=\"android-package\" value=\"com.goodstream.printplugin.PrintPlugin\" /></feature>"
                        "count": 1
                    }
                ]
            }
        }
    }
}
```

platforms/android/app/src/main/assets/www/cordova_plugins.js
```js
cordova.define('cordova/plugin_list', function(require, exports, module) {
    module.exports = [
        {
            "id": "com.goodstream.printplugin.PrintPlugin",
            "file": "plugins/com.goodstream.printplugin/www/PrintPlugin.js",
            "pluginId": "com.goodstream.printplugin",
            "clobbers": [
                "cordova.plugins.PrintPlugin"
            ]
        }
    ];
    module.exports.metadata = {
        "com.goodstream.printplugin": "0.9.0"
    }
}
```

platforms/android/platform_www/cordova_plugins.js
```js
cordova.define('cordova/plugin_list', function(require, exports, module) {
    module.exports = [
        {
            "id": "com.goodstream.printplugin.PrintPlugin",
            "file": "plugins/com.goodstream.printplugin/www/PrintPlugin.js",
            "pluginId": "com.goodstream.printplugin",
            "clobbers": [
                "cordova.plugins.PrintPlugin"
            ]
        }
    ];
    module.exports.metadata = {
        "com.goodstream.printplugin": "0.9.0"
    }
}
```

plugins/android.json
```json
{
    "installed_plugins": {
        "com.goodstream.printplugin": {
            "PACKAGE_NAME": "kr.co.bluewalnut.preorderstore"
        }
    }
}
```

plugins/fetch.json
```json
{
    "com.goodstream.printplugin": {
        "source": {
            "type": "local",
            "path": "/Users/young/work_space/cordova/took2app/node_modules/com.goodstream.printplugin" // 안 맞아도 상관 없는 듯
        },
        "is_top_level": true,
        "variables": {}
    }
}
```

config.xml
```xml
<plugin name="com.goodstream.printplugin" spec="./PrintPlugIn" />
```

package.json
```json
"cordova": {
    "plugins": {
         "com.goodstream.printplugin": {}
    }
}
```

파일도 있어야 한다.

`platforms/android/platform_www/plugins/com.goodstream.printplugin/www/` PrintPlugin.js
`platforms/android/app/src/main/java/com/goodstream/printplugin/` .java 파일들