
@State

@Binding

ObservableObject

Published

@StateObject

@Environment

@EnvironmentObject


## MVVM


## Modifier 살펴보기

Text("Hello")
.font
.bold
.lineLimit
.foregroundColor
.background
.strikethrough
.monospaced


Image("bell.fill")
.resizable - 크기
.aspectRatio - 가로세로비율 유지 크기
.renderingMode - 템플릿
.sacledToFill
.scaledToFit


Button(action: signin, label: { 
	View 가 적용됨
		- Image
		- Text
		- VStack { }
})


HStack
- alignment: VeticalAlignment
- spacing: CGFloat
- @ViewBuilder content: () => Content

VStack
- alignment: HorizontalAlignment
- spacing: CGFloat
- @ViewBuilder content: () => Content

Spacer
.frame - 간격 지정


TabView(selection: { value })
- Rectangle()
	.fill(Color.red)
	.tabitem {  }
- Rectangle()
- Rectangle()


TabViewStyle
- automatic
- page
	- always
	- never
	- automatic