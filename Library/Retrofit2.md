# Retrofit2


## ๐์์ํ๊ธฐ ์ ์!

### Retrofit2์ด๋?
<br/>

-   API ํต์ ์ ์ํด ๊ตฌํ๋ OkHTTP์ HTTP ํต์ ์ ๊ฐํธํ๊ฒ ๋ง๋ค์ด์ฃผ๋ ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ฅผ ๋ปํฉ๋๋ค!
-   Async Task๊ฐ ์์ด Background ์ฐ๋ ๋๋ฅผ ์คํ โ Call Back์ ํตํด Main Thread์์ UI ์๋ฐ์ดํธ๋ฅผ ํด์ค๋๋ค.

<br/>

### ์ฃผ์ํ  ์ 

-   ์ด ๊ธ์ Github API๋ฅผ ์ฌ์ฉํ ์์ ๋ฅผ ๊ฐ์ง๊ณ  ์์ต๋๋ค. ๊ฒฝ์ฐ์ ๋ง๊ฒ ๋ฐ๊ฟ ์ฌ์ฉํด์ฃผ์ธ์!

## ๐คRetrofit2์ ์ฅ์ ์?
<br/>

๊ฐ์ฅ ํฐ ์ฅ์  ์ธ๊ฐ๋ก ์๋, ํธ์์ฑ, ๊ฐ๋์ฑ์ด ์กด์ฌํฉ๋๋ค.

-   OkHTTP์์๋ ์ฌ์ฉ ์์ AsyncTask๋ฅผ ํตํด ๋น๋๊ธฐ๋ก ์คํํ์ฌ ์๋๊ฐ ๋๋ฆฌ๋ค๋ ์ด์๊ฐ ์์์ต๋๋ค. ํ์ง๋ง Retrofit2์์๋ ์์ฒด์  ๋น๋๊ธฐ ์คํ๊ณผ ์ค๋ ๋ ๊ด๋ฆฌ๋ก ์๋๋ฅผ ๋น ๋ฅด๊ฒ ์ฌ๋ ธ์ต๋๋ค.
-   ํจ์ ํธ์ถ ์์ ํ๋ผ๋ฏธํฐ๋ฅผ ๋๊ฒจ์ฃผ๋ฉด ๋๊ธฐ์ ์์๋์ด ํจ์ฌ ์ค์ด๋ค์์ต๋๋ค.
-   Interface ๋ด์์ ์ด๋ธํ์ด์์ ์ฌ์ฉํ์ฌ ํธ์ถ ํจ์๋ฅผ ๋ฏธ๋ฆฌ ์ง์ ์ ํด๋๊ณ , ๊ตฌํ ์์ด ํด๋น ํจ์๋ฅผ ํธ์ถ๋ง ํด์ฃผ๋ฉด ๋๊ธฐ ๋๋ฌธ์ ์ฝ๋๋ฅผ ์ฝ๊ธฐ์ ๋งค์ฐ ํธํฉ๋๋ค. ๋ํ ์ฝ๋ฐฑ ํจ์๋ฅผ ํตํ ์ง๊ด์  ์ค๊ณ๊ฐ ๊ฐ๋ฅํฉ๋๋ค.

## ๐์ฌ์ฉ ๋ฐฉ๋ฒ
<br/>

### ๋ผ์ด๋ธ๋ฌ๋ฆฌ ์ถ๊ฐํ๊ธฐ

```
implementation 'com.squareup.retrofit2:retrofit:2.9.0' 
implementation 'com.squareup.retrofit2:converter-gson:2.9.0' 
implementation 'com.squareup.okhttp3:okhttp:4.9.0' 
// ํ์ฌ ๊ธฐ์ฌ๋์ด ์๋ Retrofit2์ ๋ฒ์ ์ ์ต์  ๋ฒ์ ์๋๋ค!
```

-   ์์ ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ฅผ build.gradle ํ์ผ์ ์ถ๊ฐํ์ฌ ์ค๋๋ค.

<br/>

### ์ธํฐ๋ท ๊ถํ ์ถ๊ฐํด์ฃผ๊ธฐ


```
<uses-permission android:name="android.permission.INTERNET" />
```

-   ์์ ์ฝ๋๋ฅผ AndroidManifast.xml ํ์ผ์ ์ถ๊ฐํ์ฌ ์ค๋๋ค.

<br/>

