
# package 설치
React 컴파일러는 ESLint 플러그인, 이때 생각할 점은 **모든 eslint 위반 사항을 즉시 수정할 필요는 없습니다**
```
npm install -D eslint-plugin-react-hooks@^6.0.0-rc.1
```

react 의 버전을 직접적으로 업그레이드가 불가능 하다면?
```
npm install react-compiler-runtime@rc
```


제품 코드의 작은 디렉토리에서 실행해 보는 것이 좋다. 이를 위해 컴파일러를 특정 디렉토리 집합Set에서만 실행하도록 구성할 수 있다.

```
const ReactCompilerConfig = {  

sources: (filename) => {  

return filename.indexOf('src/path/to/dir') !== -1;  

},  

};
```