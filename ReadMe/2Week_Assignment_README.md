# 2주차 과제:fire::fire::fire: 

## 구동 화면 

![2Week_Assignment_Capture](/ReadMe/ReadMeAsset/2Week_Simulater.gif) 

## 01. AutoLayout, StackView, ScrollView사용하기  

![2Week_Assignment1](/ReadMe/ReadMeAsset/2Week_Assignment1.png)  

### 1) 스크롤 뷰가 너무 길어서 작업하기가 불편하면?? 

![2Week_Assignment2](/ReadMe/ReadMeAsset/2Week_Assignment2.png)  

Intrinsic Size 를 scrollview placeholder로 설정해준다!  
그러면 본래 뷰컨트롤러 사이즈 그대로 두고 작업이 가능하다!  

### 2) Label이 왜 잘려나오지??  
![2Week_Assignment3](/ReadMe/ReadMeAsset/2Week_Assignment3.png)  
Label 사이즈가 정해져있는데 그 사이즈를 넘어가니 잘려보이는 것!  
제플린과 똑같이 나오게 해주려면 두가지 설정을 해주어야한다. 

#### 1. Lines  
Lines는 해당 Label의 라인 수를 지정해주는 속성이다.  
제플린을 보면 알 수 있듯 우리가 넣어주는 Label text는 두줄을 넘어가지 않는다.  
그러니까 Lines를 2로 맞춰주면 잘리지 않는 걸 확인할 수 있다! 


#### 2. Line break  
Lines를 맞춰주긴했는데, 몇몇 애들은 제플린이랑 결과가 똑같이 나오지 않는다! 왜이러지ㅠㅠ  
왜냐면 Label의 줄넘김 설정이 다르기 때문!  
일반적으로 Label의 사이즈를 넘어가면 마지막 라인의 뒷부분을 생략처리 해주는 Truncate Tail이 디폴트로 설정되어있다.  
이걸 Character Wrap으로 바꿔주면 제플린과 똑같이 줄넘김 되는 걸 볼 수 있다.  
Character Wrap은 개별 문자단위로 줄넘김을 해준다는 설정이다.  
단어가 완전히 끝나지 않아도 사이즈를 넘으면 바로 다음 줄로 넘긴다.  
다른 설정들도 여러개 있는데 이제 이름 보면 어떻게 줄넘김이 되는지 짐작이 될 것!  

### 3) AutoLayout 잡기  
![2Week_Assignment4](/ReadMe/ReadMeAsset/2Week_Assignment4.png)  
사실 이번 과제 오토레이아웃은 세미나 때 배운 것 만으로 충분히 만들 수 있다!  
나는 세미나 때처럼 스택뷰를 이중으로 만들어주고, 가장 안쪽 스택뷰에 UIView를 넣고 그 안에 UIImageView와 Label을 넣어주었다.  

오토레이아웃이 제대로 잡히지 않는다면, 잡아야할 곳을 누락시켰거나 중복되게 잡았을 가능성이 크다.  
차근차근 하다보면 잘 할 수 있읍ㄴㅣ다...  
하다보면 느는 오토레이아웃 매직 ㅜㅠ  
멋진 아요 OB들 중에는 오토레이아웃 장인이 많으니 막히면 물어보면 좋다. (민희, 예슬, 지훈, 지은한테 물어보세요)   

## 02. Top up Button 만들어주기 (도전과제!🤮) 

으악! 도전과제가 저번주보다 훨씬 어렵잖아??! 악!  
하지만 할 수 있다!

### 1. Contentoffset, ContentSize
이번 도전과제를 하려면 Contentoffset, ContentSize의 개념을 알아야한다.
Contentoffset는 유저가 화면을 scrolling 했을 때 표시되는 화면의 위치(지점)를 의미한다.  
즉 유저가 scrolling을 한다면 계속해서 Contentoffset는 바뀌는 것!  

ContentSize는 말그대로 ContentView의 사이즈를 의미한다.  

![2Week_Assignment5](/ReadMe/ReadMeAsset/2Week_Assignment5.png)  
이 사진을 보면 대충 무슨 느낌인지 알 수 있다.  

이제 이 ContentOffset을 알면 TopUp IBAction 을 만들 수 있다.  
버튼을 누르면 지정된 ContentOffset으로 이동하도록하는 메소드가 있다.  
setContentOffset를 사용하면 된다!  
이번 과제 같은 경우는 맨 위로 올라가는 버튼이니까 x, y 좌표를 0으로 설정해준다!  

```swift
        soptWorkingScrollView.setContentOffset(CGPoint(x: 0, y: 0), animated: true)
```

### 2.  UIScrollViewDelegate
Topup버튼의 조건은, 버튼이 보이지 않다가 상단의 배너사진 만큼 스크롤 되었을 때 나타나도록 하는 것이다.  
이를 위해선 UIScrollViewDelegate의 메소드인 **scrollViewDidScroll**를 사용해야한다.  

![2Week_Assignment5](/ReadMe/ReadMeAsset/2Week_Assignment6.png)  
애플 developer사이트에 들어가면 해당 메소드에 대한 설명을 볼 수 있다.  
scrollViewDidScroll()은 유저가 스크롤을 할때마다 호출된다.   
scrollViewDidScroll말고도 scrollViewWillBeginDragging, scrollViewWillEndDragging 등등이 있다.  
더 다양한 친구들은 [여기서](https://yagom.net/forums/topic/uitableview에서-주로-사용되는-uiscrollviewdelegate를-알아보자/) 찾아볼 수 있다.


나는 아래처럼 scrollview의 contentoffset의 y값이 배너 사진의 height 값보다 더 커졌을때 topup button이 나타나도록 scrollViewDidScroll()에 코드를 짜주었다. 
```Swift
    func scrollViewDidScroll(_ scrollView: UIScrollView) {
        if(soptWorkingScrollView.contentOffset.y >  (bannerImg.image?.size.height)!){
                topButton.isHidden = false
        }
        else {
            topButton.isHidden = true
        }
    }
```

이를 사용하긴 위해선 class를 선언할 때 UIScrollViewDelegate를 추가해주어야하고,   

빼놓지 않고  viewdiddload()에 아래의 코드도 선언을 해주어야한다!
```Swift
soptWorkingScrollView.delegate = self
```

제일 처음 topbutton 세팅때에는 버튼을 안보이게 해주는 것도 빼먹으면 안된다!
```Swift
topButton.isHidden = true
```

그러면 도전과제까지 완성!ㅠ   
너무 어렵네여 ㅠㅠ  
