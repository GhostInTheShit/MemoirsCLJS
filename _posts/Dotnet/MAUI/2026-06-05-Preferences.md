# Preferences

偏好存储,由MAUI自己管理的键值对存储，只要使用GET和SET方法就行了，存储位置不需要管理.

```C#
Preferences.Get(key, defaultValue)
Preferences.Set(key, value);
```