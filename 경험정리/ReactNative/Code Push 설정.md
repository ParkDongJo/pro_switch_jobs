
code-push
https://github.com/microsoft/react-native-code-push

app center
https://github.com/microsoft/appcenter-cli/blob/master/README.md

code-push-cli
https://github.com/shm-open/code-push-cli?tab=readme-ov-file

code-push-server
https://github.com/shm-open/code-push-server?tab=readme-ov-file



AppCenter 를 통한 Codepush 설정
https://ingg.dev/codepush/



https://codepush.appcenter.ms/



정리를 해보자!

react-native-code-push 에서 가장 핵심은 root 에 들어 있는
CodePush.js 다.


여기서 결국 CodePush 라는 객체가 있는데 이는 codePushify() 라는 함수가 할당된다.

codePushify 함수는  가장 시작점이 되는 함수라고 볼수 있는데 여기에는

decorator() 라는 내장 함수가 있다.
이 내장 함수는 

decorator(options) 라는 객체를 받게 된다.

options 는 RootComponent 인데, 결국 App 에서 CodePush() 라는 함수 안에 <App /> 루트 컴포넌트를 넘겨주게 되어 있다.
이는 Codepush 의 사용법이다.


그다음 decorator 에서는 CodePushComponent 라는 내부 class 컴포넌트가 정의되어 있다.

이 내부 컴포넌트에서 Codepush.sync() 를 하게 되는데
이 부분이 Codpush 에서 SDK 와 sycn 를 맞추는 부분이다.

대략적으로
- 코드 의 업데이트 상태를 체크하고
- 코드 를 다운로드 하고
- 버전이나 몇가지 설정들을 체크하고 설정하는 함수로 보인다.

여기서 코드를 다운 받는 곳은 
codePushDownloadDidProgress() 이다. 이는 CodePush.sync() 에서


downloadProgressCallback 로 넘겨진다.

이 함수의 역할은 앱센터에서 넘겨받은 업데이트 정보를 App 의 react 코드에 반영하는 역할로 보인다.


sync() 함수 내부를 보면

downloadProgressCall() 함수를 받아서 실행시켜주고 있다. 이때 downloadProgressCallbackWithTryCatch() 라는 고차 함수에서 args 를 받아서 넘겨주고 있는데

downloadProgressCallbackWithTryCatch

syncInternal() 이라는 함수에 인자로 넘겨지고 있다.

syncInternal() 함수를 보면


doDownloadAndInstall() 라는 함수가 있다.

결국 doDownloadAndInstall() 함수를 통해 새로운 버전을 다운 받고  반영한다고 보면 된다.

여기서 끝난게 아니다. 이 다운로드는 doDownloadAndInstall() 함수 안에서 

remotePackage.download(downloadProgressCallback) 로 이뤄지고 있다.
그러면 remotePackage는 뭔가?

remotePackage 는 checkForUpdate() 함수에서 뱉어주고 있다.
아무래도 update 를 해야하는지 체크하고 해야한다면, update 에 대한 데이터를 리턴해주는 함수로 보인다.

checkForUpdate 를 보면


getPromisifiedSdk() 함수를 통해 sdk 를  뱉어주고 있다.
이때 requestFetchAdapter를 넘겨주고, config 를 넘겨주는데 

먼저 requestFetchAdapter 는 데이터를 받아오는 fetch 가 정의되어 있는 객체이다.
그리고 config 는 네이티브 코드에서 getConfiguration() 를 통해 config 를 가져오는 객체이다.

getPromisifiedSdk() 는

queryUpdateWithCurrentPackage
reportStatusDeploy
reportStatusDownload

가 정의되어 있는데 결국 이 내부에도 AcquisitionSdk 에서 call 로 바인딩 시키고 있다.
AcquisitionSdk 는 code-push/script/acquisition-sdk 에서 제공해주는 함수들이다.


code-push/script/acquisition-sdk 는

code-push 라는 연계된 라이브러리이다. 

여기서 가장 중요한거건 queryUpdateWithCurrentPackage() 이다


queryUpdateWithCurrentPackage 는 code-push에 정의되어 있다.
이 함수 안에서 this._httpRequester.request() 라는 코드로 이전에 넘겨받은 requestFetchAdapter에 정의된 fetch 를 이 내부에서 실행시켜준다.


안에서 보면 callback() 에 결국 최종 response 를 넘겨주는데, 이 respons 는 requestFetchAdapter에서 실행시킨 fetch() 의 응답값이다.

그리고 여기서 callback() 함수는 getPromisifiedSdk() 안에 정의된 

 (err, update) => {  
  if (err) {  
    reject(err);  
  } else {  
    resolve(update);  
  }  
}

콜벡 함수가 있는데 이것이다. 결국 sdk.queryUpdateWithCurrentPackage 안에 업데이트에 대한 정보가 담기게 된다.

이 함수는 

const update = await sdk.queryUpdateWithCurrentPackage(queryPackage); 라는 코드로

update 정보를 뱉게 되고 이 update 는 checkForUpdate() 의 결과값으로 뱉어지게 된다.

