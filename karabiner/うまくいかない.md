## Double F12_1回押し:スクショ-2回押し:画像読込テキスト化CaptureText(FunctionKey)

F12の2回押しが起動しない.

```
    {
        "description": "Double F12_1回押し:スクショ-2回押し:画像読込テキスト化CaptureText(FunctionKey)",
        "type": "basic",
        "from": {
            "key_code": "f12"
        },
        "to": [
            {
                "key_code": "quote",
                "modifiers": [
                    "right_option",
                    "right_gui"
                ]
            }
            ],
        "conditions": [
            {
                "type": "variable_if",
                "name": "f12 pressed",
                "value": 1
            },
            {
            "type": "frontmost_application_unless",
            "bundle_identifiers": [
                "^com\\.vmware\\.fusion$",
                "^com\\.vmware\\.horizon$",
                "^com\\.vmware\\.view$",
                "^com\\.parallels\\.desktop$",
                "^com\\.parallels\\.vm$",
                "^com\\.parallels\\.desktop\\.console$",
                "^org\\.virtualbox\\.app\\.VirtualBoxVM$",
                "^com\\.vmware\\.proxyApp\\.",
                "^com\\.parallels\\.winapp\\."
            ]
            }
        ]
    },
    {
        "type": "basic",
        "from": {
            "key_code": "f12"
        },
        "to": [
            {
                "set_variable": {
                    "name": "f12 pressed",
                    "value": 1
                }
            },
            {
                "key_code": "0",
                "modifiers": [
                    "right_option",
                    "right_gui"
                ]
            }
        ],
        "to_delayed_action": {
            "to_if_invoked": [
                {
                    "set_variable": {
                        "name": "f12 pressed",
                        "value": 0
                    }
                }
            ],
            "to_if_canceled": [
                {
                    "set_variable": {
                        "name": "f12 pressed",
                        "value": 0
                    }
                }
            ]
        }
},
```
---------------------