### Retrofit Builder ์์ฑ

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

-   baseUrl ํจ์์๋ ์ ๋๋ก ๋ณํ์ง ์๋ ์ฃผ์๊ฐ์ ๋ฃ์ด์ค๋๋ค
-   API์ ์ฃผ์๊ฐ [https://api.github.com/users/HYE0N1127](https://api.github.com/users/HYE0N1127) ์ผ ์์ [https://api.github.com/](https://api.github.com/) ์ฃผ์๋ง ๋ฃ์ด์ฃผ์ธ์!
-   addConverterFactory์ GsonConverter๋ฅผ ์ถ๊ฐํด์ค๋๋ค.
-   Gson์ด๋? json ๊ตฌ์กฐ๋ฅผ ๋๋ ์ง๋ ฌํ๋ ๋ฐ์ดํฐ๋ฅผ JAVA์ ๊ฐ์ฒด๋ก ์ญ์ง๋ ฌํ, ์ง๋ ฌํ ํด์ฃผ๋ ์๋ฐ ๋ผ์ด๋ธ๋ฌ๋ฆฌ์๋๋ค.

<br/>

### Data Class ์์ฑ


```kotlin
data class GithubInfo (
    val avatar_url: String?, 
    val bio: String?, 
    val login: String?, 
    val name: String? 
)
```

-   ๋ฐ์์ฌ ๋ฐ์ดํฐ๋ฅผ Data Class์ ์ถ๊ฐํด์ค๋๋ค.
-   ์ฃผ์ : ๋ฐ์์จ ๋ฐ์ดํฐ๊ฐ Null์ด ์์ ์๋ ์๊ธฐ์, Nullable ๊ธฐํธ๋ฅผ ์ฌ์ฉํด์ค๋๋ค.
<br/>

### Interface ์์ฑ

```kotlin
interface GithubAPI {

    @GET("users/HYE0N1127")
    fun getGithubInfo(): Call<GithubInfo>
}
```

-   Interface๋ฅผ ์์ฑํด์ค ๋ค์ ์ด๋ธํ์ด์์ ์ด์ฉํ์ฌ HTTP Method๋ฅผ ์ค์ ํด์ค๋๋ค.
-   ์ ์  ์์ด๋ ๋ถ๋ถ์๋ ์์ ์ ๊นํ๋ธ ์์ด๋, ํน์ ๋ค๋ฅธ ์ฌ๋์ ๊นํ๋ธ ์์ด๋๋ฅผ ๋ฃ์ด์ฃผ์ธ์
-   ํจ์๋ฅผ ๋ง๋ค์ด ์ค ๋ค Call ์์ Generic ํ์์ ์ ํฌ๊ฐ ๋ง๋ค์ด์ค Data Class์ ์ด๋ฆ์ ๋ฃ์ด์ค๋๋ค.
<br/>

### ๋ฐ์ดํฐ ๋ฐ์์ค๊ธฐ

```kotlin
class MainActivity : AppCompatActivity() {
    call.enqueue(object : Callback<GithubInfo> {

        override fun onResponse(call: Call<GithubInfo>, response: Response<GithubInfo>) {
            val userInfo = response.body()

            Log.d("response","ํต์  ์ฑ๊ณต")

        }

        override fun onFailure(call: Call<GithubInfo>, t: Throwable) {
            Log.d("error", t.message.toString())
        }
    })
}
```

-   response ํ๋ผ๋ฏธํฐ๋ฅผ github Data Class๋ฅผ ๋ฐ์์ค๋ ๊ฒ์ผ๋ก ์ค์ ์ ํด์ฃผ์๊ธฐ์ userInfo ์์์ ๋ฐ์์จ ๋ฐ์ดํฐ๋ฅผ ๋ฃ์ด์ค๋๋ค.
-   ๋ก๊ทธ๋ฅผ ์ด์ฉํ์ฌ ๋ฐ์ดํฐ๊ฐ ์ ๋ฐ์์์ก๋์ง ํ์ธํด์ฃผ์์ต๋๋ค.



์ด์์ผ๋ก Android์์ ์๋ฒํต์ ์ ํ๊ธฐ ์ํ ๊ฑฐ์ ํ์๋ผ๊ณ  ๋ณผ ์ ์๋ Retrofit์ ๋ํ์ฌ ์์๋ณด์์ต๋๋ค.