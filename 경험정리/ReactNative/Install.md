
## iOS


cocoapods 설치 시 ruby 버전으로 인한 오류

https://arc.net/l/quote/pjosltyw

ruby 버전 업데이트 시 rbenv 를 활용

이후 최소 2.7.5 버전 이상으로 맞춰준다.

이후 cocoapods 설치 후 pod 실행

```shell
$ rbenv versions
* system
  2.7.5

$ rbenv global 2.7.5

$ rbenv versions
  system
* 2.7.5 (set by /Users/hangyeongsu/.rbenv/version)

$ ruby --version
ruby 2.6.3p62 (2019-04-16 revision 67580) [universal.x86_64-darwin20]

$ which ruby
/usr/bin/ruby
```


이슈 해결

error Failed to launch the app on simulator, An error was encountered processing the command (domain=com.apple.CoreSimulator.SimError, code=405):



## Android



