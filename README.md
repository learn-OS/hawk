[![Android Arsenal](https://img.shields.io/badge/Android%20Arsenal-Hawk-brightgreen.svg?style=flat)](https://android-arsenal.com/details/1/1568)      [![API](https://img.shields.io/badge/API-8%2B-brightgreen.svg?style=flat)](https://android-arsenal.com/api?level=8)   [![Join the chat at https://gitter.im/orhanobut/hawk](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/orhanobut/hawk?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)  [![](https://img.shields.io/badge/AndroidWeekly-%23141-blue.svg)](http://androidweekly.net/issues/issue-141)

#Hawk
Secure, simple key-value storage for android

<img src='https://github.com/orhanobut/hawk/blob/master/images/hawk-logo.png' width='128' height='128'/>

Hawk uses:
- AES for the crypto
- SharedPreferences for the storage
- Gson for parsing

Hawk provides:
- Secure data persistence
- Save any type
- Save list of any type

###Add dependency
```groovy
compile 'com.orhanobut:hawk:1.14'
```

#### Initialize the hawk
```java
Hawk.init(context, PASSWORD); // might take 200ms
```
if you don't put any password, it will use another approach, it is faster but less secure.

```java
Hawk.init(context);   // very fast
```

init takes 200-400ms depends on the device. You may want to use async solution in order to avoid this. Add a callback to init and it will work asynchronous.
```java
Hawk.init(context, PASSWORD, new Hawk.Callback() {
        @Override
        public void onSuccess() {
        }

        @Override
        public void onFail(Exception e) {
        }
    }
);
```

You can use no-crypto mode if you don't want encryption. This mode will be automatically used if the device does not
support AES, PBE algorithm.
```java
Hawk.initWithoutEncryption(context);
```

#### Save
```java
Hawk.put(key, T); // Returns the result as boolean
```
or
```java
Hawk.put(key, List<T>); // Returns the result as boolean
```
You can also store multiple items at once by using chain feature. Remember to use commit() at the end. Either all of them will be saved or none.
```java
// Returns the result as boolean
Hawk.chain()
     .put(KEY_LIST, List<T>)
     .put(KEY_ANOTHER,"test")
     .commit();
```

#### Get
```java
T result = Hawk.get(key);
```
or with default value

```java
T result = Hawk.get(key, T);
```

#### Remove
```java
Hawk.remove(key); // Returns the result as boolean
```
or you can remove multiple items at once
```java
Hawk.remove(KEY_LIST, KEY_NAME); // Returns the result as boolean
```
#### Contains
```java
boolean contains = Hawk.contains(key);
```

#### Set the log output (optional)
```java
Hawk.init(context,PASSWORD, LogLevel.FULL); // as default it is NONE
```

##### More samples for save

```java
Hawk.put("key", "something"); // Save string
Hawk.put("key", true); // save boolean
Hawk.put("key", new Foo()); // save an object
Hawk.put("key", List<String>); // save list
Hawk.put("key", List<Foo>); // save list of any type
Hawk.put("key", 1234); // save numbers
```

##### More samples for get

```java
String value = Hawk.get(key);
int value = Hawk.get(key);
Foo value = Hawk.get(key);
boolean value = Hawk.get(key);
List<String> value = Hawk.get(key);
List<Foo> value = Hawk.get(key);
```
or with the defaults
```java
String value = Hawk.get(key, "");
int value = Hawk.get(key, 0);
Foo value = Hawk.get(key, new Foo());
boolean value = Hawk.get(key, false);
List<String> value = Hawk.get(key, Collections.emptyList());
List<Foo> value = Hawk.get(key, new ArrayList<Foo>);
```

##### Benchmark result (ms)
Done with Nexus 4, Android L. Note that this is not certain values, I just made a few runs and show it to give you an idea.

<img src='https://github.com/orhanobut/hawk/blob/master/images/benchmark.png'/>

##### How Hawk works

<img src='https://github.com/orhanobut/hawk/blob/master/images/flow-chart.png'/>

##### Notes
- Password should be provided by the user, we try to find better solution for this.
- Hawk.init() takes around 200-500ms depends on the phone.
- Salt key is stored plain text in the storage currently. We are checking to find a better solution for this. Any contribution about this will be great help as well.

##### Credits
I use the following implementation for the crypto and I believe it should get more attention. Thanks for this great hard work. https://github.com/tozny/java-aes-crypto and a great article about it : http://tozny.com/blog/encrypting-strings-in-android-lets-make-better-mistakes/

#### You might also like
- [Wasp](https://github.com/orhanobut/wasp) All-in-one network solution
- [Bee](https://github.com/orhanobut/bee) QA/Debug tool
- [DialogPlus](https://github.com/orhanobut/dialogplus) Easy,simple dialog solution
- [SimpleListView](https://github.com/orhanobut/simplelistview) Simple basic listview implementation with linearlayout

###License
<pre>
Copyright 2015 Orhan Obut

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
</pre>
