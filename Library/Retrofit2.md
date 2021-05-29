# Retrofit2


## 📙시작하기 전에!

### Retrofit2이란?
<br/>

-   API 통신을 위해 구현된 OkHTTP의 HTTP 통신을 간편하게 만들어주는 라이브러리를 뜻합니다!
-   Async Task가 없이 Background 쓰레드를 실행 → Call Back을 통해 Main Thread에서 UI 업데이트를 해줍니다.

<br/>

### 주의할 점

-   이 글은 Github API를 사용한 예제를 가지고 있습니다. 경우에 맞게 바꿔 사용해주세요!

## 🤔Retrofit2의 장점은?
<br/>

가장 큰 장점 세개로 속도, 편의성, 가독성이 존재합니다.

-   OkHTTP에서는 사용 시에 AsyncTask를 통해 비동기로 실행하여 속도가 느리다는 이슈가 있었습니다. 하지만 Retrofit2에서는 자체적 비동기 실행과 스레드 관리로 속도를 빠르게 올렸습니다.
-   함수 호출 시에 파라미터를 넘겨주면 되기에 작업량이 훨씬 줄어들었습니다.
-   Interface 내에서 어노테이션을 사용하여 호출 함수를 미리 지정을 해두고, 구현 없이 해당 함수를 호출만 해주면 되기 때문에 코드를 읽기에 매우 편합니다. 또한 콜백 함수를 통한 직관적 설계가 가능합니다.

## 📂사용 방법
<br/>

### 라이브러리 추가하기

```
implementation 'com.squareup.retrofit2:retrofit:2.9.0' 
implementation 'com.squareup.retrofit2:converter-gson:2.9.0' 
implementation 'com.squareup.okhttp3:okhttp:4.9.0' 
// 현재 기재되어 있는 Retrofit2의 버전은 최신 버전입니다!
```

-   위의 라이브러리를 build.gradle 파일에 추가하여 줍니다.

<br/>

### 인터넷 권한 추가해주기


```
<uses-permission android:name="android.permission.INTERNET" />
```

-   위의 코드를 AndroidManifast.xml 파일에 추가하여 줍니다.

<br/>

### Retrofit Builder 생성

```kotlin
class RetrofitBuilder {

    val retrofit = Retrofit.Builder()
        .baseUrl("https://api.github.com/")
        .client(OkHttpClient())
        .addConverterFactory(GsonConverterFactory.create())
        .build()

    val githubApi = retrofit.create(GithubAPI::class.java)
}
```

-   baseUrl 함수에는 절대로 변하지 않는 주소값을 넣어줍니다
-   API의 주소가 [https://api.github.com/users/HYE0N1127](https://api.github.com/users/HYE0N1127) 일 시에 [https://api.github.com/](https://api.github.com/) 주소만 넣어주세요!
-   addConverterFactory에 GsonConverter를 추가해줍니다.
-   Gson이란? json 구조를 띄는 직렬화된 데이터를 JAVA의 객체로 역직렬화, 직렬화 해주는 자바 라이브러리입니다.

<br/>

### Data Class 생성


```kotlin
data class GithubInfo (
    val avatar_url: String?, 
    val bio: String?, 
    val login: String?, 
    val name: String? 
)
```

-   받아올 데이터를 Data Class에 추가해줍니다.
-   주의 : 받아온 데이터가 Null이 있을 수도 있기에, Nullable 기호를 사용해줍니다.
<br/>

### Interface 생성

```kotlin
interface GithubAPI {

    @GET("users/HYE0N1127")
    fun getGithubInfo(): Call<GithubInfo>
}
```

-   Interface를 생성해준 뒤에 어노테이션을 이용하여 HTTP Method를 설정해줍니다.
-   유저 아이디 부분에는 자신의 깃허브 아이디, 혹은 다른 사람의 깃허브 아이디를 넣어주세요
-   함수를 만들어 준 뒤 Call 안의 Generic 타입은 저희가 만들어준 Data Class의 이름을 넣어줍니다.
<br/>

### 데이터 받아오기

```kotlin
class MainActivity : AppCompatActivity() {
    call.enqueue(object : Callback<GithubInfo> {

        override fun onResponse(call: Call<GithubInfo>, response: Response<GithubInfo>) {
            val userInfo = response.body()

            Log.d("response","통신 성공")

        }

        override fun onFailure(call: Call<GithubInfo>, t: Throwable) {
            Log.d("error", t.message.toString())
        }
    })
}
```

-   response 파라미터를 github Data Class를 받아오는 것으로 설정을 해주었기에 userInfo 상수에 받아온 데이터를 넣어줍니다.
-   로그를 이용하여 데이터가 잘 받아와졌는지 확인해주었습니다.



이상으로 Android에서 서버통신을 하기 위한 거의 필수라고 볼 수 있는 Retrofit에 대하여 알아보았습니다.