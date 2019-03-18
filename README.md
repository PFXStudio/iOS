#iOS9 ATS

*	![demo](screenShot.png)

````
iOS9 대응하려고 빌드에러 잡고 실행을 해 보았는데 잘되던 인증과정이 에러가 난다..
Error Domain=NSURLErrorDomain Code=-1022 "The resource could not be loaded because the App Transport Security policy requires the use of a secure connection." UserInfo=0x7fb7abdb6d50 {NSUnderlyingError=0x7fb7ac22f2b0 "The resource could not be loaded because the App Transport Security policy requires the use of a secure connection.", NSErrorFailingURLStringKey=http://xxxxxx, NSErrorFailingURLKey=http://xxxxxx, NSLocalizedDescription=The resource could not be loaded because the App Transport Security policy requires the use of a secure connection.}

당황해 하고 있다가 겨우 찾아냄

Project Info

info tab

add NSAppTransportSecurity(Type Dictionary)

add NSAllowsArbitraryLoads(Type boolean) YES

````
````
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSExceptionDomains</key>
    <dict>
        <key>pubsub.pubnub.com</key>
        <dict>
            <key>NSExceptionAllowsInsecureHTTPLoads</key>
            <true/>
            <key>NSIncludesSubdomains</key>
            <true/>
        </dict>
    </dict>
</dict>

특정 도메인 예외 처리할 경우

````

#iOS9 Bitcode Error

*   ![demo](bitcodeError.png)

````
iOS9 대응 중 Archive 시 bitcode 빌드 에러가 난다.
does not contain bitcode. You must rebuild it with bitcode enabled (Xcode setting ENABLE_BITCODE), obtain an updated library from the vendor, or disable bitcode for this target. for architecture armv7

Build Settings - Enable Bitcode - NO로 변경

````

#iOS8 위치 정보를 못 얻어 오는 경우

````
프로젝트 설정파일인 Info.plist에 위치정보를 사용하는 이유를 추가
NSLocationAlwaysUsageDescription (항상 허용)
NSLocationWhenInUseUsageDescription (사용중인 경우만 허용)
````

#iOS9 Application windows are expected to have a root view controller at the end of application launch Crash!

````
*** Terminating app due to uncaught exception 'NSInternalInconsistencyException'reason : 'Application windows are expected to have a root view controller at the end of application launch'
*** First throw call stack :
(
    0 CoreFoundation 0x000000010b9cf7c5 __exceptionPreprocess + 165
    1 libobjc.A.dylib 0x000000010afdadcd objc_exception_throw +48
    2 CoreFoundation 0x000000010b9cf62a + NSException raise : format : arguments : + 106
    3 Foundation 0x000000010ac14bf1 - [NSAssertionHandler handleFailureInMethod : object : file : lineNumber : description : + 198
    4 UIKit 0x00000001097c3eca - [UIApplication _runWithMainScene : transitionContext : completion : + 2875
    5 UIKit 0x00000001097c1208 - [UIApplication workspaceDidEndTransaction : + 188
    6 FrontBoardServices 0x000000010fee1c0f - [FBSSerialQueue _performNext + 192
    7 FrontBoardServices 0x000000010fee1f7d - [FBSSerialQueue _performNextFromRunLoopSource + 45
    8 CoreFoundation 0x000000010b8f9ba1 __CFRUNLOOP_IS_CALLING_OUT_TO_A_SOURCE0_PERFORM_FUNCTION__ + 17
    9 CoreFoundation 0x000000010b8efacc __CFRunLoopDoSources0 + 556
    10 CoreFoundation 0x000000010b8eef83 __CFRunLoopRun + 867
    11 CoreFoundation 0x000000010b8ee998 CFRunLoopRunSpecific + 488
    12 UIKit 0x00000001097c0ba5 - [UIApplication _run + 402
    13 UIKit 0x00000001097c52cb UIApplicationMain + 171
    14 TestApp 0x0000000108968d8f main + 111
    15 libdyld.dylib 0x000000010d4e292d start + 1
    16 ??? 0x0000000000000001 0x0 + 1
)

````

해결 방안

````
-  ( BOOL ) application : ( UIApplication  * ) Application  didFinishLaunchingWithOptions : ( NSDictionary  * ) launchOptions
{
    // 윈도우 초기화
    self . window  =  [[ UIWindow  alloc ]  initWithFrame : [ UIScreen  MainScreen ]  bounds ]];
    self . window . rootViewController  =  [ UIViewController  new ];
    [ self . window  makeKeyAndVisible ];

    return  YES ;
}


````

#cocoaPods

````
sudo gem install cocoapods
cocoaPod을 사용한 프로젝트 폴더로 이동 한 후
pod install

````

#Crash dyld: Library not loaded:

````
Found the solution, in the target's General tab, there is an Embedded Binaries field, add framework there, and the crash is resolved.


````

참고

http://qiita.com/peromasamune/items/716de6da66dd31faeba8

#Crash dyld: Library not loaded: @rpath/

````

I was having this problem and was able to fix it by adding the Swift framework (MySDK.framework) to the "Embedded Binaries" section of the "General" tab of the Xcode project settings.

````

참고

http://stackoverflow.com/questions/24211549/dyld-library-not-loaded-rpath-mydsk-framework-mydsk-swift-ios-8-0

#APNS 개발 중 Client already exists

````
APNS 호출 시 동일 한 키 값으로 호출을 할 경우 나타나는 문제
유니크한 키 값으로 변경 해 주면 위 현상은 나타나지 않는다

````

#Crash Swift app crashes when trying to reference Swift library libswiftCore.dylib

````
프로젝트 빌드 파일 모두 삭제 후 인증서 재 설정하고 빌드 하니까 됨
한 컴퓨터에서 여러 인증서를 관리하다 보니 뭔가 꼬인듯..
````

참고

https://developer.apple.com/library/ios/qa/qa1886/_index.html

#file was built for i386 which is not the architecture being linked (x86_64)
````
프레임워크 만들어서 빌드 하였는데
file was built for i386 which is not the architecture being linked (x86_64)
위 에러가 나타난다. 클린 빌드 하면 빌드가 되고
빌드 설정에 Build Active Architecture Only explicitly
No로 설정
````

#Include of non-modular header inside framework module
````
You can set Allow Non-modular includes in Framework Modules in Build Settings for the affected target to YES.
This is the build setting you need to edit:
빌드 속성에
Allow Non-modular includes in Framework Modules 항목을
YES로 변경 하면 됨
````

# NSTimer invalidate not working
````
@property (weak, nonatomic) NSTimer *timer;
약한 참조로 해야 한다... 강한 참조로 하면 메모리 해제가 안돼서 timer가 살아 있다
````

# 약전계 테스트

http://nshipster.com/network-link-conditioner/

https://developer.apple.com/downloads/

# Storyboard TableViewController 뷰가 아래로 내려가는 문제

코드로

navigationBar.translucent = NO;

스토리보드 NavigationBar 속성에

Translucent 체크 해제

# xcode10 framework script Build

````
accessing build database "/Users/succorer/Documents/xxxx/build.db": database is locked Possibly there are two concurrent builds running in the same filesystem location.
에러 발생.

echo "BUILD DIRECTORY ${BUILD_DIR}"
xcodebuild -project ${PROJECT_NAME}.xcodeproj -sdk iphonesimulator -target ${PROJECT_NAME} -configuration ${CONFIGURATION} build CONFIGURATION_BUILD_DIR=${BUILD_DIR}/${CONFIGURATION}-iphonesimulator OBJROOT="${OBJROOT}/DependentBuilds"

xcodebuild -project ${PROJECT_NAME}.xcodeproj -sdk iphoneos -target ${PROJECT_NAME} -configuration ${CONFIGURATION} build CONFIGURATION_BUILD_DIR=${BUILD_DIR}/${CONFIGURATION}-iphoneos OBJROOT="${OBJROOT}/DependentBuilds"
````

# Let's Swift.

````
sourcekitd.Framework - xpc 분석 툴
sourcekitten

Ruby gem
  Carthage
  Bundler를 이용하여 설치
  -> gem install Bundler
  tachikoma : cocopods 외부 라이브러리 최신 버전으로 업데이트 해 줌 tachikoma.io
  houston : 로컬에서 푸시 노티피케이션 테스트 가능
  danger : Objc 코드, 스토리보드 코드 검증
  tenma : dev, master 브런치 제작
  fastlane : 앱 스토어 업로드 및 심사 신청
  bitrise

RxRibs

ReSwift









````

# PFXStudio

Mobile : http://pfxstudio.modoo.at/

Twitter : http://twitter.com/pfxstudio

Facebook : http://facebook.com/pfxstudio

Github : https://github.com/PFXStudio

iOS AppStore : https://itunes.apple.com/kr/artist/ppark/id448017898

Google Play : https://play.google.com/store/apps/developer?id=PFXStudio