위에서 살펴봤지만 checkForUpdate() 함수는 remotePackage를 뱉어주는 역할이였고, 이 remotePackage 는 아래의 코드로 인해 다운로드가 되는것이다.

remotePackage.download(downloadProgressCallback)

.download() 함수는 update 정보에 담겨있지는 않다.

...PackageMixins.remote(sdk.reportStatusDownload) 로 데이터를 뱉어주고 있는데,

PackageMixins가 정의되어 있는 package-mixins.js로 가보면
download() 에 대한 정의가 되어 있다.

download 에서는 최종적으로 { ...downloadedPackage, ...local } 가 return 되는데

이는

NativeCodePush.downloadUpdate() 에서 만들어진다.

downloadUpdate 는
Native 코드에서 CodePushNativeModule (java 기준) 에서 정의되어 있는데 아래 코드이다.



```java
@ReactMethod  
public void downloadUpdate(final ReadableMap updatePackage, final boolean notifyProgress, final Promise promise) {  
    AsyncTask<Void, Void, Void> asyncTask = new AsyncTask<Void, Void, Void>() {  
        @Override  
        protected Void doInBackground(Void... params) {  
            try {  
                JSONObject mutableUpdatePackage = CodePushUtils.convertReadableToJsonObject(updatePackage);  
                CodePushUtils.setJSONValueForKey(mutableUpdatePackage, CodePushConstants.BINARY_MODIFIED_TIME_KEY, "" + mCodePush.getBinaryResourcesModifiedTime());  
                mUpdateManager.downloadPackage(mutableUpdatePackage, mCodePush.getAssetsBundleFileName(), new DownloadProgressCallback() {  
                    private boolean hasScheduledNextFrame = false;  
                    private DownloadProgress latestDownloadProgress = null;  
  
                    @Override  
                    public void call(DownloadProgress downloadProgress) {  
                        if (!notifyProgress) {  
                            return;  
                        }  
  
                        latestDownloadProgress = downloadProgress;  
                        // If the download is completed, synchronously send the last event.  
                        if (latestDownloadProgress.isCompleted()) {  
                            dispatchDownloadProgressEvent();  
                            return;  
                        }  
  
                        if (hasScheduledNextFrame) {  
                            return;  
                        }  
  
                        hasScheduledNextFrame = true;  
                        getReactApplicationContext().runOnUiQueueThread(new Runnable() {  
                            @Override  
                            public void run() {  
                                ReactChoreographer.getInstance().postFrameCallback(ReactChoreographer.CallbackType.TIMERS_EVENTS, new ChoreographerCompat.FrameCallback() {  
                                    @Override  
                                    public void doFrame(long frameTimeNanos) {  
                                        if (!latestDownloadProgress.isCompleted()) {  
                                            dispatchDownloadProgressEvent();  
                                        }  
  
                                        hasScheduledNextFrame = false;  
                                    }  
                                });  
                            }  
                        });  
                    }  
  
                    public void dispatchDownloadProgressEvent() {  
                        getReactApplicationContext()  
                                .getJSModule(DeviceEventManagerModule.RCTDeviceEventEmitter.class)  
                                .emit(CodePushConstants.DOWNLOAD_PROGRESS_EVENT_NAME, latestDownloadProgress.createWritableMap());  
                    }  
                }, mCodePush.getPublicKey());  
  
                JSONObject newPackage = mUpdateManager.getPackage(CodePushUtils.tryGetString(updatePackage, CodePushConstants.PACKAGE_HASH_KEY));  
                promise.resolve(CodePushUtils.convertJsonObjectToWritable(newPackage));  
            } catch (CodePushInvalidUpdateException e) {  
                CodePushUtils.log(e);  
                mSettingsManager.saveFailedUpdate(CodePushUtils.convertReadableToJsonObject(updatePackage));  
                promise.reject(e);  
            } catch (IOException | CodePushUnknownException e) {  
                CodePushUtils.log(e);  
                promise.reject(e);  
            }  
  
            return null;  
        }  
    };  
  
    asyncTask.executeOnExecutor(AsyncTask.THREAD_POOL_EXECUTOR);  
}
```

여기서도 중요한건 mUpdateManager.downloadPackage() 이다

어떤 함수인지 까지 들여다보진 않겠지만. 다운로드가 되면
dispatchDownloadProgressEvent() 를 실행시켜주고

내부 함수인 dispatchDownloadProgressEvent 는 어디엔가 최종 결과물을 전달하는 것으로 보인다.

```
getReactApplicationContext()  
        .getJSModule(DeviceEventManagerModule.RCTDeviceEventEmitter.class)  
        .emit(CodePushConstants.DOWNLOAD_PROGRESS_EVENT_NAME, latestDownloadProgress.createWritableMap());
```

를 통해서 emit 을 하는 걸 보니

그리고 그 다운로드 된 결과를 downloadProgressCallback 로 전달된된다. 

App.js 에 보면

codePushDownloadDidProgress(progress) {  
  this.setState({ progress });  
}

로 정의되어 있고, 이 progress 라는 것이 그 최종 결과로 보인다.