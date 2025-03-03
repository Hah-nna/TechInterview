### 2-1. Activity 생명주기(life cycle)에 대해서 설명해주세요

각 콜백함수가 담당하는 주 역할에 대해 정리(교재참조)

![image](https://github.com/user-attachments/assets/c7caf026-11e0-40ec-9eb6-7154e35792d2)


onCreate → onStart()→ onResume()→ onPause()→ onStop()→ onReStart()→OnDestroy()

**onCreate()** : 맨 처음 앱이 생성될때 한번만 생성되고, 여기서 텍스트인 xml을 inflate해서 Activity화면에 붙이는 역할 : setContentView()

**onStart()** :  사용자가 앱의 화면을 볼 수 있다.

**onResume(**): 사용자가 앱과 상호작용할 수 있다.

**onPause()**: 사용자가 백키 누르거나 다른 액티비티가 기존 화면 앞으로 왔을 경우 호출된다.

사용자가 다시 앱 화면으로 돌아오면 onResume()이 호출되고 사용자는 앱과 상호작용할 수있다.

사용자가 앱 화면을 완전히 나갈을 때 onStop()이 호출된다(화면이 사라질경우) 

**onStop()** : 사용자가 완전히 화면에서 벗어나 액티비티화면이 보이지 않을 경우 호출된다. 

**onReStart()**: 사용자가 다시 화면으로 돌아오면 onStop() → onRestart()상태로 변경

**onDestroy()**:사용자가 완전히 화면을 벗어나 앱을 닫으면 onStop() → onDestroy()

### 2-2. 추가 질문

생명주기가 필요한 이유는? 

- 앱은 웹에 비해서 메모리 관리가 중요한데, 앱 화면이 사용자에게 보이지 않을경우에도 계속 실행되고 있으면 메모리 낭비이다. 그래서 onPause(), onStop()상태에 들어가서 메모리 관리를 해야한다. 메모리가 부족할 경우 앱이 꺼질 수도 있다(최악의 상황)
- 사용자가 앱을 나갔다가 다시 돌아왔을 때도 사용자의 정보를 기억하고 있어야 한다. onPause() →onResume(), 되었을 때 사용자가 입력한 정보가 유지되어야 한다. 생명주기없이 화면이 보이고 사라지고 한다면 사용자의 정보를 저장하기가 힘들다.
- 생명주기를 이용해 개발자가 직접 리소스를 관리할 수 있음. 예를 들어 카메라기능(화면)이 onPause()상태에 들어가면→ 여기서는 카메라 기능 멈추고, 다운로드 화면이 onStop()에 들어가면 다운로드 중지해서 메모리사용을 줄이자 이렇게 사용자가 앱의 상황에 맞게 메모리 관리를 할 수 있음 
