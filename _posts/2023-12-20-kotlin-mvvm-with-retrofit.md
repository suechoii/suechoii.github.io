---
layout: post
title: "Kotlin - MVVM with Retrofit"
category: [Kotlin]
date: 2023-12-20 17:04 +0800
---
![diagram](/assets/img/MVVM.png)

- MVVM pattern (Model, View, ViewModel) has view-related logic dependent on ViewModel
- In other words, UI data is in **ViewModel** and **View** is responsible for **observing** this ViewModel and updating any data that has been changed. This is done with the use of **LiveData**
- View consists of xml, Activity, Fragment
- Model refers to the internal / external DB 
- When data to be used in View is requested, **Repository** is used for Room or Retrofit to return data to ViewModel. Repository performs **callback** to ViewModel and ViewModel responses to observer. 
- Room is an internal database, whereas Retrofit uses the network (external DB) to send back data to ViewModel.

Steps to take : 
1. Create a Retrofit interface (can create multiple services), and return result with Response type
  ```kotlin
  interface HomeService {
      /* 홈 화면 잔디밭 최신 게시글 */
      @GET("/home/univ/recent")
      suspend fun getUnivRecentPost(
      ): Response<HomeListResponse>
  }
  ```
2. Get Retrofit service in Repository 
  ```kotlin
  class HomeRepository
  @Inject
  constructor(private val homeService: HomeService) {
    suspend fun getUnivRecentPost():Result<Response<HomeListResponse>> = runCatching {
        homeService.getUnivRecentPost()
    }
  ```
3. Link the ViewModel with Repository 
  ```kotlin
  @HiltViewModel
  class HomeViewModel
  @Inject
  constructor(
      private val repository: HomeRepository,
  ) : ViewModel() {
      private val _banner = MutableLiveData<List<Banner>>()
      val banner: LiveData<List<Banner>> get() = _banner

      fun getBanner() {
        viewModelScope.launch(Dispatchers.IO) {
            repository.getBanner().onSuccess { response ->
                if (response.isSuccessful && response.body()?.code == 1000) {
                    Log.d("tag_success", "getBannerList: ${response.body()}")
                    _banner.postValue(response.body()!!.result)
                }
            }.onFailure {
                // TODO: 배너 불러오기 실패 오류
            }
        }
    }
  }
  ```

- What is **Coroutine** in Kotlin?
  - This allows asynchronous programming, mainly for background tasks such as retreiving data from the network. 
  - 
- What is **suspend** fun?
  - When Coroutine sees a suspend function while working on the main thread, it executes this suspend function and resumes to working on the main thread. 
  - Coroutine functions must have suspend in front of it. 
  - Context switching is also possible.
  ![diagram](/assets/img/Coroutine.png)
  *source: https://wooooooak.github.io/kotlin/2019/08/25/코틀린-코루틴-개념-익히기/*
  
- Git repo for templates : https://github.com/android/architecture-templates 