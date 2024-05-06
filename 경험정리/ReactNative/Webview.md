
참고코드

```typescript
import React from "react"

import { BackHandler, Platform, View, Linking } from "react-native"

import Share from "react-native-share"

import { NavigationStackScreenProps } from "react-navigation-stack"

import { NavigationEventSubscription } from "react-navigation"

import { observer } from "mobx-react"

import { Url } from "@zigbang/utils"

import * as Zuix from "@zigbang/zuix"

import { ToolbarB } from "@zigbang/screens/components/layout/ToolbarB"

import { ToolbarItemTransformable } from "@zigbang/screens/components/layout/ToolbarItem"

import { NetworkError } from "@zigbang/screens/components/NetworkError"

import { DeviceInfos } from "@zigbang/screens/lib"

import { ViewDimension } from "@zigbang/screens/lib/ViewDimension"

import { KeyboardAvoidingWebView, WebView } from "@zigbang/screens/lib/WebView"

import { injectedJavascriptForWebviewPostMessage } from "@zigbang/screens/lib/WebViewUtil"

import { loginStore } from "@zigbang/screens/lib/Account"

import { sentry } from "@zigbang/screens/lib/sentry"

import { Firebase } from "@zigbang/bridge/firebase"

  

enum ButtonColor {

WHITE = "WHITE",

BLACK = "BLACK",

}

  

enum HeaderType {

NONE = "NONE",

TRANSPARENT = "TRANSPARENT",

DEFAULT = "DEFAULT",

}

  

enum WebViewState {

Loading,

Loaded,

Error,

}

  

interface EventWebViewScreenProps extends NavigationStackScreenProps {

zuixToast: any

}

  

@Zuix.withToast

@observer

export default class EventWebViewScreen extends React.Component<EventWebViewScreenProps, EventWebViewScreenState> {

static navigationOptions = ({ navigation }: any) => ({ ...navigation.state.params, header: null })

webViewRef: React.RefObject<WebView> = React.createRef<WebView>()

  

state = {

url: "",

title: "",

currentState: WebViewState.Loading,

header: HeaderType.DEFAULT,

buttonColor: ButtonColor.BLACK,

userAgent: "",

canGoForward: false,

canGoBack: false,

}

  

private subscriptions: NavigationEventSubscription[] = []

private _title: string = ""

  

async componentDidMount() {

this.subscriptions = [

this.props.navigation.addListener("didFocus", () => {

this.didFocusCallback()

}),

this.props.navigation.addListener("willBlur", () => {

if (Platform.OS === "android") {

BackHandler.removeEventListener("hardwareBackPress", this.backHandlerAction)

}

}),

]

const originUrl = this.props.navigation.getParam("url")

const isLoggedIn = await loginStore.isLoggedIn()

this.sendPostMessage({ name: "removeCookie", params: { name: "_zauth" } })

const params = {}

if (isLoggedIn) {

const zauth = await loginStore.getZAuth()

const [{ user_no }, { value: adid, uuid }] = await Promise.all([

loginStore.내정보(),

DeviceInfos.getAdIdObject(),

])

if (user_no) Object.assign(params, { user_no })

if (adid) Object.assign(params, { adid })

if (uuid) Object.assign(params, { uuid })

if (zauth) Object.assign(params, { zauth })

} else {

const { value: adid, uuid } = await DeviceInfos.getAdIdObject()

if (adid) Object.assign(params, { adid })

if (uuid) Object.assign(params, { uuid })

}

const url = Url.updateQueryStringParams(originUrl, params)

const userAgent = await DeviceInfos.getUserAgent()

this.setState({ url, userAgent })

}

  

reload = () => {

this.setState({

currentState: WebViewState.Loading,

})

this.webViewRef.current?.reload()

}

  

renderHeader() {

const { buttonColor, header, title } = this.state

switch (header) {

case HeaderType.NONE:

return null

case HeaderType.TRANSPARENT:

const alpha = buttonColor === ButtonColor.BLACK ? 1 : 0

return (

<ToolbarB title="" alpha={0} paddingTop={ViewDimension.statusBarHeight}>

<ToolbarB.Left>

<ToolbarItemTransformable buttonIconType="close" onPress={this.close} alpha={alpha} />

</ToolbarB.Left>

</ToolbarB>

)

case HeaderType.DEFAULT:

default:

return (

<Zuix.ToolbarA

extendTopPadding={ViewDimension.statusBarHeight}

title={title}

navigatorIcon={{

src: require("@zigbang/screens/static/ic_actionbar_close_30x30_nor_black.png"),

onPress: this.close,

}}

/>

)

}

}

  

renderIndicator() {

const { currentState } = this.state

return (

currentState === WebViewState.Loading && (

<View style={{ width: "100%", height: "100%" }}>

<Zuix.IndicatorA />

</View>

)

)

}

  

renderError() {

const { currentState } = this.state

return currentState === WebViewState.Error && <NetworkError onPress={this.reload} />

}

  

renderWebView() {

const { header, url, userAgent } = this.state

const WebViewComponent = Platform.OS === "ios" ? KeyboardAvoidingWebView : WebView

const style = {

flex: 1,

height: "100%",

backgroundColor: "#ffffff",

paddingTop: header === HeaderType.DEFAULT ? 0 : ViewDimension.statusBarHeight,

}

if (!url) return null

return (

<WebViewComponent

originWhitelist={["*"]}

injectedJavaScript={injectedJavascriptForWebviewPostMessage}

onMessage={this.onPostMessage}

onLoadEnd={this.onLoadEnd}

onLoad={this.onLoad}

onError={this.onError}

ref={this.webViewRef}

onShouldStartLoadWithRequest={(event) => {

if (

event.url.startsWith("http://") ||

event.url.startsWith("https://") ||

event.url.startsWith("about:blank")

) {

return true

}

if (Platform.OS === "android") {

Linking.openURL(event.url.substring(7, event.url.length)).catch((err) => {

if (event.url.includes("kakao")) {

this.props.zuixToast?.show("카카오톡이 설치되어 있지 않습니다")

}

})

} else {

Linking.openURL(event.url).catch((err) => {

if (event.url.includes("kakao")) {

this.props.zuixToast?.show("카카오톡이 설치되어 있지 않습니다")

}

})

return false

}

}}

source={{ uri: url }}

style={style}

scrollEnabled

javaScriptEnabled

domStorageEnabled

allowsInlineMediaPlayback

allowsFullscreenVideo

cacheEnabled={false}

userAgent={userAgent}

onNavigationStateChange={this.onNavigationStateChange}

// onShouldStartLoadWithRequest={this.onShouldStartLoadWithRequest}

/>

)

}

  

render() {

return (

<View style={{ flex: 1, backgroundColor: "#ffffff", position: "relative" }}>

{/* {this.renderHeader()} */}

{this.renderIndicator()}

{this.renderError()}

{this.renderWebView()}

</View>

)

}

  

componentWillUnmount() {

this.subscriptions.forEach((sub) => {

sub.remove()

})

}

  

didFocusCallback = async () => {

const isLoggedIn = await loginStore.isLoggedIn()

if (isLoggedIn) {

await this.웹뷰에인증정보전달()

} else {

this.setState({

currentState: WebViewState.Loaded,

})

}

if (Platform.OS === "android") {

BackHandler.addEventListener("hardwareBackPress", this.backHandlerAction)

}

}

  

onLoad = async () => {}

  

onError = async (params) => {

this.setState({

currentState: WebViewState.Error,

title: params?.title || this._title || "",

})

}

  

onLoadEnd = async () => {

const { currentState } = this.state

  

if (currentState === WebViewState.Loading) {

await this.웹뷰에인증정보전달()

this.sendPostMessage({ name: "onLoadWebView" })

this.setState({

currentState: WebViewState.Loaded,

})

}

}

  

close = () => {

this.sendPostMessage({ name: "removeCookie", params: { name: "_zauth" } })

this.props.navigation.goBack()

}

  

share = async (options) => {

try {

const response = await Share.open(options)

if (response) {

// if needed

}

} catch (err) {}

}

  

웹뷰에인증정보전달 = async () => {

const zauth = await loginStore.getZAuth()

if (zauth) {

this.sendPostMessage({

name: "setCookie",

params: {

name: "_zauth",

value: zauth,

},

})

// this.reload()

// 현재 시나리오(미로그인시 자동으로 로그인 모달 띄우기) 상으로는 불필요한 기능임으로 주석 처리

// 만약 웹뷰의 특정 로그인 버튼을 통해 로그인 모달이 띄워지고, 로그인 후 즉각적인 webview 페이지의 업데이트가 필요하다면 reload 필요

}

}

  

private webviewInterfaceMethod = () => {

const contentLoaded = (params) => {

const { buttonColor, header, title } = params

this.setState({

currentState: WebViewState.Loaded,

title: title || this._title || "이벤트",

header: header || HeaderType.DEFAULT,

buttonColor: buttonColor || ButtonColor.BLACK,

})

}

const login = () => {

this.props.navigation.navigate("LoginModal", {

initLogin: false,

referrer: "EventWebView|interfacemethod|PUSH|NAVIGATE",

})

}

const isLoggedIn = async () => {

const isLoggedIn = await loginStore.isLoggedIn()

  

this.sendPostMessage({

name: "isLoggedIn",

params: {

isLoggedIn,

},

})

}

const moveToPage = (params) => {

const { routeName, routeParams } = params

try {

if (!routeParams) {

this.props.navigation.replace(`${routeName}`)

return

}

  

this.props.navigation.replace(`${routeName}`, routeParams)

} catch (err) {

sentry.notify(err)

}

}

  

const webviewFirebase = (eventType) => {

Firebase.analytics().logEvent(eventType, {})

}

  

return {

webviewFirebase,

contentLoaded,

login,

close: this.close,

share: this.share,

error: this.onError,

moveToPage,

isLoggedIn,

}

}

  

private sendPostMessage = (params) => {

if (this.webViewRef?.current) {

this.webViewRef.current.postMessage(JSON.stringify(params), "*")

}

}

  

private onPostMessage = (e) => {

const { nativeEvent: { data = "", title = "" } = {} } = e

this._title = title

try {

const { name, params = {} } = JSON.parse(data)

const method = this.webviewInterfaceMethod()

if (method[name]) {

method[name](params)

}

} catch (e) {}

}

  

private backHandlerAction = () => {

if (this.state.canGoBack) {

this.webViewRef.current && this.webViewRef.current.goBack?.()

} else {

this.close()

return false

}

  

return true

}

  

private onNavigationStateChange = (navState) => {

const { canGoForward, canGoBack, title } = navState

this.setState({

canGoForward,

canGoBack,

})

}

}

  

interface EventWebViewScreenState {

url: string

currentState: WebViewState

header: HeaderType

buttonColor: ButtonColor

title: string

userAgent?: string

canGoForward: boolean

canGoBack: boolean

}
```